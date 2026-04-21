---
agent: agent
---

# Table Component Guidelines

## Overview

The **Table** component (`@neuron/ui/Table`) is a powerful wrapper around PrimeReact's DataTable that provides simplified APIs for common table operations including pagination, filtering, sorting, row actions, infinite scroll, and row expansion.

**Version:** v4.4.0

### New Features in v4.4.0

- **Infinite Scroll**: Progressive data loading with configurable height and load more button
- **Row Expansion (Master-Detail)**: Expandable rows with custom templates and controlled state
- **Global Filter Callback**: Access current filter value via `onGlobalFilterChange`
- **Pagination Settings**: Enhanced control over pagination display and behavior
- **Built-in Filtering Control**: Toggle between frontend and backend filtering

---

## Critical Pattern: Dynamic Columns and Row Actions

### ⚠️ Important: Dynamic Action Properties

The Table component uses a React `key` to determine when to re-render. The key includes:

- Loading state
- Column definitions (field/header)
- Row action properties (name, type, variant, iconLeft, disabled)

**Table Key Implementation:**

```tsx
const tableKey = useMemo(() => {
  const loadingKey = isLoading ? "loading" : "not-loading";
  const columnsKey = columnDefs.map((col) => col.field || col.header).join("-") || "no-columns";
  const actionsKey =
    rowActions
      ?.map((action) => {
        const disabledState =
          typeof action.disabled === "function" ? "fn" : action.disabled === true ? "disabled" : "enabled";
        const iconKey = typeof action.iconLeft === "string" ? action.iconLeft : JSON.stringify(action.iconLeft);
        return `${action.name}-${action.type}-${action.variant}-${iconKey || ""}-${disabledState}`;
      })
      .join("-") || "no-actions";
  return `${loadingKey}-${columnsKey}-${actionsKey}`;
}, [isLoading, columnDefs, rowActions]);
```

**Supported dynamic changes:**

- ✅ Action `name`
- ✅ Action `type`
- ✅ Action `variant` (primary, secondary, danger, etc.)
- ✅ Action `iconLeft` (string or icon object)
- ✅ Action `disabled` state (boolean or function)
- ✅ Completely replacing actions array

**⚠️ Known Limitation:**
Due to PrimeReact DataTable's internal row rendering optimization, changing the `disabled` property on rowActions may not immediately reflect visually even though the table key changes. The disabled state IS functionally applied (onClick won't fire), but the button styling may not update until the next full table data refresh.

**Workaround:** If you need immediate visual feedback, consider:

1. Using conditional rendering to show/hide action columns entirely
2. Triggering a data refresh when toggling action states
3. Using function-based `disabled: (row) => condition` which evaluates per-row

### Best Practices for Dynamic Tables

#### ✅ **DO: Use useMemo for Dynamic Columns**

```tsx
const columns: TableColumn<User>[] = useMemo(() => {
  const cols: TableColumn<User>[] = [
    { field: "id", header: "ID", sortable: true },
    { field: "firstName", header: "First Name", sortable: true },
  ];

  // Dynamically add columns based on state
  if (showAge) {
    cols.push({ field: "age", header: "Age", sortable: true });
  }

  return cols;
}, [showAge]); // Dependencies trigger re-render
```

#### ✅ **DO: Use useMemo for Dynamic Row Actions**

```tsx
const [actionsEnabled, setActionsEnabled] = useState(true);

const rowActions: TableRowAction<User>[] = useMemo(
  () => [
    {
      name: "View",
      type: "view",
      variant: "secondary",
      iconLeft: "eye",
      disabled: !actionsEnabled, // ✅ Toggle disabled state
      onActionClick: (user: User) => console.log("View:", user),
    },
  ],
  [actionsEnabled],
); // Dependencies trigger re-render
```

#### ✅ **DO: Dynamically Change Action Properties**

```tsx
const [editMode, setEditMode] = useState(false);

const rowActions: TableRowAction<User>[] = useMemo(
  () => [
    {
      name: editMode ? "Save" : "Edit",
      type: editMode ? "save" : "edit",
      variant: editMode ? "primary" : "secondary", // ✅ Dynamic variant
      iconLeft: editMode ? "check" : "pen", // ✅ Dynamic icon
      onActionClick: (user: User) => {
        if (editMode) {
          saveUser(user);
        } else {
          enableEdit(user);
        }
      },
    },
  ],
  [editMode],
); // ✅ Re-renders when editMode changes
```

#### ❌ **DON'T: Define Columns or Actions Inline**

```tsx
// Bad - Creates new array on every render
<Table
  columns={[
    { field: "id", header: "ID" },
    showAge && { field: "age", header: "Age" },
  ].filter(Boolean)}
  rowActions={actionsEnabled ? [...] : undefined}
/>
```

---

## Dynamic Cell Rendering (column.body)

The Table supports fully custom per-cell rendering via `column.body`. This is the correct way to render dynamic UI (e.g., Tag vs. Switch) inside a cell that changes based on row state or row actions.

### Key points

- Provide a `body: (row) => ReactNode` function on the column.
- Memoize `columns` with `useMemo` and include all state that affects cell content (e.g., `editingRowId`, any per-row values) in the dependency array so React re-renders the cell when these values change.
- Keep `sortable` to `false` for columns rendering non-primitive dynamic content, unless you implement a separate `sortField`.
- Ensure `dataKey` is a stable unique key (e.g., `id`) so row-level updates don’t cause unintended re-renders of other rows.

### Example: Tag in view mode, Switch in edit mode

```tsx
const [editingRowId, setEditingRowId] = useState<number | null>(null);
const [rowStatus, setRowStatus] = useState<Record<number, boolean>>({});

const columns: TableColumn<User>[] = useMemo(() => {
  return [
    { field: "id", header: "ID", width: 80, sortable: true },
    { field: "firstName", header: "First Name", sortable: true },
    {
      field: "status",
      header: "Status",
      width: 140,
      sortable: false,
      body: (row) => {
        const id = (row as User).id as number | undefined;
        if (!id) return null;
        const value = !!rowStatus[id];
        const isEditing = editingRowId === id;

        return isEditing ? (
          <Switch
            name={`status-${id}`}
            checked={value}
            onChange={(checked) => setRowStatus((prev) => ({ ...prev, [id]: !!checked }))}
            labelText=""
          />
        ) : (
          <Tag text={value ? "Active" : "Inactive"} variant={value ? "success" : "danger"} />
        );
      },
    },
  ];
}, [editingRowId, rowStatus]);
```

This pattern lets a single row flip its Status cell between a Switch (editing) and a Tag (view). The re-render is scoped to that row because you keep `dataKey="id"` and only update the relevant state entries.

### Row actions to toggle the cell content

Use row actions to switch a row into edit mode and render inline controls via the dynamic `body`:

```tsx
const rowActions: TableRowAction<User>[] = useMemo(() => {
  const save: TableRowAction<User> = {
    name: "Save",
    type: "save",
    variant: "primary",
    iconLeft: baseIcons.floppyDiskSolid,
    disabled: (row) => editingRowId !== (row as User).id,
    onActionClick: (user) => {
      // Persist if needed
      setEditingRowId(null);
    },
  };

  const cancel: TableRowAction<User> = {
    name: "Cancel",
    type: "cancel",
    variant: "secondary",
    disabled: (row) => editingRowId !== (row as User).id,
    onActionClick: (user) => {
      // Revert and exit edit mode
      setEditingRowId(null);
    },
  };

  const edit: TableRowAction<User> = {
    name: "Edit",
    type: "edit",
    variant: "primary",
    iconLeft: baseIcons.penSolid,
    onActionClick: (user) => setEditingRowId((prev) => (prev === user.id ? null : (user.id as number))),
  };

  const view: TableRowAction<User> = {
    name: "View",
    type: "view",
    variant: "secondary",
    iconLeft: baseIcons.eyeSolid,
    onActionClick: (user) => console.info("View:", user),
  };

  // Put Cancel/Save first when in edit mode so they render as buttons for that row
  return editingRowId !== null ? [cancel, save, view, edit] : [view, edit, cancel, save];
}, [editingRowId]);
```

Notes:

- The table renders the first two enabled actions as buttons; the rest (including disabled) go into the dropdown.
- On small screens (desktop-s and below), all actions are placed in the dropdown.
- Order your actions accordingly; for inline edit patterns, place `Cancel` and `Save` first during edit mode so they appear as buttons for that row.

---

## Sorting with Backend Pagination

### Required Configuration

For sorting to work correctly with backend pagination, you need **THREE critical pieces**:

```tsx
const [sortCriteria, setSortCriteria] = useState<ISortCriteria>({
  sortOrder: 1, // 1 = asc, 2 = desc
  attributeName: "id", // default sort field
});

// 1. Pass sortCriteria in data object
const tableData: ITableData<User[]> = useMemo(() => {
  return {
    results: users,
    pagination: {
      offset: page * pageSize,
      limit: pageSize,
      total: totalCount,
    },
    sortCriteria, // ✅ Required for sorting
  };
}, [users, page, pageSize, totalCount, sortCriteria]);

// 2. Implement onSortChange handler
const handleSortChange = (newSortCriteria: ISortCriteria) => {
  setSortCriteria(newSortCriteria);
  setPage(0); // Reset to first page on sort
};

// 3. Pass sortMode and onSortChange to Table
<Table
  data={tableData}
  sortMode="single" // ✅ Required prop
  onSortChange={handleSortChange} // ✅ Required handler
  columns={columns} // columns must have sortable: true
/>;
```

### Column Configuration for Sorting

```tsx
const columns: TableColumn<User>[] = [
  { field: "id", header: "ID", sortable: true }, // ✅ Enable sorting
  { field: "firstName", header: "First Name", sortable: true },
  { field: "lastName", header: "Last Name", sortable: false }, // ❌ Disable sorting
];
```

---

## Filtering with Backend Pagination

### Configuration

```tsx
import { FilterMatchMode } from "primereact/api";

// Define initial filters
const initialFilters = {
  id: { value: null, matchMode: FilterMatchMode.CONTAINS },
  firstName: { value: null, matchMode: FilterMatchMode.CONTAINS },
  lastName: { value: null, matchMode: FilterMatchMode.CONTAINS },
};

// Implement filter handler (optional for monitoring)
const handleFilter = (event: DataTableStateEvent) => {
  console.log("Filters changed:", event.filters);
  // Optionally reset to first page
  setPage(0);
};

<Table
  isColumnFilterEnabled={true} // ✅ Enable column filters
  initialFilters={initialFilters} // ✅ Initialize filter inputs
  onFilter={handleFilter} // Optional monitoring
/>;
```

### Frontend vs Backend Filtering

#### Frontend Filtering (Default)

```tsx
<Table
  builtInFilteringEnabled={true} // Default behavior
  isColumnFilterEnabled={true}
  initialFilters={initialFilters}
/>
```

- Filters current page data on the client
- Works for small datasets or paginated results
- Users can only filter visible rows

#### Backend Filtering (Advanced)

```tsx
<Table
  builtInFilteringEnabled={false} // Delegate to backend
  isColumnFilterEnabled={true}
  initialFilters={initialFilters}
  onFilter={handleFilter}
/>;

const handleFilter = (event: DataTableStateEvent) => {
  const filters = event.filters;
  // Send filters to backend API
  refetch({ filters: transformFilters(filters) });
};
```

---

## Row Actions

### Correct Structure

```tsx
import { TableRowAction } from "@neuron/ui";

const rowActions: TableRowAction<User>[] = [
  {
    name: "View", // Display name
    type: "view", // Action type identifier
    variant: "secondary", // Button variant
    iconLeft: "eye", // Icon name
    onActionClick: (user: User, actionType: string) => {
      console.log("Action:", actionType, "User:", user);
    },
  },
  {
    name: "Delete",
    type: "delete",
    variant: "danger",
    iconLeft: "trash",
    disabled: (user: User) => user.id === 1, // Conditional disable
    onActionClick: (user: User, actionType: string) => {
      // Handle delete
    },
  },
];

<Table
  rowActions={rowActions}
  rowActionsHeader="Actions" // Optional column header
/>;
```

### Key Properties

- **`name`**: Display text for the action button
- **`type`**: Unique identifier for the action
- **`variant`**: Button style (`"primary"`, `"secondary"`, `"danger"`, `"success"`)
- **`iconLeft`**: Icon to display in the button
- **`onActionClick`**: Callback with `(rowData, actionType)` parameters
- **`disabled`**: Can be a boolean or function `(rowData) => boolean` to conditionally disable action

### Rendering behavior

- The first two enabled actions render as inline buttons; remaining actions are available in the actions dropdown (disabled actions show in dropdown).
- On smaller screens (≤ desktop-s breakpoints), all actions are placed in the dropdown for better responsiveness.
- Order your actions accordingly; for inline edit patterns, place `Cancel` and `Save` first during edit mode so they appear as buttons for that row.

### Conditional Disabling Patterns

#### Per-Row Conditional Disabling

```tsx
const rowActions: TableRowAction<User>[] = [
  {
    name: "Delete",
    type: "delete",
    variant: "danger",
    iconLeft: "trash",
    disabled: (user: User) => user.id === 1, // Disable for specific rows
    onActionClick: (user: User, actionType: string) => {
      // Handle delete
    },
  },
];
```

#### Global Disabling (All Rows)

```tsx
const [actionsEnabled, setActionsEnabled] = useState(true);

const rowActions: TableRowAction<User>[] = useMemo(
  () => [
    {
      name: "View",
      type: "view",
      variant: "secondary",
      iconLeft: "eye",
      disabled: !actionsEnabled, // All View buttons disabled when false
      onActionClick: (user: User) => console.log("View:", user),
    },
  ],
  [actionsEnabled],
);
```

---

## Pagination

### Backend Pagination (Recommended)

```tsx
const [page, setPage] = useState(0);
const [pageSize, setPageSize] = useState(10);

const { data: response } = useGetUsersQuery({
  limit: pageSize,
  skip: page * pageSize,
});

const tableData: ITableData<User[]> = {
  results: response?.users || [],
  pagination: {
    offset: page * pageSize,
    limit: pageSize,
    total: response?.total || 0,
  },
};

const handlePaginationChange = (pagination: { offset: number; limit: number; total: number }) => {
  const newPage = Math.floor(pagination.offset / pagination.limit);
  setPage(newPage);
  setPageSize(pagination.limit);
};

<Table data={tableData} onPaginationChange={handlePaginationChange} />;
```

---

## Infinite Scroll

### Configuration

The Table component supports infinite scroll for loading data progressively as the user scrolls down.

```tsx
const [page, setPage] = useState(0);
const [pageSize] = useState(20);

const { data: response } = useGetUsersQuery({
  limit: pageSize,
  skip: page * pageSize,
});

const tableData: ITableData<User[]> = {
  results: response?.users || [],
  pagination: {
    offset: page * pageSize,
    limit: pageSize,
    total: response?.total || 0,
  },
};

const handlePaginationChange = (pagination: { offset: number; limit: number; total: number }) => {
  const newPage = Math.floor(pagination.offset / pagination.limit);
  setPage(newPage);
};

<Table
  data={tableData}
  infiniteScroll={{
    height: "400px", // Fixed height
    total: response?.total || 0,
  }}
  onPaginationChange={handlePaginationChange}
/>;
```

### Infinite Scroll Options

```tsx
interface InfiniteScrollConfig {
  height: string; // "400px" for fixed height, "flex" for flexible height
  total: number; // Total number of records
  pageSize?: number; // Optional page size override
}
```

### Flexible Height

```tsx
<Table
  data={tableData}
  infiniteScroll={{
    height: "flex", // Adapts to container height
    total: response?.total || 0,
  }}
  onPaginationChange={handlePaginationChange}
/>
```

### Load More Button

When using infinite scroll with full dataset, a "Load More" button appears at the bottom:

```tsx
<Table
  data={tableData}
  infiniteScroll={{
    height: "400px",
    total: 500,
  }}
  isLoading={isLoading}
/>
```

---

## Row Expansion (Master-Detail)

### Basic Row Expansion

```tsx
const [expandedRows, setExpandedRows] = useState<DataTableExpandedRows>({});

const rowExpansionTemplate = (data: User) => {
  return (
    <div className="p-16">
      <h5>
        Details for {data.firstName} {data.lastName}
      </h5>
      <p>Email: {data.email}</p>
      <p>Phone: {data.phone}</p>
      <p>Address: {data.address}</p>
    </div>
  );
};

<Table
  columns={columns}
  data={tableData}
  rowExpansionTemplate={rowExpansionTemplate}
  expandedRows={expandedRows}
  onRowExpand={(e) => console.log("Row expanded:", e.data)}
  onRowCollapse={(e) => console.log("Row collapsed:", e.data)}
/>;
```

### Controlled Expanded Rows

```tsx
const [expandedRows, setExpandedRows] = useState<DataTableExpandedRows>({});

const handleRowExpand = (e: { data: User }) => {
  console.log("Expanded:", e.data);
  // Optionally load additional data
  fetchUserDetails(e.data.id);
};

const handleRowCollapse = (e: { data: User }) => {
  console.log("Collapsed:", e.data);
};

<Table
  columns={columns}
  data={tableData}
  rowExpansionTemplate={rowExpansionTemplate}
  expandedRows={expandedRows}
  onRowExpand={handleRowExpand}
  onRowCollapse={handleRowCollapse}
/>;
```

### Disable Row Expander Conditionally

```tsx
<Table
  columns={columns}
  data={tableData}
  rowExpansionTemplate={rowExpansionTemplate}
  isRowExpanderDisabled={(row: User) => !row.hasDetails}
/>
```

---

## Global Filter Callback

Access the current global filter value using the `onGlobalFilterChange` callback:

```tsx
const [globalFilterValue, setGlobalFilterValue] = useState<string>("");

const handleGlobalFilterChange = (value: string) => {
  setGlobalFilterValue(value);
  console.log("Current filter:", value);
  // Use the value for analytics, external filtering, etc.
};

<Table
  columns={columns}
  data={tableData}
  isGlobalFilterEnabled={true}
  onGlobalFilterChange={handleGlobalFilterChange}
/>;
```

---

## Pagination Settings

Control pagination display and behavior with `paginationSettings`:

```tsx
<Table
  columns={columns}
  data={tableData}
  paginationSettings={{
    showPaginationForSinglePage: false, // Hide pagination when only 1 page
    hidePagination: false, // Completely hide pagination
    hidePaginationControls: false, // Hide prev/next buttons
    hidePageSizeSelector: false, // Hide page size dropdown
    hidePaginationText: false, // Hide "Showing X to Y of Z" text
  }}
/>
```

### Common Pagination Configurations

#### Minimal Pagination

```tsx
<Table
  data={tableData}
  paginationSettings={{
    hidePaginationText: true,
    hidePageSizeSelector: true,
  }}
/>
```

#### Auto-hide for Single Page

```tsx
<Table
  data={tableData}
  paginationSettings={{
    showPaginationForSinglePage: false,
  }}
/>
```

---

## Built-in Filtering Control

Control whether filtering happens on the client or is delegated to the backend:

### Frontend Filtering (Default)

```tsx
<Table
  columns={columns}
  data={tableData}
  builtInFilteringEnabled={true} // Default
  isColumnFilterEnabled={true}
  initialFilters={initialFilters}
/>
```

### Backend Filtering

```tsx
<Table
  columns={columns}
  data={tableData}
  builtInFilteringEnabled={false} // Delegate to backend
  isColumnFilterEnabled={true}
  initialFilters={initialFilters}
  onFilter={(event) => {
    // Send filters to backend
    const filters = event.filters;
    refetch({ filters: transformFilters(filters) });
  }}
/>
```

---

## Complete Example: Dynamic Table with All Features

```tsx
import React, { useState, useMemo } from "react";
import { Table, Button, TableColumn, TableRowAction, ITableData } from "@neuron/ui";
import { ISortCriteria } from "@neuron/core";
import { FilterMatchMode } from "primereact/api";

interface User {
  id: number;
  firstName: string;
  lastName: string;
  age: number;
  email: string;
}

const initialFilters = {
  firstName: { value: null, matchMode: FilterMatchMode.CONTAINS },
  lastName: { value: null, matchMode: FilterMatchMode.CONTAINS },
};

export const DynamicTable: React.FC = () => {
  const [page, setPage] = useState(0);
  const [pageSize, setPageSize] = useState(10);
  const [sortCriteria, setSortCriteria] = useState<ISortCriteria>({
    sortOrder: 1,
    attributeName: "id",
  });
  const [showAge, setShowAge] = useState(true);
  const [actionsEnabled, setActionsEnabled] = useState(true);

  // Fetch data (replace with your API call)
  const { data: response, isLoading } = useGetUsersQuery({
    limit: pageSize,
    skip: page * pageSize,
  });

  // Dynamic columns with useMemo
  const columns: TableColumn<User>[] = useMemo(() => {
    const cols: TableColumn<User>[] = [
      { field: "id", header: "ID", width: 80, sortable: true },
      { field: "firstName", header: "First Name", sortable: true },
      { field: "lastName", header: "Last Name", sortable: true },
    ];

    if (showAge) {
      cols.push({ field: "age", header: "Age", width: 100, sortable: true });
    }

    return cols;
  }, [showAge]);

  // Dynamic row actions with useMemo
  const rowActions: TableRowAction<User>[] | undefined = useMemo(() => {
    if (!actionsEnabled) return undefined;

    return [
      {
        name: "View",
        type: "view",
        variant: "secondary",
        iconLeft: "eye",
        onActionClick: (user: User) => console.log("View:", user),
      },
      {
        name: "Edit",
        type: "edit",
        variant: "primary",
        iconLeft: "pen",
        onActionClick: (user: User) => console.log("Edit:", user),
      },
    ];
  }, [actionsEnabled]);

  // Table data with sortCriteria
  const tableData: ITableData<User[]> = useMemo(
    () => ({
      results: response?.users || [],
      pagination: {
        offset: page * pageSize,
        limit: pageSize,
        total: response?.total || 0,
      },
      sortCriteria,
    }),
    [response, page, pageSize, sortCriteria],
  );

  // Handlers
  const handlePaginationChange = (pagination: { offset: number; limit: number; total: number }) => {
    setPage(Math.floor(pagination.offset / pagination.limit));
    setPageSize(pagination.limit);
  };

  const handleSortChange = (newSortCriteria: ISortCriteria) => {
    setSortCriteria(newSortCriteria);
    setPage(0);
  };

  return (
    <div>
      <div className="d-flex gap-8 mb-16">
        <Button onClick={() => setShowAge(!showAge)} variant="secondary">
          {showAge ? "Hide" : "Show"} Age Column
        </Button>
        <Button onClick={() => setActionsEnabled(!actionsEnabled)} variant="secondary">
          {actionsEnabled ? "Disable" : "Enable"} Actions
        </Button>
      </div>

      <Table
        columns={columns}
        data={tableData}
        isLoading={isLoading}
        dataKey="id"
        sortMode="single"
        isColumnFilterEnabled={true}
        initialFilters={initialFilters}
        onPaginationChange={handlePaginationChange}
        onSortChange={handleSortChange}
        rowActions={rowActions}
      />
    </div>
  );
};
```

---

## Common Pitfalls

### ❌ Missing sortCriteria in data object

```tsx
// Bad - Sorting won't work
const tableData = {
  results: users,
  pagination: { ... },
  // Missing sortCriteria!
};
```

### ❌ Missing sortMode prop

```tsx
// Bad - Columns won't be sortable
<Table
  columns={columns}
  onSortChange={handleSort}
  // Missing sortMode="single"!
/>
```

### ❌ Forgetting to mark columns as sortable

```tsx
// Bad - Column won't have sort icon
const columns = [
  { field: "name", header: "Name" }, // Missing sortable: true
];
```

### ❌ Inline column definitions

```tsx
// Bad - Creates new array every render, breaks memoization
<Table
  columns={[
    { field: "id", header: "ID" },
    { field: "name", header: "Name" },
  ]}
/>
```

---

## Testing Dynamic Behavior

Use the starter app test page at `/table-pagination-test` to see all patterns in action:

- **Test 1**: Backend pagination with filtering
- **Test 2**: Frontend filtering on paginated data
- **Test 3**: Tab switching with cache
- **Test 4**: Force refresh scenarios
- **Test 5**: Dynamic columns and row actions

---

## Performance Optimization

### Use useMemo for Expensive Computations

```tsx
// Memoize table data transformation
const tableData: ITableData<User[]> = useMemo(() => {
  return {
    results: transformUsers(response?.users),
    pagination: { ... },
    sortCriteria,
  };
}, [response, page, pageSize, sortCriteria]);
```

### Memoize Callbacks

```tsx
const handleSortChange = useCallback((newSortCriteria: ISortCriteria) => {
  setSortCriteria(newSortCriteria);
  setPage(0);
}, []);
```

## Sync Metadata

- **Component Version:** v4.4.5
- **Component Source:** `packages/neuron/ui/src/lib/data/table/Table.tsx`
- **Guideline Command:** `/neuron-ui-table`
- **Related Skill:** `neuron-ui-data`

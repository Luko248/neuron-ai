---
agent: agent
---

# AI-Assisted Neuron Sorting Component Integration Guide

## For the AI Assistant

Your task is to integrate and configure the Neuron Sorting components in a React application. This guide provides comprehensive instructions for implementing the Sorting component, which provides custom sorting functionality with support for ascending, descending, or no sorting states using tri-state checkboxes and toggle buttons.

## Sync Metadata

- **Component Version:** v4.2.2
- **Component Source:** `packages/neuron/ui/src/lib/helpers/sorting/Sorting.tsx`
- **Guideline Command:** `/neuron-ui-sorting`
- **Related Skill:** `neuron-ui-helpers`

## Introduction

The Sorting component is a flexible helper component that allows users to configure sorting options for data displays. It supports both tri-state checkboxes (for ascending/descending/none states) and toggle buttons (for boolean sorting values), with single or multiple selection modes.

### Key Features

- **Tri-State Sorting**: Ascending, descending, or no sorting using tri-state checkboxes
- **Boolean Sorting**: Simple on/off sorting using toggle buttons
- **Single/Multiple Modes**: Support for single selection or multiple concurrent sorting
- **Mixed Action Types**: Combine tri-state checkboxes and toggle buttons in one component
- **Visual Feedback**: Icons indicate sorting direction (up arrow, down arrow, or both)
- **Controlled/Uncontrolled**: Support for both controlled and uncontrolled usage patterns
- **Accessibility**: Full keyboard navigation and screen reader support

**Storybook Reference:** You can refer to the Storybook documentation for visual examples and interactive demos of the Sorting component.

## Step 1: Basic Sorting Implementation

### 1.1 Import the Sorting Component

```tsx
import { Sorting } from "@neuron/ui";
import type { SortingActionProps } from "@neuron/ui";
```

### 1.2 Basic Uncontrolled Usage

Here's a simple implementation with mixed action types:

```tsx
import { Sorting } from "@neuron/ui";
import type { SortingActionProps } from "@neuron/ui";

const BasicSortingExample = () => {
  const sortingActions: SortingActionProps[] = [
    {
      label: "Name",
      type: "TriStateCheckbox",
    },
    {
      label: "Date",
      type: "ToggleButton",
    },
    {
      label: "Status",
      type: "TriStateCheckbox",
    },
    {
      label: "Priority",
      type: "ToggleButton",
    },
  ];

  const handleTriCheckboxChange = (index: number, value: boolean | null) => {
    console.info(`Tri-state checkbox ${index} changed to:`, value);
    // value: true = ascending, false = descending, null = no sorting
  };

  const handleToggleButtonChange = (index: number, checked: boolean) => {
    console.info(`Toggle button ${index} changed to:`, checked);
    // checked: true = sorting enabled, false = sorting disabled
  };

  return (
    <Sorting
      labelText="Sort by"
      actions={sortingActions}
      onTriCheckboxChange={handleTriCheckboxChange}
      onToggleButtonChange={handleToggleButtonChange}
    />
  );
};
```

### 1.3 Single Selection Mode

In single mode, only one sorting action can be active at a time:

```tsx
import { Sorting } from "@neuron/ui";
import type { SortingActionProps } from "@neuron/ui";

const SingleModeSorting = () => {
  const sortingActions: SortingActionProps[] = [
    {
      label: "Name",
      type: "TriStateCheckbox",
    },
    {
      label: "Date Created",
      type: "TriStateCheckbox",
    },
    {
      label: "Last Modified",
      type: "TriStateCheckbox",
    },
  ];

  return (
    <Sorting
      labelText="Sort by"
      actions={sortingActions}
      mode="single"
      onTriCheckboxChange={(index, value) => {
        console.info(`Single mode: Only action ${index} is active with value:`, value);
      }}
    />
  );
};
```

## Step 2: Controlled Sorting with State Management

### 2.1 Controlled Multiple Selection

```tsx
import { Sorting } from "@neuron/ui";
import type { SortingActionProps } from "@neuron/ui";
import { useState } from "react";

const ControlledMultipleSorting = () => {
  const [sortingValues, setSortingValues] = useState<(boolean | null)[]>([
    null, // Name: no sorting
    true, // Date: ascending (for toggle button, true = enabled)
    false, // Status: descending
    null, // Priority: no sorting
  ]);

  const sortingActions: SortingActionProps[] = [
    {
      label: "Name",
      type: "TriStateCheckbox",
    },
    {
      label: "Date",
      type: "ToggleButton",
    },
    {
      label: "Status",
      type: "TriStateCheckbox",
    },
    {
      label: "Priority",
      type: "ToggleButton",
    },
  ];

  const handleTriCheckboxChange = (index: number, value: boolean | null) => {
    const newValues = [...sortingValues];
    newValues[index] = value;
    setSortingValues(newValues);
  };

  const handleToggleButtonChange = (index: number, checked: boolean) => {
    const newValues = [...sortingValues];
    newValues[index] = checked;
    setSortingValues(newValues);
  };

  return (
    <Sorting
      labelText="Sort by"
      actions={sortingActions}
      values={sortingValues}
      mode="multiple"
      onTriCheckboxChange={handleTriCheckboxChange}
      onToggleButtonChange={handleToggleButtonChange}
    />
  );
};
```

### 2.2 Controlled Single Selection

```tsx
import { Sorting } from "@neuron/ui";
import type { SortingActionProps } from "@neuron/ui";
import { useState } from "react";

const ControlledSingleSorting = () => {
  const [sortingValues, setSortingValues] = useState<(boolean | null)[]>([
    null, // Name: no sorting
    true, // Date: ascending (only this one is active)
    null, // Status: no sorting
    null, // Priority: no sorting
  ]);

  const sortingActions: SortingActionProps[] = [
    {
      label: "Name",
      type: "TriStateCheckbox",
    },
    {
      label: "Date",
      type: "TriStateCheckbox",
    },
    {
      label: "Status",
      type: "TriStateCheckbox",
    },
    {
      label: "Priority",
      type: "TriStateCheckbox",
    },
  ];

  const handleTriCheckboxChange = (index: number, value: boolean | null) => {
    // In single mode, clear all other values
    const newValues = Array(sortingActions.length).fill(null);
    newValues[index] = value;
    setSortingValues(newValues);
  };

  return (
    <Sorting
      labelText="Sort by"
      actions={sortingActions}
      values={sortingValues}
      mode="single"
      onTriCheckboxChange={handleTriCheckboxChange}
    />
  );
};
```

## Step 3: Real-World Data Table Integration

### 3.1 Data Table with Sorting

```tsx
import { Sorting, Container, Panel } from "@neuron/ui";
import type { SortingActionProps } from "@neuron/ui";
import { useState, useMemo } from "react";

interface TableData {
  id: number;
  name: string;
  date: Date;
  status: "active" | "inactive";
  priority: number;
}

const DataTableWithSorting = () => {
  const [data] = useState<TableData[]>([
    { id: 1, name: "John Doe", date: new Date("2024-01-15"), status: "active", priority: 1 },
    { id: 2, name: "Jane Smith", date: new Date("2024-02-20"), status: "inactive", priority: 3 },
    { id: 3, name: "Bob Johnson", date: new Date("2024-01-10"), status: "active", priority: 2 },
  ]);

  const [sortingValues, setSortingValues] = useState<(boolean | null)[]>([
    null, // Name
    null, // Date
    null, // Status
    null, // Priority
  ]);

  const sortingActions: SortingActionProps[] = [
    {
      label: "Name",
      type: "TriStateCheckbox",
    },
    {
      label: "Date",
      type: "TriStateCheckbox",
    },
    {
      label: "Status",
      type: "ToggleButton",
    },
    {
      label: "Priority",
      type: "TriStateCheckbox",
    },
  ];

  const sortedData = useMemo(() => {
    let result = [...data];

    // Apply sorting based on current values
    sortingValues.forEach((value, index) => {
      if (value === null) return; // No sorting

      const action = sortingActions[index];

      switch (action.label) {
        case "Name":
          if (value === true) {
            // Ascending
            result.sort((a, b) => a.name.localeCompare(b.name));
          } else if (value === false) {
            // Descending
            result.sort((a, b) => b.name.localeCompare(a.name));
          }
          break;
        case "Date":
          if (value === true) {
            // Ascending
            result.sort((a, b) => a.date.getTime() - b.date.getTime());
          } else if (value === false) {
            // Descending
            result.sort((a, b) => b.date.getTime() - a.date.getTime());
          }
          break;
        case "Status":
          if (value === true) {
            // Toggle button enabled
            result.sort((a, b) => a.status.localeCompare(b.status));
          }
          break;
        case "Priority":
          if (value === true) {
            // Ascending
            result.sort((a, b) => a.priority - b.priority);
          } else if (value === false) {
            // Descending
            result.sort((a, b) => b.priority - a.priority);
          }
          break;
      }
    });

    return result;
  }, [data, sortingValues]);

  const handleTriCheckboxChange = (index: number, value: boolean | null) => {
    const newValues = [...sortingValues];
    newValues[index] = value;
    setSortingValues(newValues);
  };

  const handleToggleButtonChange = (index: number, checked: boolean) => {
    const newValues = [...sortingValues];
    newValues[index] = checked;
    setSortingValues(newValues);
  };

  return (
    <Container>
      <Panel>
        <Sorting
          labelText="Sort table by"
          actions={sortingActions}
          values={sortingValues}
          mode="multiple"
          onTriCheckboxChange={handleTriCheckboxChange}
          onToggleButtonChange={handleToggleButtonChange}
        />

        <div className="data-table">
          <table>
            <thead>
              <tr>
                <th>Name</th>
                <th>Date</th>
                <th>Status</th>
                <th>Priority</th>
              </tr>
            </thead>
            <tbody>
              {sortedData.map((item) => (
                <tr key={item.id}>
                  <td>{item.name}</td>
                  <td>{item.date.toLocaleDateString()}</td>
                  <td>{item.status}</td>
                  <td>{item.priority}</td>
                </tr>
              ))}
            </tbody>
          </table>
        </div>
      </Panel>
    </Container>
  );
};
```

### 3.2 Advanced Sorting with API Integration

```tsx
import { Sorting, LoadingIndicator } from "@neuron/ui";
import type { SortingActionProps } from "@neuron/ui";
import { useState, useEffect } from "react";

interface SortConfig {
  field: string;
  direction: "asc" | "desc" | null;
}

const ApiIntegratedSorting = () => {
  const [isLoading, setIsLoading] = useState(false);
  const [data, setData] = useState([]);
  const [sortingValues, setSortingValues] = useState<(boolean | null)[]>([
    null, // Name
    null, // Date
    null, // Status
  ]);

  const sortingActions: SortingActionProps[] = [
    {
      label: "Name",
      type: "TriStateCheckbox",
    },
    {
      label: "Date",
      type: "TriStateCheckbox",
    },
    {
      label: "Status",
      type: "ToggleButton",
    },
  ];

  const fetchSortedData = async (sortConfigs: SortConfig[]) => {
    setIsLoading(true);
    try {
      const queryParams = new URLSearchParams();

      sortConfigs.forEach((config, index) => {
        if (config.direction) {
          queryParams.append(`sort[${index}][field]`, config.field);
          queryParams.append(`sort[${index}][direction]`, config.direction);
        }
      });

      const response = await fetch(`/api/data?${queryParams}`);
      const result = await response.json();
      setData(result);
    } catch (error) {
      console.error("Failed to fetch sorted data:", error);
    } finally {
      setIsLoading(false);
    }
  };

  const handleSortingChange = (index: number, value: boolean | null, isToggleButton = false) => {
    const newValues = [...sortingValues];
    newValues[index] = value;
    setSortingValues(newValues);

    // Convert to API format
    const sortConfigs: SortConfig[] = newValues.map((val, idx) => {
      const action = sortingActions[idx];
      let direction: "asc" | "desc" | null = null;

      if (action.type === "TriStateCheckbox") {
        if (val === true) direction = "asc";
        else if (val === false) direction = "desc";
      } else if (action.type === "ToggleButton" && val === true) {
        direction = "asc"; // Default direction for toggle buttons
      }

      return {
        field: action.label?.toLowerCase() || "",
        direction,
      };
    });

    fetchSortedData(sortConfigs);
  };

  return (
    <div>
      <Sorting
        labelText="Sort data by"
        actions={sortingActions}
        values={sortingValues}
        mode="multiple"
        onTriCheckboxChange={(index, value) => handleSortingChange(index, value)}
        onToggleButtonChange={(index, checked) => handleSortingChange(index, checked, true)}
      />

      {isLoading && <LoadingIndicator />}

      <div className="data-display">{/* Render your data here */}</div>
    </div>
  );
};
```

## Step 4: Disabled States and Conditional Actions

### 4.1 Disabled Sorting Actions

```tsx
import { Sorting } from "@neuron/ui";
import type { SortingActionProps } from "@neuron/ui";

const DisabledSortingExample = () => {
  const sortingActions: SortingActionProps[] = [
    {
      label: "Name",
      type: "TriStateCheckbox",
    },
    {
      label: "Date",
      type: "ToggleButton",
      disabled: true, // Disabled toggle button
    },
    {
      label: "Status",
      type: "TriStateCheckbox",
      disabled: true, // Disabled tri-state checkbox
    },
    {
      label: "Priority",
      type: "ToggleButton",
    },
  ];

  return (
    <Sorting
      labelText="Sort by (some disabled)"
      actions={sortingActions}
      onTriCheckboxChange={(index, value) => {
        console.info(`Active tri-state ${index}:`, value);
      }}
      onToggleButtonChange={(index, checked) => {
        console.info(`Active toggle ${index}:`, checked);
      }}
    />
  );
};
```

### 4.2 Conditional Sorting Actions

```tsx
import { Sorting } from "@neuron/ui";
import type { SortingActionProps } from "@neuron/ui";
import { useState, useMemo } from "react";

const ConditionalSortingExample = () => {
  const [userRole, setUserRole] = useState<"admin" | "user">("user");
  const [dataType, setDataType] = useState<"basic" | "advanced">("basic");

  const sortingActions: SortingActionProps[] = useMemo(() => {
    const baseActions: SortingActionProps[] = [
      {
        label: "Name",
        type: "TriStateCheckbox",
      },
      {
        label: "Date",
        type: "TriStateCheckbox",
      },
    ];

    // Add advanced sorting for admin users
    if (userRole === "admin") {
      baseActions.push({
        label: "Priority",
        type: "TriStateCheckbox",
      });
    }

    // Add complex sorting for advanced data
    if (dataType === "advanced") {
      baseActions.push({
        label: "Score",
        type: "ToggleButton",
      });
    }

    return baseActions;
  }, [userRole, dataType]);

  return (
    <div>
      <div>
        <label>
          User Role:
          <select value={userRole} onChange={(e) => setUserRole(e.target.value as "admin" | "user")}>
            <option value="user">User</option>
            <option value="admin">Admin</option>
          </select>
        </label>

        <label>
          Data Type:
          <select value={dataType} onChange={(e) => setDataType(e.target.value as "basic" | "advanced")}>
            <option value="basic">Basic</option>
            <option value="advanced">Advanced</option>
          </select>
        </label>
      </div>

      <Sorting
        labelText="Available sorting options"
        actions={sortingActions}
        onTriCheckboxChange={(index, value) => {
          console.info(`Tri-state ${index} (${sortingActions[index].label}):`, value);
        }}
        onToggleButtonChange={(index, checked) => {
          console.info(`Toggle ${index} (${sortingActions[index].label}):`, checked);
        }}
      />
    </div>
  );
};
```

## Step 5: Sorting Props Reference

### 5.1 Core Sorting Props

| Prop                 | Type                                              | Default      | Description                          |
| -------------------- | ------------------------------------------------- | ------------ | ------------------------------------ |
| className            | `string`                                          | -            | Additional CSS classes               |
| labelText            | `string`                                          | -            | Label text for the sorting component |
| actions              | `SortingActionProps[]`                            | -            | Array of sorting actions             |
| values               | `(boolean \| null)[]`                             | -            | Controlled values for all actions    |
| mode                 | `"single" \| "multiple"`                          | `"multiple"` | Selection mode                       |
| onTriCheckboxChange  | `(index: number, value: boolean \| null) => void` | -            | Tri-state checkbox change callback   |
| onToggleButtonChange | `(index: number, checked: boolean) => void`       | -            | Toggle button change callback        |
| testId               | `string`                                          | -            | Test identifier                      |

### 5.2 SortingActionProps Interface

| Prop     | Type                                   | Description                    |
| -------- | -------------------------------------- | ------------------------------ |
| label    | `string`                               | Display label for the action   |
| type     | `"TriStateCheckbox" \| "ToggleButton"` | Type of sorting control        |
| disabled | `boolean`                              | Whether the action is disabled |

### 5.3 Tri-State Checkbox Values

| Value   | Icon               | Meaning         |
| ------- | ------------------ | --------------- |
| `true`  | ↓ (down arrow)     | Ascending sort  |
| `false` | ↑ (up arrow)       | Descending sort |
| `null`  | ↕ (up-down arrow) | No sorting      |

### 5.4 Toggle Button Values

| Value   | Meaning          |
| ------- | ---------------- |
| `true`  | Sorting enabled  |
| `false` | Sorting disabled |

## Step 6: Best Practices

### 6.1 When to Use Sorting Component

**Use Sorting for:**

- Data table sorting controls
- List view sorting options
- Search result sorting
- Custom sorting interfaces
- Multi-criteria sorting

```tsx
{
  /* Good: Data table sorting */
}
<Sorting labelText="Sort table by" actions={tableColumnActions} mode="multiple" />;

{
  /* Good: Search result sorting */
}
<Sorting labelText="Sort results by" actions={searchSortActions} mode="single" />;
```

**Don't use Sorting for:**

- Simple dropdown sorting (use Select instead)
- Single-option sorting (use RadioSet)
- Non-data sorting contexts

### 6.2 Action Type Selection

**Use TriStateCheckbox for:**

- Column sorting (asc/desc/none)
- Directional sorting options
- Three-state sorting needs

**Use ToggleButton for:**

- Boolean sorting criteria
- Simple on/off sorting
- Binary sorting states

```tsx
{/* Good: Tri-state for directional sorting */}
{
  label: "Date Created",
  type: "TriStateCheckbox"
}

{/* Good: Toggle for boolean sorting */}
{
  label: "Featured Items First",
  type: "ToggleButton"
}
```

### 6.3 Mode Selection Guidelines

**Use Single Mode for:**

- Primary sorting selection
- Mutually exclusive sorting
- Simple sorting interfaces

**Use Multiple Mode for:**

- Complex sorting criteria
- Multi-level sorting
- Advanced data tables

```tsx
{
  /* Good: Single mode for primary sort */
}
<Sorting labelText="Primary sort" actions={primarySortActions} mode="single" />;

{
  /* Good: Multiple mode for complex sorting */
}
<Sorting labelText="Advanced sorting" actions={advancedSortActions} mode="multiple" />;
```

### 6.4 State Management Best Practices

Always handle state updates properly:

```tsx
// Good: Proper controlled state
const [sortingValues, setSortingValues] = useState<(boolean | null)[]>([]);

const handleTriCheckboxChange = (index: number, value: boolean | null) => {
  const newValues = [...sortingValues];
  newValues[index] = value;
  setSortingValues(newValues);

  // Apply sorting logic
  applySorting(newValues);
};

// Good: Uncontrolled with callbacks
<Sorting
  actions={actions}
  onTriCheckboxChange={(index, value) => {
    console.info(`Sort ${index}:`, value);
    applySorting(index, value);
  }}
/>;
```

### 6.5 Accessibility Considerations

- Provide clear labels for all actions
- Use semantic action names
- Consider keyboard navigation
- Provide feedback for state changes

```tsx
{
  /* Good: Clear, semantic labels */
}
const accessibleActions: SortingActionProps[] = [
  {
    label: "Name (A-Z)",
    type: "TriStateCheckbox",
  },
  {
    label: "Date Created",
    type: "TriStateCheckbox",
  },
  {
    label: "Show Featured First",
    type: "ToggleButton",
  },
];
```

## Step 7: Common Mistakes to Avoid

### 7.1 Don't Mix Up Value Types

```tsx
{
  /* ❌ INCORRECT: Wrong value types */
}
const handleChange = (index: number, value: boolean | null) => {
  // Don't treat toggle button values as tri-state
  if (actions[index].type === "ToggleButton") {
    // value should be boolean, not null
  }
};

{
  /* ✅ CORRECT: Handle value types properly */
}
const handleTriCheckboxChange = (index: number, value: boolean | null) => {
  // Tri-state: true (asc), false (desc), null (none)
};

const handleToggleButtonChange = (index: number, checked: boolean) => {
  // Boolean: true (enabled), false (disabled)
};
```

### 7.2 Don't Forget Single Mode Behavior

```tsx
{
  /* ❌ INCORRECT: Not handling single mode properly */
}
<Sorting
  mode="single"
  values={[true, true, false]} // Multiple active values
/>;

{
  /* ✅ CORRECT: Only one active value in single mode */
}
<Sorting
  mode="single"
  values={[null, true, null]} // Only one active
/>;
```

### 7.3 Don't Ignore Action Types

```tsx
{
  /* ❌ INCORRECT: Inconsistent action handling */
}
const actions = [
  { label: "Name", type: "TriStateCheckbox" },
  { label: "Featured", type: "ToggleButton" },
];

// Don't handle all actions the same way
const handleAllChanges = (index: number, value: any) => {
  // This doesn't account for different action types
};

{
  /* ✅ CORRECT: Handle action types separately */
}
<Sorting
  actions={actions}
  onTriCheckboxChange={handleTriCheckboxChange}
  onToggleButtonChange={handleToggleButtonChange}
/>;
```

### 7.4 Don't Forget Loading States

```tsx
{
  /* ❌ INCORRECT: No loading feedback */
}
const handleSortingChange = async (index: number, value: boolean | null) => {
  await fetchSortedData(value); // No loading indicator
};

{
  /* ✅ CORRECT: Provide loading feedback */
}
const handleSortingChange = async (index: number, value: boolean | null) => {
  setIsLoading(true);
  try {
    await fetchSortedData(value);
  } finally {
    setIsLoading(false);
  }
};
```

## Key Takeaways

The Neuron Sorting component provides flexible sorting controls for data displays:

1. **Mixed Action Types** - Combine tri-state checkboxes and toggle buttons
2. **Flexible Modes** - Single or multiple selection modes
3. **Controlled/Uncontrolled** - Support for both usage patterns
4. **Visual Feedback** - Clear icons indicate sorting states
5. **Accessibility** - Full keyboard and screen reader support
6. **State Management** - Proper handling of different value types
7. **API Integration** - Easy integration with backend sorting
8. **Conditional Actions** - Dynamic action configuration
9. **Loading States** - Consider async operations
10. **Clear Labels** - Use semantic, descriptive action labels

By following these guidelines, you'll create effective sorting interfaces that provide users with intuitive control over data organization and display.

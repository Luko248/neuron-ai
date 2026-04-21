---
agent: agent
---

# Filter Component Guidelines

## Sync Metadata

- **Component Version:** v5.1.2
- **Component Source:** `packages/neuron/ui/src/lib/patterns/filter/Filter.tsx`
- **Guideline Command:** `/neuron-ui-filter`
- **Related Skill:** `neuron-ui-data`

## Overview

The Filter component provides a comprehensive solution for implementing search and filtering interfaces with two distinct UI variants: **Desktop Box Filter** and **SideSheet Filter**. It supports form state management, persistency, collapsible sections, and automatic filter chip management.

## Component Architecture

### Two UI Variants

1. **Desktop Box Filter** (`isSideSheet={false}`): A collapsible container displayed above content
2. **SideSheet Filter** (`isSideSheet={true}`): A slide-out panel with a trigger button

### Key Subcomponents

- **FilterHeader**: Title, icon, and collapse/expand controls
- **FilterChips**: Applied filters displayed as removable chips
- **FilterDesktop**: Desktop box implementation
- **FilterSideSheet**: Side panel implementation

## Basic Usage

### Desktop Box Filter

```typescript
import { Filter, useFilter, Form, AutoLayout } from '@neuron/ui';

interface FilterForm {
  firstName?: string;
  lastName?: string;
  birthDate?: string;
  status?: string;
}

const FilterExample = () => {
  const {
    form,
    appliedFilters,
    isCollapsed,
    onCollapse,
    submit,
    reset,
    resetSingle
  } = useFilter<FilterForm>({
    name: "example-filter",
    defaultValues: {
      firstName: "",
      lastName: "",
      birthDate: "",
      status: ""
    },
    keyTranslations: {
      firstName: "First Name",
      lastName: "Last Name",
      birthDate: "Birth Date",
      status: "Status"
    },
    persistencyMode: "session-storage",
    onSubmit: (formData) => console.log(formData)
  });

  return (
    <Filter<FilterForm>
      name="example-filter"
      title="Search Partners"
      form={form}
      appliedFilters={appliedFilters}
      onCollapse={onCollapse}
      submit={submit}
      resetSingle={resetSingle}
      resetAll={reset}
      buttonSize="large"
    >
      {/* Always visible primary fields */}
      <AutoLayout mode="fit" itemMinSize="200px" className="col-span-12">
        <Form.Input
          name="firstName"
          labelText="First Name"
        />
        <Form.Input
          name="lastName"
          labelText="Last Name"
        />
        <Form.DatePicker
          name="birthDate"
          labelText="Birth Date"
        />
      </AutoLayout>

      {/* Collapsible secondary fields */}
      {!isCollapsed && (
        <Fieldset
          columnCount={4}
          legend="Additional Filters"
          className="col-span-12"
        >
          <Form.Select
            name="status"
            labelText="Status"
            options={statusOptions}
          />
          <Form.Input
            name="partnerCode"
            labelText="Partner Code"
          />
        </Fieldset>
      )}
    </Filter>
  );
};
```

### SideSheet Filter

```typescript
import { Filter, FilterChips, useFilter, Button } from '@neuron/ui';

const SideSheetFilterExample = () => {
  const [isFilterOpen, setIsFilterOpen] = useState(false);

  const {
    form,
    appliedFilters,
    submit,
    reset,
    resetSingle
  } = useFilter<FilterForm>({
    name: "sidesheet-filter",
    defaultValues: { /* ... */ },
    keyTranslations: { /* ... */ },
    persistencyMode: "session-storage",
    onSubmit: (formData) => console.log(formData)
  });

  return (
    <div>
      {/* Trigger button and active filter chips */}
      <div className="d-flex align-items-center gap-3 mb-3">
        <Button
          variant="primary"
          iconLeft={baseIcons.filterRegular}
          onClick={() => setIsFilterOpen(true)}
        >
          Open Filters
        </Button>

        <FilterChips<FilterForm>
          appliedFilters={appliedFilters}
          resetSingle={resetSingle}
          resetAll={reset}
          size="large"
        />
      </div>

      {/* SideSheet Filter */}
      <Filter<FilterForm>
        title="Filter Options"
        form={form}
        appliedFilters={appliedFilters}
        submit={submit}
        resetSingle={resetSingle}
        resetAll={reset}
        isSideSheet
        isOpen={isFilterOpen}
        onOpenChange={setIsFilterOpen}
      >
        {/* Filter form fields */}
        <AutoLayout mode="fit" itemMinSize="200px">
          <Form.Input name="firstName" labelText="First Name" />
          <Form.Input name="lastName" labelText="Last Name" />
          <Form.DatePicker name="birthDate" labelText="Birth Date" />
        </AutoLayout>
      </Filter>
    </div>
  );
};
```

## useFilter Hook

### Hook Configuration

```typescript
interface UseFilterProps<FormData extends FieldValues> {
  name?: string; // Unique identifier for persistency
  defaultValues: FormData; // Initial form values
  keyTranslations: FilterKeyTranslations<FormData>; // Field display labels
  persistencyMode?: FilterPersistencyMode; // "url-params" | "session-storage"
  onSubmit: (formData: FormData) => void; // Submit handler
}

const {
  form, // React Hook Form instance
  appliedFilters, // Array of active filters
  isCollapsed, // Collapsible section state
  onCollapse, // Collapse toggle handler
  submit, // Submit function
  reset, // Reset all filters
  resetSingle, // Reset single filter
} = useFilter<FormData>({
  name: "unique-filter-name",
  defaultValues: initialValues,
  keyTranslations: fieldLabels,
  persistencyMode: "session-storage",
  onSubmit: handleSubmit,
});
```

### Field Key Translations

```typescript
type FilterKeyTranslations<FormData> = Partial<Record<Path<FormData>, string>>;

const keyTranslations: FilterKeyTranslations<FilterForm> = {
  firstName: "First Name",
  lastName: "Last Name",
  birthDate: "Birth Date",
  "nested.field": "Nested Field", // Support for nested paths
};
```

## Filter Structure Patterns

### Primary vs Secondary Fields

**Primary Fields** (Always Visible):

- Most frequently used filters
- Essential search criteria
- Limited to 3-5 fields for optimal UX

**Secondary Fields** (Collapsible):

- Advanced or optional filters
- Less frequently used criteria
- Grouped in logical sections using Fieldset

### Layout Recommendations

```typescript
// Primary fields - always visible
<AutoLayout mode="fit" itemMinSize="200px" className="col-span-12">
  <Form.Input name="searchTerm" labelText="Search" />
  <Form.Select name="status" labelText="Status" />
  <Form.DatePicker name="dateFrom" labelText="Date From" />
</AutoLayout>

// Secondary fields - collapsible
{!isCollapsed && (
  <>
    <Fieldset
      columnCount={4}
      legend="Contact Information"
      className="col-span-6"
    >
      <Form.Input name="email" labelText="Email" />
      <Form.Input name="phone" labelText="Phone" />
    </Fieldset>

    <Fieldset
      columnCount={4}
      legend="Additional Details"
      className="col-span-6"
    >
      <Form.Select name="category" labelText="Category" />
      <Form.NumberInput name="amount" labelText="Amount" />
    </Fieldset>
  </>
)}
```

## Persistency Modes

### Session Storage Persistency

```typescript
const filter = useFilter({
  name: "user-search",
  persistencyMode: "session-storage",
  // ... other config
});
```

**Characteristics:**

- Persists until browser session ends
- Isolated per browser tab
- Ideal for temporary searches
- Automatic state restoration

### URL Parameters Persistency

```typescript
const filter = useFilter({
  name: "user-search",
  persistencyMode: "url-params",
  // ... other config
});
```

**Characteristics:**

- Shareable filter states via URL
- Bookmark-friendly
- Browser history integration
- Ideal for permanent search links

## Advanced Configuration

### Form Field Types Integration

The Filter component works with all Neuron form components. Here are examples for different field types:

```typescript
<Filter>
  {/* Text inputs */}
  <Form.Input name="searchTerm" labelText="Search" />
  <Form.Input name="email" labelText="Email" type="email" />

  {/* Number inputs */}
  <Form.NumberInput name="age" labelText="Age" min={18} max={99} />
  <Form.NumberInput name="salary" labelText="Salary" currency />

  {/* Date inputs */}
  <Form.DatePicker name="birthDate" labelText="Birth Date" />
  <Form.DatePicker name="startDate" labelText="Start Date" range />

  {/* Selection inputs */}
  <Form.Select name="status" labelText="Status" options={statusOptions} />
  <Form.MultiSelect name="categories" labelText="Categories" options={categoryOptions} />
  <Form.AutoComplete name="location" labelText="Location" suggestions={locations} />

  {/* Boolean inputs */}
  <Form.Switch name="isActive" labelText="Active Only" />
  <Form.Checkbox name="verified" labelText="Verified Users" />

  {/* Radio and Checkbox sets */}
  <Form.RadioSet
    name="userType"
    labelText="User Type"
    options={[
      { value: "individual", labelText: "Individual" },
      { value: "business", labelText: "Business" }
    ]}
    inline
  />

  <Form.CheckboxSet
    name="permissions"
    labelText="Permissions"
    options={permissionOptions}
  />
</Filter>
```

### Complex Form Structures

For nested objects and arrays in filter forms:

```typescript
interface ComplexFilterForm {
  user: {
    personalInfo: {
      firstName: string;
      lastName: string;
    };
    contactInfo: {
      email: string;
      phone: string;
    };
  };
  dateRange: {
    from: string;
    to: string;
  };
  tags: string[];
  metadata: Record<string, any>;
}

const keyTranslations: FilterKeyTranslations<ComplexFilterForm> = {
  "user.personalInfo.firstName": "First Name",
  "user.personalInfo.lastName": "Last Name",
  "user.contactInfo.email": "Email",
  "user.contactInfo.phone": "Phone",
  "dateRange.from": "Date From",
  "dateRange.to": "Date To",
  "tags": "Tags"
};

<Filter>
  <Form.Input name="user.personalInfo.firstName" labelText="First Name" />
  <Form.Input name="user.personalInfo.lastName" labelText="Last Name" />
  <Form.Input name="user.contactInfo.email" labelText="Email" />
  <Form.DatePicker name="dateRange.from" labelText="From Date" />
  <Form.DatePicker name="dateRange.to" labelText="To Date" />
  <Form.MultiSelect name="tags" labelText="Tags" options={tagOptions} />
</Filter>
```

### Loading States and Error Handling

```typescript
const FilterWithLoadingStates = () => {
  const [loading, setLoading] = useState(false);
  const [error, setError] = useState<string | null>(null);

  const handleSubmit = async (formData: FilterForm) => {
    setLoading(true);
    setError(null);

    try {
      const results = await apiService.search(formData);
      onDataReceived(results);
    } catch (err) {
      setError(err instanceof Error ? err.message : 'Search failed');
    } finally {
      setLoading(false);
    }
  };

  const filter = useFilter({
    defaultValues,
    keyTranslations,
    onSubmit: handleSubmit
  });

  return (
    <>
      <Filter
        {...filter}
        applyButtonLabel={loading ? "Searching..." : "Search"}
        disabled={loading}
      >
        {/* Form fields */}
      </Filter>

      {error && (
        <MessageBox variant="error" title="Search Error" content={error} />
      )}
    </>
  );
};
```

### API Integration Patterns

```typescript
// API service example
class FilterApiService {
  async searchUsers(filters: UserFilterForm): Promise<User[]> {
    const queryParams = new URLSearchParams();

    // Convert filter form to API parameters
    Object.entries(filters).forEach(([key, value]) => {
      if (value && value !== '') {
        if (key === 'birthDate') {
          queryParams.set('birth_date', formatDateForApi(value));
        } else if (key === 'tags' && Array.isArray(value)) {
          value.forEach(tag => queryParams.append('tags[]', tag));
        } else {
          queryParams.set(key, String(value));
        }
      }
    });

    const response = await fetch(`/api/users?${queryParams}`);
    return response.json();
  }
}

// Usage in component
const UserFilter = () => {
  const [users, setUsers] = useState<User[]>([]);
  const apiService = new FilterApiService();

  const handleSearch = async (formData: UserFilterForm) => {
    const results = await apiService.searchUsers(formData);
    setUsers(results);
  };

  const filter = useFilter({
    name: "user-search",
    defaultValues: defaultUserFilters,
    keyTranslations: userFieldTranslations,
    persistencyMode: "url-params", // For shareable search URLs
    onSubmit: handleSearch
  });

  return (
    <div>
      <Filter {...filter}>
        {/* Filter fields */}
      </Filter>

      <UserTable users={users} />
    </div>
  );
};
```

### Real-time Filtering

```typescript
const RealTimeFilter = () => {
  const [searchResults, setSearchResults] = useState([]);
  const debouncedSearch = useCallback(
    debounce(async (formData: FilterForm) => {
      if (hasValidSearchCriteria(formData)) {
        const results = await searchApi(formData);
        setSearchResults(results);
      }
    }, 500),
    []
  );

  const filter = useFilter({
    defaultValues,
    keyTranslations,
    onSubmit: debouncedSearch
  });

  // Auto-submit on form changes
  useEffect(() => {
    const subscription = filter.form.watch((formData) => {
      debouncedSearch(formData);
    });
    return () => subscription.unsubscribe();
  }, [filter.form, debouncedSearch]);

  return (
    <Filter
      {...filter}
      isCollapsible={false} // Always expanded for real-time
    >
      {/* Filter fields */}
    </Filter>
  );
};
```

### Custom Filter Value Formatting

```typescript
const formatUsedFilterValue = (key: Path<FilterForm>, value: FieldValue<FilterForm>) => {
  if (key === "birthDate" && value) {
    return formatDate(value as Date);
  }
  if (key === "status" && value) {
    return statusTranslations[value] || value;
  }
  return value;
};

<Filter
  formatUsedFilterValue={formatUsedFilterValue}
  // ... other props
/>
```

### Action Zone Integration

```typescript
<Filter
  actionZone={
    <ButtonGroup gap>
      <Button variant="secondary">Advanced Search</Button>
      <Button variant="secondary">Save Filter</Button>
      <Button variant="secondary">Export</Button>
    </ButtonGroup>
  }
  // ... other props
/>
```

### Custom Button Labels and Icons

```typescript
<Filter
  resetButtonLabel="Clear All Filters"
  applyButtonLabel="Search"
  applyButtonIcon={baseIcons.magnifyingGlassSolid}
  buttonSize="large"
  // ... other props
/>
```

## Props Reference

| Prop                    | Type                                  | Required | Default         | Description                             |
| ----------------------- | ------------------------------------- | -------- | --------------- | --------------------------------------- |
| `form`                  | `UseFormReturn<FormData>`             | Yes      | -               | React Hook Form instance from useFilter |
| `submit`                | `() => void`                          | Yes      | -               | Submit handler function                 |
| `resetSingle`           | `(name: FieldPath<FormData>) => void` | Yes      | -               | Reset single filter function            |
| `resetAll`              | `() => void`                          | Yes      | -               | Reset all filters function              |
| `name`                  | `string`                              | No       | -               | Unique identifier for persistency       |
| `title`                 | `string`                              | No       | -               | Filter title/heading                    |
| `iconDef`               | `IconDefinition \| TBaseIcons`        | No       | `filterRegular` | Filter icon                             |
| `appliedFilters`        | `FilterAppliedFilter<FormData>[]`     | No       | `[]`            | Array of active filters                 |
| `buttonSize`            | `"medium" \| "large"`                 | No       | `"medium"`      | Button size                             |
| `className`             | `string`                              | No       | -               | Additional CSS class                    |
| `isContainerCollapsed`  | `boolean`                             | No       | `false`         | Initial collapsed state                 |
| `isCollapsible`         | `boolean`                             | No       | `true`          | Enable collapse functionality           |
| `isSideSheet`           | `boolean`                             | No       | `false`         | Use SideSheet variant                   |
| `isOpen`                | `boolean`                             | No       | -               | Control SideSheet open state            |
| `onOpenChange`          | `(isOpen: boolean) => void`           | No       | -               | SideSheet open state callback           |
| `onCollapse`            | `(isCollapsed: boolean) => void`      | No       | -               | Collapse state callback                 |
| `formatUsedFilterValue` | `Function`                            | No       | -               | Custom filter value formatter           |
| `actionZone`            | `React.ReactNode`                     | No       | -               | Additional action buttons               |
| `resetButtonLabel`      | `string`                              | No       | -               | Custom reset button label               |
| `applyButtonLabel`      | `string`                              | No       | -               | Custom apply button label               |
| `applyButtonIcon`       | `IconDefinition \| TBaseIcons`        | No       | `checkRegular`  | Apply button icon                       |

## Integration Patterns

### Data Loading Integration

```typescript
const ExampleWithDataLoading = () => {
  const [loading, setLoading] = useState(false);
  const [data, setData] = useState([]);

  const handleSubmit = async (formData: FilterForm) => {
    setLoading(true);
    try {
      const results = await fetchData(formData);
      setData(results);
    } finally {
      setLoading(false);
    }
  };

  const filter = useFilter({
    defaultValues: { /* ... */ },
    keyTranslations: { /* ... */ },
    onSubmit: handleSubmit
  });

  return (
    <div>
      <Filter {...filter} />

      {loading && <LoadingSpinner />}

      <DataTable data={data} />
    </div>
  );
};
```

### Responsive Layout Integration

```typescript
import { useResponsiveMediaQuery } from '@neuron/core';

const ResponsiveFilter = () => {
  const { isMobile } = useResponsiveMediaQuery();
  const [isFilterOpen, setIsFilterOpen] = useState(false);

  const filter = useFilter({ /* ... */ });

  return isMobile ? (
    <>
      <FilterChips {...filter} />
      <Filter
        {...filter}
        isSideSheet
        isOpen={isFilterOpen}
        onOpenChange={setIsFilterOpen}
      >
        {/* Simplified mobile fields */}
      </Filter>
    </>
  ) : (
    <Filter {...filter}>
      {/* Full desktop fields */}
    </Filter>
  );
};
```

### Search vs Filter Variants

```typescript
// Search variant
<Filter
  title="Search"
  iconDef={baseIcons.magnifyingGlassSolid}
  applyButtonLabel="Search"
  applyButtonIcon={baseIcons.magnifyingGlassSolid}
  resetButtonLabel="Clear Search"
  isCollapsible={false}
>
  {/* Search-focused fields */}
</Filter>

// Filter variant
<Filter
  title="Filters"
  iconDef={baseIcons.filterRegular}
  applyButtonLabel="Apply Filters"
  applyButtonIcon={baseIcons.checkRegular}
  resetButtonLabel="Clear Filters"
  isCollapsible={true}
>
  {/* Comprehensive filter fields */}
</Filter>
```

## Common Use Cases

### 1. User/Partner Search

```typescript
interface UserSearchForm {
  searchTerm?: string;
  userType?: 'individual' | 'business' | 'all';
  status?: 'active' | 'inactive' | 'pending';
  registrationDate?: string;
  country?: string;
  tags?: string[];
}

const UserSearchFilter = () => {
  const filter = useFilter<UserSearchForm>({
    name: "user-search",
    defaultValues: {
      searchTerm: "",
      userType: "all",
      status: "active",
      registrationDate: "",
      country: "",
      tags: []
    },
    keyTranslations: {
      searchTerm: "Search Term",
      userType: "User Type",
      status: "Status",
      registrationDate: "Registration Date",
      country: "Country",
      tags: "Tags"
    },
    persistencyMode: "url-params",
    onSubmit: handleUserSearch
  });

  return (
    <Filter {...filter}>
      {/* Primary filters - always visible */}
      <AutoLayout mode="fit" itemMinSize="200px" className="col-span-12">
        <Form.Input
          name="searchTerm"
          labelText="Search Users"
          placeholder="Name, email, or ID..."
        />
        <Form.RadioSet
          name="userType"
          labelText="User Type"
          options={[
            { value: "all", labelText: "All Users" },
            { value: "individual", labelText: "Individual" },
            { value: "business", labelText: "Business" }
          ]}
          inline
        />
        <Form.Select
          name="status"
          labelText="Status"
          options={[
            { value: "active", labelText: "Active" },
            { value: "inactive", labelText: "Inactive" },
            { value: "pending", labelText: "Pending" }
          ]}
        />
      </AutoLayout>

      {/* Secondary filters - collapsible */}
      {!filter.isCollapsed && (
        <Fieldset columnCount={4} legend="Additional Filters" className="col-span-12">
          <Form.DatePicker
            name="registrationDate"
            labelText="Registration Date"
            range
          />
          <Form.AutoComplete
            name="country"
            labelText="Country"
            suggestions={countryList}
          />
          <Form.MultiSelect
            name="tags"
            labelText="Tags"
            options={availableTags}
          />
        </Fieldset>
      )}
    </Filter>
  );
};
```

### 2. E-commerce Product Filter

```typescript
interface ProductFilterForm {
  category?: string;
  priceRange?: { min: number; max: number };
  brand?: string[];
  inStock?: boolean;
  rating?: number;
  features?: string[];
}

const ProductFilter = () => {
  const { isMobile } = useResponsiveMediaQuery();
  const [isFilterOpen, setIsFilterOpen] = useState(false);

  const filter = useFilter<ProductFilterForm>({
    name: "product-filter",
    defaultValues: {
      category: "",
      priceRange: { min: 0, max: 1000 },
      brand: [],
      inStock: false,
      rating: 0,
      features: []
    },
    keyTranslations: {
      category: "Category",
      "priceRange.min": "Min Price",
      "priceRange.max": "Max Price",
      brand: "Brand",
      inStock: "In Stock",
      rating: "Rating",
      features: "Features"
    },
    persistencyMode: "session-storage",
    onSubmit: handleProductFilter
  });

  if (isMobile) {
    return (
      <>
        <div className="d-flex align-items-center gap-2 mb-3">
          <Button
            variant="secondary"
            iconLeft={baseIcons.filterRegular}
            onClick={() => setIsFilterOpen(true)}
          >
            Filters {filter.appliedFilters.length > 0 && `(${filter.appliedFilters.length})`}
          </Button>
        </div>

        <FilterChips {...filter} />

        <Filter
          {...filter}
          isSideSheet
          isOpen={isFilterOpen}
          onOpenChange={setIsFilterOpen}
        >
          <Form.Select name="category" labelText="Category" options={categories} />
          <Form.NumberInput name="priceRange.min" labelText="Min Price" currency />
          <Form.NumberInput name="priceRange.max" labelText="Max Price" currency />
          <Form.MultiSelect name="brand" labelText="Brands" options={brands} />
          <Form.Switch name="inStock" labelText="In Stock Only" />
        </Filter>
      </>
    );
  }

  return (
    <Filter {...filter}>
      <AutoLayout mode="fit" itemMinSize="200px" className="col-span-12">
        <Form.Select name="category" labelText="Category" options={categories} />
        <Form.NumberInput name="priceRange.min" labelText="Min Price" currency />
        <Form.NumberInput name="priceRange.max" labelText="Max Price" currency />
        <Form.Switch name="inStock" labelText="In Stock Only" />
      </AutoLayout>

      {!filter.isCollapsed && (
        <Fieldset columnCount={3} legend="Advanced Filters" className="col-span-12">
          <Form.MultiSelect name="brand" labelText="Brands" options={brands} />
          <Form.Select
            name="rating"
            labelText="Min Rating"
            options={[
              { value: 0, labelText: "Any Rating" },
              { value: 3, labelText: "3+ Stars" },
              { value: 4, labelText: "4+ Stars" },
              { value: 5, labelText: "5 Stars" }
            ]}
          />
          <Form.CheckboxSet
            name="features"
            labelText="Features"
            options={productFeatures}
          />
        </Fieldset>
      )}
    </Filter>
  );
};
```

### 3. Data Table with Filter Integration

```typescript
const DataTableWithFilter = () => {
  const [data, setData] = useState([]);
  const [loading, setLoading] = useState(false);
  const [pagination, setPagination] = useState({ page: 1, pageSize: 10, total: 0 });

  const handleFilter = async (formData: FilterForm) => {
    setLoading(true);
    try {
      const response = await api.getData({
        ...formData,
        page: pagination.page,
        pageSize: pagination.pageSize
      });
      setData(response.data);
      setPagination(prev => ({ ...prev, total: response.total }));
    } finally {
      setLoading(false);
    }
  };

  const filter = useFilter({
    name: "data-table-filter",
    defaultValues: { /* ... */ },
    keyTranslations: { /* ... */ },
    persistencyMode: "url-params",
    onSubmit: handleFilter
  });

  return (
    <div>
      <Filter {...filter}>
        {/* Filter fields */}
      </Filter>

      <div className="mt-4">
        <DataTable
          data={data}
          loading={loading}
          columns={tableColumns}
          pagination={{
            ...pagination,
            onPageChange: (page) => {
              setPagination(prev => ({ ...prev, page }));
              filter.submit(); // Re-apply filters with new page
            }
          }}
        />
      </div>
    </div>
  );
};
```

## Validation and Error Handling

### Form Validation with Zod

```typescript
import { z } from 'zod';
import { zodResolver } from '@hookform/resolvers/zod';

const filterSchema = z.object({
  email: z.string().email("Invalid email format").optional(),
  age: z.number().min(18, "Minimum age is 18").max(99, "Maximum age is 99").optional(),
  dateRange: z.object({
    from: z.string().optional(),
    to: z.string().optional()
  }).refine(data => {
    if (data.from && data.to) {
      return new Date(data.from) <= new Date(data.to);
    }
    return true;
  }, "End date must be after start date")
});

const ValidatedFilter = () => {
  const filter = useFilter({
    defaultValues,
    keyTranslations,
    onSubmit: handleSubmit,
    resolver: zodResolver(filterSchema) // Add validation
  });

  return (
    <Filter {...filter}>
      <Form.Input
        name="email"
        labelText="Email"
        type="email"
        error={filter.form.formState.errors.email?.message}
      />
      <Form.NumberInput
        name="age"
        labelText="Age"
        error={filter.form.formState.errors.age?.message}
      />
    </Filter>
  );
};
```

### Custom Error Handling

```typescript
const FilterWithErrorHandling = () => {
  const [apiError, setApiError] = useState<string | null>(null);

  const handleSubmit = async (formData: FilterForm) => {
    setApiError(null);

    try {
      // Validate required fields
      if (!formData.searchTerm && !formData.email) {
        throw new Error("Please provide either search term or email");
      }

      const results = await searchApi(formData);
      onResultsReceived(results);
    } catch (error) {
      if (error instanceof ApiError) {
        setApiError(error.message);
      } else {
        setApiError("An unexpected error occurred");
      }
    }
  };

  const filter = useFilter({
    defaultValues,
    keyTranslations,
    onSubmit: handleSubmit
  });

  return (
    <>
      <Filter {...filter}>
        {/* Filter fields */}
      </Filter>

      {apiError && (
        <MessageBox
          variant="error"
          title="Filter Error"
          content={apiError}
          onDismiss={() => setApiError(null)}
        />
      )}
    </>
  );
};
```

## Testing Patterns

### Unit Testing Filter Components

```typescript
import { render, screen, fireEvent, waitFor } from '@testing-library/react';
import { FilterExample } from './FilterExample';

describe('FilterExample', () => {
  const mockOnSubmit = jest.fn();

  beforeEach(() => {
    mockOnSubmit.mockClear();
  });

  it('should render filter fields correctly', () => {
    render(<FilterExample onSubmit={mockOnSubmit} />);

    expect(screen.getByLabelText('First Name')).toBeInTheDocument();
    expect(screen.getByLabelText('Last Name')).toBeInTheDocument();
    expect(screen.getByRole('button', { name: /apply filters/i })).toBeInTheDocument();
  });

  it('should submit filter data when apply button is clicked', async () => {
    render(<FilterExample onSubmit={mockOnSubmit} />);

    fireEvent.change(screen.getByLabelText('First Name'), {
      target: { value: 'John' }
    });
    fireEvent.click(screen.getByRole('button', { name: /apply filters/i }));

    await waitFor(() => {
      expect(mockOnSubmit).toHaveBeenCalledWith(
        expect.objectContaining({ firstName: 'John' })
      );
    });
  });

  it('should show applied filters as chips', async () => {
    render(<FilterExample onSubmit={mockOnSubmit} />);

    // Apply a filter
    fireEvent.change(screen.getByLabelText('First Name'), {
      target: { value: 'John' }
    });
    fireEvent.click(screen.getByRole('button', { name: /apply filters/i }));

    await waitFor(() => {
      expect(screen.getByText(/first name: john/i)).toBeInTheDocument();
    });
  });

  it('should collapse and expand additional filters', () => {
    render(<FilterExample onSubmit={mockOnSubmit} />);

    const collapseButton = screen.getByRole('button', { name: /hide more filters/i });
    fireEvent.click(collapseButton);

    expect(screen.getByRole('button', { name: /show more filters/i })).toBeInTheDocument();
  });
});
```

## Advanced Performance Optimization

### Optimizing Large Filter Forms

```typescript
const OptimizedLargeFilter = () => {
  // Memoize expensive computations
  const filterOptions = useMemo(() => ({
    categories: generateCategoryOptions(),
    countries: generateCountryOptions(),
    tags: generateTagOptions()
  }), []);

  // Debounce API calls
  const debouncedSubmit = useCallback(
    debounce((formData: FilterForm) => {
      performSearch(formData);
    }, 500),
    []
  );

  const filter = useFilter({
    defaultValues,
    keyTranslations,
    onSubmit: debouncedSubmit
  });

  return (
    <Filter {...filter}>
      <AutoLayout mode="fit" itemMinSize="200px">
        <Form.Select
          name="category"
          options={filterOptions.categories}
          labelText="Category"
        />
        <Form.MultiSelect
          name="countries"
          options={filterOptions.countries}
          labelText="Countries"
          virtualized // For large option lists
        />
      </AutoLayout>
    </Filter>
  );
};
```

## Best Practices

### Field Organization

1. **Primary Fields** (Always Visible):

   - Most frequently used filters
   - Essential search criteria
   - Limit to 3-5 fields for optimal UX

2. **Secondary Fields** (Collapsible):
   - Advanced or optional filters
   - Group related fields using Fieldset
   - Use logical sections (Contact Info, Dates, etc.)

### Performance Optimization

1. **Debounced Submission**:

   ```typescript
   const debouncedSubmit = useCallback(
     debounce((formData: FilterForm) => {
       onSubmit(formData);
     }, 300),
     [],
   );
   ```

2. **Memoized Field Translations**:
   ```typescript
   const keyTranslations = useMemo(
     () => ({
       firstName: t("fields.firstName"),
       lastName: t("fields.lastName"),
     }),
     [t],
   );
   ```

### User Experience Guidelines

1. **Clear Visual Hierarchy**: Primary fields first, secondary in collapsible sections
2. **Logical Grouping**: Use Fieldset to group related filters
3. **Immediate Feedback**: Show applied filters as chips
4. **Progressive Disclosure**: Hide advanced options behind collapse
5. **Responsive Behavior**: Consider SideSheet for mobile

### Accessibility Features

- Proper ARIA labels and roles
- Keyboard navigation support
- Screen reader compatibility
- Focus management in SideSheet
- Clear visual indicators for applied filters

## Common Mistakes

❌ **Don't do this:**

```typescript
// Too many primary fields
<AutoLayout>
  <Form.Input name="field1" />
  <Form.Input name="field2" />
  <Form.Input name="field3" />
  <Form.Input name="field4" />
  <Form.Input name="field5" />
  <Form.Input name="field6" />
  <Form.Input name="field7" />
  <Form.Input name="field8" />
</AutoLayout>

// Missing key translations
const filter = useFilter({
  keyTranslations: {}, // Empty translations
  // ...
});

// No persistency name for complex filters
const filter = useFilter({
  // name: missing
  persistencyMode: "session-storage"
});
```

✅ **Do this:**

```typescript
// Balanced primary/secondary distribution
<AutoLayout mode="fit" itemMinSize="200px">
  <Form.Input name="searchTerm" />
  <Form.Select name="status" />
  <Form.DatePicker name="dateFrom" />
</AutoLayout>

{!isCollapsed && (
  <Fieldset legend="Advanced Filters">
    <Form.Input name="advancedField1" />
    <Form.Input name="advancedField2" />
  </Fieldset>
)}

// Complete key translations
const keyTranslations = {
  searchTerm: "Search Term",
  status: "Status",
  dateFrom: "Date From"
};

// Named persistency for complex filters
const filter = useFilter({
  name: "partner-search-filter",
  persistencyMode: "session-storage"
});
```

## For the AI Assistant

When implementing Filter components, you must:

### 🚨 MANDATORY Rules

1. **ALWAYS use useFilter hook** - Never implement filter state management manually
2. **PROVIDE key translations** - Every form field must have a human-readable translation
3. **STRUCTURE primary/secondary fields** - Limit primary fields to 3-5 most important ones
4. **IMPLEMENT proper persistency** - Use named persistency for complex filters
5. **HANDLE responsive behavior** - Consider SideSheet variant for mobile interfaces
6. **INCLUDE FilterChips** - Always show applied filters for SideSheet variant

### Filter Structure Requirements

**Primary Fields Section:**

- Use AutoLayout with mode="fit" and itemMinSize="200px"
- Include only most frequently used filters (3-5 fields max)
- Place in always-visible area

**Secondary Fields Section:**

- Wrap in `{!isCollapsed && ( ... )}` condition
- Group related fields using Fieldset component
- Use logical section names (Contact Info, Date Filters, etc.)

### useFilter Implementation Pattern

Always follow this pattern:

```typescript
const { form, appliedFilters, isCollapsed, onCollapse, submit, reset, resetSingle } = useFilter<FormDataType>({
  name: "descriptive-filter-name",
  defaultValues: initialFormValues,
  keyTranslations: fieldDisplayLabels,
  persistencyMode: "session-storage", // or "url-params"
  onSubmit: handleFormSubmission,
});
```

### SideSheet Integration Pattern

For SideSheet variants, always provide FilterChips outside the Filter:

```typescript
<div>
  <FilterChips<FormData>
    appliedFilters={appliedFilters}
    resetSingle={resetSingle}
    resetAll={reset}
    size="large"
  />

  <Filter
    isSideSheet
    isOpen={isOpen}
    onOpenChange={setIsOpen}
    // ... other props
  />
</div>
```

### Form Field Integration

- Use Form.\* components for all form fields
- Wrap multiple fields in AutoLayout for responsive behavior
- Apply proper CSS grid classes for responsive layout
- Group related fields in Fieldset components

### Responsive Strategy

```typescript
const { isMobile } = useResponsiveMediaQuery();

return isMobile ? (
  // SideSheet variant for mobile
  <Filter isSideSheet />
) : (
  // Desktop box variant
  <Filter />
);
```

If you encounter filtering requirements, **NEVER create custom filter components** - always use the Neuron Filter component with useFilter hook and follow these exact patterns for consistency and proper state management.

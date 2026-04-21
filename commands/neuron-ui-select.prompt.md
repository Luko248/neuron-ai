---
agent: agent
---

# Select Component AI Guidelines

## For the AI Assistant

Your task is to integrate and configure the Neuron Select component in React applications. This guide provides essential instructions for implementing Select components, which provide comprehensive dropdown selection with validation, accessibility, filtering, and keyboard navigation through React Hook Form integration across all Neuron applications.

## Overview

The Select component is a comprehensive dropdown selection input built on PrimeReact Dropdown, providing validation, accessibility, filtering, and enhanced UX features through React Hook Form integration. It supports both static options and dynamic code table integration for scalable data-driven applications.

Focus outline položek v rozbalovací nabídce je vyhrazen pouze pro navigaci klávesnicí a při hoveru nad popoverem se skryje.

## Core Usage

### Basic Implementation

```tsx
import { Form } from "@neuron/ui";
import { useForm } from "react-hook-form";
import { zodResolver } from "@hookform/resolvers/zod";
import { z } from "zod";

const schema = z.object({
  country: z.string().min(1, { message: "Please select a country" }),
  language: z.string().min(1, { message: "Language selection is required" }),
  category: z.string().optional(),
});

const countryOptions = [
  { value: "us", labelText: "United States" },
  { value: "ca", labelText: "Canada" },
  { value: "uk", labelText: "United Kingdom" },
];

const MyForm = () => {
  const { control } = useForm({
    resolver: zodResolver(schema),
    mode: "onChange",
  });

  return (
    <>
      <Form.Select
        name="country"
        labelText="Country"
        control={control}
        options={countryOptions}
        placeholder="Select a country"
        required
        requiredFlag={true}
        filter={true}
        showClear={true}
      />

      <Form.Select
        name="language"
        labelText="Preferred Language"
        control={control}
        options={languageOptions}
        description="Choose your preferred interface language"
        tooltip="This affects the display language of the application"
      />
    </>
  );
};
```

### Code Table Integration

```tsx
import { Form } from "@neuron/ui";
import { CT } from "app/common/codetables";

<Form.Select
  codeTableName={CT.CT_COUNTRIES}
  name="country"
  labelText="Country"
  control={control}
  required
  filter={true}
  showClear={true}
  handleCodeTableFilter={(items) => items.filter((item) => item.valid && item.primaryCategory === "active")}
/>;
```

## Key Props Reference

### Essential Props

- **name**: Field name for form registration
- **control**: React Hook Form control object
- **labelText**: Accessible label text for the select
- **options**: Static array of options (alternative to codeTableName)
- **codeTableName**: Code table identifier for dynamic options

### Data Source Props

- **options**: Static options array with `value` and `labelText` properties
- **groupedOptions**: Grouped options with label and items structure
- **codeTableName**: String identifier for code table
- **handleCodeTableFilter**: Function to filter code table results

### Enhanced Features

- **filter**: Enable search/filter functionality
- **showClear**: Show clear button to reset selection
- **placeholder**: Placeholder text when no option is selected
- **itemCreate**: Configuration for creating new items
- **onSelect**: Custom selection event handler
- **literalView**: Display as read-only text instead of dropdown

## Code Table Integration Showcase

### Static Code Table - Countries

```tsx
// app/common/codetables/defs/countries.codeTable.ts
import { createStaticCodeTable, ICodeTableItem } from "@neuron/core";
import { CT } from "../codeTableTypes";

const countriesData: ICodeTableItem[] = [
  {
    id: { code: "us" },
    code: "us",
    name: "United States",
    description: "United States of America",
    valid: true,
    primaryCategory: "north-america",
    extendedData: [
      { name: "iso3", value: "USA" },
      { name: "currency", value: "USD" },
      { name: "flag", value: "🇺🇸" },
    ],
  },
  {
    id: { code: "ca" },
    code: "ca",
    name: "Canada",
    description: "Canada",
    valid: true,
    primaryCategory: "north-america",
    extendedData: [
      { name: "iso3", value: "CAN" },
      { name: "currency", value: "CAD" },
      { name: "flag", value: "🇨🇦" },
    ],
  },
];

export const ctCountriesDef = createStaticCodeTable(CT.CT_COUNTRIES, countriesData);

// Usage with enhanced display
<Form.Select
  codeTableName={CT.CT_COUNTRIES}
  name="country"
  labelText="Country"
  control={control}
  required
  filter={true}
  showClear={true}
  placeholder="Select your country"
  handleCodeTableFilter={(items) =>
    items
      .filter((item) => item.valid)
      .sort((a, b) => a.name?.localeCompare(b.name || "") || 0)
      .map((item) => ({
        ...item,
        labelText: `${item.extendedData?.find((d) => d.name === "flag")?.value || ""} ${item.name}`,
        shortLabelText: item.code?.toUpperCase(),
      }))
  }
/>;
```

### Remote Code Table - Departments

```tsx
// app/common/codetables/defs/departments.codeTable.ts
export const ctDepartmentsDef = createRemoteCodeTable(CT.CT_DEPARTMENTS, "/api/departments", {
  adapter: new DepartmentsAdapter(),
  queryParams: {
    active: "true",
    includeSubdepartments: "true",
  },
});

export class DepartmentsAdapter implements ICodeTableAdapter<DepartmentRemoteResponse, ICodeTableItem> {
  public processItems = (input: DepartmentRemoteResponse): ICodeTableItem[] => {
    return input.departments.map(this.processItem);
  };

  public processItem = (input: DepartmentRemoteItem): ICodeTableItem => ({
    id: { code: input.deptCode },
    code: input.deptCode,
    name: input.departmentName,
    description: `${input.description} - Manager: ${input.managerName}`,
    valid: input.isActive,
    primaryCategory: input.category,
    extendedData: [
      { name: "manager", value: input.managerName },
      { name: "location", value: input.location },
      { name: "employeeCount", value: input.employeeCount.toString() },
    ],
  });
}

// Usage with filtering
const handleDepartmentFilter = useCallback((items: ICodeTableItem[]) => {
  return items
    .filter((item) => item.valid && !item.deleted)
    .sort((a, b) => a.name?.localeCompare(b.name || "") || 0)
    .map((item) => {
      const location = item.extendedData?.find((d) => d.name === "location")?.value || "";
      const employeeCount = item.extendedData?.find((d) => d.name === "employeeCount")?.value || "0";

      return {
        ...item,
        labelText: `${item.name} (${location})`,
        description: `${item.description} - ${employeeCount} employees`,
      };
    });
}, []);

<Form.Select
  codeTableName={CT.CT_DEPARTMENTS}
  name="department"
  labelText="Department"
  control={control}
  handleCodeTableFilter={handleDepartmentFilter}
  filter={true}
  showClear={true}
  placeholder="Select your department"
/>;
```

### Advanced Filtering with Business Logic

```tsx
const handleAdvancedFilter = useCallback((items: ICodeTableItem[]) => {
  const currentDate = new Date();
  const userRole = getCurrentUserRole();

  return items
    .filter((item) => {
      if (!item.valid || item.deleted) return false;

      // Date range validation
      if (item.validFrom && new Date(item.validFrom) > currentDate) return false;
      if (item.validTo && new Date(item.validTo) < currentDate) return false;

      // Role-based filtering
      const requiredRole = item.extendedData?.find((d) => d.name === "requiredRole")?.value;
      if (requiredRole && !hasRole(userRole, requiredRole)) return false;

      return true;
    })
    .sort((a, b) => {
      const categoryOrder = { featured: 1, standard: 2, legacy: 3 };
      const aPriority = categoryOrder[a.primaryCategory as keyof typeof categoryOrder] || 4;
      const bPriority = categoryOrder[b.primaryCategory as keyof typeof categoryOrder] || 4;

      if (aPriority !== bPriority) return aPriority - bPriority;
      return a.name?.localeCompare(b.name || "") || 0;
    })
    .map((item) => ({
      ...item,
      labelText: `${item.name} ${item.primaryCategory === "featured" ? "⭐" : ""}`,
      disabled: item.primaryCategory === "maintenance",
    }));
}, []);

<Form.Select
  codeTableName={CT.CT_ADVANCED_OPTIONS}
  name="advancedSelection"
  labelText="Advanced Selection"
  control={control}
  handleCodeTableFilter={handleAdvancedFilter}
  filter={true}
  showClear={true}
/>;
```

## Validation Patterns

### Zod Schema Validation

```tsx
// Basic validation
const schema = z.object({
  country: z.string().min(1, { message: "Please select a country" }),
  status: z.enum(["active", "inactive", "pending"], {
    required_error: "Status must be selected",
  }),
});

// Conditional validation
const conditionalSchema = z
  .object({
    accountType: z.enum(["personal", "business", "enterprise"]),
    department: z.string().optional(),
  })
  .refine(
    (data) => {
      if (["business", "enterprise"].includes(data.accountType)) {
        return data.department && data.department.length > 0;
      }
      return true;
    },
    {
      message: "Department is required for business accounts",
      path: ["department"],
    },
  );
```

## Advanced Features

### Custom Selection Handling

```tsx
const handleProductSelection = useCallback(
  (value: string) => {
    const selectedProduct = products.find((p) => p.code === value);
    if (selectedProduct) {
      const price = selectedProduct.extendedData?.find((d) => d.name === "price")?.value;
      setValue("price", price);
      setValue("category", selectedProduct.primaryCategory);
    }
  },
  [setValue, products],
);

<Form.Select
  name="product"
  labelText="Product"
  control={control}
  codeTableName={CT.CT_PRODUCTS}
  onSelect={handleProductSelection}
  filter={true}
/>;
```

### Item Creation Feature

```tsx
const handleCreateNew = useCallback(() => {
  setShowCreateModal(true);
}, []);

<Form.Select
  name="category"
  labelText="Category"
  control={control}
  codeTableName={CT.CT_CATEGORIES}
  itemCreate={{
    createText: "Can't find what you're looking for?",
    buttonText: "Create New",
    onCreate: handleCreateNew,
    isAlwaysVisible: true, // Forces footer to stay visible even without matches
  }}
/>;
```

#### Always-visible creation footer (`itemCreate.isAlwaysVisible`)

- Set `isAlwaysVisible: true` when the business flow requires the "create new" CTA even before any filtering occurs (e.g., empty option lists, mandatory entity creation).
- The flag forwards to `PopoverFooter` which sets `data-always-visible="true"`, enabling SCSS rules that keep the footer visible even when no filter results exist.
- When omitted or `false`, the footer only shows when the dropdown has filtered results (default behavior, consistent with older versions).
- Combine with localization: both `createText` and `buttonText` must use translation keys.

### Grouped Options

```tsx
const groupedCountries = [
  {
    label: "North America",
    items: [
      { value: "us", labelText: "United States" },
      { value: "ca", labelText: "Canada" },
    ],
  },
  {
    label: "Europe",
    items: [
      { value: "uk", labelText: "United Kingdom" },
      { value: "de", labelText: "Germany" },
    ],
  },
];

<Form.Select name="country" labelText="Country" control={control} groupedOptions={groupedCountries} filter={true} />;
```

## Role-Based Access Control

```tsx
const { userPermissions } = useAuth();

<Form.Select
  name="securityLevel"
  labelText="Security Level"
  control={control}
  codeTableName={CT.CT_SECURITY_LEVELS}
  fullAccess={userPermissions.includes("security_admin") ? "admin" : undefined}
  readonlyAccess={userPermissions.includes("security_read") ? "user" : undefined}
  handleCodeTableFilter={(items) =>
    items.filter((item) => {
      if (userPermissions.includes("security_admin")) return true;
      return item.primaryCategory !== "admin";
    })
  }
/>;
```

## Testing Integration

```tsx
<Form.Select
  name="testableSelect"
  labelText="Testable Select"
  control={control}
  codeTableName={CT.CT_TEST_OPTIONS}
  testId="country-selection"
/>;

// In tests
describe("Select Component", () => {
  it("should render with code table options", async () => {
    render(<TestForm />);

    const select = screen.getByTestId("country-selection");
    expect(select).toBeInTheDocument();

    await waitFor(() => {
      expect(screen.getByText("United States")).toBeInTheDocument();
    });
  });
});
```

## Performance Considerations

```tsx
const OptimizedSelect = memo(({ control }: { control: Control }) => {
  const handleFilter = useCallback((items: ICodeTableItem[]) => {
    return items.filter((item) => item.valid).sort((a, b) => a.name?.localeCompare(b.name || "") || 0);
  }, []);

  return (
    <Form.Select
      name="optimizedField"
      labelText="Optimized Select"
      control={control}
      codeTableName={CT.CT_LARGE_DATASET}
      handleCodeTableFilter={handleFilter}
      filter={true}
    />
  );
});
```

## Version Information

- **Component Version**: v4.4.7
- **Features**: React Hook Form integration, code table support, filtering, item creation with optional always-visible footer
- **Dependencies**: PrimeReact Dropdown, React Hook Form, Zod validation, @neuron/core code tables

## Sync Metadata

- **Component Version:** v4.4.7
- **Component Source:** `packages/neuron/ui/src/lib/form/select/Select.tsx`
- **Guideline Command:** `/neuron-ui-select`
- **Related Skill:** `neuron-ui-form-core`

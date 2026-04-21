---
agent: agent
---

# CheckBoxSet Component AI Guidelines

## For the AI Assistant

Your task is to integrate and configure the Neuron CheckBoxSet component in React applications. This guide provides essential instructions for implementing CheckBoxSet components, which provide comprehensive multiple selection functionality with validation, accessibility improvements, keyboard control enhancements, and enhanced UX features through React Hook Form integration across all Neuron applications.

## Overview

The CheckBoxSet component is a comprehensive multiple selection input field built on PrimeReact Checkbox, providing validation, accessibility, and enhanced UX features through React Hook Form integration. It supports both static options and dynamic code table integration for scalable data-driven applications.

## Core Usage

### Basic Implementation with Static Options

```tsx
import { Form } from "@neuron/ui";
import { useForm } from "react-hook-form";
import { zodResolver } from "@hookform/resolvers/zod";
import { z } from "zod";

const schema = z.object({
  interests: z.array(z.string()).min(1, {
    message: "Please select at least one interest",
  }),
  permissions: z.array(z.string()).optional(),
  features: z.array(z.string()).max(3, {
    message: "You can select maximum 3 features",
  }),
});

const staticOptions = [
  { value: "sports", labelText: "Sports" },
  { value: "music", labelText: "Music" },
  { value: "travel", labelText: "Travel" },
  { value: "technology", labelText: "Technology" },
];

const MyForm = () => {
  const { control } = useForm({
    resolver: zodResolver(schema),
    mode: "onChange",
  });

  return (
    <>
      <Form.CheckBoxSet
        name="interests"
        labelText="Select your interests"
        control={control}
        options={staticOptions}
        required
      />

      <Form.CheckBoxSet
        name="permissions"
        labelText="User permissions"
        control={control}
        options={permissionOptions}
        inline={true}
      />
    </>
  );
};
```

### Code Table Integration

```tsx
import { Form } from "@neuron/ui";
import { CT } from "app/common/codetables";

const MyForm = () => {
  const { control } = useForm({
    resolver: zodResolver(schema),
    mode: "onChange",
  });

  return (
    <>
      <Form.CheckBoxSet
        codeTableName={CT.CT_USER_ACCESS}
        name="access"
        labelText="User Access Rights"
        control={control}
        required
      />

      <Form.CheckBoxSet
        codeTableName={CT.CT_FEATURES}
        name="features"
        labelText="Available Features"
        control={control}
        handleCodeTableFilter={(items) => items.filter((item) => item.valid && item.category === "premium")}
      />
    </>
  );
};
```

## Key Props Reference

### Essential Props

- **name**: Field name for form registration
- **control**: React Hook Form control object
- **labelText**: Accessible label text for the checkbox set
- **options**: Static array of options (alternative to codeTableName)
- **codeTableName**: Code table identifier for dynamic options

### Data Source Props

- **options**: Static options array with `value` and `labelText` properties
- **codeTableName**: String identifier for code table (e.g., `CT.CT_USER_ACCESS`)
- **handleCodeTableFilter**: Function to filter code table results

### Validation & States

- **required**: HTML5 required attribute
- **requiredFlag**: Shows required indicator (\*)
- **optional**: Shows optional indicator
- **isValid**: Manual valid state indicator

### Enhanced Features

- **tooltip**: Help text with tooltip integration
- **description**: Helper text above the checkbox set
- **descriptionVariant**: `"default"` | `"success"` | `"warning"` | `"danger"` | `"info"`
- **literalView**: Display as read-only text instead of checkboxes
- **testId**: Custom test identifier for testing

### Layout & Styling

- **className**: CSS classes for grid positioning and custom styling
- **inline**: Display checkboxes horizontally instead of vertically
- **type**: `"checkbox"` | `"switch"` - Visual style of individual items
- **disabled**: Disable all checkbox interactions
- **readOnly**: Make all checkboxes read-only

### Access Control

- **readonlyAccess**: Role-based read access control
- **fullAccess**: Role-based full access control

## Code Table Integration

### Overview

Code tables (CT) provide unified access, loading, and usage of read-only data, whether static or loaded from remote services. They offer:

- **Interface** for unified code table representation
- **On-demand loading** and filtering of records
- **Built-in caching** (4-hour Redux store + localStorage)
- **Access control** and data transformation via adapters
- **RTK Query integration** with React hooks, selectors, and endpoints

### Code Table Types

There are two types of code tables - static and remote. Both use the same interface and can be created using factory functions.

#### Code Table Definition Structure

```tsx
// app/common/codetables/codeTableTypes.ts
export const CT = {
  CT_USER_ACCESS: "userAccess" as const, // Must be 'as const' for proper typing
  CT_FEATURES: "features" as const,
  CT_CATEGORIES: "categories" as const,
  OKEC: "LOV_OKEC" as const,
} as const;
```

### Static Code Table

For predefined, static data that doesn't change:

```tsx
// app/common/codetables/defs/userAccess.codeTable.ts
import { ICodeTableItem, createStaticCodeTable } from "@neuron/core";
import { CT } from "../codeTableTypes";

const userAccessData: ICodeTableItem[] = [
  {
    id: { code: "access1" },
    code: "access1",
    name: "Read Access",
    description: "Basic read-only access",
    valid: true,
    primaryCategory: "basic",
  },
  {
    id: { code: "access2" },
    code: "access2",
    name: "Write Access",
    description: "Read and write access",
    valid: true,
    primaryCategory: "standard",
  },
  {
    id: { code: "access3" },
    code: "access3",
    name: "Admin Access",
    description: "Full administrative access",
    valid: true,
    primaryCategory: "advanced",
  },
];

export const ctUserAccessDefinition = createStaticCodeTable(CT.CT_USER_ACCESS, userAccessData, {
  idProperties: ["code"], // Properties used for record identification
  adapter: new DefaultCodeTableAdapter(), // Optional custom adapter
  access: ["user", "admin"], // Access control (not yet implemented)
});
```

### Remote Code Table

For API-driven data with dynamic loading:

```tsx
// app/common/codetables/defs/okec.codeTable.ts
import { createRemoteCodeTable } from "@neuron/core";
import { CT } from "../codeTableTypes";
import { OkecAdapter } from "../adapter/okec.adapter";

const PATH = `${CT.OKEC}`;

export const ctOkecDef = createRemoteCodeTable(CT.OKEC, PATH, {
  baseUrl: "https://rd-provider-be.ing.k8sdc2neuroncppdev-01.ifr.services/codelist",
  queryParams: {
    active: "true",
    version: "latest",
  },
  publicFacing: false, // Internal service with auth headers
  adapter: new OkecAdapter(),
  idProperties: ["code"],
  access: ["user", "admin"],
});

// For public APIs
export const ctPublicDataDef = createRemoteCodeTable(CT.CT_PUBLIC_DATA, "/api/public", {
  publicFacing: true, // No auth headers
  adapter: new PublicDataAdapter(),
});
```

### ICodeTableItem Interface

Standard representation of a code table record based on RDM definition:

```tsx
interface ICodeTableItem {
  id?: Record<string, unknown>; // Multi-column key support
  code: string; // Primary key/value
  name?: string; // Display text for UI components
  description?: string; // Long description
  validFrom?: string; // Validity period start
  validTo?: string; // Validity period end
  valid: boolean; // Active/inactive flag (default: true)
  primaryCategory?: string; // Optional category label
  version?: number; // Code table version
  extendedData?: ICodeTableItemExtendedData[]; // Dynamic attributes
  deleted?: string; // Deletion flag
}

interface ICodeTableItemExtendedData {
  name: string;
  value: string;
}
```

### Custom Adapters

Transform remote data into ICodeTableItem format:

```tsx
// app/common/codetables/adapter/okec.adapter.ts
import { ICodeTableAdapter, ICodeTableItem } from "@neuron/core";

interface OkecRemoteItem {
  kod: string;
  nazev: string;
  popis?: string;
  platnyOd?: string;
  platnyDo?: string;
  aktivni: boolean;
}

interface OkecRemoteResponse {
  data: OkecRemoteItem[];
  total: number;
}

export class OkecAdapter implements ICodeTableAdapter<OkecRemoteResponse, ICodeTableItem> {
  public processItems = (input: OkecRemoteResponse): ICodeTableItem[] => {
    return input.data.map(this.processItem);
  };

  public processItem = (input: OkecRemoteItem): ICodeTableItem => ({
    id: { code: input.kod },
    code: input.kod,
    name: input.nazev,
    description: input.popis,
    validFrom: input.platnyOd,
    validTo: input.platnyDo,
    valid: input.aktivni,
    primaryCategory: "okec",
  });
}
```

### Code Table Registration

Code tables must be registered to generate hooks, selectors, and endpoints. Registration extends the CT API and generates type-safe functions:

```tsx
// app/common/codetables/defs/userAccess.codeTable.ts
import { registerCodeTable } from "@neuron/core";

// Assuming ctUserAccessDefinition.type is "userAccess" as const
export const {
  useUserAccessQuery, // React hook for component usage
  selectUserAccess, // Selector for single record
  selectUserAccesss, // Selector for multiple records
  userAccessEndpoint, // RTK Query endpoint
} = registerCodeTable(ctUserAccessDefinition);
```

### Application Setup

Code tables require Redux store configuration:

```tsx
// store/rootReducer.ts
import { codeTableApi, insertReducer } from "@neuron/core";
import { combineReducers } from "@reduxjs/toolkit";

export const rootReducer = combineReducers({
  // Common reducers
  ...insertReducer(authenticationSlice),
  ...insertReducer(appContextSlice),
  ...insertReducer(codeTableApi), // Required for code tables

  // Other reducers...
});
```

```tsx
// store/configureStore.ts
import { configureStore } from "@reduxjs/toolkit";
import { codeTableApi } from "@neuron/core";

export const store = configureStore({
  reducer: rootReducer,
  middleware: (getDefaultMiddleware) => getDefaultMiddleware().concat(codeTableApi.middleware), // Required middleware
  // Other middlewares...
});
```

### Usage Patterns

#### React Hook Usage (Recommended)

```tsx
// Using generated hook
const { data: userAccessOptions, isFetching, error } = useUserAccessQuery();

// With selectFromResult for performance
const userAccessOptions = useUserAccessQuery(undefined, {
  selectFromResult: ({ data, isFetching }) => ({
    options: data?.filter((item) => item.valid) || [],
    isLoading: isFetching,
  }),
});
```

#### Generic Hook Usage

```tsx
import { useCodeTableQuery } from "@neuron/core";

// Generic approach with type safety
type UserAccessCTDefinition = typeof ctUserAccessDefinition;

const { data, isFetching, error } = useCodeTableQuery<UserAccessCTDefinition>(CT.CT_USER_ACCESS);
```

#### Selector Usage

```tsx
// Single record selection
const userAccess = useSelector(selectUserAccess({ code: "admin" }));

// Multiple records selection
const selectedAccess = useSelector(selectUserAccesss([{ code: "read" }, { code: "write" }]));

// Generic selectors
const userAccess = useSelector(selectCodeTableOne<UserAccessCTDefinition>(CT.CT_USER_ACCESS)({ code: "admin" }));
```

#### Manual Endpoint Usage

```tsx
// For usage outside components
import { store } from "store/configureStore";

// Initiate code table loading
store.dispatch(userAccessEndpoint.initiate());

// Safe initiation (checks if already initiated)
store.dispatch(initiateCodeTableSafe(CT.CT_USER_ACCESS));
```

### Code Table Filtering

Filter code table results before displaying:

```tsx
const handleAccessFilter = useCallback((items: ICodeTableItem[]) => {
  return items.filter((item) => item.valid && item.category === "standard" && !item.deprecated);
}, []);

<Form.CheckBoxSet
  codeTableName={CT.CT_USER_ACCESS}
  name="access"
  labelText="Available Access Rights"
  control={control}
  handleCodeTableFilter={handleAccessFilter}
/>;
```

## Validation Patterns

### Zod Schema Validation

```tsx
// Array validation with constraints
const preferencesSchema = z.object({
  interests: z
    .array(z.string())
    .min(1, { message: "Please select at least one interest" })
    .max(5, { message: "You can select maximum 5 interests" }),

  permissions: z.array(z.string()).refine((arr) => arr.includes("read"), {
    message: "Read permission is required",
  }),

  features: z.array(z.string()).optional(),
});

// Conditional validation
const settingsSchema = z
  .object({
    notifications: z.array(z.string()),
    channels: z.array(z.string()),
  })
  .refine(
    (data) => {
      // If notifications are selected, channels must be selected too
      if (data.notifications.length > 0) {
        return data.channels.length > 0;
      }
      return true;
    },
    {
      message: "Please select notification channels",
      path: ["channels"],
    },
  );
```

### Complex Validation Rules

```tsx
// Business logic validation
const accessSchema = z
  .object({
    userRoles: z.array(z.string()),
    permissions: z.array(z.string()),
  })
  .refine(
    (data) => {
      // Admin role requires specific permissions
      if (data.userRoles.includes("admin")) {
        const requiredPermissions = ["read", "write", "delete"];
        return requiredPermissions.every((perm) => data.permissions.includes(perm));
      }
      return true;
    },
    {
      message: "Admin role requires read, write, and delete permissions",
      path: ["permissions"],
    },
  );
```

## Advanced Features

### Custom Change Handling

```tsx
const handleCheckBoxSetChange = useCallback((values: string[], event: CheckboxChangeEvent) => {
  console.log("Selected values:", values);

  // Custom logic based on selections
  if (values.includes("admin")) {
    // Auto-select required permissions for admin
    const adminPermissions = ["read", "write", "delete"];
    const updatedValues = [...new Set([...values, ...adminPermissions])];
    // Handle the updated values
  }

  // Prevent default if needed
  if (someCondition) {
    event.preventDefault();
  }
}, []);

<Form.CheckBoxSet
  name="permissions"
  labelText="User Permissions"
  control={control}
  codeTableName={CT.CT_PERMISSIONS}
  onChange={handleCheckBoxSetChange}
/>;
```

### Literal View Mode

Display selected values as text instead of interactive checkboxes:

```tsx
<Form.CheckBoxSet
  name="selectedFeatures"
  labelText="Selected Features"
  control={control}
  codeTableName={CT.CT_FEATURES}
  literalView={true} // Shows "Feature 1 | Feature 2 | Feature 3"
/>
```

### Inline Layout

Display checkboxes horizontally:

```tsx
<Form.CheckBoxSet
  name="quickOptions"
  labelText="Quick Options"
  control={control}
  options={quickOptions}
  inline={true}
/>
```

### Tooltip Integration

```tsx
<Form.CheckBoxSet
  name="advancedFeatures"
  labelText="Advanced Features"
  control={control}
  codeTableName={CT.CT_ADVANCED_FEATURES}
  tooltip={{
    text: "These features require additional configuration",
    placement: "right",
  }}
/>
```

## Role-Based Access Control

### Access Control Props

```tsx
<Form.CheckBoxSet
  name="adminFeatures"
  labelText="Admin Features"
  control={control}
  codeTableName={CT.CT_ADMIN_FEATURES}
  fullAccess="admin,superuser" // Only these roles can interact
  readonlyAccess="user,guest" // These roles see read-only version
/>
```

## Testing Integration

### Test ID Usage

```tsx
<Form.CheckBoxSet
  name="testableField"
  labelText="Testable checkbox set"
  control={control}
  options={testOptions}
  testId="feature-selection"
/>;

// In tests
const checkboxSet = screen.getByTestId("feature-selection");
expect(checkboxSet).toBeInTheDocument();

// Test individual checkboxes
const option1 = screen.getByLabelText("Option 1");
fireEvent.click(option1);
```

## Best Practices

### 1. Clear and Descriptive Labels

```tsx
// Good: Clear, actionable labels
<Form.CheckBoxSet
  name="notifications"
  labelText="Email notification preferences"
  control={control}
  options={notificationOptions}
/>

// Avoid: Vague or unclear labels
<Form.CheckBoxSet
  name="options"
  labelText="Options"
  control={control}
  options={someOptions}
/>
```

### 2. Logical Option Ordering

```tsx
const priorityOptions = [
  { value: "high", labelText: "High Priority" },
  { value: "medium", labelText: "Medium Priority" },
  { value: "low", labelText: "Low Priority" },
];

// Order options logically (high to low, alphabetical, etc.)
<Form.CheckBoxSet name="priority" labelText="Task Priority Levels" control={control} options={priorityOptions} />;
```

### 3. Proper Validation Messages

```tsx
const schema = z.object({
  requiredFeatures: z.array(z.string()).min(1, {
    message: "Please select at least one required feature to continue",
  }),
  optionalFeatures: z.array(z.string()).max(3, {
    message: "You can select up to 3 optional features",
  }),
});
```

### 4. Code Table Performance

```tsx
// Use filtering to reduce options when appropriate
const handleLargeCodeTableFilter = useCallback((items: ICodeTableItem[]) => {
  return items
    .filter((item) => item.valid && item.active)
    .sort((a, b) => a.name.localeCompare(b.name))
    .slice(0, 50); // Limit to 50 most relevant options
}, []);

<Form.CheckBoxSet
  codeTableName={CT.CT_LARGE_DATASET}
  name="selection"
  labelText="Select Options"
  control={control}
  handleCodeTableFilter={handleLargeCodeTableFilter}
/>;
```

## Common Patterns

### Permission Management

```tsx
const permissionSchema = z.object({
  userPermissions: z.array(z.string()).min(1, {
    message: "User must have at least one permission",
  }),
});

<Form.CheckBoxSet
  codeTableName={CT.CT_USER_PERMISSIONS}
  name="userPermissions"
  labelText="User Permissions"
  control={control}
  required
  description="Select the permissions this user should have"
/>;
```

### Feature Selection

```tsx
const featureSchema = z.object({
  enabledFeatures: z.array(z.string()).optional(),
  premiumFeatures: z.array(z.string()).refine(
    (features) => {
      // Premium features require subscription
      return features.length === 0 || hasSubscription;
    },
    {
      message: "Premium subscription required for selected features",
    },
  ),
});

<Form.CheckBoxSet
  codeTableName={CT.CT_AVAILABLE_FEATURES}
  name="enabledFeatures"
  labelText="Enabled Features"
  control={control}
  handleCodeTableFilter={(items) => items.filter((item) => item.category === "standard")}
/>;
```

### Category Selection

```tsx
const categorySchema = z.object({
  categories: z.array(z.string()).min(1, {
    message: "Please select at least one category",
  }),
});

<Form.CheckBoxSet
  codeTableName={CT.CT_CATEGORIES}
  name="categories"
  labelText="Product Categories"
  control={control}
  inline={true}
  required
/>;
```

### Conditional Logic

```tsx
const { watch } = useForm({ control });
const selectedFeatures = watch("features");
const hasAdvancedFeatures = selectedFeatures?.includes("advanced");

<Form.CheckBoxSet name="features" labelText="Basic Features" control={control} codeTableName={CT.CT_BASIC_FEATURES} />;

{
  hasAdvancedFeatures && (
    <Form.CheckBoxSet
      name="advancedOptions"
      labelText="Advanced Options"
      control={control}
      codeTableName={CT.CT_ADVANCED_OPTIONS}
    />
  );
}
```

## Performance Considerations

### Memoization for Large Sets

```tsx
const CheckBoxSetGroup = memo(({ control }: { control: Control }) => {
  const handleFilter = useCallback((items: ICodeTableItem[]) => {
    return items.filter((item) => item.valid);
  }, []);

  return (
    <Form.CheckBoxSet
      name="largeSet"
      labelText="Large Option Set"
      control={control}
      codeTableName={CT.CT_LARGE_SET}
      handleCodeTableFilter={handleFilter}
    />
  );
});
```

### Optimized Change Handlers

```tsx
const handleChange = useCallback(
  (values: string[], event: CheckboxChangeEvent) => {
    // Optimized change handling
    console.log("Selection changed:", values);
  },
  [], // Empty dependency array for stable reference
);
```

## Error Handling

### Validation Error Display

The CheckBoxSet component automatically displays validation errors through the InputWrapper:

```tsx
// Validation errors are shown automatically
const schema = z.object({
  required: z.array(z.string()).min(1, {
    message: "At least one option must be selected",
  }),
});
```

### Code Table Error Handling

```tsx
// Code table loading errors are handled automatically
// Component shows loading state and error states as needed
<Form.CheckBoxSet
  codeTableName={CT.CT_REMOTE_DATA}
  name="remoteOptions"
  labelText="Remote Options"
  control={control}
  // Component handles loading/error states internally
/>
```

## Integration with Other Components

### Form Integration

```tsx
const CompleteForm = () => {
  const { control, handleSubmit, watch } = useForm({
    resolver: zodResolver(schema),
    mode: "onChange",
  });

  const selectedPermissions = watch("permissions");

  const onSubmit = (data: FormData) => {
    console.log("Form data:", data);
    console.log("Selected permissions:", data.permissions);
  };

  return (
    <form onSubmit={handleSubmit(onSubmit)}>
      <Form.CheckBoxSet
        codeTableName={CT.CT_USER_PERMISSIONS}
        name="permissions"
        labelText="User Permissions"
        control={control}
        required
      />

      <Form.CheckBoxSet
        name="features"
        labelText="Optional Features"
        control={control}
        options={featureOptions}
        description={`Selected permissions: ${selectedPermissions?.length || 0}`}
      />

      <button type="submit">Submit</button>
    </form>
  );
};
```

## Troubleshooting

### Common Issues

1. **Options not loading**: Check code table definition and registration
2. **Validation not working**: Verify Zod schema array validation setup
3. **Code table filtering not applied**: Ensure handleCodeTableFilter returns filtered array
4. **Performance issues with large sets**: Implement proper filtering and memoization

### Debug Tips

```tsx
// Debug form state
const { watch } = useForm({ control });
const checkboxSetValue = watch("fieldName");
console.log("Current selection:", checkboxSetValue);

// Debug code table data
const { data: codeTableData } = useCodeTableQuery(CT.CT_YOUR_TABLE);
console.log("Code table data:", codeTableData);
```

### Code Table Setup Checklist

1. ✅ Code table constant defined in `codeTableTypes.ts`
2. ✅ Code table definition created in `defs/` folder
3. ✅ Code table registered in application setup
4. ✅ Adapter implemented (for remote code tables)
5. ✅ Component uses correct `codeTableName` prop

## Version Information

- **Component Version**: v4.1.0
- **Features**: React Hook Form integration, code table support, role-based access control
- **Dependencies**: PrimeReact Checkbox, React Hook Form, Zod validation, @neuron/core code tables

## Sync Metadata

- **Component Version:** v4.1.1
- **Component Source:** `packages/neuron/ui/src/lib/form/checkBoxSet/CheckBoxSet.tsx`
- **Guideline Command:** `/neuron-ui-checkboxset`
- **Related Skill:** `neuron-ui-form-choice`

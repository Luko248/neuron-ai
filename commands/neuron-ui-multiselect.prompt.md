---
agent: agent
---

# MultiSelect Component AI Guidelines

## For the AI Assistant

Your task is to integrate and configure the Neuron MultiSelect component in React applications. This guide provides essential instructions for implementing MultiSelect components, which provide comprehensive multi-selection dropdown functionality with array-based selection, validation, filtering, accessibility improvements, and enhanced UX features through React Hook Form integration across all Neuron applications.

## Overview

The MultiSelect component is a comprehensive multi-selection dropdown built on PrimeReact MultiSelect, providing array-based selection, validation, filtering, and enhanced UX features through React Hook Form integration.

## Core Usage

### Basic Implementation

```tsx
import { Form } from "@neuron/ui";
import { useForm } from "react-hook-form";
import { zodResolver } from "@hookform/resolvers/zod";
import { z } from "zod";

const schema = z.object({
  interests: z.array(z.string()).min(1, { message: "Select at least one interest" }),
  skills: z
    .array(z.string())
    .min(2, { message: "Select at least two skills" })
    .max(5, { message: "Maximum 5 skills allowed" }),
});

const options = [
  { value: "programming", labelText: "Programming" },
  { value: "design", labelText: "Design" },
  { value: "marketing", labelText: "Marketing" },
  { value: "writing", labelText: "Writing" },
];

const MyForm = () => {
  const { control } = useForm({
    resolver: zodResolver(schema),
    mode: "onChange",
    defaultValues: {
      interests: [],
      skills: [],
    },
  });

  return (
    <>
      <Form.MultiSelect
        name="interests"
        labelText="Interests"
        control={control}
        options={options}
        placeholder="Select your interests..."
      />

      <Form.MultiSelect
        name="skills"
        labelText="Skills"
        control={control}
        options={options}
        requiredFlag={true}
        filter={true}
        selectAll={true}
      />
    </>
  );
};
```

## Key Props Reference

### Essential Props

- **name**: Field name for form registration
- **control**: React Hook Form control object
- **labelText**: Accessible label text
- **options**: Array of IOption objects for selection

### Data Sources

- **options**: Static array of IOption objects
- **codeTableName**: Name of registered code table for dynamic options
- **groupedOptions**: Array with `{ label: string; items: IOption[]; }[]` structure
- **handleCodeTableFilter**: Custom filter function for code table results

### Validation & States

- **required**: HTML5 required attribute
- **requiredFlag**: Shows required indicator (\*)
- **optional**: Shows optional indicator
- **isValid**: Manual valid state indicator

### Selection Features

- **selectAll**: Shows "Select All" checkbox in dropdown
- **filter**: Enables search/filter functionality in dropdown
- **showChipShorthand**: Uses `shortLabelText` instead of `labelText` for chips
- **groupSelectedValues**: Reorders flat option lists so selected options stay at the top while unselected options keep original order and return to original position after deselection
- **maxSelectionCount**: Limits how many options can be selected at once; after reaching the limit, other non-selected options are temporarily disabled until the user deselects some item
  - When set, the dropdown footer always shows a max-selection helper text with the selected limit.
  - Can be combined with `itemCreate`; footer text is merged into one sentence and rendered on a single line.
  - When set, `selectAll` functionality is disabled automatically to avoid collisions with selection limits.

### Enhanced Features

- **tooltip**: Help text with tooltip integration
- **description**: Helper text below input
- **descriptionVariant**: `"default"` | `"success"` | `"warning"` | `"danger"` | `"info"`
- **placeholder**: Placeholder text when no items selected

### Layout & Styling

- **inline**: Horizontal label/input layout
- **literalView**: Display as read-only text with separators
- **className**: CSS classes for grid positioning
- **leftAddonContent**: Components before input (buttons, icons)
- **rightAddonContent**: Components after input (buttons, icons)

### Item Creation

- **itemCreate**: Enables adding new items with footer button

  ```tsx
  itemCreate={{
    createText: "No option found?",
    buttonText: "Create New",
    onCreate: () => handleCreateNew(),
    isAlwaysVisible: true, // keeps footer visible regardless of filter results
  }}
  ```

  - **`isAlwaysVisible`** (v4.5.0): Forces the PopoverFooter to stay rendered even when the dropdown has no matches (e.g., empty datasets, immediate creation flows). Leave undefined/`false` to keep legacy behavior where the footer only appears when filtered suggestions exist.

### Event Handlers

- **onChange**: Callback when selection changes
- **onSelectAll**: Callback when Select All is used
- **onRemove**: Callback when chip is removed
- **onFilter**: Callback when filtering occurs

### Access Control

- **readOnly**: Make input read-only
- **disabled**: Disable input interaction
- **readonlyAccess**: Role-based read access
- **fullAccess**: Role-based full access

## Validation Patterns

### Array Validation with Zod

```tsx
// Basic array validation
const userSchema = z.object({
  interests: z
    .array(z.string())
    .min(1, { message: "Select at least one interest" })
    .max(5, { message: "Maximum 5 interests allowed" }),

  categories: z.array(z.string()).nonempty({ message: "At least one category is required" }),

  tags: z.array(z.string()).optional(), // Optional array field

  permissions: z
    .array(z.string())
    .min(2, { message: "Select at least 2 permissions" })
    .refine((values) => values.includes("read"), { message: "Read permission is required" }),
});

// Usage with form
const { control } = useForm({
  resolver: zodResolver(userSchema),
  mode: "onChange",
  defaultValues: {
    interests: [],
    categories: [],
    tags: [],
    permissions: [],
  },
});
```

### Conditional Validation

```tsx
const conditionalSchema = z
  .object({
    userType: z.string(),
    permissions: z.array(z.string()),
  })
  .refine(
    (data) => {
      if (data.userType === "admin") {
        return data.permissions.length >= 3;
      }
      return data.permissions.length >= 1;
    },
    {
      message: "Admins need at least 3 permissions, users need at least 1",
      path: ["permissions"],
    },
  );

<Form.MultiSelect
  name="permissions"
  labelText="Permissions"
  control={control}
  options={permissionOptions}
  deps={["userType"]} // Re-validate when userType changes
/>;
```

## Options Configuration

### IOption Interface Properties

Each option in the MultiSelect supports rich configuration through the IOption interface:

```tsx
interface IOption {
  /** Value of the option (string | number | boolean) */
  value: string | number | boolean;
  /** Display text for the option */
  labelText: string;
  /** Short version of labelText for chips */
  shortLabelText?: string;
  /** Icon configuration for the option */
  icon?: IconProps;
  /** Description text shown in option */
  descriptionText?: string;
  /** Whether the option is disabled */
  disabled?: boolean;
  /** Tag text displayed with the option */
  tagText?: string;
  /** Nested options for grouping */
  options?: IOption[];
}
```

### Basic Static Options

```tsx
const skills = [
  { value: "react", labelText: "React" },
  { value: "vue", labelText: "Vue.js" },
  { value: "angular", labelText: "Angular" },
  { value: "nodejs", labelText: "Node.js" },
];

<Form.MultiSelect name="skills" labelText="Technical Skills" control={control} options={skills} />;
```

### Complex Options with All Properties

```tsx
import { baseIcons } from "@neuron/ui";

const complexOptions = [
  {
    value: "frontend-react",
    labelText: "React Frontend Development",
    shortLabelText: "React",
    icon: { iconDef: baseIcons.reactBrand },
    descriptionText: "Modern React with hooks and TypeScript",
    tagText: "Popular",
    disabled: false,
  },
  {
    value: "backend-node",
    labelText: "Node.js Backend Development",
    shortLabelText: "Node.js",
    icon: { iconDef: baseIcons.serverSolid },
    descriptionText: "Server-side JavaScript with Express",
    tagText: "Backend",
    disabled: false,
  },
  {
    value: "legacy-jquery",
    labelText: "jQuery Development",
    shortLabelText: "jQuery",
    icon: { iconDef: baseIcons.codeSolid },
    descriptionText: "Legacy frontend library",
    tagText: "Legacy",
    disabled: true, // Disabled option
  },
  {
    value: "database-sql",
    labelText: "SQL Database Management",
    shortLabelText: "SQL",
    icon: { iconDef: baseIcons.databaseSolid },
    descriptionText: "Relational database design and optimization",
    tagText: "Data",
  },
];

<Form.MultiSelect
  name="expertiseAreas"
  labelText="Areas of Expertise"
  control={control}
  options={complexOptions}
  showChipShorthand={true} // Shows short labels in chips
  filter={true}
  tooltip="Select your technical expertise areas"
/>;
```

### Options with Shorthands

```tsx
const countries = [
  {
    value: "cz",
    labelText: "Czech Republic",
    shortLabelText: "CZ",
  },
  {
    value: "sk",
    labelText: "Slovakia",
    shortLabelText: "SK",
  },
  {
    value: "de",
    labelText: "Germany",
    shortLabelText: "DE",
  },
];

<Form.MultiSelect
  name="countries"
  labelText="Countries"
  control={control}
  options={countries}
  showChipShorthand={true} // Shows "CZ, SK" instead of "Czech Republic, Slovakia"
/>;
```

### Code Table Integration

Code tables provide unified access to dynamic data sources with built-in caching, loading states, and data transformation.

#### Basic Code Table Usage

```tsx
import { CT } from "app/common/codetables";

// Using registered code table
<Form.MultiSelect
  name="departments"
  labelText="Departments"
  control={control}
  codeTableName={CT.CT_DEPARTMENTS}
  placeholder="Select departments..."
  filter={true}
  selectAll={true}
/>

// With custom filtering
<Form.MultiSelect
  name="activeFeatures"
  labelText="Active Features"
  control={control}
  codeTableName={CT.CT_FEATURES}
  handleCodeTableFilter={(items) =>
    items.filter(item => item.valid && item.primaryCategory === "premium")
  }
  tooltip="Only premium features are available for selection"
/>
```

#### Code Table Definition Example

```tsx
// app/common/codetables/codeTableTypes.ts
export const CT = {
  CT_DEPARTMENTS: "departments" as const,
  CT_FEATURES: "features" as const,
  CT_USER_ROLES: "userRoles" as const,
} as const;

// app/common/codetables/defs/departments.codeTable.ts
import { createRemoteCodeTable, registerCodeTable } from "@neuron/core";
import { CT } from "../codeTableTypes";
import { DepartmentsAdapter } from "../adapter/departments.adapter";

export const ctDepartmentsDef = createRemoteCodeTable(CT.CT_DEPARTMENTS, "/api/departments", {
  adapter: new DepartmentsAdapter(),
  queryParams: { active: "true" },
  publicFacing: false,
});

// Register to generate hooks and selectors
export const { useDepartmentsQuery, selectDepartments, selectDepartmentss, departmentsEndpoint } =
  registerCodeTable(ctDepartmentsDef);
```

#### Static Code Table

```tsx
// app/common/codetables/defs/userRoles.codeTable.ts
import { createStaticCodeTable, ICodeTableItem } from "@neuron/core";
import { CT } from "../codeTableTypes";

const userRolesData: ICodeTableItem[] = [
  {
    id: { code: "admin" },
    code: "admin",
    name: "Administrator",
    description: "Full system access",
    valid: true,
    primaryCategory: "system",
  },
  {
    id: { code: "user" },
    code: "user",
    name: "Standard User",
    description: "Limited system access",
    valid: true,
    primaryCategory: "standard",
  },
  {
    id: { code: "guest" },
    code: "guest",
    name: "Guest User",
    description: "Read-only access",
    valid: true,
    primaryCategory: "limited",
  },
];

export const ctUserRolesDef = createStaticCodeTable(CT.CT_USER_ROLES, userRolesData);

<Form.MultiSelect
  name="userRoles"
  labelText="User Roles"
  control={control}
  codeTableName={CT.CT_USER_ROLES}
  selectAll={true}
/>;
```

#### Advanced Code Table Filtering

```tsx
const handleDepartmentFilter = useCallback((items: ICodeTableItem[]) => {
  return items
    .filter((item) => item.valid && !item.deleted)
    .sort((a, b) => a.name?.localeCompare(b.name || "") || 0)
    .map((item) => ({
      ...item,
      // Add custom properties for enhanced display
      labelText: `${item.name} (${item.code})`,
      shortLabelText: item.code,
      disabled: item.primaryCategory === "restricted",
    }));
}, []);

<Form.MultiSelect
  name="enhancedDepartments"
  labelText="Departments"
  control={control}
  codeTableName={CT.CT_DEPARTMENTS}
  handleCodeTableFilter={handleDepartmentFilter}
  showChipShorthand={true} // Uses shortLabelText for chips
  filter={true}
/>;
```

### Grouped Options

```tsx
const groupedTechStack = [
  {
    label: "Frontend Technologies",
    items: [
      {
        value: "react",
        labelText: "React",
        shortLabelText: "React",
        icon: { iconDef: baseIcons.reactBrand },
        descriptionText: "Component-based UI library",
        tagText: "Popular",
      },
      {
        value: "vue",
        labelText: "Vue.js",
        shortLabelText: "Vue",
        icon: { iconDef: baseIcons.vueBrand },
        descriptionText: "Progressive JavaScript framework",
        tagText: "Modern",
      },
      {
        value: "angular",
        labelText: "Angular",
        shortLabelText: "Angular",
        icon: { iconDef: baseIcons.angularBrand },
        descriptionText: "TypeScript-based web framework",
        tagText: "Enterprise",
        disabled: false,
      },
    ],
  },
  {
    label: "Backend Technologies",
    items: [
      {
        value: "nodejs",
        labelText: "Node.js",
        shortLabelText: "Node",
        icon: { iconDef: baseIcons.nodejsBrand },
        descriptionText: "JavaScript runtime environment",
        tagText: "JavaScript",
      },
      {
        value: "python",
        labelText: "Python",
        shortLabelText: "Python",
        icon: { iconDef: baseIcons.pythonBrand },
        descriptionText: "High-level programming language",
        tagText: "Versatile",
      },
      {
        value: "java",
        labelText: "Java",
        shortLabelText: "Java",
        icon: { iconDef: baseIcons.javaBrand },
        descriptionText: "Object-oriented programming language",
        tagText: "Enterprise",
      },
    ],
  },
];

<Form.MultiSelect
  name="techStack"
  labelText="Technology Stack"
  control={control}
  groupedOptions={groupedTechStack}
  optionGroupChildren="items"
  filter={true}
  showChipShorthand={true}
  tooltip="Select technologies for your project"
/>;
```

### Code Table Integration with Complex Options

```tsx
import { HandleCodeTableFilter } from "@neuron/core";

// Custom filter to enhance code table options with rich properties
const enhanceCodeTableOptions: HandleCodeTableFilter = (finalOptions) => {
  return finalOptions.map((option) => ({
    ...option,
    shortLabelText: option.labelText.length > 15 ? option.labelText.substring(0, 12) + "..." : option.labelText,
    icon: getIconForDepartment(option.value),
    descriptionText: getDepartmentDescription(option.value),
    tagText: getDepartmentType(option.value),
    disabled: !isDepartmentActive(option.value),
  }));
};

// Helper functions for enhancing options
const getIconForDepartment = (code: string) => {
  const iconMap = {
    IT: { iconDef: baseIcons.laptopSolid },
    HR: { iconDef: baseIcons.usersSolid },
    FIN: { iconDef: baseIcons.calculatorSolid },
    MKT: { iconDef: baseIcons.bullhornSolid },
  };
  return iconMap[code] || { iconDef: baseIcons.buildingSolid };
};

const getDepartmentDescription = (code: string) => {
  const descriptions = {
    IT: "Information Technology and Digital Services",
    HR: "Human Resources and People Management",
    FIN: "Finance and Accounting Operations",
    MKT: "Marketing and Communications",
  };
  return descriptions[code] || "Department operations";
};

<Form.MultiSelect
  name="departments"
  labelText="Departments"
  control={control}
  codeTableHook={useDepartmentsQuery()}
  handleCodeTableFilter={enhanceCodeTableOptions}
  showChipShorthand={true}
  filter={true}
  selectAll={true}
  tooltip="Select applicable departments"
/>;
```

## Advanced Selection Features

### Select All Functionality

```tsx
<Form.MultiSelect
  name="permissions"
  labelText="User Permissions"
  control={control}
  options={permissionOptions}
  selectAll={true} // Shows "Select All" checkbox
  onSelectAll={(event) => {
    console.info("Select all toggled:", event.checked);
  }}
/>
```

### Filtering and Search

```tsx
<Form.MultiSelect
  name="users"
  labelText="Assign Users"
  control={control}
  options={userOptions}
  filter={true} // Enables search input
  placeholder="Search and select users..."
  onFilter={(event) => {
    console.info("Filter query:", event.filter);
  }}
/>
```

### Dynamic Item Creation

```tsx
const handleCreateNewTag = () => {
  // Custom logic to add new tag
  const newTag = prompt("Enter new tag:");
  if (newTag) {
    // Add to options and update form
  }
};

<Form.MultiSelect
  name="tags"
  labelText="Tags"
  control={control}
  options={tagOptions}
  filter={true}
  itemCreate={{
    createText: "Tag not found?",
    buttonText: "Create New Tag",
    onCreate: handleCreateNewTag,
    isAlwaysVisible: true,
  }}
/>;
```

#### Always-visible creation footer behavior

- Use `isAlwaysVisible` when UX requires the creation CTA before the user filters anything or when `options` may be empty.
- The flag propagates to `PopoverFooter`, setting `data-always-visible="true"` for SCSS rules that keep the footer visible.
- Remember to close the dropdown and reset filters in `onCreate` if you open modals or inline forms.
- Keep `createText`/`buttonText` localized via translation keys.

## Visual Enhancement

### Addons

```tsx
import { Button, Icon, baseIcons } from "@neuron/ui";

// Icon addons
<Form.MultiSelect
  name="categories"
  labelText="Categories"
  control={control}
  options={categoryOptions}
  leftAddonContent={<Icon iconDef={baseIcons.tagSolid} />}
  rightAddonContent={<Icon iconDef={baseIcons.plusSolid} />}
/>

// Button addons
<Form.MultiSelect
  name="features"
  labelText="Features"
  control={control}
  options={featureOptions}
  leftAddonContent={<Button size="small" variant="info">Filter</Button>}
  rightAddonContent={<Button size="small" variant="primary">Manage</Button>}
/>
```

### Description and Tooltip

```tsx
<Form.MultiSelect
  name="notifications"
  labelText="Notification Types"
  control={control}
  options={notificationOptions}
  tooltip="Select which notifications you want to receive"
  description="You can change these settings later"
  descriptionVariant="info"
/>
```

## Layout Integration

### Grid Layout

```tsx
import { Fieldset } from "@neuron/ui";

<Fieldset legend="User Preferences" columnCount={12}>
  <Form.MultiSelect
    className="g-col-12 g-col-md-6"
    name="interests"
    labelText="Interests"
    control={control}
    options={interestOptions}
  />

  <Form.MultiSelect
    className="g-col-12 g-col-md-6"
    name="skills"
    labelText="Skills"
    control={control}
    options={skillOptions}
    filter={true}
  />

  <Form.MultiSelect
    className="g-col-12"
    name="departments"
    labelText="Department Access"
    control={control}
    options={departmentOptions}
    selectAll={true}
    requiredFlag={true}
  />
</Fieldset>;
```

### Inline Layout

```tsx
<Form.MultiSelect
  name="quickFilters"
  labelText="Quick Filters"
  control={control}
  options={filterOptions}
  inline={true}
/>
```

## Advanced Usage

### Custom Event Handlers

```tsx
const handleSelectionChange = (event: MultiSelectChangeEvent) => {
  console.info("Selected values:", event.value);
  // Custom logic for selection changes
};

const handleChipRemove = (event: MultiSelectRemoveEvent) => {
  console.info("Removed value:", event.value);
  // Custom logic for chip removal
};

<Form.MultiSelect
  name="tags"
  labelText="Tags"
  control={control}
  options={tagOptions}
  onChange={handleSelectionChange}
  onRemove={handleChipRemove}
/>;
```

### Literal View (Read-Only Display)

```tsx
// Display selected values as text with separators
<Form.MultiSelect
  name="selectedCategories"
  labelText="Selected Categories"
  control={control}
  options={categoryOptions}
  literalView={true} // Shows "Category 1 | Category 2 | Category 3"
/>
```

### Dependencies and Conditional Options

```tsx
const schema = z
  .object({
    country: z.string(),
    cities: z.array(z.string()),
  })
  .refine(
    (data) => {
      if (data.country && data.cities.length === 0) {
        return false;
      }
      return true;
    },
    {
      message: "Select at least one city when country is selected",
      path: ["cities"],
    },
  );

<Form.MultiSelect
  name="cities"
  labelText="Cities"
  control={control}
  options={getCitiesForCountry(selectedCountry)}
  deps={["country"]} // Re-validate when country changes
/>;
```

## Form Integration Patterns

### Complete Form Example

```tsx
import { Form, SubmitButton, Fieldset } from "@neuron/ui";
import { useForm, SubmitHandler } from "react-hook-form";
import { zodResolver } from "@hookform/resolvers/zod";
import { z } from "zod";

interface FormData {
  skills: string[];
  interests: string[];
  permissions: string[];
  languages: string[];
}

const formSchema = z.object({
  skills: z.array(z.string()).min(2, "Select at least 2 skills"),
  interests: z.array(z.string()).min(1, "Select at least 1 interest"),
  permissions: z.array(z.string()).min(1, "Select at least 1 permission"),
  languages: z.array(z.string()).optional(),
});

const skillOptions = [
  { value: "react", labelText: "React" },
  { value: "vue", labelText: "Vue.js" },
  { value: "angular", labelText: "Angular" },
];

const UserPreferencesForm = () => {
  const { control, handleSubmit } = useForm<FormData>({
    resolver: zodResolver(formSchema),
    mode: "onChange",
    defaultValues: {
      skills: [],
      interests: [],
      permissions: [],
      languages: [],
    },
  });

  const onSubmit: SubmitHandler<FormData> = (data) => {
    console.info("Form submitted:", data);
  };

  return (
    <form onSubmit={handleSubmit(onSubmit)}>
      <Fieldset legend="User Preferences" columnCount={12}>
        <Form.MultiSelect
          className="g-col-12 g-col-md-6"
          name="skills"
          labelText="Technical Skills"
          control={control}
          options={skillOptions}
          requiredFlag={true}
          filter={true}
          tooltip="Select your technical skills"
        />

        <Form.MultiSelect
          className="g-col-12 g-col-md-6"
          name="interests"
          labelText="Interests"
          control={control}
          options={interestOptions}
          requiredFlag={true}
          selectAll={true}
        />

        <Form.MultiSelect
          className="g-col-12 g-col-md-6"
          name="permissions"
          labelText="Permissions"
          control={control}
          codeTableName="CT_PERMISSIONS"
          requiredFlag={true}
          description="Select user permissions"
        />

        <Form.MultiSelect
          className="g-col-12 g-col-md-6"
          name="languages"
          labelText="Languages"
          control={control}
          options={languageOptions}
          optional={true}
          showChipShorthand={true}
        />

        <div className="g-col-12">
          <SubmitButton control={control}>Save Preferences</SubmitButton>
        </div>
      </Fieldset>
    </form>
  );
};
```

## Best Practices

### Validation Strategy

1. **Use array validation** with min/max constraints for meaningful selection limits
2. **Set mode to "onChange"** for real-time feedback on selection changes
3. **Provide clear error messages** indicating required minimum/maximum selections
4. **Use conditional validation** when selection depends on other fields
5. **Initialize with empty arrays** in defaultValues to prevent undefined issues

### UX Considerations

1. **Always provide labelText** for accessibility
2. **Use meaningful placeholders** that indicate selection purpose
3. **Enable filtering for large option sets** (>10 items)
4. **Show Select All for permission-style selections** where users typically need multiple items
5. **Use shorthands for chips** when full text would be too long with `showChipShorthand`
6. **Add tooltips for complex selections** with specific business rules
7. **Leverage rich option properties** for better user experience:
   - **Icons** for visual recognition and category distinction
   - **Descriptions** for additional context about complex options
   - **Tags** for categorization and filtering hints
   - **Short labels** for compact chip display
   - **Disabled state** for unavailable or conditional options
8. **Use grouped options** for logical categorization of large datasets
9. **Enhance code table options** with `HandleCodeTableFilter` for richer displays

### Performance

1. **Memoize large option arrays** to prevent recreating on re-renders
2. **Use code tables for dynamic data** instead of large static arrays
3. **Implement handleCodeTableFilter** for client-side filtering when needed
4. **Use deps array** for conditional validation dependencies

### Accessibility

1. **Ensure proper form labeling** with labelText
2. **Provide required/optional indicators** for screen readers
3. **Include helpful descriptions** for complex multi-selection rules
4. **Use semantic grouping** with grouped options when applicable
5. **Počítejte s keyboard-only focus stavem** u položek v popoveru, protože outline se při hoveru myší skryje

## Common Implementation Patterns

### Permission Selection

```tsx
<Form.MultiSelect
  name="userPermissions"
  labelText="User Permissions"
  control={control}
  codeTableName="CT_PERMISSIONS"
  selectAll={true}
  filter={true}
  requiredFlag={true}
  description="Select all applicable permissions"
/>
```

### Tag/Category Selection

```tsx
<Form.MultiSelect
  name="articleTags"
  labelText="Article Tags"
  control={control}
  options={tagOptions}
  filter={true}
  itemCreate={{
    createText: "Tag not found?",
    buttonText: "Create New",
    onCreate: handleCreateTag,
  }}
  placeholder="Search and select tags..."
/>
```

### Multi-Language Selection

```tsx
<Form.MultiSelect
  name="supportedLanguages"
  labelText="Supported Languages"
  control={control}
  options={languageOptions}
  showChipShorthand={true}
  filter={true}
  tooltip="Select languages for application support"
/>
```

### Department/Team Assignment

```tsx
<Form.MultiSelect
  name="departments"
  labelText="Department Access"
  control={control}
  groupedOptions={departmentsByRegion}
  optionGroupChildren="departments"
  filter={true}
  selectAll={true}
  description="Select departments user can access"
/>
```

This component provides comprehensive multi-selection functionality with validation, filtering, grouping, and enhanced UX features for complex form requirements.

## Version Information

- **Component Version**: v4.8.0
- **Key Features**: Array-based selection, selected-value grouping for flat option lists, select-all support, filtering, grouped options, selection limit via `maxSelectionCount`, item creation with optional always-visible footer
- **Dependencies**: PrimeReact MultiSelect, React Hook Form, Zod validation, @neuron/core code tables

## Sync Metadata

- **Component Version:** v4.8.0
- **Component Source:** `packages/neuron/ui/src/lib/form/multiSelect/MultiSelect.tsx`
- **Guideline Command:** `/neuron-ui-multiselect`
- **Related Skill:** `neuron-ui-form-core`

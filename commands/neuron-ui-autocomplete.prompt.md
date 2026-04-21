---
agent: agent
---

# AutoComplete Component AI Guidelines

## For the AI Assistant

Your task is to integrate and configure the Neuron AutoComplete component in React applications. This guide provides essential instructions for implementing AutoComplete components, which provide comprehensive search-enabled input functionality with single/multiple selection, validation, accessibility, and form clearing capabilities through React Hook Form integration across all Neuron applications.

## Overview

The AutoComplete component is a comprehensive search-enabled input field built on PrimeReact AutoComplete, providing both single and multiple selection modes with validation, accessibility, and enhanced UX features through React Hook Form integration.

## Core Features

- **Single and Multiple Selection**: Toggle between single option selection and multiple chip-based selection
- **Search and Filtering**: Real-time filtering with client-side or server-side search capabilities
- **Rich Option Support**: Full IOption interface with icons, descriptions, tags, and grouping
- **Form Integration**: Complete React Hook Form and Zod validation support
- **Accessibility**: ARIA labels, keyboard navigation, and screen reader support
- **Focus Styling**: Focus outline položek našeptávače je vyhrazen pouze pro navigaci klávesnicí a při hoveru nad popoverem se skryje
- **Dropdown Mode**: Optional dropdown button for browsing all options
- **Item Creation**: Built-in support for creating new options on-the-fly

## Basic Implementation

### Single Selection AutoComplete

```tsx
import { Form } from "@neuron/ui";
import { useForm } from "react-hook-form";
import { zodResolver } from "@hookform/resolvers/zod";
import { z } from "zod";

const schema = z.object({
  selectedFruit: z.string().min(1, { message: "Please select a fruit" }),
});

const SingleSelectionForm = () => {
  const { control } = useForm({
    resolver: zodResolver(schema),
    mode: "onChange",
  });

  const fruitOptions = [
    { value: "apple", labelText: "Apple" },
    { value: "banana", labelText: "Banana" },
    { value: "orange", labelText: "Orange" },
    { value: "strawberry", labelText: "Strawberry" },
  ];

  return (
    <Form.AutoComplete
      name="selectedFruit"
      labelText="Choose a Fruit"
      control={control}
      options={fruitOptions}
      placeholderText="Search for fruits..."
      requiredFlag={true}
    />
  );
};
```

### Multiple Selection AutoComplete

```tsx
import { Form } from "@neuron/ui";
import { useForm } from "react-hook-form";
import { zodResolver } from "@hookform/resolvers/zod";
import { z } from "zod";

const schema = z.object({
  selectedSkills: z.array(z.string()).min(1, { message: "Select at least one skill" }),
});

const MultipleSelectionForm = () => {
  const { control } = useForm({
    resolver: zodResolver(schema),
    mode: "onChange",
    defaultValues: {
      selectedSkills: [],
    },
  });

  const skillOptions = [
    { value: "react", labelText: "React" },
    { value: "typescript", labelText: "TypeScript" },
    { value: "nodejs", labelText: "Node.js" },
    { value: "python", labelText: "Python" },
  ];

  return (
    <Form.AutoComplete
      name="selectedSkills"
      labelText="Technical Skills"
      control={control}
      options={skillOptions}
      isMultiple={true}
      placeholderText="Search and select skills..."
      requiredFlag={true}
      tooltip="Select your primary technical skills"
    />
  );
};
```

## IOption Interface Properties

Each option in the AutoComplete supports rich configuration through the IOption interface:

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

### Complex Options with Rich Properties

```tsx
import { baseIcons } from "@neuron/ui";

const complexUserOptions = [
  {
    value: "user-1",
    labelText: "Alice Johnson",
    shortLabelText: "Alice",
    icon: { iconDef: baseIcons.userSolid },
    descriptionText: "Senior Developer - Frontend Team",
    tagText: "Admin",
    disabled: false,
  },
  {
    value: "user-2",
    labelText: "Bob Smith",
    shortLabelText: "Bob",
    icon: { iconDef: baseIcons.userTieSolid },
    descriptionText: "Project Manager - Backend Team",
    tagText: "Manager",
    disabled: false,
  },
  {
    value: "user-3",
    labelText: "Charlie Brown",
    shortLabelText: "Charlie",
    icon: { iconDef: baseIcons.userSlashSolid },
    descriptionText: "Former Employee",
    tagText: "Inactive",
    disabled: true,
  },
];

<Form.AutoComplete
  name="assignedUser"
  labelText="Assign to User"
  control={control}
  options={complexUserOptions}
  placeholderText="Search for team members..."
  tooltip="Select a team member to assign this task"
/>;
```

## Search and Filtering

### Client-Side Filtering

```tsx
// Default behavior - searches in labelText
<Form.AutoComplete
  name="fruits"
  labelText="Fruits"
  control={control}
  options={fruitOptions}
  placeholderText="Type to search..."
  delay={300} // Debounce delay in milliseconds
/>;

// Custom client-side filter
const customFilter = ({ query, option }) => {
  const searchText = query.toLowerCase();
  return (
    option.labelText.toLowerCase().includes(searchText) ||
    option.descriptionText?.toLowerCase().includes(searchText) ||
    option.tagText?.toLowerCase().includes(searchText)
  );
};

<Form.AutoComplete
  name="users"
  labelText="Users"
  control={control}
  options={userOptions}
  filterOptions={customFilter}
  placeholderText="Search by name, role, or department..."
/>;
```

### Server-Side Search

```tsx
const [searchResults, setSearchResults] = useState([]);

const handleSearch = async (query: string) => {
  if (query.length >= 2) {
    const results = await searchAPI.users(query);
    setSearchResults(results);
  } else {
    setSearchResults([]);
  }
};

<Form.AutoComplete
  name="remoteUsers"
  labelText="Search Users"
  control={control}
  options={searchResults}
  onSearch={handleSearch}
  delay={500} // Longer delay for server calls
  placeholderText="Type at least 2 characters to search..."
  emptyText="No users found"
/>;
```

## Grouped Options

```tsx
const groupedTechOptions = [
  {
    value: "frontend-group",
    labelText: "Frontend Technologies",
    options: [
      {
        value: "react",
        labelText: "React",
        icon: { iconDef: baseIcons.reactBrand },
        descriptionText: "Component-based UI library",
      },
      {
        value: "vue",
        labelText: "Vue.js",
        icon: { iconDef: baseIcons.vueBrand },
        descriptionText: "Progressive JavaScript framework",
      },
    ],
  },
  {
    value: "backend-group",
    labelText: "Backend Technologies",
    options: [
      {
        value: "nodejs",
        labelText: "Node.js",
        icon: { iconDef: baseIcons.nodejsBrand },
        descriptionText: "JavaScript runtime environment",
      },
      {
        value: "python",
        labelText: "Python",
        icon: { iconDef: baseIcons.pythonBrand },
        descriptionText: "High-level programming language",
      },
    ],
  },
];

<Form.AutoComplete
  name="technology"
  labelText="Select Technology"
  control={control}
  options={groupedTechOptions}
  optionGroupChildren="options"
  placeholderText="Search technologies..."
/>;
```

## Dropdown Mode

```tsx
// Enable dropdown button for browsing all options
<Form.AutoComplete
  name="country"
  labelText="Country"
  control={control}
  options={countryOptions}
  dropdown={true}
  placeholderText="Search or select country..."
  onDropdownClick={() => console.info("Dropdown clicked")}
/>
```

## Code Table Integration

AutoComplete components can integrate with code tables for dynamic data loading with built-in caching and transformation.

### Basic Code Table Usage

```tsx
import { CT } from "app/common/codetables";

// Single selection with code table
<Form.AutoComplete
  name="department"
  labelText="Department"
  control={control}
  codeTableName={CT.CT_DEPARTMENTS}
  placeholderText="Search departments..."
  dropdown={true}
/>

// Multiple selection with code table
<Form.AutoComplete
  name="skills"
  labelText="Skills"
  control={control}
  codeTableName={CT.CT_SKILLS}
  isMultiple={true}
  placeholderText="Search and select skills..."
  handleCodeTableFilter={(items) =>
    items.filter(item => item.valid && item.primaryCategory === "technical")
  }
/>
```

### Code Table with Server-Side Search

```tsx
// For large datasets, combine code table with server search
const [searchResults, setSearchResults] = useState([]);
const { data: allUsers } = useUsersQuery(); // Code table hook

const handleUserSearch = async (query: string) => {
  if (query.length >= 2) {
    // Server-side search for better performance
    const results = await searchAPI.users(query);
    setSearchResults(results);
  } else {
    // Fallback to code table data
    setSearchResults(allUsers?.slice(0, 10) || []);
  }
};

<Form.AutoComplete
  name="assignedUsers"
  labelText="Assigned Users"
  control={control}
  options={searchResults}
  onSearch={handleUserSearch}
  isMultiple={true}
  delay={500}
  placeholderText="Search users..."
  emptyText="No users found"
/>;
```

### Enhanced Code Table Options

```tsx
// Transform code table data with rich properties
const handleDepartmentFilter = useCallback((items: ICodeTableItem[]) => {
  return items
    .filter((item) => item.valid && !item.deleted)
    .map((item) => ({
      value: item.code,
      labelText: item.name || item.code,
      shortLabelText: item.code,
      descriptionText: item.description,
      tagText: item.primaryCategory,
      icon: getDepartmentIcon(item.code),
      disabled: item.primaryCategory === "restricted",
    }));
}, []);

const getDepartmentIcon = (code: string) => {
  const iconMap = {
    IT: { iconDef: baseIcons.laptopSolid },
    HR: { iconDef: baseIcons.usersSolid },
    FIN: { iconDef: baseIcons.calculatorSolid },
  };
  return iconMap[code] || { iconDef: baseIcons.buildingSolid };
};

<Form.AutoComplete
  name="enhancedDepartment"
  labelText="Department"
  control={control}
  codeTableName={CT.CT_DEPARTMENTS}
  handleCodeTableFilter={handleDepartmentFilter}
  placeholderText="Search departments..."
  dropdown={true}
/>;
```

### Code Table Definition Example

```tsx
// app/common/codetables/codeTableTypes.ts
export const CT = {
  CT_DEPARTMENTS: "departments" as const,
  CT_SKILLS: "skills" as const,
  CT_USERS: "users" as const,
} as const;

// app/common/codetables/defs/skills.codeTable.ts
import { createRemoteCodeTable, registerCodeTable } from "@neuron/core";
import { CT } from "../codeTableTypes";
import { SkillsAdapter } from "../adapter/skills.adapter";

export const ctSkillsDef = createRemoteCodeTable(CT.CT_SKILLS, "/api/skills", {
  adapter: new SkillsAdapter(),
  queryParams: { active: "true", category: "all" },
  publicFacing: false,
});

// Register for hooks and selectors
export const { useSkillsQuery, selectSkills, selectSkillss, skillsEndpoint } = registerCodeTable(ctSkillsDef);
```

### Static Code Table for AutoComplete

```tsx
// app/common/codetables/defs/priorities.codeTable.ts
import { createStaticCodeTable, ICodeTableItem } from "@neuron/core";
import { CT } from "../codeTableTypes";

const priorityData: ICodeTableItem[] = [
  {
    id: { code: "high" },
    code: "high",
    name: "High Priority",
    description: "Urgent tasks requiring immediate attention",
    valid: true,
    primaryCategory: "urgent",
  },
  {
    id: { code: "medium" },
    code: "medium",
    name: "Medium Priority",
    description: "Important tasks with flexible deadlines",
    valid: true,
    primaryCategory: "standard",
  },
  {
    id: { code: "low" },
    code: "low",
    name: "Low Priority",
    description: "Tasks that can be completed when time allows",
    valid: true,
    primaryCategory: "flexible",
  },
];

export const ctPrioritiesDef = createStaticCodeTable(CT.CT_PRIORITIES, priorityData);

<Form.AutoComplete
  name="taskPriority"
  labelText="Task Priority"
  control={control}
  codeTableName={CT.CT_PRIORITIES}
  dropdown={true}
  placeholderText="Select priority level..."
/>;
```

## Item Creation

```tsx
const [customOptions, setCustomOptions] = useState(predefinedOptions);

const handleCreateOption = () => {
  const newTag = prompt("Enter new tag:");
  if (newTag) {
    const newOption = {
      value: newTag.toLowerCase().replace(/\s+/g, "-"),
      labelText: newTag,
    };
    setCustomOptions([...customOptions, newOption]);
  }
};

<Form.AutoComplete
  name="tags"
  labelText="Tags"
  control={control}
  options={customOptions}
  isMultiple={true}
  itemCreate={{
    createText: "Tag not found?",
    buttonText: "Create New Tag",
    onCreate: handleCreateOption,
    cleanValueOnCreate: true,
  }}
  placeholderText="Search or create tags..."
/>;
```

## Visual Enhancements

### Search Input with Icon

```tsx
import { baseIcons, Icon } from "@neuron/ui";

<Form.AutoComplete
  name="search"
  labelText="Search Products"
  control={control}
  options={productOptions}
  prefix={<Icon iconDef={baseIcons.magnifyingGlassSolid} color="var(--colorInputTextPlaceholder)" />}
  placeholderText="Search products..."
/>;
```

### With Addons

```tsx
<Form.AutoComplete
  name="priceRange"
  labelText="Price Range"
  control={control}
  options={priceOptions}
  leftAddonContent="$"
  rightAddonContent="USD"
  placeholderText="Select price range..."
/>
```

## Validation Patterns

### Required Selection

```tsx
const schema = z.object({
  category: z.string().min(1, { message: "Category is required" }),
  tags: z.array(z.string()).min(1, { message: "At least one tag is required" }),
});
```

### Conditional Validation

```tsx
const schema = z
  .object({
    userType: z.string(),
    department: z.string().optional(),
    skills: z.array(z.string()),
  })
  .refine(
    (data) => {
      if (data.userType === "employee") {
        return data.department && data.department.length > 0;
      }
      return true;
    },
    {
      message: "Department is required for employees",
      path: ["department"],
    },
  );
```

### Custom Validation with Dependencies

```tsx
<Form.AutoComplete
  name="department"
  labelText="Department"
  control={control}
  options={departmentOptions}
  deps={["userType"]} // Revalidate when userType changes
  requiredFlag={watch("userType") === "employee"}
/>
```

## Event Handling

### Selection Events

```tsx
const handleSelect = (event) => {
  console.info("Selected option:", event.value);
  // Custom logic when option is selected
};

const handleUnselect = (event) => {
  console.info("Unselected option:", event.value);
  // Custom logic when option is removed (multiple mode)
};

<Form.AutoComplete
  name="technologies"
  labelText="Technologies"
  control={control}
  options={techOptions}
  isMultiple={true}
  onSelect={handleSelect}
  onUnselect={handleUnselect}
/>;
```

### Clear Event

```tsx
const handleClear = () => {
  console.info("AutoComplete cleared");
  // Additional cleanup logic
};

<Form.AutoComplete
  name="selections"
  labelText="Selections"
  control={control}
  options={options}
  onClear={handleClear}
/>;
```

## Accessibility Features

### Screen Reader Support

```tsx
<Form.AutoComplete
  name="accessible"
  labelText="Accessible AutoComplete"
  control={control}
  options={options}
  required={true}
  tooltip="Use arrow keys to navigate options"
  description="Select one or more options from the list"
/>
```

### Keyboard Navigation

- **Arrow Keys**: Navigate through options
- **Enter**: Select highlighted option
- **Escape**: Close dropdown
- **Tab**: Move to next form element
- **Backspace**: Remove last selected item (multiple mode)

## State Management

### Read-Only and Disabled States

```tsx
// Read-only state
<Form.AutoComplete
  name="readOnlyField"
  labelText="Read-Only Selection"
  control={control}
  options={options}
  readOnly={true}
/>

// Disabled state
<Form.AutoComplete
  name="disabledField"
  labelText="Disabled Selection"
  control={control}
  options={options}
  disabled={true}
/>
```

### Validation States

```tsx
// Valid state
<Form.AutoComplete
  name="validField"
  labelText="Valid Selection"
  control={control}
  options={options}
  isValid={true}
/>

// Invalid state
<Form.AutoComplete
  name="invalidField"
  labelText="Invalid Selection"
  control={control}
  options={options}
  isValid={false}
/>
```

## Performance Optimization

### Large Option Sets

```tsx
// For large datasets, use server-side search
const [options, setOptions] = useState([]);
const [loading, setLoading] = useState(false);

const handleLargeDatasetSearch = async (query: string) => {
  if (query.length < 3) return;

  setLoading(true);
  try {
    const results = await api.searchLargeDataset(query);
    setOptions(results.slice(0, 50)); // Limit results
  } finally {
    setLoading(false);
  }
};

<Form.AutoComplete
  name="largeDataset"
  labelText="Search Large Dataset"
  control={control}
  options={options}
  onSearch={handleLargeDatasetSearch}
  delay={500}
  emptyText={loading ? "Searching..." : "No results found"}
  placeholderText="Type at least 3 characters..."
/>;
```

### Memoized Options

```tsx
const memoizedOptions = useMemo(() => {
  return expensiveOptionsTransformation(rawData);
}, [rawData]);

<Form.AutoComplete name="optimized" labelText="Optimized Options" control={control} options={memoizedOptions} />;
```

## Best Practices

### Validation Strategy

1. **Use appropriate validation** based on selection mode (string for single, array for multiple)
2. **Set mode to "onChange"** for real-time feedback during search and selection
3. **Provide clear error messages** indicating selection requirements
4. **Initialize with appropriate defaults** (null for single, empty array for multiple)

### UX Considerations

1. **Always provide labelText** for accessibility and form clarity
2. **Use meaningful placeholders** that indicate the search/selection purpose
3. **Enable server-side search for large datasets** (>100 items)
4. **Add tooltips for complex selection rules** or business logic
5. **Leverage rich option properties** for better user experience:
   - **Icons** for visual recognition and categorization
   - **Descriptions** for additional context about options
   - **Tags** for status indicators and filtering hints
   - **Short labels** for compact chip display in multiple mode
   - **Disabled state** for unavailable or conditional options
6. **Use grouped options** for logical categorization of large datasets
7. **Implement dropdown mode** when users need to browse all available options

### Performance

1. **Implement search delays** (300-500ms) to prevent excessive API calls
2. **Use client-side filtering** for small to medium datasets (<100 items)
3. **Limit search results** to reasonable numbers (50-100 items max)
4. **Memoize complex option transformations** to prevent recalculation

### Accessibility

1. **Ensure proper form labeling** with labelText and descriptions
2. **Provide required/optional indicators** for screen readers
3. **Include helpful placeholders and empty text** for guidance
4. **Use semantic grouping** with grouped options when applicable

## Common Implementation Patterns

### User/Team Member Selection

```tsx
<Form.AutoComplete
  name="assignee"
  labelText="Assign to Team Member"
  control={control}
  options={teamMemberOptions}
  placeholderText="Search team members..."
  tooltip="Select a team member to assign this task"
/>
```

### Tag/Category Management

```tsx
<Form.AutoComplete
  name="categories"
  labelText="Article Categories"
  control={control}
  options={categoryOptions}
  isMultiple={true}
  itemCreate={{
    createText: "Category not found?",
    onCreate: handleCreateCategory,
  }}
  placeholderText="Search or create categories..."
/>
```

### Location/Geographic Selection

```tsx
<Form.AutoComplete
  name="location"
  labelText="Location"
  control={control}
  options={locationOptions}
  dropdown={true}
  prefix={<Icon iconDef={baseIcons.mapMarkerSolid} />}
  placeholderText="Search or select location..."
/>
```

### Technology/Skills Selection

```tsx
<Form.AutoComplete
  name="skills"
  labelText="Technical Skills"
  control={control}
  options={skillOptions}
  isMultiple={true}
  optionGroupChildren="options" // If using grouped options
  placeholderText="Search and select skills..."
/>
```

This component provides comprehensive search and selection functionality with rich option support, validation, and accessibility features for complex form requirements. Use single mode for exclusive selections and multiple mode for collecting arrays of values with chip-based display.

## Sync Metadata

- **Component Version:** v5.3.1
- **Component Source:** `packages/neuron/ui/src/lib/form/autoComplete/AutoComplete.tsx`
- **Guideline Command:** `/neuron-ui-autocomplete`
- **Related Skill:** `neuron-ui-form-core`

---
agent: agent
---

# RadioButton Component AI Guidelines

## For the AI Assistant

Your task is to integrate and configure the Neuron RadioButton component in React applications. This guide provides essential instructions for implementing RadioButton components, which provide single-choice selection functionality with validation, tooltip support, and accessibility features through React Hook Form integration across all Neuron applications.

## Overview

The RadioButton and RadioSet components provide comprehensive single-selection input functionality built on PrimeReact RadioButton, with validation, accessibility, and enhanced UX features through React Hook Form integration. RadioSet supports both static options and dynamic code table integration for scalable data-driven applications.

## Component Types

### RadioButton

Individual radio button component for single boolean selection. Typically used when you need a single radio button with specific behavior.

### RadioSet (Recommended)

Group of radio buttons for single-choice selection from multiple options. This is the more commonly used pattern for radio button functionality.

## Core Usage

### Basic RadioSet Implementation

```tsx
import { Form } from "@neuron/ui";
import { useForm } from "react-hook-form";
import { zodResolver } from "@hookform/resolvers/zod";
import { z } from "zod";

const schema = z.object({
  gender: z.enum(["male", "female", "other"], {
    required_error: "Please select your gender",
  }),
  priority: z.string().min(1, { message: "Priority is required" }),
  notification: z.enum(["email", "sms", "push"]).optional(),
});

const genderOptions = [
  { value: "male", labelText: "Male" },
  { value: "female", labelText: "Female" },
  { value: "other", labelText: "Other" },
];

const priorityOptions = [
  { value: "high", labelText: "High Priority" },
  { value: "medium", labelText: "Medium Priority" },
  { value: "low", labelText: "Low Priority" },
];

const MyForm = () => {
  const { control } = useForm({
    resolver: zodResolver(schema),
    mode: "onChange",
  });

  return (
    <>
      <Form.RadioSet name="gender" labelText="Gender" control={control} options={genderOptions} required />

      <Form.RadioSet
        name="priority"
        labelText="Task Priority"
        control={control}
        options={priorityOptions}
        inline={true}
        requiredFlag={true}
      />

      <Form.RadioSet
        name="notification"
        labelText="Preferred Notification Method"
        control={control}
        options={notificationOptions}
        description="Choose how you'd like to receive notifications"
      />
    </>
  );
};
```

### Individual RadioButton Usage

```tsx
const MyForm = () => {
  const { control } = useForm({
    resolver: zodResolver(schema),
    mode: "onChange",
  });

  return (
    <>
      <Form.RadioButton name="acceptTerms" labelText="I accept the terms and conditions" control={control} required />

      <Form.RadioButton name="subscribeNewsletter" labelText="Subscribe to newsletter" control={control} />
    </>
  );
};
```

## Key Props Reference

### Essential Props (RadioSet)

- **name**: Field name for form registration
- **control**: React Hook Form control object
- **labelText**: Accessible label text for the radio set
- **options**: Static array of options (alternative to codeTableName)
- **codeTableName**: Code table identifier for dynamic options

### Essential Props (RadioButton)

- **name**: Field name for form registration
- **control**: React Hook Form control object
- **labelText**: Accessible label text for the radio button
- **checked**: Controlled checked state (optional)

### Data Source Props (RadioSet)

- **options**: Static options array with `value` and `labelText` properties
- **codeTableName**: String identifier for code table (e.g., `CT.CT_PRIORITIES`)
- **handleCodeTableFilter**: Function to filter code table results

### Validation & States

- **required**: HTML5 required attribute
- **requiredFlag**: Shows required indicator (\*)
- **optional**: Shows optional indicator
- **isValid**: Manual valid state indicator

### Enhanced Features

- **tooltip**: Help text with tooltip integration
- **description**: Helper text above the radio set
- **descriptionVariant**: `"default"` | `"success"` | `"warning"` | `"danger"` | `"info"`
- **literalView**: Display as read-only text instead of radio buttons
- **testId**: Custom test identifier for testing

### Layout & Styling

- **className**: CSS classes for grid positioning and custom styling
- **inline**: Display radio buttons horizontally instead of vertically (RadioSet only)
- **disabled**: Disable all radio button interactions
- **readOnly**: Make all radio buttons read-only

### Access Control

- **readonlyAccess**: Role-based read access control
- **fullAccess**: Role-based full access control

## Code Table Integration (RadioSet)

### Basic Code Table Usage

```tsx
import { CT } from "app/common/codetables";

<Form.RadioSet
  codeTableName={CT.CT_PRIORITIES}
  name="priority"
  labelText="Task Priority"
  control={control}
  required
/>

<Form.RadioSet
  codeTableName={CT.CT_DEPARTMENTS}
  name="department"
  labelText="Department"
  control={control}
  handleCodeTableFilter={(items) =>
    items.filter(item => item.valid && item.primaryCategory === "active")
  }
/>
```

### Code Table Definition Example

```tsx
// app/common/codetables/codeTableTypes.ts
export const CT = {
  CT_PRIORITIES: "priorities" as const,
  CT_DEPARTMENTS: "departments" as const,
  CT_USER_TYPES: "userTypes" as const,
} as const;

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
```

### Remote Code Table

```tsx
// app/common/codetables/defs/departments.codeTable.ts
import { createRemoteCodeTable, registerCodeTable } from "@neuron/core";
import { CT } from "../codeTableTypes";
import { DepartmentsAdapter } from "../adapter/departments.adapter";

export const ctDepartmentsDef = createRemoteCodeTable(CT.CT_DEPARTMENTS, "/api/departments", {
  adapter: new DepartmentsAdapter(),
  queryParams: { active: "true" },
  publicFacing: false,
});

// Register for hooks and selectors
export const { useDepartmentsQuery, selectDepartments, selectDepartmentss, departmentsEndpoint } =
  registerCodeTable(ctDepartmentsDef);
```

## Validation Patterns

### Zod Schema Validation

```tsx
// Enum validation for predefined values
const settingsSchema = z.object({
  theme: z.enum(["light", "dark", "auto"], {
    required_error: "Please select a theme",
  }),

  language: z.string().min(1, { message: "Language is required" }),

  notifications: z.enum(["all", "important", "none"]).optional(),
});

// Union type validation
const userTypeSchema = z.object({
  userType: z.union([z.literal("admin"), z.literal("user"), z.literal("guest")], {
    required_error: "User type is required",
  }),
});

// Conditional validation
const accessSchema = z
  .object({
    userRole: z.string(),
    department: z.string().optional(),
  })
  .refine(
    (data) => {
      // Department required for non-admin users
      if (data.userRole !== "admin") {
        return data.department && data.department.length > 0;
      }
      return true;
    },
    {
      message: "Department is required for non-admin users",
      path: ["department"],
    },
  );
```

### Complex Business Logic Validation

```tsx
const subscriptionSchema = z
  .object({
    plan: z.enum(["basic", "premium", "enterprise"]),
    features: z.array(z.string()).optional(),
  })
  .refine(
    (data) => {
      // Premium features require premium or enterprise plan
      const premiumFeatures = data.features?.filter((f) => f.includes("premium")) || [];
      if (premiumFeatures.length > 0) {
        return ["premium", "enterprise"].includes(data.plan);
      }
      return true;
    },
    {
      message: "Premium features require premium or enterprise plan",
      path: ["plan"],
    },
  );
```

## Advanced Features

### Custom Change Handling

```tsx
const handlePriorityChange = useCallback(
  (value: string, event: RadioButtonChangeEvent) => {
    console.log("Priority changed:", value);

    // Custom logic based on selection
    if (value === "high") {
      // Auto-enable urgent notifications
      setValue("urgentNotifications", true);
    }

    // Prevent default if needed
    if (someCondition) {
      event.preventDefault();
    }
  },
  [setValue],
);

<Form.RadioSet
  name="priority"
  labelText="Task Priority"
  control={control}
  options={priorityOptions}
  onChange={handlePriorityChange}
/>;
```

### Literal View Mode

Display selected value as text instead of interactive radio buttons:

```tsx
<Form.RadioSet
  name="selectedPriority"
  labelText="Current Priority"
  control={control}
  options={priorityOptions}
  literalView={true} // Shows selected option text
/>
```

### Inline Layout

Display radio buttons horizontally:

```tsx
<Form.RadioSet name="quickChoice" labelText="Quick Choice" control={control} options={quickOptions} inline={true} />
```

### Tooltip Integration

```tsx
<Form.RadioSet
  name="securityLevel"
  labelText="Security Level"
  control={control}
  options={securityOptions}
  tooltip={{
    text: "Higher security levels may impact performance",
    placement: "right",
  }}
/>
```

## Role-Based Access Control

### Access Control Props

```tsx
<Form.RadioSet
  name="adminFeatures"
  labelText="Admin Features"
  control={control}
  options={adminOptions}
  fullAccess="admin,superuser" // Only these roles can interact
  readonlyAccess="user,guest" // These roles see read-only version
/>
```

## Testing Integration

### Test ID Usage

```tsx
<Form.RadioSet
  name="testableField"
  labelText="Testable radio set"
  control={control}
  options={testOptions}
  testId="priority-selection"
/>;

// In tests
const radioSet = screen.getByTestId("priority-selection");
expect(radioSet).toBeInTheDocument();

// Test individual radio buttons
const highPriority = screen.getByLabelText("High Priority");
fireEvent.click(highPriority);
```

## Best Practices

### 1. Clear and Descriptive Labels

```tsx
// Good: Clear, mutually exclusive options
const statusOptions = [
  { value: "active", labelText: "Active" },
  { value: "inactive", labelText: "Inactive" },
  { value: "pending", labelText: "Pending Approval" },
];

<Form.RadioSet name="status" labelText="Account Status" control={control} options={statusOptions} />;

// Avoid: Vague or unclear labels
const badOptions = [
  { value: "option1", labelText: "Option 1" },
  { value: "option2", labelText: "Option 2" },
];
```

### 2. Logical Option Ordering

```tsx
// Order options logically (priority, alphabetical, frequency of use)
const priorityOptions = [
  { value: "high", labelText: "High Priority" },
  { value: "medium", labelText: "Medium Priority" },
  { value: "low", labelText: "Low Priority" },
];

const sizeOptions = [
  { value: "xs", labelText: "Extra Small" },
  { value: "s", labelText: "Small" },
  { value: "m", labelText: "Medium" },
  { value: "l", labelText: "Large" },
  { value: "xl", labelText: "Extra Large" },
];
```

### 3. Proper Validation Messages

```tsx
const schema = z.object({
  paymentMethod: z.enum(["credit", "debit", "paypal"], {
    required_error: "Please select a payment method to continue",
  }),
  shippingSpeed: z.string().min(1, {
    message: "Please choose a shipping option",
  }),
});
```

### 4. Appropriate Use Cases

```tsx
// Use RadioSet for mutually exclusive choices (2-7 options)
<Form.RadioSet
  name="deliveryMethod"
  labelText="Delivery Method"
  control={control}
  options={deliveryOptions} // 3-4 options
/>

// Use Select for many options (8+ options)
<Form.Select
  name="country"
  labelText="Country"
  control={control}
  codeTableName={CT.CT_COUNTRIES} // 100+ options
/>

// Use CheckBoxSet for multiple selections
<Form.CheckBoxSet
  name="features"
  labelText="Features"
  control={control}
  options={featureOptions} // Multiple selections allowed
/>
```

## Common Patterns

### Settings and Preferences

```tsx
const preferencesSchema = z.object({
  theme: z.enum(["light", "dark", "auto"]),
  language: z.enum(["en", "cs", "de"]),
  notifications: z.enum(["all", "important", "none"]),
});

<Form.RadioSet
  name="theme"
  labelText="Theme Preference"
  control={control}
  options={[
    { value: "light", labelText: "Light Mode" },
    { value: "dark", labelText: "Dark Mode" },
    { value: "auto", labelText: "Auto (System)" },
  ]}
  inline={true}
/>;
```

### Status Selection

```tsx
const statusSchema = z.object({
  taskStatus: z.enum(["todo", "in_progress", "review", "done"]),
  priority: z.enum(["low", "medium", "high", "urgent"]),
});

<Form.RadioSet
  name="taskStatus"
  labelText="Task Status"
  control={control}
  options={[
    { value: "todo", labelText: "To Do" },
    { value: "in_progress", labelText: "In Progress" },
    { value: "review", labelText: "Under Review" },
    { value: "done", labelText: "Completed" },
  ]}
  required
/>;
```

### User Type Selection

```tsx
const userSchema = z.object({
  userType: z.enum(["individual", "business", "enterprise"]),
  accountType: z.string().optional(),
});

<Form.RadioSet
  codeTableName={CT.CT_USER_TYPES}
  name="userType"
  labelText="Account Type"
  control={control}
  required
  description="Choose the type that best describes your account"
/>;
```

### Conditional Logic

```tsx
const { watch } = useForm({ control });
const selectedPlan = watch("plan");
const isEnterprise = selectedPlan === "enterprise";

<Form.RadioSet name="plan" labelText="Subscription Plan" control={control} options={planOptions} />;

{
  isEnterprise && (
    <Form.RadioSet
      name="support"
      labelText="Support Level"
      control={control}
      options={supportOptions}
      description="Enterprise customers can choose their support level"
    />
  );
}
```

## Performance Considerations

### Memoization for Large Sets

```tsx
const RadioSetGroup = memo(({ control }: { control: Control }) => {
  const handleFilter = useCallback((items: ICodeTableItem[]) => {
    return items.filter((item) => item.valid && !item.deleted);
  }, []);

  return (
    <Form.RadioSet
      name="department"
      labelText="Department"
      control={control}
      codeTableName={CT.CT_DEPARTMENTS}
      handleCodeTableFilter={handleFilter}
    />
  );
});
```

### Optimized Change Handlers

```tsx
const handleChange = useCallback(
  (value: string, event: RadioButtonChangeEvent) => {
    // Optimized change handling
    console.log("Selection changed:", value);
  },
  [], // Empty dependency array for stable reference
);
```

## Error Handling

### Validation Error Display

Radio components automatically display validation errors through the InputWrapper:

```tsx
// Validation errors are shown automatically
const schema = z.object({
  required: z.enum(["option1", "option2"], {
    required_error: "Please select an option",
  }),
});
```

### Code Table Error Handling

```tsx
// Code table loading errors are handled automatically
// Component shows loading state and error states as needed
<Form.RadioSet
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

  const selectedPriority = watch("priority");

  const onSubmit = (data: FormData) => {
    console.log("Form data:", data);
    console.log("Selected priority:", data.priority);
  };

  return (
    <form onSubmit={handleSubmit(onSubmit)}>
      <Form.RadioSet name="priority" labelText="Task Priority" control={control} options={priorityOptions} required />

      <Form.Input
        name="description"
        labelText="Description"
        control={control}
        description={`Priority level: ${selectedPriority || "Not selected"}`}
      />

      <button type="submit">Submit</button>
    </form>
  );
};
```

## Troubleshooting

### Common Issues

1. **Options not loading**: Check code table definition and registration
2. **Validation not working**: Verify Zod schema enum/string validation setup
3. **Code table filtering not applied**: Ensure handleCodeTableFilter returns filtered array
4. **Radio buttons not mutually exclusive**: Ensure all radio buttons in group have same `name` prop

### Debug Tips

```tsx
// Debug form state
const { watch } = useForm({ control });
const radioValue = watch("fieldName");
console.log("Current selection:", radioValue);

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

- **RadioButton Version**: v3.1.2
- **RadioSet Version**: v4.0.0
- **Features**: React Hook Form integration, code table support, role-based access control, read-only keyboard blocking
- **Dependencies**: PrimeReact RadioButton, React Hook Form, Zod validation, @neuron/core code tables

## Sync Metadata

- **Component Version:** v3.2.1
- **Component Source:** `packages/neuron/ui/src/lib/form/radioButton/RadioButton.tsx`
- **Guideline Command:** `/neuron-ui-radiobutton`
- **Related Skill:** `neuron-ui-form-choice`

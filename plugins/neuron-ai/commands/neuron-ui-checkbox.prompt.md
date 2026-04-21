---
agent: agent
---

# Checkbox Component AI Guidelines

## For the AI Assistant

Your task is to integrate and configure the Neuron Checkbox component in React applications. This guide provides essential instructions for implementing Checkbox components, which provide boolean input functionality with validation, accessibility, and enhanced UX features through React Hook Form integration across all Neuron applications.

## Overview

The CheckBox component is a comprehensive boolean input field built on PrimeReact Checkbox, providing validation, accessibility, and enhanced UX features through React Hook Form integration. It includes read-only keyboard blocking, role-based access control, and literal view support.

## Core Usage

### Basic Implementation

```tsx
import { Form } from "@neuron/ui";
import { useForm } from "react-hook-form";
import { zodResolver } from "@hookform/resolvers/zod";
import { z } from "zod";

const schema = z.object({
  acceptTerms: z.boolean().refine((val) => val === true, {
    message: "You must accept the terms and conditions",
  }),
  newsletter: z.boolean().optional(),
  notifications: z.boolean().default(false),
});

const MyForm = () => {
  const { control } = useForm({
    resolver: zodResolver(schema),
    mode: "onChange",
  });

  return (
    <>
      <Form.CheckBox name="acceptTerms" labelText="I accept the terms and conditions" control={control} required />

      <Form.CheckBox name="newsletter" labelText="Subscribe to newsletter" control={control} />

      <Form.CheckBox
        name="notifications"
        labelText="Enable push notifications"
        control={control}
        tooltip="Receive real-time updates about your account"
      />
    </>
  );
};
```

## Key Props Reference

### Essential Props

- **name**: Field name for form registration
- **control**: React Hook Form control object
- **labelText**: Accessible label text for the checkbox
- **checked**: Controlled checked state (optional, form state used if undefined)

### Validation & States

- **required**: HTML5 required attribute
- **isValid**: Manual valid state indicator
- **onChange**: Custom change handler with value and event parameters

### Enhanced Features

- **tooltip**: Help text with tooltip integration
- **literalView**: Display as read-only text instead of checkbox
- **testId**: Custom test identifier for testing

### Layout & Styling

- **className**: CSS classes for grid positioning and custom styling
- **disabled**: Disable checkbox interaction
- **readOnly**: Make checkbox read-only (blocks keyboard interactions)

### Access Control

- **readonlyAccess**: Role-based read access control
- **fullAccess**: Role-based full access control

## Validation Patterns

### Zod Schema Validation

```tsx
// Boolean validation with custom messages
const agreementSchema = z.object({
  terms: z.boolean().refine((val) => val === true, {
    message: "You must accept the terms to continue",
  }),
  privacy: z.boolean().refine((val) => val === true, {
    message: "Privacy policy acceptance is required",
  }),
  marketing: z.boolean().optional(), // Optional checkbox
});

// Complex conditional validation
const preferencesSchema = z
  .object({
    enableNotifications: z.boolean(),
    emailNotifications: z.boolean(),
    smsNotifications: z.boolean(),
  })
  .refine(
    (data) => {
      // If notifications are enabled, at least one method must be selected
      if (data.enableNotifications) {
        return data.emailNotifications || data.smsNotifications;
      }
      return true;
    },
    {
      message: "Please select at least one notification method",
      path: ["emailNotifications"], // Show error on this field
    },
  );
```

### Required Checkbox Validation

```tsx
// Required checkbox (must be true)
const consentSchema = z.object({
  dataProcessing: z.boolean().refine((val) => val === true, {
    message: "Data processing consent is required",
  }),
});

// Optional checkbox with default
const settingsSchema = z.object({
  autoSave: z.boolean().default(true),
  darkMode: z.boolean().default(false),
});
```

## Advanced Features

### Read-Only Mode with Keyboard Blocking

The CheckBox component automatically blocks keyboard interactions when in read-only mode using the `useReadonlyKeyboardBlock` hook:

```tsx
<Form.CheckBox
  name="readOnlyField"
  labelText="Read-only checkbox"
  control={control}
  readOnly={true} // Blocks spacebar, enter, and arrow keys
/>
```

### Literal View Mode

Display checkbox value as text instead of interactive element:

```tsx
<Form.CheckBox
  name="status"
  labelText="Agreement Status"
  control={control}
  literalView={true} // Shows "Yes" or "No" text
/>
```

### Custom Change Handling

```tsx
const handleCheckboxChange = useCallback((value: boolean, event: CheckboxChangeEvent) => {
  console.log("Checkbox changed:", value);

  // Prevent default form handling if needed
  if (someCondition) {
    event.preventDefault();
  }

  // Custom logic
  if (value) {
    // Handle checked state
  } else {
    // Handle unchecked state
  }
}, []);

<Form.CheckBox name="customHandling" labelText="Custom handling" control={control} onChange={handleCheckboxChange} />;
```

### Tooltip Integration

```tsx
<Form.CheckBox
  name="complexField"
  labelText="Enable advanced features"
  control={control}
  tooltip={{
    text: "This will enable experimental features that may affect performance",
    placement: "right",
  }}
/>
```

## Role-Based Access Control

### Access Control Props

```tsx
<Form.CheckBox
  name="adminFeature"
  labelText="Admin-only feature"
  control={control}
  fullAccess="admin,superuser" // Only these roles can interact
  readonlyAccess="user,guest" // These roles see read-only version
/>
```

## Testing Integration

### Test ID Usage

```tsx
<Form.CheckBox name="testableField" labelText="Testable checkbox" control={control} testId="agreement-checkbox" />;

// In tests
const checkbox = screen.getByTestId("agreement-checkbox");
expect(checkbox).toBeInTheDocument();
```

## Best Practices

### 1. Clear and Descriptive Labels

```tsx
// Good: Clear, actionable label
<Form.CheckBox
  name="newsletter"
  labelText="Send me weekly product updates via email"
  control={control}
/>

// Avoid: Vague or unclear labels
<Form.CheckBox
  name="option1"
  labelText="Option 1"
  control={control}
/>
```

### 2. Proper Validation Messages

```tsx
const schema = z.object({
  terms: z.boolean().refine((val) => val === true, {
    message: "Please read and accept the terms of service to continue",
  }),
});
```

### 3. Logical Grouping

```tsx
// Group related checkboxes
<fieldset>
  <legend>Notification Preferences</legend>
  <Form.CheckBox name="emailNotifications" labelText="Email notifications" control={control} />
  <Form.CheckBox name="smsNotifications" labelText="SMS notifications" control={control} />
  <Form.CheckBox name="pushNotifications" labelText="Push notifications" control={control} />
</fieldset>
```

### 4. Accessibility Considerations

```tsx
<Form.CheckBox
  name="accessibleField"
  labelText="Enable accessibility features"
  control={control}
  tooltip="Improves screen reader compatibility and keyboard navigation"
  required
/>
```

## Common Patterns

### Agreement/Consent Checkboxes

```tsx
const agreementSchema = z.object({
  terms: z.boolean().refine((val) => val === true, {
    message: "You must accept the terms and conditions"
  }),
  privacy: z.boolean().refine((val) => val === true, {
    message: "Privacy policy acceptance is required"
  }),
  marketing: z.boolean().optional()
});

<Form.CheckBox
  name="terms"
  labelText="I have read and agree to the Terms of Service"
  control={control}
  required
/>
<Form.CheckBox
  name="privacy"
  labelText="I accept the Privacy Policy"
  control={control}
  required
/>
<Form.CheckBox
  name="marketing"
  labelText="I would like to receive marketing communications (optional)"
  control={control}
/>
```

### Settings/Preferences

```tsx
const settingsSchema = z.object({
  autoSave: z.boolean().default(true),
  notifications: z.boolean().default(false),
  darkMode: z.boolean().default(false),
});

<Form.CheckBox
  name="autoSave"
  labelText="Automatically save changes"
  control={control}
  tooltip="Changes will be saved automatically every 30 seconds"
/>;
```

### Conditional Logic

```tsx
const { watch } = useForm({ control });
const enableNotifications = watch("enableNotifications");

<Form.CheckBox name="enableNotifications" labelText="Enable notifications" control={control} />;

{
  enableNotifications && (
    <>
      <Form.CheckBox name="emailNotifications" labelText="Email notifications" control={control} />
      <Form.CheckBox name="smsNotifications" labelText="SMS notifications" control={control} />
    </>
  );
}
```

## Performance Considerations

### Memoization for Complex Forms

```tsx
const CheckboxGroup = memo(({ control }: { control: Control }) => (
  <>
    <Form.CheckBox name="option1" labelText="Option 1" control={control} />
    <Form.CheckBox name="option2" labelText="Option 2" control={control} />
  </>
));
```

### Optimized Change Handlers

```tsx
const handleChange = useCallback(
  (value: boolean, event: CheckboxChangeEvent) => {
    // Optimized change handling
  },
  [], // Empty dependency array for stable reference
);
```

## Error Handling

### Validation Error Display

The CheckBox component automatically displays validation errors through the InputWrapper:

```tsx
// Validation errors are shown automatically
const schema = z.object({
  required: z.boolean().refine((val) => val === true, {
    message: "This field is required",
  }),
});
```

### Custom Error States

```tsx
<Form.CheckBox
  name="customError"
  labelText="Custom error handling"
  control={control}
  isValid={false} // Manual validation state
/>
```

## Integration with Other Components

### Form Integration

```tsx
const CompleteForm = () => {
  const { control, handleSubmit } = useForm({
    resolver: zodResolver(schema),
    mode: "onChange",
  });

  const onSubmit = (data: FormData) => {
    console.log("Form data:", data);
  };

  return (
    <form onSubmit={handleSubmit(onSubmit)}>
      <Form.CheckBox name="agreement" labelText="I agree to the terms" control={control} required />

      <button type="submit">Submit</button>
    </form>
  );
};
```

## Troubleshooting

### Common Issues

1. **Checkbox not updating**: Ensure `control` prop is passed correctly
2. **Validation not working**: Check Zod schema and resolver setup
3. **Read-only not working**: Verify `readOnly` prop and access control setup
4. **Keyboard interactions in read-only**: The component automatically blocks keyboard interactions when read-only

### Debug Tips

```tsx
// Debug form state
const { watch } = useForm({ control });
const checkboxValue = watch("fieldName");
console.log("Current value:", checkboxValue);
```

## Version Information

- **Component Version**: v3.2.1
- **Features**: React Hook Form integration, read-only keyboard blocking, role-based access control
- **Dependencies**: PrimeReact Checkbox, React Hook Form, Zod validation

## Sync Metadata

- **Component Version:** v3.2.5
- **Component Source:** `packages/neuron/ui/src/lib/form/checkBox/CheckBox.tsx`
- **Guideline Command:** `/neuron-ui-checkbox`
- **Related Skill:** `neuron-ui-form-choice`

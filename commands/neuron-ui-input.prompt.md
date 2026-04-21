---
agent: agent
---

# Input Component AI Guidelines

## For the AI Assistant

Your task is to integrate and configure the Neuron Input component in React applications. This guide provides essential instructions for implementing Input components, which provide comprehensive text input functionality with validation, formatting, accessibility, and enhanced UX features through React Hook Form integration across all Neuron applications.

## Overview

The Input component is a comprehensive text input field built on PrimeReact InputText, providing validation, accessibility, and enhanced UX features through React Hook Form integration.

## Core Usage

### Basic Implementation

```tsx
import { Form } from "@neuron/ui";
import { useForm } from "react-hook-form";
import { zodResolver } from "@hookform/resolvers/zod";
import { z } from "zod";

const schema = z.object({
  username: z.string().min(3, { message: "Username must be at least 3 characters" }),
  email: z.string().email({ message: "Invalid email format" }),
});

const MyForm = () => {
  const { control } = useForm({
    resolver: zodResolver(schema),
    mode: "onChange",
  });

  return (
    <>
      <Form.Input name="username" labelText="Username" control={control} placeholder="Enter username" />

      <Form.Input
        name="email"
        labelText="Email Address"
        type="email"
        control={control}
        placeholder="user@example.com"
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
- **type**: HTML input type (`"text"`, `"email"`, `"password"`, `"tel"`, `"url"`, etc.)

### Validation & States

- **required**: HTML5 required attribute
- **requiredFlag**: Shows required indicator (\*)
- **optional**: Shows optional indicator
- **isValid**: Manual valid state indicator
- **pattern**: HTML5 pattern for basic validation

### Enhanced Features

- **tooltip**: Help text with tooltip integration
- **description**: Helper text below input
- **descriptionVariant**: `"default"` | `"success"` | `"warning"` | `"danger"` | `"info"`
- **placeholder**: Placeholder text
- **format**: Regex pattern for keystroke validation

### Layout & Styling

- **inline**: Horizontal label/input layout
- **literalView**: Display as read-only text
- **className**: CSS classes for grid positioning
- **prefix**: Text prefix inside input
- **suffix**: Text suffix inside input
- **leftAddonContent**: Components before input (buttons, icons)
- **rightAddonContent**: Components after input (buttons, icons)

### Access Control

- **readOnly**: Make input read-only
- **disabled**: Disable input interaction
- **readonlyAccess**: Role-based read access
- **fullAccess**: Role-based full access

## Validation Patterns

### Zod Schema Validation

```tsx
// String validation with constraints
const userSchema = z.object({
  name: z
    .string()
    .min(5, { message: "Name must be at least 5 characters" })
    .max(50, { message: "Name cannot exceed 50 characters" }),

  email: z.string().email({ message: "Invalid email format" }).min(1, { message: "Email is required" }),

  phone: z
    .string()
    .min(11, { message: "Phone must be 11 digits" })
    .regex(/^\+?[\d\s-()]+$/, { message: "Invalid phone format" }),

  website: z.string().url({ message: "Invalid URL format" }).optional(),

  personalId: z.string().regex(/^\d{9,10}$/, { message: "Personal ID must be 9-10 digits" }),
});

// Usage with form
const { control } = useForm({
  resolver: zodResolver(userSchema),
  mode: "onChange", // Real-time validation
  defaultValues: {
    name: "",
    email: "",
    phone: "",
    website: "",
    personalId: "",
  },
});
```

### Format Validation (Keystroke Prevention)

```tsx
// Only allow digits and asterisk
<Form.Input
  name="code"
  labelText="Access Code"
  control={control}
  format="^(?:\d{0,9}\*|\d{1,10})$"
  tooltip="Enter up to 10 digits, or up to 9 digits followed by an asterisk"
/>

// Only allow letters and spaces
<Form.Input
  name="fullName"
  labelText="Full Name"
  control={control}
  format="^[a-zA-Z\s]*$"
/>

// Currency input (digits, decimal point, commas)
<Form.Input
  name="amount"
  labelText="Amount"
  control={control}
  format="^[\d.,]*$"
  prefix="$"
/>
```

### Manual Validation States

```tsx
// Valid state indication
<Form.Input
  name="validatedField"
  labelText="Pre-validated Field"
  control={control}
  isValid={true}
/>

// Pattern validation (HTML5)
<Form.Input
  name="postalCode"
  labelText="Postal Code"
  control={control}
  pattern="[0-9]{5}"
  placeholder="12345"
/>
```

## Visual Enhancement

### Prefix and Suffix

```tsx
// Text prefix/suffix
<Form.Input name="username" labelText="Username" control={control} prefix="@" suffix=".com" />;

// Icon prefix/suffix using baseIcons
import { baseIcons, Icon } from "@neuron/ui";

<Form.Input
  name="email"
  labelText="Email"
  control={control}
  prefix={<Icon iconDef={baseIcons.envelopeSolid} />}
  suffix="%"
/>;
```

### Addons (External Components)

```tsx
import { Button, Icon, baseIcons } from "@neuron/ui";

// Button addons
<Form.Input
  name="searchTerm"
  labelText="Search"
  control={control}
  leftAddonContent={<Button size="small" variant="info">Filter</Button>}
  rightAddonContent={<Button size="small" variant="primary">Search</Button>}
/>

// Icon addons
<Form.Input
  name="location"
  labelText="Address"
  control={control}
  leftAddonContent={<Icon iconDef={baseIcons.mapMarkerSolid} />}
  rightAddonContent={<Icon iconDef={baseIcons.globeSolid} />}
/>

// Mixed addons with prefix/suffix
<Form.Input
  name="price"
  labelText="Price"
  control={control}
  prefix="$"
  suffix=".00"
  leftAddonContent={<Icon iconDef={baseIcons.dollarSignSolid} />}
  rightAddonContent={<Button size="small">Calculate</Button>}
/>
```

## Input Types

### Common Input Types

```tsx
// Text input (default)
<Form.Input
  name="description"
  labelText="Description"
  type="text"
  control={control}
/>

// Email with validation
<Form.Input
  name="email"
  labelText="Email"
  type="email"
  control={control}
/>

// Phone number
<Form.Input
  name="phone"
  labelText="Phone"
  type="tel"
  control={control}
/>

// URL input
<Form.Input
  name="website"
  labelText="Website"
  type="url"
  control={control}
/>

// Search input
<Form.Input
  name="searchQuery"
  labelText="Search"
  type="search"
  control={control}
/>
```

## Layout Integration

### Grid Layout

```tsx
import { Fieldset } from "@neuron/ui";

<Fieldset legend="User Information" columnCount={12}>
  <Form.Input className="g-col-12 g-col-md-6 g-col-lg-4" name="firstName" labelText="First Name" control={control} />

  <Form.Input className="g-col-12 g-col-md-6 g-col-lg-4" name="lastName" labelText="Last Name" control={control} />

  <Form.Input className="g-col-12 g-col-lg-4" name="email" labelText="Email" type="email" control={control} />
</Fieldset>;
```

### Inline Layout

```tsx
// Horizontal label and input
<Form.Input name="inlineField" labelText="Inline Field" control={control} inline={true} />
```

## Accessibility Features

### Required Field Indicators

```tsx
// HTML5 required with visual indicator
<Form.Input
  name="requiredField"
  labelText="Required Field"
  control={control}
  required={true}
  requiredFlag={true}
/>

// Optional field indicator
<Form.Input
  name="optionalField"
  labelText="Optional Field"
  control={control}
  optional={true}
/>
```

### Help Text and Descriptions

```tsx
// Tooltip help
<Form.Input
  name="complexField"
  labelText="Complex Field"
  control={control}
  tooltip="This field requires specific formatting rules"
/>

// Description text with variants
<Form.Input
  name="statusField"
  labelText="Status"
  control={control}
  description="Current system status"
  descriptionVariant="info"
/>

// Success description
<Form.Input
  name="validatedField"
  labelText="Validated"
  control={control}
  description="Validation passed"
  descriptionVariant="success"
  isValid={true}
/>
```

## Advanced Usage

### Dependencies and Conditional Validation

```tsx
const schema = z
  .object({
    type: z.string(),
    specificField: z.string().optional(),
  })
  .refine(
    (data) => {
      if (data.type === "specific") {
        return data.specificField && data.specificField.length > 0;
      }
      return true;
    },
    {
      message: "Specific field is required when type is 'specific'",
      path: ["specificField"],
    },
  );

<Form.Input
  name="specificField"
  labelText="Specific Field"
  control={control}
  deps={["type"]} // Re-validate when 'type' changes
/>;
```

### Literal View (Read-Only Display)

```tsx
// Display as formatted text instead of input
<Form.Input name="displayValue" labelText="Display Value" control={control} literalView={true} />
```

### Custom Change Handlers

```tsx
const handleInputChange = (value: string, event: ChangeEvent) => {
  console.info("Input changed:", value);
  // Custom logic here
};

<Form.Input name="customField" labelText="Custom Field" control={control} onChange={handleInputChange} />;
```

## Form Integration Patterns

### Complete Form Example

```tsx
import { Form, SubmitButton, Fieldset } from "@neuron/ui";
import { useForm, SubmitHandler } from "react-hook-form";
import { zodResolver } from "@hookform/resolvers/zod";
import { z } from "zod";

interface FormData {
  username: string;
  email: string;
  website?: string;
  bio: string;
}

const formSchema = z.object({
  username: z.string().min(3).max(20),
  email: z.string().email(),
  website: z.string().url().optional(),
  bio: z.string().max(500),
});

const ProfileForm = () => {
  const { control, handleSubmit } = useForm<FormData>({
    resolver: zodResolver(formSchema),
    mode: "onChange",
    defaultValues: {
      username: "",
      email: "",
      website: "",
      bio: "",
    },
  });

  const onSubmit: SubmitHandler<FormData> = (data) => {
    console.info("Form submitted:", data);
  };

  return (
    <form onSubmit={handleSubmit(onSubmit)}>
      <Fieldset legend="Profile Information" columnCount={12}>
        <Form.Input
          className="g-col-12 g-col-md-6"
          name="username"
          labelText="Username"
          control={control}
          requiredFlag={true}
          prefix="@"
          placeholder="Enter username"
        />

        <Form.Input
          className="g-col-12 g-col-md-6"
          name="email"
          labelText="Email Address"
          type="email"
          control={control}
          requiredFlag={true}
          placeholder="user@example.com"
        />

        <Form.Input
          className="g-col-12"
          name="website"
          labelText="Website"
          type="url"
          control={control}
          optional={true}
          placeholder="https://example.com"
        />

        <Form.Input
          className="g-col-12"
          name="bio"
          labelText="Bio"
          control={control}
          placeholder="Tell us about yourself"
          description="Maximum 500 characters"
        />

        <div className="g-col-12">
          <SubmitButton control={control}>Save Profile</SubmitButton>
        </div>
      </Fieldset>
    </form>
  );
};
```

## Best Practices

### Validation Strategy

1. **Use Zod schemas** for comprehensive validation rules
2. **Set mode to "onChange"** for real-time feedback
3. **Provide clear error messages** with specific requirements
4. **Use format prop** for immediate keystroke filtering
5. **Combine with HTML5 validation** for accessibility

### UX Considerations

1. **Always provide labelText** for accessibility
2. **Use meaningful placeholders** that show expected format
3. **Add tooltips for complex fields** with specific requirements
4. **Show validation state immediately** for better user feedback
5. **Use appropriate input types** for better mobile keyboards

### Performance

1. **Memoize validation schemas** to prevent recreating on re-renders
2. **Use deps array** for conditional validation dependencies
3. **Avoid complex onChange handlers** that might impact typing performance

### Accessibility

1. **Use semantic HTML input types** (email, tel, url)
2. **Provide required/optional indicators** for screen readers
3. **Ensure proper form labeling** with labelText
4. **Include helpful descriptions** for complex validation rules

## Common Patterns

### Search Input

```tsx
<Form.Input
  name="searchQuery"
  labelText="Search"
  type="search"
  control={control}
  placeholder="Search users..."
  leftAddonContent={<Icon iconDef={baseIcons.searchSolid} />}
/>
```

### Currency Input

```tsx
<Form.Input name="amount" labelText="Amount" control={control} prefix="$" format="^\d*\.?\d{0,2}$" placeholder="0.00" />
```

### Username with Validation

```tsx
<Form.Input
  name="username"
  labelText="Username"
  control={control}
  prefix="@"
  format="^[a-zA-Z0-9_]*$"
  tooltip="Only letters, numbers, and underscores allowed"
/>
```

This component provides comprehensive text input functionality with validation, accessibility, and enhanced UX features for form-heavy applications.

## Sync Metadata

- **Component Version:** v4.1.1
- **Component Source:** `packages/neuron/ui/src/lib/form/input/Input.tsx`
- **Guideline Command:** `/neuron-ui-input`
- **Related Skill:** `neuron-ui-form-core`

---
agent: agent
---

# AI-Assisted Neuron Fieldset Component Integration Guide

## For the AI Assistant

Your task is to integrate and configure the Neuron Fieldset components in a React application. This guide provides comprehensive instructions for implementing the Fieldset component, which serves as a semantic form grouping element with built-in CSS Grid layout system for organizing form fields across all Neuron applications.

## Sync Metadata

- **Component Version:** v3.0.1
- **Component Source:** `packages/neuron/ui/src/lib/form/fieldset/Fieldset.tsx`
- **Guideline Command:** `/neuron-ui-fieldset`
- **Related Skill:** `neuron-ui-form-core`

## Introduction

The Fieldset system is a form layout component of the Neuron UI framework, designed to create semantic, accessible, and organized form sections with built-in CSS Grid layout capabilities across all Neuron applications.

### What is the Fieldset System?

The Fieldset component provides a standardized form grouping interface for your application with support for:

- Semantic HTML fieldset and legend elements
- Built-in CSS Grid layout system (12-column)
- Configurable column count
- Form field organization and grouping
- Accessibility compliance
- Responsive form layouts
- Proper form semantics

### Key Features

- **Semantic HTML**: Uses proper `<fieldset>` and `<legend>` elements for accessibility
- **CSS Grid Integration**: Built-in 12-column grid system for form layouts
- **Flexible Column Count**: Configurable grid columns (1-12)
- **Form Gap Spacing**: Consistent spacing between form elements
- **Accessibility**: Proper semantic grouping for screen readers
- **TypeScript Support**: Full type safety with comprehensive prop interfaces
- **Container Integration**: Works seamlessly with Container component

**Storybook Reference:** You can refer to the Storybook documentation for visual examples and interactive demos of the Fieldset component.

## Step 1: Basic Fieldset Implementation

### 1.1 Import the Fieldset Component

```tsx
import { Fieldset } from "@neuron/ui";
```

### 1.2 Basic Usage

Here's a simple implementation of the Fieldset component:

```tsx
import { Fieldset, Input } from "@neuron/ui";

const MyComponent = () => {
  return (
    <Fieldset legend="Personal Information">
      <Input name="firstName" labelText="First Name" className="g-col-6" />
      <Input name="lastName" labelText="Last Name" className="g-col-6" />
      <Input name="email" labelText="Email" className="g-col-12" />
    </Fieldset>
  );
};
```

### 1.3 Fieldset with Container

Use Fieldset within Container for proper layout:

```tsx
import { Fieldset, Input, Container } from "@neuron/ui";

const FormWithFieldset = () => {
  return (
    <Container>
      <Fieldset legend="User Details">
        <Input name="username" labelText="Username" className="g-col-6" />
        <Input name="password" labelText="Password" className="g-col-6" />
        <Input name="confirmPassword" labelText="Confirm Password" className="g-col-12" />
      </Fieldset>
    </Container>
  );
};
```

## Step 2: Grid Layout System

### 2.1 12-Column Grid Layout

The Fieldset uses a 12-column CSS Grid system by default:

```tsx
import { Fieldset, Input } from "@neuron/ui";

const GridLayoutExample = () => {
  return (
    <Fieldset legend="Contact Information" columnCount={12}>
      {/* Full width */}
      <Input name="fullName" labelText="Full Name" className="g-col-12" />

      {/* Half width */}
      <Input name="firstName" labelText="First Name" className="g-col-6" />
      <Input name="lastName" labelText="Last Name" className="g-col-6" />

      {/* Third width */}
      <Input name="day" labelText="Day" className="g-col-4" />
      <Input name="month" labelText="Month" className="g-col-4" />
      <Input name="year" labelText="Year" className="g-col-4" />

      {/* Quarter width */}
      <Input name="field1" labelText="Field 1" className="g-col-3" />
      <Input name="field2" labelText="Field 2" className="g-col-3" />
      <Input name="field3" labelText="Field 3" className="g-col-3" />
      <Input name="field4" labelText="Field 4" className="g-col-3" />
    </Fieldset>
  );
};
```

### 2.2 Custom Column Count

Adjust the grid column count based on your needs:

```tsx
import { Fieldset, Input } from "@neuron/ui";

const CustomColumnExample = () => {
  return (
    <div>
      {/* 6-column grid */}
      <Fieldset legend="Simple Layout" columnCount={6}>
        <Input name="field1" labelText="Field 1" className="g-col-2" />
        <Input name="field2" labelText="Field 2" className="g-col-2" />
        <Input name="field3" labelText="Field 3" className="g-col-2" />
        <Input name="field4" labelText="Full Width" className="g-col-6" />
      </Fieldset>

      {/* 4-column grid */}
      <Fieldset legend="Compact Layout" columnCount={4}>
        <Input name="compact1" labelText="Field 1" className="g-col-1" />
        <Input name="compact2" labelText="Field 2" className="g-col-1" />
        <Input name="compact3" labelText="Field 3" className="g-col-1" />
        <Input name="compact4" labelText="Field 4" className="g-col-1" />
      </Fieldset>
    </div>
  );
};
```

## Step 3: Form Organization Patterns

### 3.1 Personal Information Form

Organize personal information fields:

```tsx
import { Fieldset, Input, Container } from "@neuron/ui";

const PersonalInfoForm = () => {
  return (
    <Container>
      <Fieldset legend="Personal Information">
        <Input name="firstName" labelText="First Name" className="g-col-6" />
        <Input name="lastName" labelText="Last Name" className="g-col-6" />
        <Input name="birthDate" labelText="Birth Date" className="g-col-4" />
        <Input name="age" labelText="Age" className="g-col-2" />
        <Input name="gender" labelText="Gender" className="g-col-6" />
        <Input name="email" labelText="Email Address" className="g-col-12" />
        <Input name="phone" labelText="Phone Number" className="g-col-6" />
        <Input name="emergencyContact" labelText="Emergency Contact" className="g-col-6" />
      </Fieldset>
    </Container>
  );
};
```

### 3.2 Address Information Form

Structure address fields logically:

```tsx
import { Fieldset, Input, Container } from "@neuron/ui";

const AddressForm = () => {
  return (
    <Container>
      <Fieldset legend="Address Information">
        <Input name="streetAddress" labelText="Street Address" className="g-col-8" />
        <Input name="apartmentUnit" labelText="Apt/Unit" className="g-col-4" />
        <Input name="city" labelText="City" className="g-col-6" />
        <Input name="state" labelText="State/Province" className="g-col-3" />
        <Input name="zipCode" labelText="ZIP/Postal Code" className="g-col-3" />
        <Input name="country" labelText="Country" className="g-col-12" />
      </Fieldset>
    </Container>
  );
};
```

### 3.3 Multiple Fieldsets in One Form

Organize complex forms with multiple fieldsets:

```tsx
import { Fieldset, Input, TextArea, Container } from "@neuron/ui";

const CompleteUserForm = () => {
  return (
    <Container>
      <form>
        <Fieldset legend="Personal Information">
          <Input name="firstName" labelText="First Name" className="g-col-6" />
          <Input name="lastName" labelText="Last Name" className="g-col-6" />
          <Input name="email" labelText="Email" className="g-col-8" />
          <Input name="phone" labelText="Phone" className="g-col-4" />
        </Fieldset>

        <Fieldset legend="Address Details">
          <Input name="street" labelText="Street Address" className="g-col-9" />
          <Input name="zip" labelText="ZIP Code" className="g-col-3" />
          <Input name="city" labelText="City" className="g-col-6" />
          <Input name="country" labelText="Country" className="g-col-6" />
        </Fieldset>

        <Fieldset legend="Additional Information">
          <TextArea name="bio" labelText="Biography" className="g-col-12" />
          <Input name="website" labelText="Website" className="g-col-6" />
          <Input name="linkedin" labelText="LinkedIn" className="g-col-6" />
        </Fieldset>
      </form>
    </Container>
  );
};
```

## Step 4: Responsive Form Layouts

### 4.1 Responsive Grid Classes

Use responsive grid classes for different screen sizes:

```tsx
import { Fieldset, Input } from "@neuron/ui";

const ResponsiveForm = () => {
  return (
    <Fieldset legend="Responsive Layout">
      {/* Full width on mobile, half on desktop */}
      <Input name="field1" labelText="Responsive Field 1" className="g-col-12 g-col-md-6" />
      <Input name="field2" labelText="Responsive Field 2" className="g-col-12 g-col-md-6" />

      {/* Full width on mobile, third on desktop */}
      <Input name="field3" labelText="Field 3" className="g-col-12 g-col-md-4" />
      <Input name="field4" labelText="Field 4" className="g-col-12 g-col-md-4" />
      <Input name="field5" labelText="Field 5" className="g-col-12 g-col-md-4" />
    </Fieldset>
  );
};
```

### 4.2 Mobile-First Approach

Design forms with mobile-first responsive patterns:

```tsx
import { Fieldset, Input, Select } from "@neuron/ui";

const MobileFirstForm = () => {
  return (
    <Fieldset legend="Mobile-Optimized Form">
      {/* Stack on mobile, side-by-side on tablet+ */}
      <Input name="firstName" labelText="First Name" className="g-col-12 g-col-sm-6" />
      <Input name="lastName" labelText="Last Name" className="g-col-12 g-col-sm-6" />

      {/* Full width on mobile and tablet, smaller on desktop */}
      <Input name="email" labelText="Email Address" className="g-col-12 g-col-lg-8" />
      <Select name="country" labelText="Country" className="g-col-12 g-col-lg-4" options={[]} />
    </Fieldset>
  );
};
```

## Step 5: Form Integration Patterns

### 5.1 React Hook Form Integration

Integrate with React Hook Form for validation:

```tsx
import { Fieldset, Input, SubmitButton } from "@neuron/ui";
import { useForm } from "react-hook-form";

const HookFormExample = () => {
  const { control, handleSubmit } = useForm();

  const onSubmit = (data) => {
    console.info("Form data:", data);
  };

  return (
    <form onSubmit={handleSubmit(onSubmit)}>
      <Fieldset legend="User Registration">
        <Input
          name="username"
          labelText="Username"
          className="g-col-6"
          control={control}
          rules={{ required: "Username is required" }}
        />
        <Input
          name="email"
          labelText="Email"
          className="g-col-6"
          control={control}
          rules={{ required: "Email is required" }}
        />
        <Input
          name="password"
          labelText="Password"
          type="password"
          className="g-col-12"
          control={control}
          rules={{ required: "Password is required" }}
        />

        <div className="g-col-12">
          <SubmitButton control={control}>Register</SubmitButton>
        </div>
      </Fieldset>
    </form>
  );
};
```

### 5.2 Conditional Field Display

Show/hide fields based on conditions:

```tsx
import { Fieldset, Input, CheckBox } from "@neuron/ui";
import { useState } from "react";

const ConditionalFieldsForm = () => {
  const [showBillingAddress, setShowBillingAddress] = useState(false);

  return (
    <div>
      <Fieldset legend="Shipping Information">
        <Input name="shippingStreet" labelText="Street Address" className="g-col-8" />
        <Input name="shippingZip" labelText="ZIP Code" className="g-col-4" />
        <Input name="shippingCity" labelText="City" className="g-col-6" />
        <Input name="shippingState" labelText="State" className="g-col-6" />

        <div className="g-col-12">
          <CheckBox
            name="differentBilling"
            labelText="Billing address is different"
            checked={showBillingAddress}
            onChange={(e) => setShowBillingAddress(e.target.checked)}
          />
        </div>
      </Fieldset>

      {showBillingAddress && (
        <Fieldset legend="Billing Information">
          <Input name="billingStreet" labelText="Street Address" className="g-col-8" />
          <Input name="billingZip" labelText="ZIP Code" className="g-col-4" />
          <Input name="billingCity" labelText="City" className="g-col-6" />
          <Input name="billingState" labelText="State" className="g-col-6" />
        </Fieldset>
      )}
    </div>
  );
};
```

## Step 6: Fieldset Props Reference

### 6.1 Core Fieldset Props

| Prop        | Type        | Default | Description                   |
| ----------- | ----------- | ------- | ----------------------------- |
| children    | `ReactNode` | -       | Form fields and content       |
| legend      | `string`    | -       | Fieldset legend text          |
| columnCount | `number`    | `12`    | Number of grid columns (1-12) |
| className   | `string`    | -       | Additional CSS classes        |
| testId      | `string`    | -       | Test identifier               |

### 6.2 Grid Column Classes

Use these CSS classes on child elements:

| Class                         | Description                    |
| ----------------------------- | ------------------------------ |
| `g-col-1` to `g-col-12`       | Column span (1-12 columns)     |
| `g-col-sm-1` to `g-col-sm-12` | Small screen column span       |
| `g-col-md-1` to `g-col-md-12` | Medium screen column span      |
| `g-col-lg-1` to `g-col-lg-12` | Large screen column span       |
| `g-col-xl-1` to `g-col-xl-12` | Extra large screen column span |

## Step 7: Best Practices

### 7.1 Semantic Grouping

Group related form fields logically:

```tsx
{/* Good: Logical grouping */}
<Fieldset legend="Contact Information">
  <Input name="email" labelText="Email" className="g-col-6" />
  <Input name="phone" labelText="Phone" className="g-col-6" />
</Fieldset>

<Fieldset legend="Address">
  <Input name="street" labelText="Street" className="g-col-8" />
  <Input name="zip" labelText="ZIP" className="g-col-4" />
</Fieldset>
```

### 7.2 Grid Layout Guidelines

- Use appropriate column spans for field importance
- Consider mobile-first responsive design
- Maintain consistent spacing with form-gap

```tsx
{
  /* Good: Appropriate column usage */
}
<Fieldset legend="User Details">
  <Input name="firstName" labelText="First Name" className="g-col-6" />
  <Input name="lastName" labelText="Last Name" className="g-col-6" />
  <Input name="email" labelText="Email Address" className="g-col-12" />
</Fieldset>;
```

### 7.3 Legend Usage

- Always provide meaningful legends
- Keep legends concise and descriptive
- Use legends to describe the purpose of the field group

```tsx
{/* Good: Descriptive legends */}
<Fieldset legend="Personal Information">
<Fieldset legend="Billing Address">
<Fieldset legend="Account Preferences">
```

### 7.4 Accessibility Considerations

- Use proper semantic HTML with fieldset and legend
- Ensure logical tab order
- Group related fields semantically

```tsx
{
  /* Good: Accessible fieldset */
}
<Fieldset legend="Payment Information">
  <Input name="cardNumber" labelText="Card Number" className="g-col-8" />
  <Input name="cvv" labelText="CVV" className="g-col-4" />
  <Input name="expiryDate" labelText="Expiry Date" className="g-col-6" />
  <Input name="cardholderName" labelText="Cardholder Name" className="g-col-6" />
</Fieldset>;
```

## Step 8: Common Mistakes to Avoid

### 8.1 Don't Forget Grid Classes

```tsx
{
  /* ❌ INCORRECT: No grid classes */
}
<Fieldset legend="Form Fields">
  <Input name="field1" labelText="Field 1" />
  <Input name="field2" labelText="Field 2" />
</Fieldset>;

{
  /* ✅ CORRECT: Proper grid classes */
}
<Fieldset legend="Form Fields">
  <Input name="field1" labelText="Field 1" className="g-col-6" />
  <Input name="field2" labelText="Field 2" className="g-col-6" />
</Fieldset>;
```

### 8.2 Don't Exceed Column Count

```tsx
{
  /* ❌ INCORRECT: Column span exceeds columnCount */
}
<Fieldset legend="Fields" columnCount={6}>
  <Input name="field" labelText="Field" className="g-col-12" />
</Fieldset>;

{
  /* ✅ CORRECT: Column span within limits */
}
<Fieldset legend="Fields" columnCount={6}>
  <Input name="field" labelText="Field" className="g-col-6" />
</Fieldset>;
```

### 8.3 Don't Skip Container Integration

```tsx
{
  /* ❌ INCORRECT: No container wrapper */
}
<Fieldset legend="Form">
  <Input name="field" labelText="Field" className="g-col-12" />
</Fieldset>;

{
  /* ✅ CORRECT: Proper container usage */
}
<Container>
  <Fieldset legend="Form">
    <Input name="field" labelText="Field" className="g-col-12" />
  </Fieldset>
</Container>;
```

### 8.4 Don't Use Meaningless Legends

```tsx
{
  /* ❌ INCORRECT: Generic legend */
}
<Fieldset legend="Fields">
  <Input name="email" labelText="Email" className="g-col-12" />
</Fieldset>;

{
  /* ✅ CORRECT: Descriptive legend */
}
<Fieldset legend="Contact Information">
  <Input name="email" labelText="Email" className="g-col-12" />
</Fieldset>;
```

## Key Takeaways

The Neuron Fieldset component provides a semantic, accessible, and organized foundation for form layouts. Key points to remember:

1. **Semantic HTML** - Uses proper fieldset and legend elements for accessibility
2. **CSS Grid integration** - Built-in 12-column grid system with configurable columns
3. **Container integration** - Works seamlessly with Container component
4. **Responsive design** - Support for responsive grid classes
5. **Form organization** - Logical grouping of related form fields
6. **Accessibility compliance** - Proper semantic structure for screen readers
7. **Grid classes required** - Always use g-col-\* classes on child elements
8. **Meaningful legends** - Provide descriptive legends for field groups
9. **Mobile-first approach** - Design with responsive patterns in mind
10. **Form integration** - Works with React Hook Form and validation libraries

By following these guidelines, you'll create well-organized, accessible, and responsive form layouts that enhance your Neuron applications' user experience and maintainability.

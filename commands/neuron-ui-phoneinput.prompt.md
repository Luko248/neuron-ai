---
agent: agent
---

# PhoneInput Component AI Guidelines

## For the AI Assistant

Your task is to integrate and configure the Neuron PhoneInput component in React applications. This guide provides essential instructions for implementing PhoneInput components, which provide a telephone number input with built-in country code selection, automatic country detection from pasted values, and dynamic input masks using React Hook Form integration.

## Overview

PhoneInput is a specialized form component built on top of `Form.MaskedInput`. It provides two modes of operation:

- **Auto mode** (default): Built-in country select dropdown is rendered as a left addon. The country code, mask, and placeholder are managed automatically. Country is auto-detected when pasting an international phone number.
- **Custom left addon mode**: Pass `leftAddonContent` to fully control the left side. Mask and placeholder must be managed externally by the consumer.

## Core Usage — Auto Mode (Recommended)

```tsx
import { Form } from "@neuron/ui";
import { zodResolver } from "@hookform/resolvers/zod";
import { useForm } from "react-hook-form";
import * as z from "zod";

const schema = z.object({
  phone: z.object({
    countryCode: z.string().min(1, { message: "Country code is required" }),
    phoneNumber: z.string().min(5, { message: "Phone number is required" }),
  }),
});

type FormValues = z.infer<typeof schema>;

const MyForm = () => {
  const { control, handleSubmit } = useForm<FormValues>({
    defaultValues: {
      phone: {
        countryCode: "CZ",
        phoneNumber: "+420",
      },
    },
    resolver: zodResolver(schema),
    mode: "onChange",
  });

  return (
    <form onSubmit={handleSubmit(console.log)}>
      <Form.PhoneInput name="phone" control={control} labelText="Phone Number" mask="" required />
    </form>
  );
};
```

> **Note**: In auto mode the `mask` prop is required by TypeScript but overridden internally — pass an empty string or the default mask. The component derives the mask from the selected country automatically.

## Custom Left Addon Mode

Use `leftAddonContent` to override internal country handling and manage the mask externally:

```tsx
import { Form } from "@neuron/ui";
import { useMemo } from "react";
import { useForm, useWatch } from "react-hook-form";
import * as z from "zod";

const COUNTRY_OPTIONS = [
  { value: "CZ", labelText: "CZ", descriptionText: "Czech Republic", tagText: "+420" },
  { value: "SK", labelText: "SK", descriptionText: "Slovakia", tagText: "+421" },
  { value: "DE", labelText: "DE", descriptionText: "Germany", tagText: "+49" },
];

const schema = z.object({
  countryCode: z.string().min(1),
  phoneNumber: z.string().min(5, { message: "Phone number is required" }),
});

type FormValues = z.infer<typeof schema>;

const MyForm = () => {
  const { control } = useForm<FormValues>({
    defaultValues: { countryCode: "CZ", phoneNumber: "+420" },
  });

  const selectedCountry = useWatch({ control, name: "countryCode" });
  const mask = selectedCountry === "CZ" ? "+999 999 999 999" : "+99 999 999 999";

  const countryAddon = useMemo(
    () => <Form.Select name="countryCode" control={control} options={COUNTRY_OPTIONS} filter />,
    [control],
  );

  return (
    <Form.PhoneInput
      name="phoneNumber"
      control={control}
      labelText="Phone Number"
      mask={mask}
      placeholder="+420 123 456 789"
      leftAddonContent={countryAddon}
    />
  );
};
```

## Key Props

| Prop                 | Type                               | Required | Description                                                        |
| -------------------- | ---------------------------------- | -------- | ------------------------------------------------------------------ |
| `name`               | `string`                           | ✅       | Field name for form registration (parent object name in auto mode) |
| `control`            | `Control`                          | ✅       | React Hook Form control object                                     |
| `mask`               | `string`                           | ✅       | Input mask pattern — overridden internally in auto mode            |
| `labelText`          | `string`                           | —        | Accessible label text                                              |
| `placeholder`        | `string`                           | —        | Placeholder text — auto-generated in auto mode                     |
| `leftAddonContent`   | `ReactNode`                        | —        | Custom left addon; providing this disables auto country handling   |
| `rightAddonContent`  | `ReactNode`                        | —        | Custom right addon                                                 |
| `disabled`           | `boolean`                          | —        | Disables the input                                                 |
| `readOnly`           | `boolean`                          | —        | Makes the input read-only                                          |
| `required`           | `boolean`                          | —        | Marks field as required                                            |
| `requiredFlag`       | `boolean`                          | —        | Shows required asterisk without validation                         |
| `optional`           | `boolean`                          | —        | Shows optional indicator                                           |
| `inline`             | `boolean`                          | —        | Renders label and input in a single row                            |
| `literalView`        | `boolean`                          | —        | Renders value as read-only text (Literal component)                |
| `isValid`            | `boolean`                          | —        | Marks the field as valid (shows success state)                     |
| `description`        | `string`                           | —        | Helper text shown above the input                                  |
| `descriptionVariant` | `"default" \| "info" \| "warning"` | —        | Style variant for the description                                  |
| `prefix`             | `string`                           | —        | Static text prefix inside the input                                |
| `suffix`             | `string`                           | —        | Static text suffix inside the input                                |
| `tooltip`            | `string`                           | —        | Tooltip text on the label                                          |
| `className`          | `string`                           | —        | Additional CSS class                                               |
| `testId`             | `string`                           | —        | Custom test identifier                                             |
| `readonlyAccess`     | `string`                           | —        | Role key for readonly access override                              |
| `fullAccess`         | `string`                           | —        | Role key for full access override                                  |

## Supported Countries (Auto Mode)

The built-in country list used in auto mode:

| Code | Country        | Numeric Code |
| ---- | -------------- | ------------ |
| `CZ` | Czech Republic | +420         |
| `SK` | Slovakia       | +421         |
| `DE` | Germany        | +49          |
| `AT` | Austria        | +43          |
| `PL` | Poland         | +48          |
| `GB` | United Kingdom | +44          |
| `US` | United States  | +1           |
| `FR` | France         | +33          |
| `IT` | Italy          | +39          |
| `ES` | Spain          | +34          |
| `NL` | Netherlands    | +31          |
| `BE` | Belgium        | +32          |
| `CH` | Switzerland    | +41          |
| `HU` | Hungary        | +36          |

## Auto-Detection from Paste

In auto mode, when the user pastes an international phone number (e.g. `+420723456789`), the component:

1. Parses the clipboard text
2. Matches the country by the longest numeric prefix
3. Updates the country select to the matched country
4. Normalizes the phone value to the correct format

No additional configuration is needed — this behavior is automatic.

## Form Schema Patterns

### Auto Mode (nested object)

```ts
const schema = z.object({
  phone: z.object({
    countryCode: z.string().min(1, { message: "Select a country" }),
    phoneNumber: z.string().min(5, { message: "Enter a valid phone number" }),
  }),
});

// Default values
const defaultValues = {
  phone: {
    countryCode: "CZ", // ISO 2-letter code (e.g. "CZ", "SK", "DE")
    phoneNumber: "+420", // Prefilled with numeric country code
  },
};
```

### Custom Addon Mode (flat fields)

```ts
const schema = z.object({
  countryCode: z.string().min(1),
  phoneNumber: z.string().min(5, { message: "Enter a valid phone number" }),
});

const defaultValues = {
  countryCode: "CZ",
  phoneNumber: "+420",
};
```

## Validation

```ts
// Auto mode — validate the phone number value
const schema = z.object({
  phone: z.object({
    countryCode: z.string().min(1, { message: "Country code is required" }),
    phoneNumber: z
      .string()
      .min(5, { message: "Phone number is too short" })
      .regex(/^\+\d{1,3}\s\d[\d\s\-().]*$/, { message: "Invalid phone number format" }),
  }),
});
```

## States

```tsx
// Disabled
<Form.PhoneInput name="phone" control={control} mask="" labelText="Phone" disabled />

// Read-only
<Form.PhoneInput name="phone" control={control} mask="" labelText="Phone" readOnly />

// Literal view (read-only text rendering)
<Form.PhoneInput name="phone" control={control} mask="" labelText="Phone" literalView />

// With description
<Form.PhoneInput
  name="phone"
  control={control}
  mask=""
  labelText="Phone"
  description="Include country code, e.g. +420 for Czech Republic"
/>

// Valid state
<Form.PhoneInput name="phone" control={control} mask="" labelText="Phone" isValid />
```

## Role-Based Access Control

```tsx
const { userPermissions } = useAuth();

<Form.PhoneInput
  name="phone"
  control={control}
  mask=""
  labelText="Phone Number"
  fullAccess={userPermissions.includes("edit_contact") ? "editor" : undefined}
  readonlyAccess={userPermissions.includes("view_contact") ? "viewer" : undefined}
/>;
```

## Testing Integration

```tsx
<Form.PhoneInput name="phone" control={control} mask="" labelText="Phone Number" testId="phone-input" />;

// In tests
describe("PhoneInput", () => {
  it("should accept valid phone number", async () => {
    render(<TestForm />);
    const input = screen.getByTestId("phone-input");
    fireEvent.change(input, { target: { value: "+420 723 456 789" } });
    await waitFor(() => {
      expect(input).toHaveValue("+420 723 456 789");
    });
  });
});
```

## Common Mistakes

1. **Do not use `Form.MaskedInput` directly for phone numbers** — use `Form.PhoneInput` instead, which handles country-aware masking and auto-detection.

2. **In auto mode, define a nested object in the schema** — the component creates a sub-scope on the passed `name` containing `countryCode` and `phoneNumber`.

3. **Do not pass `leftAddonContent` if you want auto country handling** — providing any truthy value (even an empty string) disables internal country management.

4. **Always provide a starting `countryCode` in default values** — without it, the component falls back to Czech Republic (`CZ`) as the default country.

## Version Information

- **Component Version**: v1.0.0
- **Dependencies**: `Form.MaskedInput`, `Form.Select`, React Hook Form, PrimeReact InputMask

## Sync Metadata

- **Component Version:** v2.0.0
- **Component Source:** `packages/neuron/ui/src/lib/form/phoneInput/PhoneInput.tsx`
- **Guideline Command:** `/neuron-ui-phoneinput`
- **Related Skill:** `neuron-ui-form-core`

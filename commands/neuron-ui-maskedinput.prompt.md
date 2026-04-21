---
agent: agent
---

# MaskedInput Component AI Guidelines

## For the AI Assistant

Your task is to integrate and configure the Neuron MaskedInput component in React applications. This guide provides essential instructions for implementing MaskedInput components, which enforce specific input patterns for structured data like phone numbers, credit cards, dates, and custom formats using PrimeReact InputMask with React Hook Form integration across all Neuron applications.

## Overview

The MaskedInput component enforces specific input patterns for structured data like phone numbers, credit cards, dates, and custom formats using PrimeReact InputMask with React Hook Form integration.

## Core Usage

```tsx
import { Form, maskedInputMasks } from "@neuron/ui";
import { useForm } from "react-hook-form";
import { z } from "zod";

const schema = z.object({
  phoneNumber: z.string().min(1, { message: "Phone number is required" }),
  creditCard: z.string().min(1, { message: "Credit card number is required" }),
  ssn: z.string().optional(),
});

const MyForm = () => {
  const { control } = useForm({ resolver: zodResolver(schema) });

  return (
    <>
      <Form.MaskedInput
        name="phoneNumber"
        labelText="Phone Number"
        control={control}
        mask={maskedInputMasks.phoneNumberWithPrefix.value}
        placeholder={maskedInputMasks.phoneNumberWithPrefix.placeholder}
        required
      />

      <Form.MaskedInput
        name="creditCard"
        labelText="Credit Card"
        control={control}
        mask={maskedInputMasks.creditCard.value}
        placeholder={maskedInputMasks.creditCard.placeholder}
      />

      <Form.MaskedInput
        name="ssn"
        labelText="Social Security Number"
        control={control}
        mask="999-99-9999"
        placeholder="123-45-6789"
        optional
      />
    </>
  );
};
```

## Key Props

- **name**: Field name for form registration
- **control**: React Hook Form control object
- **labelText**: Accessible label text
- **mask**: Input pattern (e.g., "999-99-9999" for SSN)
- **placeholder**: Example text showing expected format
- **required/optional**: Validation state indicators
- **literalView**: Display as read-only text
- **disabled/readOnly**: Interaction states
- **testId**: Custom test identifier

## Built-in Masks

```tsx
import { maskedInputMasks } from "@neuron/ui";

// Available masks:
// maskedInputMasks.phoneNumber.value = "999 999 999"
// maskedInputMasks.phoneNumberWithPrefix.value = "(+429) 999 999 999"
// maskedInputMasks.creditCard.value = "9999-9999-9999-9999"

<Form.MaskedInput mask={maskedInputMasks.phoneNumber.value} placeholder={maskedInputMasks.phoneNumber.placeholder} />;
```

## Mask Patterns

```tsx
// Mask characters:
// 9 = Numeric (0-9)
// a = Alphabetic (A-Z, a-z)
// * = Alphanumeric (A-Z, a-z, 0-9)
// Formatting: ( ) - . : / space

// Common patterns:
const masks = {
  ssn: "999-99-9999", // 123-45-6789
  phone: "(999) 999-9999", // (123) 456-7890
  date: "99/99/9999", // 12/31/2023
  time: "99:99", // 14:30
  zipCode: "99999", // 12345
  creditCard: "9999-9999-9999-9999", // 1234-5678-9012-3456
  employeeId: "EMP-99999", // EMP-12345
  productCode: "aaa-999", // ABC-123
};
```

## Validation

```tsx
// Zod validation with regex matching mask format
const schema = z.object({
  ssn: z.string().regex(/^\d{3}-\d{2}-\d{4}$/, "Invalid SSN format"),
  phone: z.string().regex(/^\(\d{3}\) \d{3}-\d{4}$/, "Invalid phone format"),
  date: z.string().regex(/^\d{2}\/\d{2}\/\d{4}$/, "Invalid date format"),
});
```

## Common Use Cases

```tsx
// Financial forms
<Form.MaskedInput mask="9999-9999-9999-9999" /> // Credit card
<Form.MaskedInput mask="999-99-9999" />         // SSN
<Form.MaskedInput mask="999999999" />           // Routing number

// Contact information
<Form.MaskedInput mask="(999) 999-9999" />      // US phone
<Form.MaskedInput mask="99999" />               // ZIP code

// Dates and times
<Form.MaskedInput mask="99/99/9999" />          // Date MM/DD/YYYY
<Form.MaskedInput mask="99:99" />               // Time HH:MM

// Business codes
<Form.MaskedInput mask="EMP-99999" />           // Employee ID
<Form.MaskedInput mask="aaa-999" />             // Product code
```

## Version Information

- **Component Version**: v4.0.0
- **Dependencies**: PrimeReact InputMask, React Hook Form
  const basicSchema = z.object({
  phoneNumber: z.string()
  .min(1, { message: "Phone number is required" })
  .regex(/^\(\+\d{3}\) \d{3} \d{3} \d{3}$/, {
  message: "Invalid phone number format"
  }),
  creditCard: z.string()
  .min(1, { message: "Credit card number is required" })
  .regex(/^\d{4}-\d{4}-\d{4}-\d{4}$/, {
  message: "Invalid credit card format"
  }),
  ssn: z.string()
  .regex(/^\d{3}-\d{2}-\d{4}$/, {
  message: "Invalid SSN format (XXX-XX-XXXX)"
  })
  .optional()
  });

// Advanced validation with business rules
const advancedSchema = z.object({
birthDate: z.string()
.regex(/^\d{2}\/\d{2}\/\d{4}$/, { message: "Date must be MM/DD/YYYY format" })
.refine((date) => {
const [month, day, year] = date.split("/").map(Number);
const inputDate = new Date(year, month - 1, day);
const today = new Date();
const age = today.getFullYear() - inputDate.getFullYear();
return age >= 18 && age <= 120;
}, { message: "Age must be between 18 and 120 years" }),

employeeId: z.string()
.regex(/^EMP-\d{5}$/, { message: "Employee ID must be EMP-XXXXX format" })
.refine(async (id) => {
// Check if employee ID exists in database
return await validateEmployeeId(id);
}, { message: "Employee ID does not exist" }),

productCode: z.string()
.regex(/^PRD-[A-Z]{3}-\d{3}$/, {
message: "Product code must be PRD-XXX-XXX format"
})
});

// Conditional validation based on other fields
const conditionalSchema = z.object({
accountType: z.enum(["personal", "business"]),
ssn: z.string().optional(),
ein: z.string().optional()
}).refine((data) => {
if (data.accountType === "personal") {
return data.ssn && /^\d{3}-\d{2}-\d{4}$/.test(data.ssn);
  } else if (data.accountType === "business") {
    return data.ein && /^\d{2}-\d{7}$/.test(data.ein);
}
return true;
}, {
message: "Personal accounts require SSN, business accounts require EIN",
path: ["ssn"] // or ["ein"] based on accountType
});

````

### Custom Validation Functions

```tsx
// Phone number validation
const validatePhoneNumber = (value: string) => {
  const cleaned = value.replace(/\D/g, "");
  if (cleaned.length !== 10) {
    return "Phone number must be 10 digits";
  }
  if (cleaned.startsWith("0") || cleaned.startsWith("1")) {
    return "Phone number cannot start with 0 or 1";
  }
  return true;
};

// Credit card validation (Luhn algorithm)
const validateCreditCard = (value: string) => {
  const cleaned = value.replace(/\D/g, "");
  if (cleaned.length !== 16) {
    return "Credit card must be 16 digits";
  }

  // Luhn algorithm implementation
  let sum = 0;
  let isEven = false;

  for (let i = cleaned.length - 1; i >= 0; i--) {
    let digit = parseInt(cleaned[i]);

    if (isEven) {
      digit *= 2;
      if (digit > 9) digit -= 9;
    }

    sum += digit;
    isEven = !isEven;
  }

  return sum % 10 === 0 || "Invalid credit card number";
};

// Date validation
const validateDate = (value: string, minAge = 0, maxAge = 150) => {
  const [month, day, year] = value.split("/").map(Number);

  if (month < 1 || month > 12) return "Invalid month";
  if (day < 1 || day > 31) return "Invalid day";
  if (year < 1900 || year > new Date().getFullYear()) return "Invalid year";

  const date = new Date(year, month - 1, day);
  if (date.getMonth() !== month - 1) return "Invalid date";

  const age = new Date().getFullYear() - year;
  if (age < minAge) return `Minimum age is ${minAge}`;
  if (age > maxAge) return `Maximum age is ${maxAge}`;

  return true;
};

// Usage with custom validation
const FormWithCustomValidation = () => {
  const schema = z.object({
    phone: z.string().refine(validatePhoneNumber),
    creditCard: z.string().refine(validateCreditCard),
    birthDate: z.string().refine((val) => validateDate(val, 18, 120))
  });

  const { control } = useForm({
    resolver: zodResolver(schema),
    mode: "onChange"
  });

  return (
    <>
      <Form.MaskedInput
        name="phone"
        labelText="Phone Number"
        control={control}
        mask="(999) 999-9999"
        placeholder="(123) 456-7890"
      />

      <Form.MaskedInput
        name="creditCard"
        labelText="Credit Card"
        control={control}
        mask="9999-9999-9999-9999"
        placeholder="1234-5678-9012-3456"
      />

      <Form.MaskedInput
        name="birthDate"
        labelText="Birth Date"
        control={control}
        mask="99/99/9999"
        placeholder="MM/DD/YYYY"
      />
    </>
  );
};
````

## Advanced Features

### Literal View Mode

```tsx
<Form.MaskedInput
  name="formattedPhone"
  labelText="Phone Number"
  control={control}
  mask="(999) 999-9999"
  literalView={true} // Shows formatted value as read-only text
/>
```

### Custom Styling with Prefix/Suffix

```tsx
<Form.MaskedInput
  name="salary"
  labelText="Annual Salary"
  control={control}
  mask="999,999,999"
  placeholder="100,000,000"
  prefix="$"
  suffix="USD"
  description="Enter your annual salary"
/>

<Form.MaskedInput
  name="weight"
  labelText="Weight"
  control={control}
  mask="999.9"
  placeholder="150.5"
  suffix="kg"
  leftAddonContent={<Icon name="scale" />}
/>
```

### Inline Layout

```tsx
<Form.MaskedInput
  name="zipCode"
  labelText="ZIP Code"
  control={control}
  mask="99999"
  placeholder="12345"
  inline={true}
  className="col-md-4"
/>
```

## Role-Based Access Control

```tsx
const { userPermissions } = useAuth();

<Form.MaskedInput
  name="ssn"
  labelText="Social Security Number"
  control={control}
  mask="999-99-9999"
  fullAccess={userPermissions.includes("view_ssn") ? "admin" : undefined}
  readonlyAccess={userPermissions.includes("view_partial_ssn") ? "user" : undefined}
  tooltip="Restricted access based on user permissions"
/>;
```

## Testing Integration

```tsx
<Form.MaskedInput
  name="testableInput"
  labelText="Testable Masked Input"
  control={control}
  mask="999-99-9999"
  testId="ssn-input"
/>;

// In tests
describe("MaskedInput Component", () => {
  it("should enforce mask pattern", async () => {
    render(<TestForm />);

    const input = screen.getByTestId("ssn-input");
    fireEvent.change(input, { target: { value: "123456789" } });

    await waitFor(() => {
      expect(input).toHaveValue("123-45-6789");
    });
  });

  it("should validate format", async () => {
    render(<TestForm />);

    const input = screen.getByTestId("ssn-input");
    fireEvent.change(input, { target: { value: "12345" } });
    fireEvent.blur(input);

    await waitFor(() => {
      expect(screen.getByText("Invalid SSN format")).toBeInTheDocument();
    });
  });
});
```

## Performance Considerations

```tsx
const OptimizedMaskedInput = memo(({ control, mask, ...props }) => {
  // Memoize mask to prevent unnecessary re-renders
  const memoizedMask = useMemo(() => mask, [mask]);

  return <Form.MaskedInput control={control} mask={memoizedMask} {...props} />;
});
```

## Common Use Cases

### Financial Forms

```tsx
const FinancialForm = () => {
  return (
    <>
      <Form.MaskedInput
        name="accountNumber"
        labelText="Account Number"
        control={control}
        mask="9999-9999-9999"
        placeholder="1234-5678-9012"
      />

      <Form.MaskedInput
        name="routingNumber"
        labelText="Routing Number"
        control={control}
        mask="999999999"
        placeholder="123456789"
      />

      <Form.MaskedInput
        name="amount"
        labelText="Amount"
        control={control}
        mask="999,999.99"
        placeholder="1,000.00"
        prefix="$"
      />
    </>
  );
};
```

### Personal Information Forms

```tsx
const PersonalInfoForm = () => {
  return (
    <>
      <Form.MaskedInput
        name="ssn"
        labelText="Social Security Number"
        control={control}
        mask="999-99-9999"
        placeholder="123-45-6789"
      />

      <Form.MaskedInput
        name="driverLicense"
        labelText="Driver's License"
        control={control}
        mask="a9999999"
        placeholder="A1234567"
      />

      <Form.MaskedInput
        name="passport"
        labelText="Passport Number"
        control={control}
        mask="999999999"
        placeholder="123456789"
      />
    </>
  );
};
```

### Business Forms

```tsx
const BusinessForm = () => {
  return (
    <>
      <Form.MaskedInput
        name="ein"
        labelText="Employer ID Number"
        control={control}
        mask="99-9999999"
        placeholder="12-3456789"
      />

      <Form.MaskedInput
        name="businessPhone"
        labelText="Business Phone"
        control={control}
        mask="(999) 999-9999 ext. 9999"
        placeholder="(555) 123-4567 ext. 1234"
      />

      <Form.MaskedInput
        name="taxId"
        labelText="Tax ID"
        control={control}
        mask="999-999-999"
        placeholder="123-456-789"
      />
    </>
  );
};
```

## Troubleshooting

### Common Issues and Solutions

1. **Mask not applying correctly**

   ```tsx
   // Ensure mask prop is properly formatted
   mask = "999-99-9999"; // Correct
   mask = "999999999"; // Will work but no formatting
   ```

2. **Validation not working with masked values**

   ```tsx
   // Use regex that matches the masked format
   z.string().regex(/^\d{3}-\d{2}-\d{4}$/); // For SSN with dashes
   ```

3. **Placeholder not showing**
   ```tsx
   // Provide explicit placeholder
   <Form.MaskedInput
     mask="999-99-9999"
     placeholder="123-45-6789" // Explicit placeholder
   />
   ```

## Version Information

- **Component Version**: v4.0.0
- **Features**: React Hook Form integration, custom masks, validation, accessibility
- **Dependencies**: PrimeReact InputMask, React Hook Form, Zod validation

## Sync Metadata

- **Component Version:** v4.2.1
- **Component Source:** `packages/neuron/ui/src/lib/form/maskedInput/MaskedInput.tsx`
- **Guideline Command:** `/neuron-ui-maskedinput`
- **Related Skill:** `neuron-ui-form-core`

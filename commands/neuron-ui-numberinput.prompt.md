---
agent: agent
---

# NumberInput Component AI Guidelines

## For the AI Assistant

Your task is to integrate and configure the Neuron NumberInput component in React applications. This guide provides essential instructions for implementing NumberInput components, which provide specialized numeric input fields with precise formatting, validation, boundary controls, and enhanced UX features through React Hook Form integration across all Neuron applications.

## Overview

The NumberInput component is a specialized numeric input field built on PrimeReact InputNumber, providing precise number formatting, validation, and enhanced UX features through React Hook Form integration. It handles integer and decimal values with built-in number formatting, grouping, and boundary controls.

## Core Features

- **Numeric Value Handling**: Only accepts numeric input with automatic formatting
- **Precision Control**: Configurable decimal places and fraction digits
- **Boundary Validation**: Min/max value constraints with step increments
- **Number Formatting**: Thousand separators, decimal handling, and locale support
- **Form Integration**: Complete React Hook Form and Zod validation support
- **Accessibility**: ARIA labels, keyboard navigation, and screen reader support
- **Text Alignment**: Configurable alignment (start/end) for different use cases

## Basic Implementation

### Simple Number Input

```tsx
import { Form } from "@neuron/ui";
import { useForm } from "react-hook-form";
import { zodResolver } from "@hookform/resolvers/zod";
import { z } from "zod";

const schema = z.object({
  age: z.number().min(18, { message: "Must be at least 18 years old" }).max(120, { message: "Invalid age" }),
  weight: z.number().positive({ message: "Weight must be positive" }),
});

const BasicNumberForm = () => {
  const { control } = useForm({
    resolver: zodResolver(schema),
    mode: "onChange",
    defaultValues: {
      age: null,
      weight: null,
    },
  });

  return (
    <>
      <Form.NumberInput
        name="age"
        labelText="Age"
        control={control}
        min={18}
        max={120}
        step={1}
        requiredFlag={true}
        placeholder="Enter your age"
      />

      <Form.NumberInput
        name="weight"
        labelText="Weight"
        control={control}
        min={0}
        max={500}
        step={0.1}
        suffix="kg"
        maxFractionDigits={1}
        tooltip="Enter weight in kilograms"
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
- **min**: Minimum allowed value
- **max**: Maximum allowed value
- **step**: Increment/decrement step value (default: 1)

### Validation & Constraints

- **required**: HTML5 required attribute
- **requiredFlag**: Shows required indicator (\*)
- **optional**: Shows optional indicator
- **isValid**: Manual valid state indicator
- **maxLength**: Maximum character length

### Number Formatting

- **useGrouping**: Enable/disable thousand separators (default: true)
- **maxFractionDigits**: Maximum decimal places
- **minFractionDigits**: Minimum decimal places
- **roundingMode**: Rounding behavior (`"halfExpand"`, `"halfEven"`, etc.)
- **format**: Enable custom number formatting

### Layout & Styling

- **textAlign**: Text alignment (`"start"` | `"end"`) - default: `"end"`
- **inline**: Horizontal label/input layout
- **literalView**: Display as read-only formatted text
- **className**: CSS classes for grid positioning
- **prefix**: Text prefix inside input
- **suffix**: Text suffix inside input
- **leftAddonContent**: Components before input (icons, buttons)
- **rightAddonContent**: Components after input (icons, buttons)

### Access Control

- **readOnly**: Make input read-only
- **disabled**: Disable input interaction
- **readonlyAccess**: Role-based read access
- **fullAccess**: Role-based full access

## Validation Patterns

### Zod Schema Validation

```tsx
// Basic number validation
const productSchema = z.object({
  price: z
    .number()
    .positive({ message: "Price must be positive" })
    .max(999999, { message: "Price too high" })
    .multipleOf(0.01, { message: "Price must be in cents" }),

  quantity: z
    .number()
    .int({ message: "Quantity must be whole number" })
    .min(1, { message: "Minimum quantity is 1" })
    .max(1000, { message: "Maximum quantity is 1000" }),

  discount: z
    .number()
    .min(0, { message: "Discount cannot be negative" })
    .max(100, { message: "Discount cannot exceed 100%" })
    .optional(),

  weight: z
    .number()
    .positive({ message: "Weight must be positive" })
    .refine((val) => val <= 50, { message: "Weight cannot exceed 50kg" }),
});

// Usage with form
const { control } = useForm({
  resolver: zodResolver(productSchema),
  mode: "onChange",
  defaultValues: {
    price: null,
    quantity: 1,
    discount: null,
    weight: null,
  },
});
```

### Conditional Validation

```tsx
const orderSchema = z
  .object({
    basePrice: z.number().positive(),
    hasDiscount: z.boolean(),
    discountAmount: z.number().optional(),
    shippingWeight: z.number().positive(),
  })
  .refine(
    (data) => {
      if (data.hasDiscount) {
        return data.discountAmount && data.discountAmount > 0 && data.discountAmount <= data.basePrice;
      }
      return true;
    },
    {
      message: "Discount amount must be between 0 and base price",
      path: ["discountAmount"],
    },
  );
```

### Custom Validation with Dependencies

```tsx
<Form.NumberInput
  name="discountAmount"
  labelText="Discount Amount"
  control={control}
  deps={["basePrice", "hasDiscount"]} // Revalidate when these fields change
  min={0}
  max={watch("basePrice") || 0}
  disabled={!watch("hasDiscount")}
  prefix="$"
/>
```

## Number Formatting Examples

### Currency Input

```tsx
<Form.NumberInput
  name="price"
  labelText="Price"
  control={control}
  min={0}
  max={999999.99}
  step={0.01}
  prefix="$"
  maxFractionDigits={2}
  minFractionDigits={2}
  useGrouping={true}
  placeholder="0.00"
  tooltip="Enter price in USD"
/>
```

### Percentage Input

```tsx
<Form.NumberInput
  name="discount"
  labelText="Discount"
  control={control}
  min={0}
  max={100}
  step={0.1}
  suffix="%"
  maxFractionDigits={1}
  textAlign="end"
  placeholder="0.0"
/>
```

### Weight/Measurement Input

```tsx
<Form.NumberInput
  name="weight"
  labelText="Weight"
  control={control}
  min={0}
  max={500}
  step={0.1}
  suffix="kg"
  maxFractionDigits={1}
  useGrouping={false}
  textAlign="end"
/>
```

### Quantity/Count Input

```tsx
<Form.NumberInput
  name="quantity"
  labelText="Quantity"
  control={control}
  min={1}
  max={9999}
  step={1}
  maxFractionDigits={0} // No decimals for quantities
  useGrouping={true}
  textAlign="end"
/>
```

### Temperature Input

```tsx
<Form.NumberInput
  name="temperature"
  labelText="Temperature"
  control={control}
  min={-273.15}
  max={1000}
  step={0.1}
  suffix="°C"
  maxFractionDigits={1}
  textAlign="end"
/>
```

## Advanced Features

### With Visual Enhancements

```tsx
import { baseIcons, Icon } from "@neuron/ui";

<Form.NumberInput
  name="budget"
  labelText="Budget"
  control={control}
  min={0}
  max={1000000}
  step={100}
  prefix="$"
  leftAddonContent={<Icon iconDef={baseIcons.dollarSignSolid} />}
  maxFractionDigits={2}
  useGrouping={true}
  placeholder="0.00"
/>;
```

### Min/Max with Step Control

```tsx
<Form.NumberInput
  name="price"
  labelText="Price Range"
  control={control}
  min={500}
  max={50000}
  step={100} // Increment by 100
  prefix="$"
  tooltip="Price range: $500 - $50,000 in $100 increments"
  useGrouping={true}
/>
```

### Precision Control

```tsx
// High precision for scientific calculations
<Form.NumberInput
  name="measurement"
  labelText="Measurement"
  control={control}
  step={0.001}
  maxFractionDigits={3}
  minFractionDigits={3}
  suffix="mm"
  useGrouping={false}
  textAlign="end"
/>
```

### Text Alignment Examples

```tsx
// Left-aligned for labels/identifiers
<Form.NumberInput
  name="productCode"
  labelText="Product Code"
  control={control}
  textAlign="start"
  maxFractionDigits={0}
  useGrouping={false}
/>

// Right-aligned for monetary values (default)
<Form.NumberInput
  name="amount"
  labelText="Amount"
  control={control}
  textAlign="end"
  prefix="$"
  maxFractionDigits={2}
/>
```

## Event Handling

### Change Events

```tsx
const handleValueChange = (event) => {
  console.info("Value changed:", event.value);
  // Custom logic when value changes
};

const handleInput = (event) => {
  console.info("Input event:", event.originalEvent?.target.value);
  // Raw input handling for real-time validation
};

<Form.NumberInput
  name="amount"
  labelText="Amount"
  control={control}
  onChange={handleValueChange}
  onInput={handleInput}
  min={0}
  max={10000}
/>;
```

### Keyboard Events

```tsx
const handleKeyDown = (event) => {
  if (event.key === "Enter") {
    // Handle enter key for form submission
  }
};

<Form.NumberInput name="quantity" labelText="Quantity" control={control} onKeyDown={handleKeyDown} />;
```

## State Management

### Read-Only and Disabled States

```tsx
// Read-only state for display
<Form.NumberInput
  name="calculatedTotal"
  labelText="Calculated Total"
  control={control}
  readOnly={true}
  prefix="$"
  maxFractionDigits={2}
/>

// Disabled state
<Form.NumberInput
  name="lockedValue"
  labelText="Locked Value"
  control={control}
  disabled={true}
/>

// Conditional disabled based on other fields
<Form.NumberInput
  name="discount"
  labelText="Discount"
  control={control}
  disabled={!watch("allowDiscounts")}
  min={0}
  max={100}
  suffix="%"
/>
```

### Validation States

```tsx
// Valid state indicator
<Form.NumberInput
  name="validAmount"
  labelText="Valid Amount"
  control={control}
  isValid={true}
  prefix="$"
/>

// With description variants
<Form.NumberInput
  name="warningValue"
  labelText="Value with Warning"
  control={control}
  description="This value seems high"
  descriptionVariant="warning"
/>
```

### Literal View for Display

```tsx
// Display formatted number as read-only text
<Form.NumberInput
  name="displayValue"
  labelText="Final Amount"
  control={control}
  literalView={true}
  prefix="$"
  maxFractionDigits={2}
  useGrouping={true}
/>
```

## Form Integration Patterns

### Complete Product Form

```tsx
interface ProductFormData {
  name: string;
  price: number;
  quantity: number;
  weight: number;
  discount?: number;
}

const productSchema = z.object({
  name: z.string().min(1),
  price: z.number().positive().multipleOf(0.01),
  quantity: z.number().int().min(1),
  weight: z.number().positive(),
  discount: z.number().min(0).max(100).optional(),
});

const ProductForm = () => {
  const { control, handleSubmit, watch } = useForm<ProductFormData>({
    resolver: zodResolver(productSchema),
    mode: "onChange",
    defaultValues: {
      name: "",
      price: null,
      quantity: 1,
      weight: null,
      discount: null,
    },
  });

  const onSubmit = (data: ProductFormData) => {
    console.info("Product data:", data);
  };

  return (
    <form onSubmit={handleSubmit(onSubmit)}>
      <Fieldset legend="Product Information" columnCount={12}>
        <Form.Input
          className="g-col-12 g-col-md-6"
          name="name"
          labelText="Product Name"
          control={control}
          requiredFlag={true}
        />

        <Form.NumberInput
          className="g-col-12 g-col-md-6"
          name="price"
          labelText="Price"
          control={control}
          min={0}
          max={99999.99}
          step={0.01}
          prefix="$"
          maxFractionDigits={2}
          minFractionDigits={2}
          requiredFlag={true}
        />

        <Form.NumberInput
          className="g-col-12 g-col-md-6"
          name="quantity"
          labelText="Quantity"
          control={control}
          min={1}
          max={9999}
          step={1}
          maxFractionDigits={0}
          requiredFlag={true}
        />

        <Form.NumberInput
          className="g-col-12 g-col-md-6"
          name="weight"
          labelText="Weight"
          control={control}
          min={0}
          max={1000}
          step={0.1}
          suffix="kg"
          maxFractionDigits={1}
          requiredFlag={true}
        />

        <Form.NumberInput
          className="g-col-12 g-col-md-6"
          name="discount"
          labelText="Discount"
          control={control}
          min={0}
          max={100}
          step={0.1}
          suffix="%"
          maxFractionDigits={1}
          optional={true}
        />
      </Fieldset>
    </form>
  );
};
```

## Performance Optimization

### Memoized Schemas

```tsx
const numberSchema = useMemo(
  () =>
    z.object({
      amount: z.number().positive().max(1000000),
    }),
  [],
);

const { control } = useForm({
  resolver: zodResolver(numberSchema),
  mode: "onChange",
});
```

### Controlled Updates

```tsx
const [isUpdating, setIsUpdating] = useState(false);

const handleExpensiveCalculation = useCallback(
  debounce((value: number) => {
    setIsUpdating(true);
    // Expensive calculation
    performCalculation(value);
    setIsUpdating(false);
  }, 500),
  [],
);
```

## Best Practices

### Validation Strategy

1. **Use number-specific Zod validators** (`.positive()`, `.int()`, `.multipleOf()`)
2. **Set appropriate min/max boundaries** to prevent invalid ranges
3. **Use step values** that match expected precision
4. **Set mode to "onChange"** for real-time feedback on numeric input
5. **Initialize with null** for optional fields, specific values for required fields

### UX Considerations

1. **Always provide labelText** for accessibility and form clarity
2. **Use meaningful suffixes/prefixes** to indicate units and currency
3. **Set appropriate text alignment** (end for numbers, start for codes)
4. **Configure decimal places** based on data precision needs
5. **Use consistent decimal formatting** with `minFractionDigits={2}` and `maxFractionDigits={2}` for currency and monetary values
6. **Add tooltips for range constraints** or business rules
7. **Use grouping for large numbers** to improve readability
8. **Disable when values are calculated** or locked

### Performance

1. **Memoize complex validation schemas** to prevent recreating on re-renders
2. **Use debouncing for expensive calculations** triggered by value changes
3. **Avoid complex onChange handlers** that might impact input responsiveness
4. **Use deps array** for conditional validation dependencies

### Accessibility

1. **Ensure proper form labeling** with labelText and descriptions
2. **Provide min/max information** in tooltips or descriptions
3. **Use required/optional indicators** for screen readers
4. **Include unit information** in labels or suffixes

## Common Implementation Patterns

### Price/Currency Fields

```tsx
<Form.NumberInput
  name="price"
  labelText="Price"
  control={control}
  min={0}
  max={999999.99}
  step={0.01}
  prefix="$"
  maxFractionDigits={2}
  minFractionDigits={2}
  useGrouping={true}
  requiredFlag={true}
/>
```

### Age/Count Fields

```tsx
<Form.NumberInput
  name="age"
  labelText="Age"
  control={control}
  min={0}
  max={120}
  step={1}
  maxFractionDigits={0}
  useGrouping={false}
  textAlign="end"
/>
```

### Weight/Measurement Fields

```tsx
<Form.NumberInput
  name="weight"
  labelText="Weight"
  control={control}
  min={0}
  max={500}
  step={0.1}
  suffix="kg"
  maxFractionDigits={1}
  tooltip="Enter weight in kilograms"
/>
```

### Percentage Fields

```tsx
<Form.NumberInput
  name="percentage"
  labelText="Completion"
  control={control}
  min={0}
  max={100}
  step={0.1}
  suffix="%"
  maxFractionDigits={1}
  textAlign="end"
/>
```

This component provides comprehensive numeric input functionality with precise formatting, validation, and accessibility features optimized for financial, measurement, and counting use cases.

## Sync Metadata

- **Component Version:** v4.3.9
- **Component Source:** `packages/neuron/ui/src/lib/form/numberInput/NumberInput.tsx`
- **Guideline Command:** `/neuron-ui-numberinput`
- **Related Skill:** `neuron-ui-form-core`

---
agent: agent
---

# Slider Component AI Guidelines

## For the AI Assistant

Your task is to integrate and configure the Neuron Slider component in React applications. This guide provides essential instructions for implementing Slider components, which provide intuitive visual value selection with support for single values, ranges, horizontal/vertical orientation, and dynamic value display through React Hook Form integration across all Neuron applications.

## Sync Metadata

- **Component Version:** v1.2.0
- **Component Source:** `packages/neuron/ui/src/lib/form/slider/Slider.tsx`
- **Guideline Command:** `/neuron-ui-slider`
- **Related Skill:** `neuron-ui-form-core`

## Overview

The Slider component is a specialized input control built on PrimeReact Slider, providing intuitive visual value selection with support for single values, ranges, horizontal and vertical orientations, and real-time value display in the label. It's ideal for settings, filters, and any scenario requiring numeric value selection within a defined range.

## Core Features

- **Single & Range Values**: Select single numeric values or value ranges
- **Orientation Support**: Horizontal (default) or vertical slider orientation
- **Real-time Value Display**: Current value(s) automatically shown in label
- **Step Control**: Configurable increment/decrement step values
- **Boundary Validation**: Min/max value constraints with form validation
- **Form Integration**: Complete React Hook Form and Zod validation support
- **Literal View**: Display selected value(s) as read-only text
- **Accessibility**: ARIA labels, keyboard navigation, and screen reader support
- **Event Handling**: onChange for value changes, onSlideEnd for completed slides
- **External Step Control**: Imperative `stepForward`/`stepBackward` via `sliderActionsRef`
- **Value Change Callback**: `onValueChange` for tracking current value (raw + formatted)

## Basic Implementation

### Simple Single Value Slider

```tsx
import { Form } from "@neuron/ui";
import { useForm } from "react-hook-form";
import { zodResolver } from "@hookform/resolvers/zod";
import { z } from "zod";

const schema = z.object({
  volume: z.number().min(0).max(100),
  brightness: z.number().min(0).max(100),
});

type FormData = z.infer<typeof schema>;

const BasicSliderForm = () => {
  const { control } = useForm<FormData>({
    resolver: zodResolver(schema),
    mode: "onChange",
    defaultValues: {
      volume: 50,
      brightness: 75,
    },
  });

  return (
    <div className="grid form-gap">
      <div className="g-col-12">
        <Form.Slider
          name="volume"
          control={control}
          labelText="Volume"
          description="Adjust the volume level"
          min={0}
          max={100}
          step={1}
        />
      </div>

      <div className="g-col-12">
        <Form.Slider
          name="brightness"
          control={control}
          labelText="Brightness"
          description="Adjust the brightness level"
          min={0}
          max={100}
          step={5}
        />
      </div>
    </div>
  );
};
```

## Key Props Reference

### Essential Props

- **name**: Field name for form registration
- **control**: React Hook Form control object
- **labelText**: Label text (automatically appends current value)
- **min**: Minimum boundary value (default: 0)
- **max**: Maximum boundary value (default: 100)
- **step**: Increment/decrement step value (default: 1)

### Slider Configuration

- **range**: Enable range selection with two handles (default: false)
- **orientation**: Slider orientation (`"horizontal"` | `"vertical"`) (default: "horizontal")

### Validation & Constraints

- **required**: HTML5 required attribute
- **requiredFlag**: Shows required indicator (\*)
- **optional**: Shows optional indicator
- **isValid**: Manual valid state indicator
- **deps**: Field dependencies for revalidation

### Display & Styling

- **description**: Help text below slider
- **descriptionVariant**: Description style (`"default"` | `"info"` | `"warning"` | `"error"`)
- **literalView**: Display as read-only formatted text
- **className**: CSS classes for grid positioning
- **tooltip**: Tooltip text with info icon

### Event Handlers

- **onChange**: Callback invoked on value change via slide
- **onSlideEnd**: Callback invoked when slide ends
- **onValueChange**: Callback `(value: number | number[], formattedValue: string) => void` invoked on every value change (slide, programmatic step, form reset). Use to mirror current value in external UI.

### External Step Control

- **sliderActionsRef**: `React.Ref<SliderActionsHandle>` — imperative handle exposing `stepForward()` and `stepBackward()` methods. Not available in range mode.

### Access Control

- **readOnly**: Make slider read-only
- **disabled**: Disable slider interaction
- **readonlyAccess**: Role-based read access
- **fullAccess**: Role-based full access

### Testing

- **testId**: Custom test ID for the slider element

## Validation Patterns

### Zod Schema Validation

```tsx
// Single value sliders with boundaries
const settingsSchema = z.object({
  volume: z.number().min(0, { message: "Volume cannot be negative" }).max(100, { message: "Volume cannot exceed 100" }),

  brightness: z
    .number()
    .min(0, { message: "Brightness cannot be negative" })
    .max(100, { message: "Brightness cannot exceed 100" }),

  temperature: z
    .number()
    .min(16, { message: "Minimum temperature is 16°C" })
    .max(30, { message: "Maximum temperature is 30°C" }),
});

// Range slider validation
const filterSchema = z.object({
  priceRange: z
    .array(z.number())
    .length(2, { message: "Price range must have two values" })
    .refine(([min, max]) => min < max, { message: "Minimum price must be less than maximum" }),

  ageRange: z
    .array(z.number())
    .length(2)
    .refine(([min, max]) => min >= 18, { message: "Minimum age must be 18 or higher" }),
});
```

### Conditional Validation with Dependencies

```tsx
// Slider dependent on checkbox state
const discountSchema = z
  .object({
    hasDiscount: z.boolean(),
    discountPercentage: z.number().min(0).max(100).optional(),
  })
  .refine(
    (data) => {
      if (data.hasDiscount) {
        return data.discountPercentage !== undefined && data.discountPercentage > 0;
      }
      return true;
    },
    {
      message: "Discount percentage is required when discount is enabled",
      path: ["discountPercentage"],
    },
  );

// Usage with dependencies
<Form.Slider
  name="discountPercentage"
  control={control}
  labelText="Discount"
  deps={["hasDiscount"]} // Revalidate when hasDiscount changes
  disabled={!watch("hasDiscount")}
  min={0}
  max={100}
  step={5}
/>;
```

## Range Slider Implementation

### Price Range Filter

```tsx
const filterSchema = z.object({
  priceRange: z
    .array(z.number())
    .length(2)
    .refine(([min, max]) => min < max, "Invalid range"),
});

type FilterData = z.infer<typeof filterSchema>;

const PriceFilter = () => {
  const { control } = useForm<FilterData>({
    resolver: zodResolver(filterSchema),
    defaultValues: {
      priceRange: [200, 800],
    },
  });

  return (
    <Form.Slider
      name="priceRange"
      control={control}
      labelText="Price Range"
      description="Select your price range"
      range // Enable range mode
      min={0}
      max={1000}
      step={50}
    />
  );
};
```

### Date Range (Years)

```tsx
const yearRangeSchema = z.object({
  yearRange: z
    .array(z.number())
    .length(2)
    .refine(([start, end]) => end >= start, "Invalid year range"),
});

<Form.Slider name="yearRange" control={control} labelText="Year Range" range min={2000} max={2024} step={1} />;
```

## Orientation Examples

### Vertical Slider

```tsx
// Vertical slider for temperature control
<div style={{ height: "300px" }}>
  <Form.Slider
    name="temperature"
    control={control}
    labelText="Temperature"
    orientation="vertical" // Vertical orientation
    min={0}
    max={100}
    step={1}
    description="Adjust temperature vertically"
  />
</div>
```

**Note**: Vertical sliders require a container with explicit height for proper rendering.

### Horizontal Slider (Default)

```tsx
// Horizontal slider (default orientation)
<Form.Slider
  name="opacity"
  control={control}
  labelText="Opacity"
  orientation="horizontal" // Optional, this is the default
  min={0}
  max={100}
  step={5}
/>
```

## Step Control Patterns

### Fine-Grained Control

```tsx
// Decimal steps for precise control
<Form.Slider
  name="rating"
  control={control}
  labelText="Rating"
  min={0}
  max={5}
  step={0.1} // Fine-grained decimal steps
/>
```

### Coarse Control

```tsx
// Large steps for quick selection
<Form.Slider
  name="budget"
  control={control}
  labelText="Budget"
  min={0}
  max={10000}
  step={500} // Large increments for quick selection
/>
```

## Event Handling

### onChange Event

```tsx
const handleVolumeChange = (e: SliderChangeEvent) => {
  console.log("New value:", e.value);
  // Additional logic on value change
};

<Form.Slider name="volume" control={control} labelText="Volume" onChange={handleVolumeChange} min={0} max={100} />;
```

### onSlideEnd Event

```tsx
const handleSlideEnd = (e: SliderChangeEvent) => {
  console.log("Slide ended at:", e.value);
  // Trigger action only when user finishes sliding
  // Useful for API calls or heavy operations
};

<Form.Slider
  name="brightness"
  control={control}
  labelText="Brightness"
  onSlideEnd={handleSlideEnd} // Only fires when slide completes
  min={0}
  max={100}
/>;
```

## External Step Control

Use `sliderActionsRef` and `onValueChange` to control the slider from external buttons and display the current value in a separate element.

```tsx
import { baseIcons, Button, Form, Label } from "@neuron/ui";
import { useRef, useState } from "react";
import { useForm } from "react-hook-form";
import type { SliderActionsHandle } from "@neuron/ui";

const ExternalStepControl = () => {
  const { control } = useForm({
    defaultValues: { stepSlider: 50 },
  });
  const sliderActionsRef = useRef<SliderActionsHandle>(null);
  const [displayValue, setDisplayValue] = useState("50");

  return (
    <div className="form-item">
      <Label text={`Aktuální hodnota: ${displayValue}`} />
      <div className="d-flex flex-row flex-nowrap gap-4 justify-content-between">
        <Button
          iconLeft={baseIcons.chevronLeftSolid}
          size="small"
          variant="secondary"
          onClick={() => sliderActionsRef.current?.stepBackward()}
        />
        <Form.Slider
          className="flex-grow-1"
          control={control}
          max={100}
          min={0}
          name="stepSlider"
          sliderActionsRef={sliderActionsRef}
          step={10}
          onValueChange={(_value, formatted) => setDisplayValue(formatted)}
        />
        <Button
          iconLeft={baseIcons.chevronRightSolid}
          size="small"
          variant="secondary"
          onClick={() => sliderActionsRef.current?.stepForward()}
        />
      </div>
    </div>
  );
};
```

### SliderActionsHandle API

| Method         | Description                                  |
| -------------- | -------------------------------------------- |
| `stepForward`  | Increments value by `step`, clamped to `max` |
| `stepBackward` | Decrements value by `step`, clamped to `min` |

**Note**: Both methods are no-ops when the slider is in `range` mode or disabled.

## Complete Form Example

```tsx
import { Button, Form } from "@neuron/ui";
import { useForm } from "react-hook-form";
import { zodResolver } from "@hookform/resolvers/zod";
import { z } from "zod";

const settingsSchema = z.object({
  volume: z.number().min(0).max(100),
  brightness: z.number().min(0).max(100),
  priceRange: z.array(z.number()).length(2),
  temperature: z.number().min(16).max(30),
});

type SettingsData = z.infer<typeof settingsSchema>;

const SettingsForm = () => {
  const { control, handleSubmit, reset } = useForm<SettingsData>({
    resolver: zodResolver(settingsSchema),
    defaultValues: {
      volume: 50,
      brightness: 75,
      priceRange: [200, 800],
      temperature: 22,
    },
  });

  const onSubmit = (data: SettingsData) => {
    console.log("Form submitted:", data);
  };

  return (
    <form onSubmit={handleSubmit(onSubmit)}>
      <div className="grid form-gap">
        <div className="g-col-12">
          <Form.Slider
            name="volume"
            control={control}
            labelText="Volume"
            description="Adjust audio volume level"
            min={0}
            max={100}
            step={1}
            requiredFlag
          />
        </div>

        <div className="g-col-12">
          <Form.Slider
            name="brightness"
            control={control}
            labelText="Brightness"
            description="Adjust screen brightness"
            min={0}
            max={100}
            step={5}
          />
        </div>

        <div className="g-col-12">
          <Form.Slider
            name="priceRange"
            control={control}
            labelText="Price Range"
            description="Select your price range"
            range
            min={0}
            max={1000}
            step={50}
          />
        </div>

        <div className="g-col-12">
          <Form.Slider
            name="temperature"
            control={control}
            labelText="Temperature"
            description="Set desired temperature in °C"
            min={16}
            max={30}
            step={1}
          />
        </div>

        <div className="g-col-12 d-flex gap-8">
          <Button type="submit" variant="primary">
            Save Settings
          </Button>
          <Button type="button" variant="secondary" onClick={() => reset()}>
            Reset
          </Button>
        </div>
      </div>
    </form>
  );
};
```

## Real-World Use Cases

### Volume/Brightness Controls

```tsx
const mediaSchema = z.object({
  volume: z.number().min(0).max(100),
  brightness: z.number().min(0).max(100),
  contrast: z.number().min(0).max(100),
});

<Form.Slider name="volume" control={control} labelText="Volume" min={0} max={100} step={1} />;
```

### Filter Ranges

```tsx
const filterSchema = z.object({
  priceRange: z.array(z.number()).length(2),
  sizeRange: z.array(z.number()).length(2),
  ratingRange: z.array(z.number()).length(2),
});

<Form.Slider name="priceRange" control={control} labelText="Price" range min={0} max={5000} step={100} />;
```

### Rating Systems

```tsx
const ratingSchema = z.object({
  satisfaction: z.number().min(0).max(10),
  quality: z.number().min(0).max(5).multipleOf(0.5),
});

<Form.Slider name="satisfaction" control={control} labelText="Satisfaction Level" min={0} max={10} step={1} />;
```

### Temperature/Climate Controls

```tsx
const climateSchema = z.object({
  temperature: z.number().min(16).max(30),
  humidity: z.number().min(30).max(70),
});

<Form.Slider
  name="temperature"
  control={control}
  labelText="Target Temperature"
  min={16}
  max={30}
  step={0.5}
  description="Adjust room temperature in °C"
/>;
```

## Access Control Patterns

### Read-Only Mode

```tsx
<Form.Slider
  name="systemVolume"
  control={control}
  labelText="System Volume"
  readOnly // Makes slider read-only
  min={0}
  max={100}
/>
```

### Role-Based Access

```tsx
<Form.Slider
  name="maxPrice"
  control={control}
  labelText="Maximum Price"
  readonlyAccess="ROLE_VIEWER" // Read-only for viewers
  fullAccess="ROLE_ADMIN" // Full access for admins
  min={0}
  max={10000}
/>
```

### Conditional Disabled

```tsx
<Form.Slider
  name="discount"
  control={control}
  labelText="Discount Percentage"
  disabled={!watch("hasDiscount")} // Disabled when checkbox unchecked
  min={0}
  max={100}
/>
```

## Literal View Pattern

```tsx
// Display slider value as read-only text
<Form.Slider
  name="completionPercentage"
  control={control}
  labelText="Completion"
  literalView // Shows formatted value instead of slider
  min={0}
  max={100}
/>
```

## Best Practices

### ✅ DO

- **Use appropriate step values** for the context (e.g., 1 for whole numbers, 0.1 for decimals)
- **Set realistic min/max boundaries** based on use case
- **Provide descriptive labels** that include units when applicable
- **Use range mode** for filter scenarios requiring min/max selection
- **Add descriptions** to clarify purpose and expected values
- **Validate range sliders** to ensure min < max in schema
- **Use onSlideEnd** for expensive operations to avoid excessive calls
- **Set explicit height** for vertical sliders

### ❌ DON'T

- **Don't use sliders for precise numeric input** - use NumberInput instead
- **Don't omit min/max boundaries** - always define reasonable limits
- **Don't use tiny step values** (e.g., 0.01 for 0-100 range) - makes selection difficult
- **Don't use sliders for more than 2 values** - use multiple sliders instead
- **Don't forget validation** for range sliders (ensure min < max)
- **Don't use vertical sliders without container height** - they won't render properly
- **Don't rely solely on labels** - provide tooltips or descriptions for context
- **Don't use sliders for binary choices** - use Switch or CheckBox instead

## Common Patterns

### Settings Panel

```tsx
const settingsSchema = z.object({
  volume: z.number().min(0).max(100),
  brightness: z.number().min(0).max(100),
  fontSize: z.number().min(12).max(24),
});

<div className="grid form-gap">
  <div className="g-col-12">
    <Form.Slider name="volume" control={control} labelText="Volume" min={0} max={100} step={1} />
  </div>
  <div className="g-col-12">
    <Form.Slider name="brightness" control={control} labelText="Brightness" min={0} max={100} step={5} />
  </div>
  <div className="g-col-12">
    <Form.Slider name="fontSize" control={control} labelText="Font Size" min={12} max={24} step={1} />
  </div>
</div>;
```

### Advanced Filter

```tsx
const filterSchema = z.object({
  priceRange: z.array(z.number()).length(2),
  distanceRange: z.array(z.number()).length(2),
  rating: z.number().min(0).max(5),
});

<div className="grid form-gap">
  <div className="g-col-12">
    <Form.Slider name="priceRange" control={control} labelText="Price" range min={0} max={5000} step={100} />
  </div>
  <div className="g-col-12">
    <Form.Slider name="distanceRange" control={control} labelText="Distance (km)" range min={0} max={100} step={5} />
  </div>
  <div className="g-col-12">
    <Form.Slider name="rating" control={control} labelText="Minimum Rating" min={0} max={5} step={0.5} />
  </div>
</div>;
```

## Component Version

- **Version**: Work in Progress
- **Built on**: PrimeReact Slider
- **Package**: `@neuron/ui`
- **Import**: `import { Form } from "@neuron/ui";`

## Related Components

- **NumberInput**: For precise numeric input with typing
- **Input**: For general text/numeric input
- **RadioSet**: For selecting from predefined discrete values
- **Switch**: For binary on/off selections
- **CheckBox**: For boolean inputs

## Technical Implementation Details

### Value Display

The Slider component automatically appends the current value(s) to the label text:

- **Single value**: `"Volume: 50"`
- **Range**: `"Price Range: 200 - 800"`

### Default Values

- **min**: 0
- **max**: 100
- **step**: 1
- **orientation**: "horizontal"
- **range**: false

### Form Integration

The component uses React Hook Form's `useController` for form state management and automatically handles:

- Value changes via `onChange`
- Validation state display
- Error messages
- Field registration
- Dependencies revalidation

### Accessibility

- Uses proper ARIA attributes (`aria-required`)
- Keyboard navigation support
- Screen reader compatible
- Proper focus management
- Disabled and read-only states properly announced

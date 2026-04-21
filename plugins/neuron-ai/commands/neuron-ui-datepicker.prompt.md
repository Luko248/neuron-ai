---
agent: agent
---

# DatePicker Component AI Guidelines

## For the AI Assistant

Your task is to integrate and configure the Neuron DatePicker component in React applications. This guide provides essential instructions for implementing DatePicker components, which provide comprehensive date/time selection with calendar interface, range selection, localization, and shadowDOM support through React Hook Form integration across all Neuron applications.

## Overview

The DatePicker component is a comprehensive date selection field built on PrimeReact Calendar, providing date/time selection, range selection, and localization features through React Hook Form integration. It includes shadowDOM support for embedded environments and enhanced keyboard navigation for improved accessibility.

## Core Usage

### Basic Implementation

```tsx
import { Form } from "@neuron/ui";
import { useForm } from "react-hook-form";
import { zodResolver } from "@hookform/resolvers/zod";
import { z } from "zod";

const schema = z.object({
  birthDate: z.date({ message: "Birth date is required" }),
  startDate: z.date().optional(),
  endDate: z.date().optional(),
});

const MyForm = () => {
  const { control } = useForm({
    resolver: zodResolver(schema),
    mode: "onChange",
  });

  return (
    <>
      <Form.DatePicker name="birthDate" labelText="Birth Date" control={control} placeholder="Select date" required />

      <Form.DatePicker
        name="appointmentDate"
        labelText="Appointment Date"
        control={control}
        showTime
        placeholder="Select date and time"
      />
    </>
  );
};
```

## Key Props Reference

### Essential Props

- **`name`** (string, required): Field name for form data
- **`labelText`** (string, optional): Label text displayed above the input
- **`control`** (Control, optional): React Hook Form control instance
- **`placeholder`** (string, optional): Placeholder text when no date selected
- **`required`** (boolean, optional): Marks field as required

### Date Configuration

- **`view`** ("date" | "year" | "month", optional): View mode for date selection
- **`showTime`** (boolean, optional): Enable time selection alongside date
- **`showTimeOnly`** (boolean, optional): Show only time picker (no date)
- **`range`** (boolean, optional): Enable date range selection
- **`startDate`** (Date, optional): Start date for range selection
- **`endDate`** (Date, optional): End date for range selection

### Localization & Formatting

- **`locale`** (SupportedLocales, optional): Locale for date formatting (default: "cs")

- **`dateFormat`** (string, optional): Custom date format pattern

### Display & Behavior

- **`inline`** (boolean, optional): Render calendar inline (always visible)
- **`literalView`** (boolean, optional): Display as read-only formatted text
- **`disabled`** (boolean, optional): Disable the input
- **`readOnly`** (boolean, optional): Make input read-only

### Validation & State

- **`error`** (FieldError, optional): Validation error object
- **`isValid`** (boolean, optional): Indicates valid state
- **`onChange`** (function, optional): Custom change handler (triggered after confirmation for time inputs)

## Advanced Usage Patterns

### Date Range Selection

```tsx
<Form.DatePicker
  name="dateRange"
  labelText="Select Date Range"
  control={control}
  range
  placeholder="Start date - End date"
  startDate={new Date()}
  endDate={new Date(Date.now() + 7 * 24 * 60 * 60 * 1000)} // 7 days later
/>
```

### Time Selection

```tsx
// Date with time
<Form.DatePicker
  name="appointmentDateTime"
  labelText="Appointment Date & Time"
  control={control}
  showTime
  placeholder="dd.mm.yyyy hh:mm"
/>

// Time only
<Form.DatePicker
  name="meetingTime"
  labelText="Meeting Time"
  control={control}
  showTimeOnly
  placeholder="hh:mm"
/>
```

### Different View Modes

```tsx
// Year selection
<Form.DatePicker
  name="graduationYear"
  labelText="Graduation Year"
  control={control}
  view="year"
  placeholder="Select year"
/>

// Month selection
<Form.DatePicker
  name="reportMonth"
  labelText="Report Month"
  control={control}
  view="month"
  placeholder="mm.yyyy"
/>
```

### Inline Calendar

```tsx
<Form.DatePicker
  name="calendarDate"
  labelText="Select Date"
  control={control}
  inline
  // No placeholder needed for inline mode
/>
```

### Literal View (Read-only Display)

```tsx
<Form.DatePicker name="createdDate" labelText="Created Date" control={control} literalView value={new Date()} />
```

## Validation Patterns

### Zod Schema Examples

```tsx
import { z } from "zod";

// Basic date validation
const basicSchema = z.object({
  birthDate: z.date({
    required_error: "Birth date is required",
    invalid_type_error: "Please enter a valid date",
  }),
});

// Date range validation
const rangeSchema = z
  .object({
    startDate: z.date(),
    endDate: z.date(),
  })
  .refine((data) => data.endDate >= data.startDate, {
    message: "End date must be after start date",
    path: ["endDate"],
  });

// Future date validation
const futureSchema = z.object({
  appointmentDate: z.date().refine((date) => date > new Date(), {
    message: "Appointment must be in the future",
  }),
});

// Age validation (18+ years)
const ageSchema = z.object({
  birthDate: z.date().refine(
    (date) => {
      const age = new Date().getFullYear() - date.getFullYear();
      return age >= 18;
    },
    {
      message: "Must be at least 18 years old",
    },
  ),
});
```

## Localization Support

### Supported Locales

- **`cs`**: Czech (default) - dd.mm.yyyy format
- Additional locales supported through `SupportedLocales` type

### Custom Date Formats

```tsx
// Czech format (default)
<Form.DatePicker
  name="date"
  locale="cs"
  // Displays as: 28.07.2025
/>;

// Custom formatting through helpers
import { getDateFormats } from "@neuron/ui/form/datePicker/DatePicker.helpers";

const formats = getDateFormats("cs", "date");
// Returns: { prime: "dd.mm.yy", daysjs: "DD.MM.YYYY" }
```

## Accessibility Features

### Built-in Accessibility

- **ARIA labels**: Automatically applied based on `labelText`
- **Keyboard navigation**: Enhanced keyboard support for date selection with manual time input capability
- **Screen reader support**: Proper announcements for date changes
- **Focus management**: Logical tab order and focus indicators

### Custom Accessibility

```tsx
<Form.DatePicker
  name="eventDate"
  labelText="Event Date"
  control={control}
  aria-describedby="date-help"
  testId="event-date-picker"

/>
<div id="date-help">Select the date for your event</div>
```

## Performance Optimization

### Memoization

```tsx
import { useMemo } from "react";

const DateForm = () => {
  const dateFormats = useMemo(() => getDateFormats("cs", "date"), []);

  const handleDateChange = useCallback((value, event) => {
    // Custom change logic
  }, []);

  return (
    <Form.DatePicker name="optimizedDate" labelText="Optimized Date" control={control} onChange={handleDateChange} />
  );
};
```

## Common Use Cases

### 1. Birth Date Selection

```tsx
<Form.DatePicker
  name="birthDate"
  labelText="Date of Birth"
  control={control}
  placeholder="dd.mm.yyyy"
  required
  // Typically no future dates allowed
/>
```

### 2. Appointment Scheduling

```tsx
<Form.DatePicker
  name="appointmentDateTime"
  labelText="Appointment Date & Time"
  control={control}
  showTime
  placeholder="Select date and time"
  // Usually future dates only
/>
```

### 3. Report Period Selection

```tsx
<Form.DatePicker
  name="reportPeriod"
  labelText="Report Period"
  control={control}
  range
  placeholder="Start date - End date"
/>
```

### 4. Event Planning

```tsx
<Form.DatePicker
  name="eventDate"
  labelText="Event Date"
  control={control}
  inline
  // Inline calendar for better UX
/>
```

## Error Handling

### Common Error Scenarios

```tsx
// Invalid date format
<Form.DatePicker
  name="date"
  labelText="Date"
  control={control}
  error={{ message: "Please enter a valid date format" }}
/>

// Date out of range
<Form.DatePicker
  name="futureDate"
  labelText="Future Date"
  control={control}
  error={{ message: "Date must be in the future" }}
/>


// Required field validation
<Form.DatePicker
  name="requiredDate"
  labelText="Required Date"
  control={control}
  required
  error={{ message: "This field is required" }}
/>
```

## Integration with Form Libraries

### React Hook Form Integration

```tsx
import { useForm, Controller } from "react-hook-form";

const FormWithDatePicker = () => {
  const { control, handleSubmit, watch } = useForm();

  const watchedDate = watch("selectedDate");

  return (
    <form onSubmit={handleSubmit(onSubmit)}>
      <Controller
        name="selectedDate"
        control={control}
        render={({ field, fieldState }) => (
          <Form.DatePicker {...field} labelText="Select Date" error={fieldState.error} placeholder="Choose a date" />
        )}
      />

      {watchedDate && <p>Selected: {watchedDate.toLocaleDateString()}</p>}
    </form>
  );
};
```

## Best Practices

### ✅ Do's

- Use appropriate `view` mode for the use case (year for birth year, month for reports)
- Enable `showTime` for appointments and scheduling
- Use `range` for period selections
- Provide clear `placeholder` text indicating expected format

- Use proper validation with meaningful error messages
- Consider `inline` mode for better UX when space allows
- Use `literalView` for read-only date displays

### ❌ Don'ts

- Don't use regular Input for date fields - always use DatePicker

- Don't forget to handle timezone considerations for time selection
- Don't use overly complex date formats that confuse users
- Don't skip validation for date inputs
- Don't use DatePicker for simple text that happens to contain dates
- Don't forget to test with different locales if supporting multiple languages

## Troubleshooting

### Common Issues

1. **Date not updating**: Ensure proper `control` prop and form setup
2. **Format issues**: Check `locale` and `view` props alignment
3. **Validation not working**: Verify Zod schema and error handling

4. **Time selection problems**: Confirm `showTime` or `showTimeOnly` props
5. **Range selection issues**: Ensure both `startDate` and `endDate` are handled

### Debug Tips

```tsx
// Add debug logging
<Form.DatePicker
  name="debugDate"
  labelText="Debug Date"
  control={control}
  onChange={(value, event) => {
    console.log("DatePicker changed:", { value, event });
  }}
/>
```

## Related Components

- **Input**: For text-based date entry (not recommended)
- **Select**: For predefined date options
- **NumberInput**: For year-only selection (alternative to DatePicker with view="year")

## Sync Metadata

- **Component Version:** v4.8.3
- **Component Source:** `packages/neuron/ui/src/lib/form/datePicker/DatePicker.tsx`
- **Guideline Command:** `/neuron-ui-datepicker`
- **Related Skill:** `neuron-ui-form-core`

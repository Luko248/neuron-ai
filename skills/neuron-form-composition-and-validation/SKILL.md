---
name: neuron-form-composition-and-validation
description: Implement forms using React Hook Form and Zod validation in Neuron applications. Use when creating forms with validation, required fields, or dynamic field arrays. Covers schema definition, default values, form composition, field arrays, conditional validation, and required field patterns. Provides type-safe form implementation with i18n support.
---

# Form Composition & Validation

Implement type-safe forms using React Hook Form and Zod validation.

## Process

1. **Define schema** - Create Zod validation schema
2. **Set default values** - Define initial form state
3. **Create form component** - Implement main form with useForm
4. **Add field components** - Create form sections with fields
5. **Handle submission** - Process validated form data
6. **Add field arrays** - Implement dynamic fields if needed

## File Structure

```
ExampleForm/
├── ExampleForm.tsx              # Main form component
├── ExampleFormSchema.ts         # Zod validation schema
├── ExampleFormDefaultValues.ts  # Default values
├── ExampleFormTypes.ts          # TypeScript types
└── sections/
    ├── BasicInfoSection.tsx     # Form sections
    └── ItemsSection.tsx
```

```
exampleForm/
├── ExampleForm.tsx            # Main form component
├── ExampleFormFields.tsx      # Form fields components
├── ExampleFormSchema.ts       # Form validation schema
├── ExampleFormDefaultValues.ts # Default values for the form
├── ExampleFormTypes.ts        # TypeScript types for the form
└── index.ts                   # Exports
```

## Step 1: Form Schema Definition

### 1.1 Form Schema File (`ExampleFormSchema.ts`)

The schema file should contain the complete definition of the form validation schema using Zod. The schema should be defined independently without product definition dependencies. Use `z.nativeEnum()` for enums defined in the types file.

```tsx
import * as z from "zod";
import { useTranslation } from "react-i18next";

export const getFormSchema = (t: (key: string) => string) => {
  const ItemSchema = z.object({
    type: z.enum(["primary", "secondary"]),
    value: z.string().min(1, { message: t("validation.required") }),
  });

  return z.object({
    firstName: z.string().min(1, { message: t("validation.required") }),
    lastName: z.string().optional(),
    dateField: z.string().min(1, { message: t("validation.required") }),
    items: z.array(ItemSchema).min(1, { message: t("validation.atLeastOne") }),
    hasAdditional: z.boolean(),
    additionalName: z
      .string()
      .optional()
      .refine(
        (val, ctx) => {
          if (ctx.parent.hasAdditional && !val) return false;
          return true;
        },
        { message: t("validation.conditionallyRequired") },
      ),
  });
};

export type FormSchema = z.infer<ReturnType<typeof getFormSchema>>;
```

## Default Values

**ExampleFormDefaultValues.ts:**

```tsx
import { FormSchema } from "./ExampleFormSchema";
import dayjs from "dayjs";

export const getFormDefaultValues = (): FormSchema => {
  return {
    firstName: "",
    lastName: "",
    dateField: dayjs().format("YYYY-MM-DD"),
    items: [{ type: "primary", value: "" }],
    hasAdditional: false,
    additionalName: undefined,
  };
};
```

## Form Component

**ExampleForm.tsx:**

```tsx
import { useForm } from "react-hook-form";
import { zodResolver } from "@hookform/resolvers/zod";
import { useTranslation } from "react-i18next";
import { getFormSchema, FormSchema } from "./ExampleFormSchema";
import { getFormDefaultValues } from "./ExampleFormDefaultValues";
import { Input, DatePicker, Button } from "@neuron/ui";

export const ExampleForm = () => {
  const { t } = useTranslation();
  const schema = getFormSchema(t);

  const {
    control,
    handleSubmit,
    formState: { errors },
  } = useForm<FormSchema>({
    resolver: zodResolver(schema),
    defaultValues: getFormDefaultValues(),
  });

  const onSubmit = (data: FormSchema) => {
    console.log("Form data:", data);
  };

  return (
    <form onSubmit={handleSubmit(onSubmit)} className="grid form-gap">
      <Input className="g-col-6" name="firstName" control={control} labelText={t("form.firstName")} required />
      <Input className="g-col-6" name="lastName" control={control} labelText={t("form.lastName")} />
      <DatePicker className="g-col-6" name="dateField" control={control} labelText={t("form.date")} required />
      <Button type="submit">{t("actions.submit")}</Button>
    </form>
  );
};
```

}

/\*\*

- Field type enum for categorization
  \*/
  export enum FieldType {

## Field Arrays

**Dynamic fields with useFieldArray:**

```tsx
import { useFieldArray } from "react-hook-form";

export const ItemsSection = ({ control }) => {
  const { fields, append, remove } = useFieldArray({
    control,
    name: "items",
  });

  return (
    <div className="grid form-gap">
      {fields.map((field, index) => (
        <div key={field.id} className="g-col-12 d-flex gap-2">
          <Input
            className="flex-grow-1"
            name={`items.${index}.value`}
            control={control}
            labelText={`Item ${index + 1}`}
            required
          />
          <Button onClick={() => remove(index)}>Remove</Button>
        </div>
      ))}
      <Button onClick={() => append({ type: "primary", value: "" })}>Add Item</Button>
    </div>
  );
};
```

## Required Field Pattern

**Visual indicator for required fields:**

```tsx
<Input
  name="firstName"
  control={control}
  labelText={t("form.firstName")}
  required // Shows asterisk in label
/>
```

**Schema validation:**

```tsx
firstName: z.string().min(1, { message: t("validation.required") });
```

## Conditional Validation

**Conditionally required field:**

```tsx
additionalName: z.string()
  .optional()
  .refine(
    (val, ctx) => {
      if (ctx.parent.hasAdditional && !val) return false;
      return true;
    },
    { message: t("validation.conditionallyRequired") },
  );
```

## Examples

**Example 1: Basic form**

```tsx
const { control, handleSubmit } = useForm<FormSchema>({
  resolver: zodResolver(getFormSchema(t)),
  defaultValues: getFormDefaultValues(),
});

<form onSubmit={handleSubmit(onSubmit)} className="grid form-gap">
  <Input className="g-col-6" name="firstName" control={control} required />
  <Input className="g-col-6" name="lastName" control={control} />
</form>;
```

**Example 2: Form with date picker**

```tsx
<DatePicker className="g-col-6" name="dateField" control={control} labelText={t("form.date")} required />
```

**Example 3: Form with field array**

```tsx
const { fields, append, remove } = useFieldArray({ control, name: "items" });

{
  fields.map((field, index) => <Input key={field.id} name={`items.${index}.value`} control={control} required />);
}
```

## Best Practices

- Use Zod for schema validation
- Infer types from schema with `z.infer`
- Use `getFormDefaultValues()` function for dynamic defaults
- Use dayjs for date handling
- Wrap forms in `<form>` element with `grid form-gap` classes
- Use `required` prop for visual indicator
- Use `useFieldArray` for dynamic fields
- Add i18n to all labels and validation messages
- Use `zodResolver` with React Hook Form
- Handle form submission with `handleSubmit`

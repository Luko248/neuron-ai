---
agent: agent
---

# Validator Component Guidelines

## For the AI Assistant

This document provides comprehensive guidelines for implementing and configuring the Neuron Validator component. It explains proper usage, integration with React Hook Form, validation message display patterns, and best practices for creating validation navigation systems in complex forms.

## Overview

The Validator component provides a **validation message display and navigation system** that mirrors validation errors from React Hook Form (RHF) and renders them as clickable buttons for field navigation. It does **NOT perform validation** - validation is handled by RHF with schema validation (Zod/Yup). The Validator component has two modes: **RHF Integration** for displaying form validation messages with automatic field focusing, and **Custom Manual Mode** for complex validation scenarios requiring custom navigation patterns.

## 🚨 CRITICAL: Implementation Guidelines

**ONLY implement Validator component when explicitly requested by user requirements.**

- **Default validation approach**: Use React Hook Form with schema validation (Zod/Yup) as described in Form Validation guidelines
- **Validator is OPTIONAL**: Only add when user specifically asks for validation message display or navigation features
- **Not required for basic forms**: Standard form validation works without Validator component
- **Use case**: Complex forms where users need centralized validation message display and quick navigation to problematic fields

## Component Architecture

### Two Operation Modes

1. **React Hook Form Mode** (`control` prop provided): Displays validation messages from RHF with automatic field focusing
2. **Custom Manual Mode** (`control` prop not provided): Manual validation display with custom navigation patterns

**Important**: The Validator component does NOT validate data - it only displays validation results from React Hook Form and provides navigation to problematic fields.

### Key Subcomponents

- **ValidatorRHF**: React Hook Form integration variant
- **ValidatorCustom**: Manual custom validation variant
- **ValidatorBox**: Container for validation messages with title and badge
- **ValidatorMessage**: Individual validation message with click handling
- **ValidatorAccordeon**: Collapsible section for grouping related validations

## Basic Usage

### React Hook Form Mode

```typescript
import { Validator } from '@neuron/ui';
import { useForm } from 'react-hook-form';
import { zodResolver } from '@hookform/resolvers/zod';

interface FormData {
  name: string;
  email: string;
  password: string;
}

const FormWithValidator = () => {
  const { control, handleSubmit, setFocus } = useForm<FormData>({
    resolver: zodResolver(validationSchema),
    mode: "onTouched"
  });

  return (
    <div>
      {/* Validator with automatic RHF integration */}
      <Validator
        control={control as any}
        setFocus={setFocus as any}
        title="Form Validation"
      />

      <form onSubmit={handleSubmit(onSubmit)}>
        <Form.Input
          name="name"
          labelText="Name"
          control={control}
        />
        <Form.Input
          name="email"
          labelText="Email"
          control={control}
        />
        <Form.PasswordInput
          name="password"
          labelText="Password"
          control={control}
        />
        <SubmitButton control={control}>Submit</SubmitButton>
      </form>
    </div>
  );
};
```

### Custom Manual Mode

```typescript
import {
  Validator,
  ValidatorBox,
  ValidatorMessage,
  ValidatorAccordeon
} from '@neuron/ui';

const CustomValidatorExample = () => {
  const [activeTab, setActiveTab] = useState(0);
  const [isAccordionOpen, setIsAccordionOpen] = useState(false);

  const handleComplexValidation = () => {
    // Custom logic: open specific tab, expand accordion, set focus
    setActiveTab(2);                    // Navigate to tab
    setIsAccordionOpen(true);           // Expand accordion
    setTimeout(() => {                  // Set focus after navigation
      document.getElementById('complex-field')?.focus();
    }, 100);
  };

  const handleRelationshipValidation = () => {
    // Focus on multiple related fields
    document.getElementById('field1')?.scrollIntoView();
    document.getElementById('field1')?.focus();
  };

  return (
    <div>
      <Validator
        title="Custom Validation Rules"
        enableErrorDetection={false}
      >
        {() => (
          <>
            {/* Error section */}
            <ValidatorBox
              title="Errors"
              variant="danger"
              enableErrorDetection={false}
            >
              <ValidatorMessage
                variant="danger"
                onClick={handleRelationshipValidation}
              >
                Fields 'Start Date' and 'End Date' have conflicting values
              </ValidatorMessage>

              <ValidatorMessage
                variant="danger"
                onClick={handleComplexValidation}
              >
                Required field missing in advanced settings
              </ValidatorMessage>

              <ValidatorAccordeon title="Form Section Errors">
                <ValidatorMessage
                  variant="danger"
                  onClick={() => navigateToSection('personal-info')}
                >
                  Personal information section incomplete
                </ValidatorMessage>

                <ValidatorMessage
                  variant="danger"
                  onClick={() => navigateToSection('contact-info')}
                >
                  Contact information requires validation
                </ValidatorMessage>
              </ValidatorAccordeon>
            </ValidatorBox>

            {/* Warning section */}
            <ValidatorBox
              title="Warnings"
              variant="warning"
              enableErrorDetection={false}
            >
              <ValidatorMessage variant="warning">
                Consider filling optional field for better experience
              </ValidatorMessage>

              <ValidatorMessage
                variant="warning"
                onClick={() => highlightOptionalFields()}
              >
                Some recommended fields are empty
              </ValidatorMessage>
            </ValidatorBox>
          </>
        )}
      </Validator>
    </div>
  );
};
```

### Controlled Open State

By default the Validator manages its own open/closed state (uncontrolled). When you need to control it from the outside — for example to open the panel programmatically after a failed submit — use the `open` + `onOpenChange` pair.

```typescript
const FormWithControlledValidator = () => {
  const [validatorOpen, setValidatorOpen] = useState(false);
  const { control, handleSubmit, setFocus } = useForm<FormData>({
    resolver: zodResolver(schema),
    mode: "onTouched",
  });

  const onInvalid = () => {
    // Force the panel open whenever submission fails
    setValidatorOpen(true);
  };

  return (
    <>
      <Validator
        control={control}
        setFocus={setFocus}
        open={validatorOpen}
        onOpenChange={setValidatorOpen}
      />

      <form onSubmit={handleSubmit(onSubmit, onInvalid)}>
        {/* form fields */}
        <SubmitButton control={control}>Submit</SubmitButton>
      </form>
    </>
  );
};
```

**Rules for controlled mode:**

- **Always pair `open` with `onOpenChange`** — without the callback the user cannot close the panel.
- **`isOpen` is ignored** when `open` is provided — do not pass both.
- **The component does not toggle itself** in controlled mode; every open/close goes through `onOpenChange`.

## Advanced Custom Scenarios

### Complex Navigation Interactions

```typescript
const ComplexFormValidator = () => {
  const [currentStep, setCurrentStep] = useState(0);
  const tabRef = useRef<any>(null);
  const accordionRef = useRef<any>(null);

  const handleNestedValidation = async () => {
    // Multi-step navigation: Tab → Accordion → Focus

    // 1. Navigate to specific tab
    setCurrentStep(2);
    await new Promise(resolve => setTimeout(resolve, 100));

    // 2. Open accordion panel in that tab
    if (accordionRef.current) {
      accordionRef.current.expand();
    }
    await new Promise(resolve => setTimeout(resolve, 200));

    // 3. Focus on specific field
    const targetField = document.querySelector('[name="nested-field"]');
    if (targetField) {
      targetField.scrollIntoView({ behavior: 'smooth', block: 'center' });
      (targetField as HTMLElement).focus();
    }
  };

  return (
    <Validator enableErrorDetection={false}>
      {() => (
        <ValidatorBox title="Complex Validations" variant="danger" enableErrorDetection={false}>
          <ValidatorMessage
            variant="danger"
            onClick={handleNestedValidation}
          >
            Nested field validation failed in Step 3 → Advanced Settings
          </ValidatorMessage>

          <ValidatorMessage
            variant="danger"
            onClick={() => {
              // Multi-field relationship validation
              const fields = ['startDate', 'endDate', 'duration'];
              fields.forEach(fieldName => {
                const element = document.querySelector(`[name="${fieldName}"]`);
                element?.classList.add('highlight-error');
              });

              // Focus on first problematic field
              document.querySelector('[name="startDate"]')?.focus();
            }}
          >
            Date range calculation error affects multiple fields
          </ValidatorMessage>
        </ValidatorBox>
      )}
    </Validator>
  );
};
```

### Conditional Validation Display

```typescript
const ConditionalValidator = () => {
  const [formData, setFormData] = useState({});
  const [validationState, setValidationState] = useState({
    hasErrors: false,
    hasWarnings: false,
    crossFieldErrors: []
  });

  // Custom validation logic
  useEffect(() => {
    const errors = [];
    const warnings = [];

    // Cross-field validation
    if (formData.input1 && formData.input2 && formData.input3) {
      warnings.push({
        message: "When Input 1 and Input 2 are filled, Input 3 should remain empty",
        fields: ['input1', 'input2', 'input3'],
        action: () => {
          // Highlight all related fields
          ['input1', 'input2', 'input3'].forEach(name => {
            document.querySelector(`[name="${name}"]`)?.classList.add('warning-highlight');
          });
        }
      });
    }

    // Business logic validation
    if (formData.amount > 10000 && !formData.approvalCode) {
      errors.push({
        message: "Amounts over $10,000 require approval code",
        fields: ['amount', 'approvalCode'],
        action: () => {
          document.querySelector('[name="approvalCode"]')?.focus();
        }
      });
    }

    setValidationState({
      hasErrors: errors.length > 0,
      hasWarnings: warnings.length > 0,
      crossFieldErrors: [...errors, ...warnings]
    });
  }, [formData]);

  return (
    <Validator enableErrorDetection={false}>
      {() => (
        <>
          {validationState.hasErrors && (
            <ValidatorBox title="Business Rule Violations" variant="danger" enableErrorDetection={false}>
              {validationState.crossFieldErrors
                .filter(item => item.severity === 'error')
                .map((error, index) => (
                  <ValidatorMessage
                    key={index}
                    variant="danger"
                    onClick={error.action}
                  >
                    {error.message}
                  </ValidatorMessage>
                ))}
            </ValidatorBox>
          )}

          {validationState.hasWarnings && (
            <ValidatorBox title="Recommendations" variant="warning" enableErrorDetection={false}>
              {validationState.crossFieldErrors
                .filter(item => item.severity === 'warning')
                .map((warning, index) => (
                  <ValidatorMessage
                    key={index}
                    variant="warning"
                    onClick={warning.action}
                  >
                    {warning.message}
                  </ValidatorMessage>
                ))}
            </ValidatorBox>
          )}
        </>
      )}
    </Validator>
  );
};
```

## Component Integration Patterns

### With React Hook Form

```typescript
// UserCreationForm.tsx example
const UserCreationForm = () => {
  const { control, handleSubmit, setFocus, watch } = useForm<IUser>({
    defaultValues: initUser(),
    resolver: zodResolver(UserSchema),
    mode: "onTouched"
  });

  return (
    <>
      {/* RHF Validator - automatic validation handling */}
      <Validator control={control as any} setFocus={setFocus as any} />

      <form onSubmit={handleSubmit(handleSubmit, handleError)}>
        <Fieldset legend="User Information" columnCount={12}>
          <Form.Input
            name="name"
            labelText="Name"
            control={control}
          />
          <Form.PasswordInput
            name="password"
            labelText="Password"
            control={control}
          />
          <Form.NumberInput
            name="weight"
            labelText="Weight"
            suffix="kg"
            control={control}
          />
          {/* More form fields... */}
        </Fieldset>
      </form>
    </>
  );
};
```

### With Tabbed Interfaces

```typescript
const TabbedFormWithValidator = () => {
  const [activeTab, setActiveTab] = useState(0);
  const tabValidationErrors = useTabValidation();

  const navigateToTabWithError = (tabIndex: number, fieldName: string) => {
    setActiveTab(tabIndex);
    setTimeout(() => {
      document.querySelector(`[name="${fieldName}"]`)?.focus();
    }, 100);
  };

  return (
    <div>
      <Validator enableErrorDetection={false}>
        {() => (
          <ValidatorBox title="Tab Validation Errors" variant="danger" enableErrorDetection={false}>
            {tabValidationErrors.map((error, index) => (
              <ValidatorMessage
                key={index}
                variant="danger"
                onClick={() => navigateToTabWithError(error.tabIndex, error.fieldName)}
              >
                {error.tabName}: {error.message}
              </ValidatorMessage>
            ))}
          </ValidatorBox>
        )}
      </Validator>

      <TabControl activeIndex={activeTab} onTabChange={setActiveTab}>
        <Tab title="Personal Info">
          {/* Tab 1 content */}
        </Tab>
        <Tab title="Contact Info">
          {/* Tab 2 content */}
        </Tab>
        <Tab title="Advanced Settings">
          {/* Tab 3 content */}
        </Tab>
      </TabControl>
    </div>
  );
};
```

## Props Reference

### Validator Props

| Prop                   | Type                                         | Required | Default | Description                                                                                                                                    |
| ---------------------- | -------------------------------------------- | -------- | ------- | ---------------------------------------------------------------------------------------------------------------------------------------------- |
| `control`              | `Control<Record<string, unknown>>`           | No       | -       | RHF control object (enables RHF mode)                                                                                                          |
| `setFocus`             | `UseFormSetFocus<Record<string, unknown>>`   | No       | -       | RHF setFocus function                                                                                                                          |
| `title`                | `string`                                     | No       | -       | Validator panel title                                                                                                                          |
| `className`            | `string`                                     | No       | -       | Additional CSS class                                                                                                                           |
| `children`             | `React.ReactNode \| (() => React.ReactNode)` | No       | -       | Custom validation content                                                                                                                      |
| `enableErrorDetection` | `boolean`                                    | No       | `true`  | Enable automatic RHF error detection                                                                                                           |
| `isOpen`               | `boolean`                                    | No       | `true`  | Initial open state (uncontrolled)                                                                                                              |
| `open`                 | `boolean`                                    | No       | -       | Controlled open state. When provided, the component reflects this value and no longer manages its own state. Use together with `onOpenChange`. |
| `onOpenChange`         | `(open: boolean) => void`                    | No       | -       | Callback fired with the new desired open state when the user interacts with the toggle (controlled mode).                                      |
| `testId`               | `string`                                     | No       | -       | Custom test ID                                                                                                                                 |

### ValidatorBox Props

| Prop                   | Type                                         | Required | Default | Description                        |
| ---------------------- | -------------------------------------------- | -------- | ------- | ---------------------------------- |
| `title`                | `string`                                     | Yes      | -       | Section title                      |
| `variant`              | `"danger" \| "warning"`                      | Yes      | -       | Visual variant                     |
| `messages`             | `string[]`                                   | No       | `[]`    | Auto-generated messages (RHF mode) |
| `fields`               | `string[]`                                   | No       | `[]`    | Field names for auto-focus         |
| `children`             | `React.ReactNode \| (() => React.ReactNode)` | No       | -       | Custom validation messages         |
| `setFocus`             | `UseFormSetFocus<Record<string, unknown>>`   | No       | -       | Focus function                     |
| `enableErrorDetection` | `boolean`                                    | No       | `true`  | Enable RHF integration             |
| `onMessageClick`       | `(index: number) => void`                    | No       | -       | Message click handler              |

### ValidatorMessage Props

| Prop       | Type                                         | Required | Default | Description                               |
| ---------- | -------------------------------------------- | -------- | ------- | ----------------------------------------- |
| `variant`  | `"danger" \| "warning"`                      | Yes      | -       | Message type                              |
| `onClick`  | `() => void`                                 | No       | -       | Click handler (makes message interactive) |
| `children` | `React.ReactNode \| (() => React.ReactNode)` | Yes      | -       | Message content                           |
| `index`    | `number`                                     | No       | -       | Message index                             |

## Responsive Behavior

### Desktop Layout

- Displays as dock panel on the right side
- Always visible when validation errors exist
- Direct field focusing without closing panel

### Mobile/Tablet Layout

- Displays as SideSheet overlay
- Triggered by dock button with notification badge
- Auto-closes when message is clicked to allow field focusing

```typescript
// Responsive behavior is handled automatically
<Validator
  control={control}
  setFocus={setFocus}
  title="Validation"
/>
```

## Accessibility Features

### ARIA Support

- `role="alert"` for validation containers
- Proper focus management
- Screen reader compatible message structure

### Keyboard Navigation

- Tab navigation through interactive messages
- Enter/Space activation for message clicks
- Escape to close SideSheet on mobile

### Focus Management

```typescript
// Automatic focus management in RHF mode
<Validator control={control} setFocus={setFocus} />

// Custom focus management
<ValidatorMessage
  onClick={() => {
    const field = document.querySelector('[name="fieldName"]');
    field?.scrollIntoView({ behavior: 'smooth', block: 'center' });
    field?.focus();
  }}
>
  Custom validation message
</ValidatorMessage>
```

## Best Practices

### RHF Mode Guidelines

1. **Always provide setFocus function**:

   ```typescript
   const { control, setFocus } = useForm();
   <Validator control={control} setFocus={setFocus} />
   ```

2. **Use proper form validation mode**:

   ```typescript
   const form = useForm({
     mode: "onTouched", // Recommended for better UX
     resolver: zodResolver(schema),
   });
   ```

3. **Implement comprehensive validation schemas**:
   ```typescript
   const schema = z.object({
     name: z.string().min(1, "Name is required"),
     email: z.string().email("Invalid email format"),
     password: z.string().min(8, "Password must be at least 8 characters"),
   });
   ```

### Custom Mode Guidelines

1. **Group related validations**:

   ```typescript
   <ValidatorBox title="Form Section A" variant="danger">
     {/* Related validation messages */}
   </ValidatorBox>
   ```

2. **Implement meaningful navigation**:

   ```typescript
   const handleNavigateToError = () => {
     // Navigate to tab/section
     // Expand accordion if needed
     // Focus on field
     // Highlight related fields
   };
   ```

3. **Use appropriate message variants**:
   - **Danger**: Critical errors that prevent form submission
   - **Warning**: Recommendations or optional improvements

### Performance Optimization

1. **Memoize custom validation logic**:

   ```typescript
   const validationMessages = useMemo(() => {
     return generateCustomValidations(formData);
   }, [formData]);
   ```

2. **Debounce complex validations**:
   ```typescript
   const debouncedValidation = useCallback(
     debounce((data) => setValidationState(validate(data)), 300),
     [],
   );
   ```

## Common Mistakes

❌ **Don't do this:**

```typescript
// Missing setFocus in RHF mode
<Validator control={control} />

// Mixing RHF and custom modes incorrectly
<Validator control={control} enableErrorDetection={false}>
  <ValidatorMessage>Custom message</ValidatorMessage>
</Validator>

// Not handling async navigation
<ValidatorMessage
  onClick={() => {
    setActiveTab(2);
    document.querySelector('[name="field"]')?.focus(); // Too fast!
  }}
>
```

✅ **Do this:**

```typescript
// Proper RHF integration
<Validator control={control} setFocus={setFocus} />

// Clear separation of modes
<Validator enableErrorDetection={false}>
  <ValidatorBox enableErrorDetection={false}>
    <ValidatorMessage>Custom message</ValidatorMessage>
  </ValidatorBox>
</Validator>

// Proper async navigation
<ValidatorMessage
  onClick={async () => {
    setActiveTab(2);
    await new Promise(resolve => setTimeout(resolve, 100));
    document.querySelector('[name="field"]')?.focus();
  }}
>
```

## Integration Examples

### Real-world Form Validation

```typescript
// Based on UserCreationForm.tsx
const ProductionFormWithValidator = () => {
  const { control, handleSubmit, setFocus, watch } = useForm<FormData>({
    defaultValues: getInitialValues(),
    resolver: zodResolver(validationSchema),
    mode: "onTouched"
  });

  // Watch for cross-field validation
  const watchedValues = watch(['startDate', 'endDate', 'amount']);

  return (
    <>
      {/* Primary RHF validation */}
      <Validator control={control} setFocus={setFocus} />

      {/* Additional custom business rules */}
      <Validator enableErrorDetection={false}>
        {() => (
          <CustomBusinessRuleValidation
            values={watchedValues}
            onNavigate={handleCustomNavigation}
          />
        )}
      </Validator>

      <form onSubmit={handleSubmit(onSubmit)}>
        <Fieldset legend="Basic Information">
          <Form.Input name="name" control={control} />
          <Form.DatePicker name="startDate" control={control} />
          <Form.DatePicker name="endDate" control={control} />
        </Fieldset>

        <Fieldset legend="Financial Details">
          <Form.NumberInput name="amount" control={control} />
          <Form.Input name="approvalCode" control={control} />
        </Fieldset>
      </form>
    </>
  );
};
```

## For the AI Assistant

When implementing Validator components, you must:

### 🚨 MANDATORY Rules

1. **IMPLEMENT ONLY when explicitly requested** - Validator is optional, not default validation approach
2. **CHOOSE correct mode** - Use `control` prop for RHF message display, omit for custom mode
3. **PROVIDE setFocus for RHF mode** - Always pass setFocus function when using control prop
4. **UNDERSTAND purpose** - Validator displays validation messages and provides navigation, does NOT validate data
5. **USE RHF validation as default** - Standard forms use React Hook Form validation without Validator component
6. **STRUCTURE custom messages properly** - Use ValidatorBox → ValidatorMessage hierarchy for custom mode
7. **IMPLEMENT proper navigation** - Handle async tab/accordion navigation with delays
8. **USE appropriate variants** - "danger" for errors, "warning" for recommendations

### Mode Selection Strategy

**Use RHF Mode when:**

- User explicitly requests validation message display interface
- Complex forms where centralized validation navigation is needed
- Standard form validation with React Hook Form already implemented
- Simple field-to-field focus navigation required

**Use Custom Mode when:**

- User requests custom validation message display
- Cross-field business logic validation messages
- Complex navigation patterns (tabs → accordions → fields)
- Custom validation messages with specific actions
- Multi-step form validation display
- Warning messages that don't prevent submission

**Default Approach (NO Validator needed):**

- Standard form validation using React Hook Form with schema validation
- Individual field error display using Form components
- Basic form validation without centralized message display

### RHF Integration Pattern

Always follow this pattern for RHF integration:

```typescript
const { control, setFocus } = useForm({
  resolver: zodResolver(schema),
  mode: "onTouched"
});

<Validator
  control={control as any}
  setFocus={setFocus as any}
  title="Form Validation"
/>
```

### Custom Mode Pattern

Always follow this pattern for custom validation:

```typescript
<Validator enableErrorDetection={false}>
  {() => (
    <>
      <ValidatorBox title="Errors" variant="danger" enableErrorDetection={false}>
        <ValidatorMessage
          variant="danger"
          onClick={handleCustomNavigation}
        >
          Custom error message
        </ValidatorMessage>
      </ValidatorBox>

      <ValidatorBox title="Warnings" variant="warning" enableErrorDetection={false}>
        <ValidatorMessage variant="warning">
          Static warning message
        </ValidatorMessage>
      </ValidatorBox>
    </>
  )}
</Validator>
```

### Navigation Implementation

For complex navigation, always implement proper timing:

```typescript
const handleComplexNavigation = async () => {
  // 1. Navigate to tab
  setActiveTab(targetTab);
  await new Promise((resolve) => setTimeout(resolve, 100));

  // 2. Expand accordion if needed
  accordionRef.current?.expand();
  await new Promise((resolve) => setTimeout(resolve, 200));

  // 3. Focus field
  document.querySelector(`[name="${fieldName}"]`)?.focus();
};
```

If you encounter validation display requirements, **NEVER create custom validation components** - always use the Neuron Validator component in the appropriate mode and follow these exact patterns. Remember: **Validator is OPTIONAL** - only implement when explicitly requested by user requirements for validation message display and navigation features.

## Sync Metadata

- **Component Version:** v4.5.2
- **Component Source:** `packages/neuron/ui/src/lib/patterns/validator/Validator.tsx`
- **Guideline Command:** `/neuron-ui-validator`
- **Related Skill:** `neuron-ui-data`

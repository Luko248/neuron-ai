---
agent: agent
---

# PasswordInput Component AI Guidelines

## For the AI Assistant

Your task is to integrate and configure the Neuron PasswordInput component in React applications. This guide provides essential instructions for implementing PasswordInput components, which provide secure password entry functionality with strength validation, visibility toggle, icon color fixes, and enhanced UX features through React Hook Form integration across all Neuron applications.

## Overview

The PasswordInput component is a specialized input field for password entry built on PrimeReact Password, providing password strength validation, visibility toggle, secure input handling, and enhanced UX features through React Hook Form integration.

## Core Usage

### Basic Implementation

```tsx
import { Form } from "@neuron/ui";
import { useForm } from "react-hook-form";
import { zodResolver } from "@hookform/resolvers/zod";
import { z } from "zod";

const schema = z
  .object({
    password: z
      .string()
      .min(8, { message: "Password must be at least 8 characters" })
      .regex(/^(?=.*[a-z])(?=.*[A-Z])(?=.*\d)(?=.*\W).{8,}$/, {
        message: "Password must contain uppercase, lowercase, number, and special character",
      }),
    confirmPassword: z.string(),
  })
  .refine((data) => data.password === data.confirmPassword, {
    message: "Passwords don't match",
    path: ["confirmPassword"],
  });

const PasswordForm = () => {
  const { control } = useForm({
    resolver: zodResolver(schema),
    mode: "onChange",
  });

  return (
    <>
      <Form.PasswordInput
        name="password"
        labelText="Password"
        control={control}
        placeholder="Enter your password"
        requiredFlag={true}
      />

      <Form.PasswordInput
        name="confirmPassword"
        labelText="Confirm Password"
        control={control}
        placeholder="Confirm your password"
        requiredFlag={true}
      />
    </>
  );
};
```

## Key Props Reference

### Essential Props

- **name**: Field name for form registration
- **control**: React Hook Form control object
- **labelText**: Accessible label text for the password field

### Password Strength Configuration

- **mediumRegex**: Custom regex for medium strength validation (default: 8+ chars with mixed case, numbers, symbols)
- **strongRegex**: Custom regex for strong strength validation (default: 12+ chars with mixed case, numbers, symbols)

### Validation & States

- **required**: HTML5 required attribute
- **requiredFlag**: Shows required indicator (\*)
- **optional**: Shows optional indicator
- **isValid**: Manual valid state indicator

### Enhanced Features

- **tooltip**: Help text with tooltip integration
- **description**: Helper text below input
- **descriptionVariant**: `"default"` | `"success"` | `"warning"` | `"danger"` | `"info"`
- **placeholder**: Placeholder text

### Layout & Styling

- **inline**: Horizontal label/input layout
- **className**: CSS classes for grid positioning
- **leftAddonContent**: Components before input (buttons, icons)
- **rightAddonContent**: Components after input (buttons, icons)

### Access Control

- **readOnly**: Make input read-only
- **disabled**: Disable input interaction
- **readonlyAccess**: Role-based read access
- **fullAccess**: Role-based full access

## Password Strength Validation

### Default Strength Rules

The component provides built-in strength validation:

- **Weak**: Basic input validation
- **Medium** (default regex): `^(?=.*[a-z])(?=.*[A-Z])(?=.*\d)(?=.*\W).{8,}$`
  - At least 8 characters
  - At least one lowercase letter
  - At least one uppercase letter
  - At least one digit
  - At least one special character
- **Strong** (default regex): `^(?=.*[a-z])(?=.*[A-Z])(?=.*\d)(?=.*\W).{12,}$`
  - At least 12 characters
  - At least one lowercase letter
  - At least one uppercase letter
  - At least one digit
  - At least one special character

### Custom Strength Rules

```tsx
// Custom strength requirements
<Form.PasswordInput
  name="customPassword"
  labelText="Custom Password"
  control={control}
  mediumRegex="^(?=.*[a-z])(?=.*[A-Z])(?=.*\d).{6,}$"
  strongRegex="^(?=.*[a-z])(?=.*[A-Z])(?=.*\d)(?=.*[!@#$%^&*]).{10,}$"
/>

// Enterprise-level password requirements
<Form.PasswordInput
  name="enterprisePassword"
  labelText="Enterprise Password"
  control={control}
  mediumRegex="^(?=.*[a-z])(?=.*[A-Z])(?=.*\d)(?=.*[!@#$%^&*(),.?\":{}|<>]).{10,}$"
  strongRegex="^(?=.*[a-z])(?=.*[A-Z])(?=.*\d)(?=.*[!@#$%^&*(),.?\":{}|<>])(?!.*(.)\1{2,}).{14,}$"
/>
```

## Validation Patterns

### Zod Schema Password Validation

```tsx
// Basic password validation
const basicPasswordSchema = z.object({
  password: z
    .string()
    .min(8, { message: "Password must be at least 8 characters" })
    .max(128, { message: "Password cannot exceed 128 characters" }),
});

// Advanced password validation
const advancedPasswordSchema = z.object({
  password: z
    .string()
    .min(8, { message: "Password must be at least 8 characters" })
    .max(128, { message: "Password cannot exceed 128 characters" })
    .regex(/^(?=.*[a-z])/, { message: "Password must contain at least one lowercase letter" })
    .regex(/^(?=.*[A-Z])/, { message: "Password must contain at least one uppercase letter" })
    .regex(/^(?=.*\d)/, { message: "Password must contain at least one number" })
    .regex(/^(?=.*[!@#$%^&*(),.?\":{}|<>])/, { message: "Password must contain at least one special character" })
    .refine((password) => !/(.)\1{2,}/.test(password), {
      message: "Password cannot contain repeating characters",
    })
    .refine((password) => !/^(password|123456|qwerty)/i.test(password), {
      message: "Password cannot be a common password",
    }),
});

// Password confirmation validation
const passwordConfirmSchema = z
  .object({
    password: z.string().min(8),
    confirmPassword: z.string(),
  })
  .refine((data) => data.password === data.confirmPassword, {
    message: "Passwords don't match",
    path: ["confirmPassword"],
  });

// Change password form validation
const changePasswordSchema = z
  .object({
    currentPassword: z.string().min(1, { message: "Current password is required" }),
    newPassword: z
      .string()
      .min(8, { message: "New password must be at least 8 characters" })
      .regex(/^(?=.*[a-z])(?=.*[A-Z])(?=.*\d)(?=.*\W).{8,}$/, {
        message: "Password must contain uppercase, lowercase, number, and special character",
      }),
    confirmNewPassword: z.string(),
  })
  .refine((data) => data.newPassword === data.confirmNewPassword, {
    message: "New passwords don't match",
    path: ["confirmNewPassword"],
  })
  .refine((data) => data.currentPassword !== data.newPassword, {
    message: "New password must be different from current password",
    path: ["newPassword"],
  });
```

### Real-time Password Strength Feedback

```tsx
const PasswordCreationForm = () => {
  const { control, watch } = useForm({
    resolver: zodResolver(advancedPasswordSchema),
    mode: "onChange",
  });

  const password = watch("password");

  return (
    <Form.PasswordInput
      name="password"
      labelText="Create Password"
      control={control}
      placeholder="Enter a strong password"
      description="Password strength will be evaluated as you type"
      descriptionVariant="info"
      tooltip="Use a mix of uppercase, lowercase, numbers, and special characters"
    />
  );
};
```

## Visual Enhancement

### Addons and Icons

```tsx
import { Button, Icon, baseIcons } from "@neuron/ui";

// Generate password button
<Form.PasswordInput
  name="generatedPassword"
  labelText="Password"
  control={control}
  rightAddonContent={
    <Button
      size="small"
      variant="secondary"
      onClick={generateRandomPassword}
    >
      Generate
    </Button>
  }
/>

// Security level indicator
<Form.PasswordInput
  name="securePassword"
  labelText="Secure Password"
  control={control}
  leftAddonContent={<Icon iconDef={baseIcons.shieldSolid} />}
  description="High security password required"
  descriptionVariant="warning"
/>

// Password manager integration
<Form.PasswordInput
  name="managedPassword"
  labelText="Password"
  control={control}
  rightAddonContent={
    <Button
      size="small"
      variant="info"
      onClick={openPasswordManager}
    >
      <Icon iconDef={baseIcons.keySolid} />
    </Button>
  }
/>
```

## Layout Integration

### Grid Layout

```tsx
import { Fieldset } from "@neuron/ui";

<Fieldset legend="Security Settings" columnCount={12}>
  <Form.PasswordInput
    className="g-col-12 g-col-md-6"
    name="currentPassword"
    labelText="Current Password"
    control={control}
    requiredFlag={true}
  />

  <Form.PasswordInput
    className="g-col-12 g-col-md-6"
    name="newPassword"
    labelText="New Password"
    control={control}
    requiredFlag={true}
  />

  <Form.PasswordInput
    className="g-col-12"
    name="confirmPassword"
    labelText="Confirm New Password"
    control={control}
    requiredFlag={true}
  />
</Fieldset>;
```

### Inline Layout

```tsx
// Horizontal label and input
<Form.PasswordInput name="inlinePassword" labelText="Password" control={control} inline={true} />
```

## Accessibility Features

### Required Field Indicators

```tsx
// Required password with visual indicator
<Form.PasswordInput
  name="requiredPassword"
  labelText="Password"
  control={control}
  required={true}
  requiredFlag={true}
/>

// Optional password field
<Form.PasswordInput
  name="optionalPassword"
  labelText="Optional Password"
  control={control}
  optional={true}
/>
```

### Help Text and Descriptions

```tsx
// Tooltip with password requirements
<Form.PasswordInput
  name="guidedPassword"
  labelText="Create Password"
  control={control}
  tooltip="Password must be 8-128 characters with uppercase, lowercase, numbers, and symbols"
  description="Real-time strength indicator will guide you"
  descriptionVariant="info"
/>

// Success indication for valid password
<Form.PasswordInput
  name="validPassword"
  labelText="Password"
  control={control}
  description="Password meets all requirements"
  descriptionVariant="success"
  isValid={true}
/>
```

## Advanced Usage Patterns

### Password Creation Form

```tsx
import { Form, SubmitButton, Fieldset } from "@neuron/ui";
import { useForm, SubmitHandler } from "react-hook-form";
import { zodResolver } from "@hookform/resolvers/zod";
import { z } from "zod";

interface PasswordCreationData {
  password: string;
  confirmPassword: string;
}

const passwordSchema = z
  .object({
    password: z
      .string()
      .min(8, { message: "Password must be at least 8 characters" })
      .regex(/^(?=.*[a-z])(?=.*[A-Z])(?=.*\d)(?=.*\W).{8,}$/, {
        message: "Password must contain uppercase, lowercase, number, and special character",
      }),
    confirmPassword: z.string(),
  })
  .refine((data) => data.password === data.confirmPassword, {
    message: "Passwords don't match",
    path: ["confirmPassword"],
  });

const PasswordCreationForm = () => {
  const { control, handleSubmit } = useForm<PasswordCreationData>({
    resolver: zodResolver(passwordSchema),
    mode: "onChange",
    defaultValues: {
      password: "",
      confirmPassword: "",
    },
  });

  const onSubmit: SubmitHandler<PasswordCreationData> = (data) => {
    console.info("Password created:", { passwordLength: data.password.length });
  };

  return (
    <form onSubmit={handleSubmit(onSubmit)}>
      <Fieldset legend="Create Your Password" columnCount={12}>
        <Form.PasswordInput
          className="g-col-12"
          name="password"
          labelText="Password"
          control={control}
          requiredFlag={true}
          placeholder="Create a strong password"
          tooltip="Use a mix of uppercase, lowercase, numbers, and special characters"
          description="Password strength will be evaluated in real-time"
          descriptionVariant="info"
        />

        <Form.PasswordInput
          className="g-col-12"
          name="confirmPassword"
          labelText="Confirm Password"
          control={control}
          requiredFlag={true}
          placeholder="Re-enter your password"
          description="Must match the password above"
        />

        <div className="g-col-12">
          <SubmitButton control={control}>Create Password</SubmitButton>
        </div>
      </Fieldset>
    </form>
  );
};
```

### Change Password Form

```tsx
interface ChangePasswordData {
  currentPassword: string;
  newPassword: string;
  confirmNewPassword: string;
}

const changePasswordSchema = z
  .object({
    currentPassword: z.string().min(1, { message: "Current password is required" }),
    newPassword: z
      .string()
      .min(8, { message: "New password must be at least 8 characters" })
      .regex(/^(?=.*[a-z])(?=.*[A-Z])(?=.*\d)(?=.*\W).{8,}$/, {
        message: "Password must contain uppercase, lowercase, number, and special character",
      }),
    confirmNewPassword: z.string(),
  })
  .refine((data) => data.newPassword === data.confirmNewPassword, {
    message: "New passwords don't match",
    path: ["confirmNewPassword"],
  })
  .refine((data) => data.currentPassword !== data.newPassword, {
    message: "New password must be different from current password",
    path: ["newPassword"],
  });

const ChangePasswordForm = () => {
  const { control, handleSubmit } = useForm<ChangePasswordData>({
    resolver: zodResolver(changePasswordSchema),
    mode: "onChange",
    defaultValues: {
      currentPassword: "",
      newPassword: "",
      confirmNewPassword: "",
    },
  });

  const onSubmit: SubmitHandler<ChangePasswordData> = async (data) => {
    try {
      await changePassword(data.currentPassword, data.newPassword);
      console.info("Password changed successfully");
    } catch (error) {
      console.error("Password change failed:", error);
    }
  };

  return (
    <form onSubmit={handleSubmit(onSubmit)}>
      <Fieldset legend="Change Password" columnCount={12}>
        <Form.PasswordInput
          className="g-col-12"
          name="currentPassword"
          labelText="Current Password"
          control={control}
          requiredFlag={true}
          placeholder="Enter your current password"
        />

        <Form.PasswordInput
          className="g-col-12 g-col-md-6"
          name="newPassword"
          labelText="New Password"
          control={control}
          requiredFlag={true}
          placeholder="Create a new password"
          description="Must be different from current password"
          descriptionVariant="info"
        />

        <Form.PasswordInput
          className="g-col-12 g-col-md-6"
          name="confirmNewPassword"
          labelText="Confirm New Password"
          control={control}
          requiredFlag={true}
          placeholder="Confirm your new password"
        />

        <div className="g-col-12">
          <SubmitButton control={control}>Change Password</SubmitButton>
        </div>
      </Fieldset>
    </form>
  );
};
```

### Login Form with Password

```tsx
interface LoginData {
  username: string;
  password: string;
}

const loginSchema = z.object({
  username: z.string().min(1, { message: "Username is required" }),
  password: z.string().min(1, { message: "Password is required" }),
});

const LoginForm = () => {
  const { control, handleSubmit } = useForm<LoginData>({
    resolver: zodResolver(loginSchema),
    mode: "onSubmit", // Only validate on submit for login
    defaultValues: {
      username: "",
      password: "",
    },
  });

  const onSubmit: SubmitHandler<LoginData> = async (data) => {
    try {
      await login(data.username, data.password);
      console.info("Login successful");
    } catch (error) {
      console.error("Login failed:", error);
    }
  };

  return (
    <form onSubmit={handleSubmit(onSubmit)}>
      <Fieldset legend="Sign In" columnCount={12}>
        <Form.Input
          className="g-col-12"
          name="username"
          labelText="Username"
          control={control}
          requiredFlag={true}
          placeholder="Enter your username"
        />

        <Form.PasswordInput
          className="g-col-12"
          name="password"
          labelText="Password"
          control={control}
          requiredFlag={true}
          placeholder="Enter your password"
          description="Forgot your password? Contact support"
          descriptionVariant="info"
        />

        <div className="g-col-12">
          <SubmitButton control={control}>Sign In</SubmitButton>
        </div>
      </Fieldset>
    </form>
  );
};
```

## Security Best Practices

### Password Validation Rules

1. **Minimum Length**: At least 8 characters, preferably 12+
2. **Character Diversity**: Mix of uppercase, lowercase, numbers, symbols
3. **No Common Patterns**: Avoid dictionary words, sequences, repetition
4. **No Personal Information**: Don't allow user data in passwords
5. **Maximum Length**: Set reasonable upper limit (e.g., 128 characters)

### Implementation Security

```tsx
// Secure password validation
const securePasswordSchema = z.object({
  password: z
    .string()
    .min(12, { message: "Password must be at least 12 characters" })
    .max(128, { message: "Password cannot exceed 128 characters" })
    .regex(/^(?=.*[a-z])/, { message: "Must contain lowercase letter" })
    .regex(/^(?=.*[A-Z])/, { message: "Must contain uppercase letter" })
    .regex(/^(?=.*\d)/, { message: "Must contain number" })
    .regex(/^(?=.*[!@#$%^&*(),.?\":{}|<>])/, { message: "Must contain special character" })
    .refine((password) => !/(.)\1{2,}/.test(password), {
      message: "Cannot contain 3+ consecutive identical characters",
    })
    .refine((password) => !/^(password|123456|qwerty|admin)/i.test(password), {
      message: "Cannot use common passwords",
    }),
});

// Enterprise-level password with custom validation
<Form.PasswordInput
  name="enterprisePassword"
  labelText="Enterprise Password"
  control={control}
  mediumRegex="^(?=.*[a-z])(?=.*[A-Z])(?=.*\d)(?=.*[!@#$%^&*]).{12,}$"
  strongRegex="^(?=.*[a-z])(?=.*[A-Z])(?=.*\d)(?=.*[!@#$%^&*])(?!.*(.)\1{2,}).{16,}$"
  description="Must meet enterprise security requirements"
  descriptionVariant="warning"
/>;
```

## Best Practices

### Validation Strategy

1. **Use comprehensive Zod schemas** for password validation
2. **Set mode to "onChange"** for password creation forms
3. **Use "onSubmit" mode** for login forms to avoid premature validation
4. **Provide clear, specific error messages** for each validation rule
5. **Use custom regex patterns** for organization-specific requirements

### UX Considerations

1. **Always show password strength feedback** during creation
2. **Use clear, accessible labels** and help text
3. **Provide tooltips** with password requirements
4. **Show character requirements** in the footer
5. **Use appropriate descriptions** to guide users

### Security Considerations

1. **Never log or store passwords** in plain text
2. **Use proper form validation** to prevent weak passwords
3. **Implement rate limiting** for password attempts
4. **Provide secure password reset** mechanisms
5. **Consider password manager integration**

### Accessibility

1. **Use semantic labeling** with labelText
2. **Provide clear validation feedback** with error messages
3. **Use description text** to explain requirements
4. **Ensure keyboard navigation** works properly
5. **Test with screen readers** for accessibility

## Common Patterns

### Registration Password

```tsx
<Form.PasswordInput
  name="registrationPassword"
  labelText="Create Password"
  control={control}
  requiredFlag={true}
  placeholder="Create a strong password"
  tooltip="Password must be 8+ characters with mixed case, numbers, and symbols"
  description="Choose a password you haven't used elsewhere"
  descriptionVariant="info"
/>
```

### Administrative Password Reset

```tsx
<Form.PasswordInput
  name="adminResetPassword"
  labelText="New Password"
  control={control}
  requiredFlag={true}
  mediumRegex="^(?=.*[a-z])(?=.*[A-Z])(?=.*\d)(?=.*[!@#$%^&*]).{12,}$"
  strongRegex="^(?=.*[a-z])(?=.*[A-Z])(?=.*\d)(?=.*[!@#$%^&*])(?!.*(.)\1{2,}).{16,}$"
  description="Administrative password requires enhanced security"
  descriptionVariant="warning"
/>
```

### Temporary Password

```tsx
<Form.PasswordInput
  name="temporaryPassword"
  labelText="Temporary Password"
  control={control}
  readOnly={true}
  description="This temporary password must be changed on first login"
  descriptionVariant="warning"
  rightAddonContent={
    <Button size="small" onClick={copyToClipboard}>
      Copy
    </Button>
  }
/>
```

This component provides comprehensive password input functionality with strength validation, security features, and enhanced UX for authentication and security-focused applications.

## Sync Metadata

- **Component Version:** v4.0.2
- **Component Source:** `packages/neuron/ui/src/lib/form/passwordInput/PasswordInput.tsx`
- **Guideline Command:** `/neuron-ui-passwordinput`
- **Related Skill:** `neuron-ui-form-core`

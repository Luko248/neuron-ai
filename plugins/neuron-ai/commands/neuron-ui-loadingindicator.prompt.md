---
agent: agent
---

# AI-Assisted Neuron LoadingIndicator Component Integration Guide

## For the AI Assistant

Your task is to integrate and configure the Neuron LoadingIndicator components in a React application. This guide provides instructions for implementing the LoadingIndicator component, which serves as a small loading animation typically used in form component suffix properties.

## Sync Metadata

- **Component Version:** v4.0.1
- **Component Source:** `packages/neuron/ui/src/lib/helpers/loadingIndicator/LoadingIndicator.tsx`
- **Guideline Command:** `/neuron-ui-loadingindicator`
- **Related Skill:** `neuron-ui-helpers`

## Introduction

The LoadingIndicator is a simple helper component that displays a small loading animation. It's designed for use in form input suffixes to indicate loading states during data requests or validation processes.

### Key Features

- **Small Loading Animation**: Lightweight loading indicator for form inputs
- **Customizable Color**: Configurable color using CSS custom properties
- **Form Integration**: Designed for use in form component suffix properties
- **Minimal Footprint**: Simple, performant loading animation
- **Request State Indication**: Perfect for showing data loading or request waiting states

**Storybook Reference:** You can refer to the Storybook documentation for visual examples and interactive demos of the LoadingIndicator component.

## Step 1: Basic LoadingIndicator Implementation

### 1.1 Import the LoadingIndicator Component

```tsx
import { LoadingIndicator } from "@neuron/ui";
```

### 1.2 Basic Usage in Form Suffix

The primary use case is in form component suffix properties during data loading:

```tsx
import { Form, LoadingIndicator } from "@neuron/ui";
import { useForm } from "react-hook-form";
import { useState } from "react";

const SearchWithLoading = () => {
  const { control } = useForm();
  const [isSearching, setIsSearching] = useState(false);

  const handleSearch = async (value: string) => {
    setIsSearching(true);
    try {
      // Simulate API call
      await fetch(`/api/search?q=${value}`);
    } finally {
      setIsSearching(false);
    }
  };

  return (
    <Form.Search
      name="search"
      control={control}
      labelText="Search"
      placeholder="Enter search terms..."
      suffix={isSearching ? <LoadingIndicator /> : undefined}
      onSearch={handleSearch}
    />
  );
};
```

### 1.3 Input with Validation Loading State

```tsx
import { Form, LoadingIndicator } from "@neuron/ui";
import { useForm } from "react-hook-form";
import { useState } from "react";

const InputWithValidation = () => {
  const { control } = useForm();
  const [isValidating, setIsValidating] = useState(false);

  const validateUsername = async (username: string) => {
    setIsValidating(true);
    try {
      // Simulate username availability check
      await fetch(`/api/validate-username?username=${username}`);
    } finally {
      setIsValidating(false);
    }
  };

  return (
    <Form.Input
      name="username"
      control={control}
      labelText="Username"
      placeholder="Enter username..."
      suffix={isValidating ? <LoadingIndicator /> : undefined}
      onBlur={(e) => validateUsername(e.target.value)}
    />
  );
};
```

## Step 2: Custom Color Configuration

### 2.1 Custom Color LoadingIndicator

```tsx
import { LoadingIndicator } from "@neuron/ui";

const CustomColorLoading = () => {
  return (
    <div>
      {/* Default color */}
      <LoadingIndicator />

      {/* Primary theme color */}
      <LoadingIndicator color="var(--colorPrimary)" />

      {/* Success state color */}
      <LoadingIndicator color="var(--colorSuccess)" />

      {/* Custom hex color */}
      <LoadingIndicator color="#ff6b6b" />
    </div>
  );
};
```

### 2.2 Context-Aware Color Usage

```tsx
import { Form, LoadingIndicator } from "@neuron/ui";
import { useForm } from "react-hook-form";
import { useState } from "react";

const ContextAwareLoading = () => {
  const { control } = useForm();
  const [isLoading, setIsLoading] = useState(false);
  const [hasError, setHasError] = useState(false);

  return (
    <Form.Input
      name="email"
      control={control}
      labelText="Email Address"
      placeholder="Enter email..."
      suffix={
        isLoading ? <LoadingIndicator color={hasError ? "var(--colorDanger)" : "var(--colorPrimary)"} /> : undefined
      }
    />
  );
};
```

## Step 3: Real-World Integration Patterns

### 3.1 AutoComplete with Loading

```tsx
import { Form, LoadingIndicator } from "@neuron/ui";
import { useForm } from "react-hook-form";
import { useState } from "react";

const AutoCompleteWithLoading = () => {
  const { control } = useForm();
  const [isLoadingSuggestions, setIsLoadingSuggestions] = useState(false);
  const [suggestions, setSuggestions] = useState([]);

  const loadSuggestions = async (query: string) => {
    if (query.length < 2) return;

    setIsLoadingSuggestions(true);
    try {
      const response = await fetch(`/api/suggestions?q=${query}`);
      const data = await response.json();
      setSuggestions(data);
    } finally {
      setIsLoadingSuggestions(false);
    }
  };

  return (
    <Form.AutoComplete
      name="location"
      control={control}
      labelText="Location"
      placeholder="Start typing location..."
      suggestions={suggestions}
      suffix={isLoadingSuggestions ? <LoadingIndicator /> : undefined}
      onInputChange={loadSuggestions}
    />
  );
};
```

### 3.2 Select with Dynamic Options Loading

```tsx
import { Form, LoadingIndicator } from "@neuron/ui";
import { useForm } from "react-hook-form";
import { useState, useEffect } from "react";

const DynamicSelect = () => {
  const { control } = useForm();
  const [isLoadingOptions, setIsLoadingOptions] = useState(false);
  const [options, setOptions] = useState([]);

  useEffect(() => {
    const loadOptions = async () => {
      setIsLoadingOptions(true);
      try {
        const response = await fetch("/api/categories");
        const data = await response.json();
        setOptions(data);
      } finally {
        setIsLoadingOptions(false);
      }
    };

    loadOptions();
  }, []);

  return (
    <Form.Select
      name="category"
      control={control}
      labelText="Category"
      placeholder="Select category..."
      options={options}
      suffix={isLoadingOptions ? <LoadingIndicator /> : undefined}
      disabled={isLoadingOptions}
    />
  );
};
```

### 3.3 Multi-Step Form with Loading States

```tsx
import { Form, LoadingIndicator, Button } from "@neuron/ui";
import { useForm } from "react-hook-form";
import { useState } from "react";

const MultiStepFormWithLoading = () => {
  const { control, handleSubmit } = useForm();
  const [loadingStates, setLoadingStates] = useState({
    email: false,
    phone: false,
    address: false,
  });

  const validateField = async (fieldName: string, value: string) => {
    setLoadingStates((prev) => ({ ...prev, [fieldName]: true }));
    try {
      await fetch(`/api/validate/${fieldName}`, {
        method: "POST",
        body: JSON.stringify({ value }),
      });
    } finally {
      setLoadingStates((prev) => ({ ...prev, [fieldName]: false }));
    }
  };

  return (
    <form>
      <Form.Input
        name="email"
        control={control}
        labelText="Email Address"
        placeholder="Enter email..."
        suffix={loadingStates.email ? <LoadingIndicator /> : undefined}
        onBlur={(e) => validateField("email", e.target.value)}
      />

      <Form.Input
        name="phone"
        control={control}
        labelText="Phone Number"
        placeholder="Enter phone..."
        suffix={loadingStates.phone ? <LoadingIndicator /> : undefined}
        onBlur={(e) => validateField("phone", e.target.value)}
      />

      <Form.Input
        name="address"
        control={control}
        labelText="Address"
        placeholder="Enter address..."
        suffix={loadingStates.address ? <LoadingIndicator /> : undefined}
        onBlur={(e) => validateField("address", e.target.value)}
      />
    </form>
  );
};
```

## Step 4: LoadingIndicator Props Reference

### 4.1 Core LoadingIndicator Props

| Prop  | Type     | Default                  | Description                    |
| ----- | -------- | ------------------------ | ------------------------------ |
| color | `string` | `"var(--colorTextBase)"` | Color of the loading indicator |

### 4.2 Common Color Values

| Color Token            | Usage           | Description          |
| ---------------------- | --------------- | -------------------- |
| `var(--colorTextBase)` | Default         | Standard text color  |
| `var(--colorPrimary)`  | Primary actions | Brand primary color  |
| `var(--colorSuccess)`  | Success states  | Green success color  |
| `var(--colorWarning)`  | Warning states  | Orange warning color |
| `var(--colorDanger)`   | Error states    | Red error color      |
| `var(--colorInfo)`     | Info states     | Blue info color      |

## Step 5: Best Practices

### 5.1 When to Use LoadingIndicator

**Use LoadingIndicator for:**

- Form input suffix loading states
- Data validation in progress
- API request waiting states
- Small loading animations in tight spaces
- Real-time search or autocomplete loading

```tsx
{
  /* Good: Form input loading during validation */
}
<Form.Input name="field" suffix={isValidating ? <LoadingIndicator /> : undefined} />;

{
  /* Good: Search loading state */
}
<Form.Search name="search" suffix={isSearching ? <LoadingIndicator /> : undefined} />;
```

**Don't use LoadingIndicator for:**

- Page-level loading (use Layout skeletons)
- Large content areas (use Spinner or Skeleton)
- Button loading states (use Button loading prop)
- Full-screen loading overlays

### 5.2 Loading State Management

Always manage loading states properly:

```tsx
// Good: Proper state management
const [isLoading, setIsLoading] = useState(false);

const handleAsyncOperation = async () => {
  setIsLoading(true);
  try {
    await apiCall();
  } finally {
    setIsLoading(false); // Always reset in finally
  }
};

// Good: Conditional rendering
suffix={isLoading ? <LoadingIndicator /> : undefined}
```

### 5.3 Color Guidelines

- Use default color for most cases
- Use theme colors for semantic states
- Ensure sufficient contrast
- Match the context (error states use danger color)

```tsx
{
  /* Good: Default color */
}
<LoadingIndicator />;

{
  /* Good: Context-appropriate color */
}
<LoadingIndicator color="var(--colorPrimary)" />;

{
  /* Good: Error state color */
}
<LoadingIndicator color="var(--colorDanger)" />;
```

### 5.4 Accessibility Considerations

- LoadingIndicator is purely visual
- Consider adding aria-live regions for screen readers
- Disable form fields during loading when appropriate

```tsx
{
  /* Good: Accessible loading state */
}
<Form.Input
  name="username"
  suffix={isValidating ? <LoadingIndicator /> : undefined}
  disabled={isValidating}
  aria-describedby={isValidating ? "username-loading" : undefined}
/>;
{
  isValidating && (
    <div id="username-loading" aria-live="polite" className="sr-only">
      Validating username...
    </div>
  );
}
```

## Step 6: Common Mistakes to Avoid

### 6.1 Don't Use for Wrong Contexts

```tsx
{
  /* ❌ INCORRECT: Page-level loading */
}
<div className="page-loading">
  <LoadingIndicator />
</div>;

{
  /* ✅ CORRECT: Use Layout skeleton for page loading */
}
<Layout.Skeleton />;
```

### 6.2 Don't Forget State Management

```tsx
{
  /* ❌ INCORRECT: No loading state management */
}
<Form.Input
  name="field"
  suffix={<LoadingIndicator />} // Always showing
/>;

{
  /* ✅ CORRECT: Conditional loading state */
}
<Form.Input name="field" suffix={isLoading ? <LoadingIndicator /> : undefined} />;
```

### 6.3 Don't Use Inappropriate Colors

```tsx
{
  /* ❌ INCORRECT: Random colors */
}
<LoadingIndicator color="#rainbow" />;

{
  /* ✅ CORRECT: Theme-appropriate colors */
}
<LoadingIndicator color="var(--colorPrimary)" />;
```

### 6.4 Don't Forget Error Handling

```tsx
{
  /* ❌ INCORRECT: No error handling */
}
const handleValidation = async () => {
  setIsLoading(true);
  await apiCall(); // What if this fails?
  setIsLoading(false);
};

{
  /* ✅ CORRECT: Proper error handling */
}
const handleValidation = async () => {
  setIsLoading(true);
  try {
    await apiCall();
  } catch (error) {
    // Handle error
  } finally {
    setIsLoading(false);
  }
};
```

## Key Takeaways

The LoadingIndicator component provides a simple, lightweight loading animation for form inputs:

1. **Primary use case** - Form component suffix properties for data loading states
2. **Customizable color** - Use CSS custom properties or theme colors
3. **Minimal footprint** - Lightweight and performant
4. **Form integration** - Designed specifically for form input loading states
5. **State management** - Always manage loading states properly with try/finally
6. **Accessibility** - Consider screen reader users with proper ARIA attributes
7. **Context-appropriate** - Use semantic colors for different states
8. **Error handling** - Always handle errors and reset loading states

By following these guidelines, you'll use LoadingIndicator appropriately for small loading states in form components, providing users with clear feedback during data requests and validation processes.

---
agent: agent
---

# AI-Assisted Neuron Spinner Component Integration Guide

## For the AI Assistant

Your task is to integrate and configure the Neuron Spinner components in a React application. This guide provides instructions for implementing the Spinner component for specific operations while Layout skeletons are preferred for general content loading.

## Sync Metadata

- **Component Version:** v3.0.1
- **Component Source:** `packages/neuron/ui/src/lib/misc/spinner/Spinner.tsx`
- **Guideline Command:** `/neuron-ui-spinner`
- **Related Skill:** `neuron-ui-misc`

## Introduction

The Spinner component provides loading feedback for specific operations like form submissions and modal overlays.

### When to Use Spinner vs Layout Skeletons

**⚠️ CRITICAL: Layout Skeletons are the preferred approach for content loading**

**Use Layout Skeletons for:**

- Page content loading
- Main content areas
- Sidebar content
- General data fetching

**Use Spinner for:**

- Form submissions
- Modal/overlay loading
- Specific button actions
- File uploads
- Legacy implementations

## Step 1: Basic Spinner Implementation

### 1.1 Import the Spinner Component

```tsx
import { Spinner } from "@neuron/ui";
```

### 1.2 Basic Usage

```tsx
import { Spinner } from "@neuron/ui";
import { useState } from "react";

const MyComponent = () => {
  const [isLoading, setIsLoading] = useState(false);

  return (
    <Spinner show={isLoading}>
      <div>Content shown when not loading</div>
    </Spinner>
  );
};
```

## Step 2: Form Submission Loading

### 2.1 Form Processing Spinner

```tsx
import { Spinner, Panel } from "@neuron/ui";
import { useState } from "react";

const FormWithSpinner = () => {
  const [isSubmitting, setIsSubmitting] = useState(false);

  const handleSubmit = async (data) => {
    setIsSubmitting(true);
    try {
      await submitFormData(data);
    } finally {
      setIsSubmitting(false);
    }
  };

  return (
    <>
      <Spinner show={isSubmitting} />

      <Panel title="User Form" variant="outline">
        <form onSubmit={handleSubmit}>
          <button type="submit" disabled={isSubmitting}>
            {isSubmitting ? "Submitting..." : "Submit"}
          </button>
        </form>
      </Panel>
    </>
  );
};
```

### 2.2 Redux Integration Pattern

```tsx
import { Spinner, Panel, MessageBox } from "@neuron/ui";
import { useAppDispatch, useAppSelector } from "app/store/hooks";

const UserCreationForm = () => {
  const dispatch = useAppDispatch();
  const userCreationProcessing = useAppSelector(getUserCreationProcessing);
  const userCreationSuccess = useIsSuccess(useAppSelector(getUserCreationState));
  const userCreationError = useIsError(useAppSelector(getUserCreationState));

  const handleCreateUser = (userData) => {
    dispatch(userCreate(userData));
  };

  return (
    <>
      <Spinner show={userCreationProcessing} />

      {userCreationError && <MessageBox variant="danger" content="User creation failed." />}

      {userCreationSuccess && <MessageBox variant="success" content="User created successfully!" />}

      <Panel title="Create New User" variant="outline">
        <UserForm onSubmit={handleCreateUser} />
      </Panel>
    </>
  );
};
```

## Step 3: Overlay Mode

### 3.1 Cover Overlay

Use cover mode to overlay the spinner on parent content:

```tsx
import { Spinner } from "@neuron/ui";
import { useState } from "react";

const OverlaySpinner = () => {
  const [isLoading, setIsLoading] = useState(false);

  return (
    <div style={{ position: "relative", minHeight: "200px" }}>
      <Spinner show={isLoading} cover={true}>
        <div>
          <h3>Content Title</h3>
          <p>This content will be covered by the spinner overlay.</p>
          <button onClick={() => setIsLoading(!isLoading)}>Toggle Loading</button>
        </div>
      </Spinner>
    </div>
  );
};
```

## Step 4: Migration from Spinner to Layout Skeletons

### 4.1 Before: Using Spinner for Page Loading

```tsx
// ❌ LEGACY: Using Spinner for page content loading
import { Spinner } from "@neuron/ui";

const LegacyPageComponent = () => {
  const [isLoading, setIsLoading] = useState(true);

  return (
    <Spinner show={isLoading}>
      <div>
        <h1>Page Title</h1>
        <p>Page content</p>
      </div>
    </Spinner>
  );
};
```

### 4.2 After: Using Layout Skeletons

```tsx
// ✅ PREFERRED: Using Layout skeletons for page content loading
import { Layout } from "@neuron/ui";

const ModernPageComponent = () => {
  const [isLoading, setIsLoading] = useState(true);

  return (
    <Layout isMainContentLoading={isLoading}>
      <div>
        <h1>Page Title</h1>
        <p>Page content</p>
      </div>
    </Layout>
  );
};
```

## Step 5: Spinner Props Reference

### 5.1 Core Spinner Props

| Prop     | Type        | Default | Description                           |
| -------- | ----------- | ------- | ------------------------------------- |
| show     | `boolean`   | -       | Whether to display spinner (required) |
| children | `ReactNode` | -       | Content shown when spinner is hidden  |
| cover    | `boolean`   | `false` | Show overlay covering parent element  |

### 5.2 Cover Mode Requirements

When using `cover={true}`:

- Parent element must have `position: relative`
- Spinner creates an overlay covering the entire parent
- Spinner is centered in the overlay

## Step 6: Best Practices

### 6.1 When to Use Spinner

**✅ Use Spinner for:**

- Form submissions and processing
- Modal/overlay loading states
- Specific button actions
- File uploads and downloads

**❌ Don't use Spinner for:**

- Page content loading (use Layout skeletons)
- Main content areas (use Layout skeletons)
- General data fetching (use Layout skeletons)

### 6.2 Loading State Management

- Always provide user feedback during loading
- Disable interactive elements during loading
- Use descriptive loading text when possible

```tsx
{
  /* Good: Complete loading state management */
}
<Spinner show={isSubmitting}>
  <form onSubmit={handleSubmit}>
    <button type="submit" disabled={isSubmitting}>
      {isSubmitting ? "Submitting..." : "Submit"}
    </button>
  </form>
</Spinner>;
```

## Step 7: Common Mistakes to Avoid

### 7.1 Don't Use Spinner for Page Content

```tsx
{
  /* ❌ INCORRECT: Using Spinner for main content */
}
<Spinner show={isPageLoading}>
  <div>
    <h1>Page Title</h1>
    <div>Main page content</div>
  </div>
</Spinner>;

{
  /* ✅ CORRECT: Use Layout skeletons */
}
<Layout isMainContentLoading={isPageLoading}>
  <div>
    <h1>Page Title</h1>
    <div>Main page content</div>
  </div>
</Layout>;
```

### 7.2 Don't Forget Cover Mode Requirements

```tsx
{
  /* ❌ INCORRECT: Cover mode without relative positioning */
}
<div>
  <Spinner show={isLoading} cover={true}>
    <div>Content</div>
  </Spinner>
</div>;

{
  /* ✅ CORRECT: Proper positioning for cover mode */
}
<div style={{ position: "relative", minHeight: "100px" }}>
  <Spinner show={isLoading} cover={true}>
    <div>Content</div>
  </Spinner>
</div>;
```

## Key Takeaways

The Neuron Spinner component provides specialized loading feedback for specific operations. Key points to remember:

1. **Layout skeletons are preferred** for general content loading
2. **Use Spinner for specific operations** like form submissions and modals
3. **Cover mode requires** proper parent positioning with `position: relative`
4. **Always provide user feedback** during loading operations
5. **Disable interactive elements** during loading states
6. **Consider migration** from Spinner to Layout skeletons for page content

By following these guidelines, you'll create appropriate loading experiences that use Spinner for specific operations while leveraging Layout skeletons for general content loading.

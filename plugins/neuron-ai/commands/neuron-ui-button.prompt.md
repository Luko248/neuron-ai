---
agent: agent
---

# AI-Assisted Neuron Button Component Integration Guide

## For the AI Assistant

Your task is to integrate and configure the Neuron Button components in a React application. This guide provides comprehensive instructions for implementing Button and SubmitButton components, which serve as the foundation for user interactions across all Neuron applications.

## Sync Metadata

- **Component Version:** v4.1.2
- **Component Source:** `packages/neuron/ui/src/lib/buttons/button/Button.tsx`
- **Guideline Command:** `/neuron-ui-button`
- **Related Skill:** `neuron-ui-buttons`

## Introduction

The Button system is a core part of the Neuron UI framework, designed to create consistent, accessible, and interactive user interface elements across all Neuron applications.

### What is the Button System?

The Button component provides standardized interactive elements for your application with support for:

- Multiple variants (primary, secondary, plain, info, success, warning, danger)
- Different sizes (small, medium, large)
- Automatic loading states with API state integration
- Icon support (left and right positioning, icon-only buttons)
- Access control integration for permission-based UI
- Built-in tooltip functionality
- Disabled state handling
- Form submission with SubmitButton variant

### Key Features

- **Consistent Styling**: Standardized button appearance across applications
- **Access Control**: Built-in readonly and full access permission handling
- **Automatic Loading States**: Loading indication based on API fetching states
- **Manual Loading Control**: Override loading state when needed
- **Icon Integration**: Support for baseIcons from the Neuron icon system
- **Tooltip Support**: Built-in tooltip functionality with placement options
- **Disabled State**: Proper disabled state handling with visual feedback
- **Click Prevention**: Automatic click prevention during loading states
- **Link Rendering**: Supports `href`, `to`, and `target` for anchor or React Router links
- **TypeScript Support**: Full type safety with comprehensive prop interfaces
- **Accessibility**: Built-in accessibility features and ARIA support

**Storybook Reference:** You can refer to the Storybook documentation for visual examples and interactive demos of the Button component.

### Figma Code Connect Integration

**🎨 Design-to-Code Connection Available**

The Button component has **full Figma Code Connect integration**, enabling direct design-to-code generation from Figma designs.

**Key Features:**

- **Automatic Code Generation**: Figma MCP tools can generate accurate Button component code
- **Complete Variant Support**: All button variants, sizes, and states are mapped
- **Icon Configuration**: Proper handling of left icons, right icons, and icon-only buttons
- **State Management**: Disabled, active, and loading states are correctly mapped
- **Tooltip Integration**: Tooltip configurations are automatically included

**Code Connect Mappings:**

- **Variants**: primary, secondary, plain, success, danger, warning, info
- **Sizes**: small, medium, large
- **States**: disabled, active, loading
- **Icons**: left icon, right icon, icon-only configurations
- **Features**: tooltip support, text content mapping

**Usage with Figma MCP:**

1. Use `mcp4_get_code` with Button component node-id from Figma
2. Generated code will use proper `@neuron/ui/Button` component structure
3. All props will be correctly mapped from design specifications
4. No manual interpretation required for standard button configurations

**Figma Design System Reference:**

- Node ID: `2116-89750` (VIGo Design System)
- All button variants and configurations are connected
- Direct code generation available through Figma MCP integration

## Step 1: Basic Button Implementation

### 1.1 Import the Button Component

```tsx
import { Button } from "@neuron/ui";
```

### 1.2 Basic Usage

Here's a simple implementation of the Button component:

```tsx
import { Button } from "@neuron/ui";

const MyComponent = () => {
  const handleClick = () => {
    console.info("Button clicked!");
  };

  return <Button onClick={handleClick}>Click Me</Button>;
};
```

### 1.3 Link Buttons (Anchor and Router Links)

Buttons can behave as links using `href` (HTML anchor) or `to` (React Router). Use `target` with `href` or `to` when needed.

```tsx
import { Button, baseIcons } from "@neuron/ui";

const LinkButtons = () => {
  return (
    <div className="button-links">
      <Button href="https://www.google.com" target="_blank" iconRight={baseIcons.arrowUpRightFromSquareSolid}>
        Open Google
      </Button>
      <Button to="/dashboard" iconRight={baseIcons.chevronRightSolid}>
        Navigate with Router
      </Button>
      <Button to="/dashboard" target="_blank" iconRight={baseIcons.arrowUpRightFromSquareSolid}>
        Open Dashboard
      </Button>
    </div>
  );
};
```

### 1.3 Button Variants

The Button component supports multiple variants for different use cases:

```tsx
import { Button } from "@neuron/ui";

const ButtonVariants = () => {
  return (
    <div className="button-variants">
      {/* Primary button - main actions */}
      <Button variant="primary">Primary</Button>

      {/* Secondary button - alternative actions */}
      <Button variant="secondary">Secondary</Button>

      {/* Plain button - minimal styling */}
      <Button variant="plain">Plain</Button>

      {/* Status-based buttons */}
      <Button variant="info">Info</Button>
      <Button variant="success">Success</Button>
      <Button variant="warning">Warning</Button>
      <Button variant="danger">Danger</Button>
    </div>
  );
};
```

## Step 2: Button Sizes and Icon Integration

### 2.1 Button Sizes

Choose the appropriate size based on your UI hierarchy:

```tsx
import { Button } from "@neuron/ui";

const ButtonSizes = () => {
  return (
    <div className="button-sizes">
      <Button size="small">Small</Button>
      <Button size="medium">Medium</Button> {/* Default */}
      <Button size="large">Large</Button>
    </div>
  );
};
```

### 2.2 Icon Integration

Add icons to enhance button functionality and visual appeal using baseIcons:

```tsx
import { Button, baseIcons } from "@neuron/ui";

const IconButtons = () => {
  return (
    <div className="icon-buttons">
      {/* Icon on the left */}
      <Button iconLeft={baseIcons.circlePlusSolid}>Add Item</Button>

      {/* Icon on the right */}
      <Button iconRight={baseIcons.floppyDiskSolid}>Save Changes</Button>

      {/* Icon only button - always use iconLeft */}
      <Button iconLeft={baseIcons.circleInfoSolid} />

      {/* Both icons */}
      <Button iconLeft={baseIcons.trashCanSolid} iconRight={baseIcons.circlePlusSolid}>
        Replace
      </Button>
    </div>
  );
};
```

### 2.3 Icon-Only Buttons

For icon-only buttons, always use `iconLeft` for consistency:

```tsx
import { Button, baseIcons } from "@neuron/ui";

const IconOnlyButtons = () => {
  return (
    <div className="icon-only-buttons">
      {/* Standard icon-only button */}
      <Button iconLeft={baseIcons.circleInfoSolid} />

      {/* Different variants */}
      <Button variant="secondary" iconLeft={baseIcons.circleInfoSolid} />
      <Button variant="plain" iconLeft={baseIcons.circleInfoSolid} />

      {/* Different sizes */}
      <Button size="small" iconLeft={baseIcons.circleInfoSolid} />
      <Button size="large" iconLeft={baseIcons.circleInfoSolid} />
    </div>
  );
};
```

## Step 3: Loading States and Disabled Functionality

### 3.1 Automatic Loading States with API Integration

The Button component automatically handles loading states when integrated with API calls. The loading state is determined by the `fetchingState` from the API state:

```tsx
import { Button } from "@neuron/ui";
import { useApiCall } from "@neuron/core";

const ApiButton = () => {
  const [fetchData, apiState] = useApiCall(api.getData);

  return (
    <Button apiState={apiState} onClick={fetchData}>
      Fetch Data
    </Button>
  );
};
```

**Important**: When a button is in loading state, it automatically becomes unclickable and shows a loading indicator.

### 3.2 Manual Loading State Control

You can override the automatic loading state by setting the `loading` prop manually:

```tsx
import { Button } from "@neuron/ui";
import { useState } from "react";

const ManualLoadingButton = () => {
  const [isLoading, setIsLoading] = useState(false);

  const handleAsyncAction = async () => {
    setIsLoading(true);
    try {
      await new Promise((resolve) => setTimeout(resolve, 2000));
      console.info("Action completed");
    } finally {
      setIsLoading(false);
    }
  };

  return (
    <Button loading={isLoading} onClick={handleAsyncAction}>
      Process Data
    </Button>
  );
};
```

### 3.3 Loading States with Different Variants

Loading states work with all button variants:

```tsx
import { Button } from "@neuron/ui";

const LoadingVariants = () => {
  return (
    <div className="loading-variants">
      <Button loading={true}>Primary Loading</Button>
      <Button loading={true} variant="secondary">
        Secondary Loading
      </Button>
      <Button loading={true} variant="plain">
        Plain Loading
      </Button>
    </div>
  );
};
```

### 3.4 Disabled State

Use the `disabled` prop to make buttons unclickable with visual feedback:

```tsx
import { Button } from "@neuron/ui";

const DisabledButtons = () => {
  return (
    <div className="disabled-buttons">
      <Button disabled>Disabled Button</Button>
      <Button disabled iconLeft={baseIcons.circleInfoSolid} />
    </div>
  );
};
```

**Note**: Disabled buttons are visually distinguished and cannot be clicked, but unlike loading buttons, they don't show a loading indicator.

## Step 4: Tooltip Integration

### 4.1 Basic Tooltip Usage

Add helpful tooltips to provide additional context or information:

```tsx
import { Button } from "@neuron/ui";

const TooltipButtons = () => {
  return (
    <div className="tooltip-buttons">
      {/* Simple string tooltip */}
      <Button tooltip="This button performs an important action">Action Button</Button>

      {/* Tooltip for icon-only button */}
      <Button iconLeft={baseIcons.circleInfoSolid} tooltip="Information" />
    </div>
  );
};
```

### 4.2 Advanced Tooltip Configuration

Use tooltip objects for more control over placement and content:

```tsx
import { Button, baseIcons } from "@neuron/ui";

const AdvancedTooltips = () => {
  return (
    <div className="advanced-tooltips">
      <Button
        tooltip={{
          text: "Save your changes to the database",
          placement: "top",
        }}
        iconLeft={baseIcons.floppyDiskSolid}
      >
        Save
      </Button>

      <Button
        tooltip={{
          text: "Delete this item permanently",
          placement: "right",
        }}
        variant="danger"
      >
        Delete
      </Button>
    </div>
  );
};
```

**Tooltip Placement Options**: `top`, `bottom`, `left`, `right`, `auto`

## Step 5: Access Control Integration

### 5.1 Permission-Based Button Visibility

Control button accessibility based on user permissions:

```tsx
import { Button } from "@neuron/ui";

const AccessControlButtons = () => {
  return (
    <div className="access-control-buttons">
      {/* Button with readonly access - shows but disabled */}
      <Button readonlyAccess={["viewer"]}>Edit Item</Button>

      {/* Button with full access control */}
      <Button fullAccess={["admin", "editor"]} readonlyAccess={["viewer"]} variant="danger">
        Delete Item
      </Button>
    </div>
  );
};
```

### 5.2 Access Control Behavior

- **fullAccess**: Array of roles that have full access to the button
- **readonlyAccess**: Array of roles that can see the button but it's disabled
- Users without matching roles won't see the button at all

## Step 6: SubmitButton for Form Integration

### 6.1 Basic SubmitButton Usage

Use SubmitButton for form submissions with automatic loading state management:

```tsx
import { SubmitButton } from "@neuron/ui";
import { useForm } from "react-hook-form";

const MyForm = () => {
  const { control, handleSubmit } = useForm();

  const onSubmit = async (data) => {
    // Submit form data
    await submitFormData(data);
  };

  return (
    <form onSubmit={handleSubmit(onSubmit)}>
      {/* Form fields here */}

      <SubmitButton control={control}>Submit Form</SubmitButton>
    </form>
  );
};
```

### 6.2 SubmitButton Features

The SubmitButton automatically handles:

- Loading state during form submission
- Disabled state based on form validation
- Integration with React Hook Form

```tsx
import { SubmitButton, baseIcons } from "@neuron/ui";
import { useForm } from "react-hook-form";

const FormExample = () => {
  const { control, handleSubmit } = useForm();

  const onSubmit = async (data) => {
    await api.submitForm(data);
  };

  return (
    <form onSubmit={handleSubmit(onSubmit)}>
      {/* Form fields */}

      <SubmitButton control={control} variant="success" iconLeft={baseIcons.floppyDiskSolid}>
        Save Changes
      </SubmitButton>
    </form>
  );
};
```

## Step 7: Special Button Features

### 6.1 Core Button Props

| Prop      | Type                                                                                  | Default     | Description                |
| --------- | ------------------------------------------------------------------------------------- | ----------- | -------------------------- |
| variant   | `"primary" \| "secondary" \| "plain" \| "info" \| "success" \| "warning" \| "danger"` | `"primary"` | Button visual style        |
| size      | `"small" \| "medium" \| "large"`                                                      | `"medium"`  | Button size                |
| type      | `"button" \| "submit"`                                                                | `"button"`  | HTML button type           |
| children  | `ReactNode`                                                                           | -           | Button content/text        |
| className | `string`                                                                              | -           | Additional CSS classes     |
| disabled  | `boolean`                                                                             | `false`     | Disable button interaction |

### 6.2 Icon Props

| Prop      | Type                           | Default | Description                 |
| --------- | ------------------------------ | ------- | --------------------------- |
| iconLeft  | `IconDefinition \| TBaseIcons` | -       | Icon displayed on the left  |
| iconRight | `IconDefinition \| TBaseIcons` | -       | Icon displayed on the right |

### 6.3 Loading and State Props

| Prop       | Type        | Default | Description                     |
| ---------- | ----------- | ------- | ------------------------------- |
| loading    | `boolean`   | `false` | Manual loading state            |
| apiState   | `IApiState` | -       | API state for automatic loading |
| active     | `boolean`   | `false` | Simulate :active CSS state      |
| fitContent | `boolean`   | `false` | Button width fits content       |

### 6.4 Access Control Props

| Prop           | Type       | Default | Description                |
| -------------- | ---------- | ------- | -------------------------- |
| readonlyAccess | `string[]` | -       | Roles with readonly access |
| fullAccess     | `string[]` | -       | Roles with full access     |

### 6.5 Tooltip Props

| Prop    | Type                      | Default | Description                   |
| ------- | ------------------------- | ------- | ----------------------------- |
| tooltip | `string \| TooltipConfig` | -       | Tooltip text or configuration |

### 6.6 SubmitButton Additional Props

| Prop    | Type                              | Default | Description                    |
| ------- | --------------------------------- | ------- | ------------------------------ |
| control | `Control<TFieldValues, TContext>` | -       | React Hook Form control object |

## Step 7: Best Practices

### 7.1 When to Use Each Variant

**Primary Buttons:**

- Main call-to-action buttons
- Form submission buttons
- Primary navigation actions

```tsx
<Button variant="primary">Save Changes</Button>
<Button variant="primary">Create Account</Button>
```

**Secondary Buttons:**

- Alternative actions
- Cancel buttons
- Secondary navigation

```tsx
<Button variant="secondary">Cancel</Button>
<Button variant="secondary">View Details</Button>
```

**Status Buttons:**

- Actions with semantic meaning
- State-based operations

```tsx
<Button variant="success">Approve</Button>
<Button variant="warning">Warning Action</Button>
<Button variant="danger">Delete</Button>
```

### 7.2 Icon Usage Guidelines

- Use icons to enhance button meaning, not replace text
- Prefer left icons for action buttons
- Use icon-only buttons sparingly and with tooltips
- Ensure icons are semantically meaningful

```tsx
{
  /* Good: Icon enhances the action */
}
<Button iconLeft={faPlus}>Add User</Button>;

{
  /* Good: Icon-only with tooltip */
}
<Button iconLeft={faEdit} tooltip="Edit item" />;

{
  /* Avoid: Icon doesn't match action */
}
<Button iconLeft={faTrash}>Save</Button>;
```

### 7.3 Loading State Best Practices

- Always provide feedback during async operations
- Use descriptive loading text when possible
- Prefer API state integration for automatic handling

```tsx
{
  /* Good: Clear loading feedback */
}
<Button loading={isLoading} onClick={handleSave}>
  {isLoading ? "Saving..." : "Save Changes"}
</Button>;

{
  /* Better: Automatic API integration */
}
<Button apiState={saveApiState} onClick={handleSave}>
  Save Changes
</Button>;
```

### 7.4 Accessibility Considerations

- Always provide meaningful button text or aria-labels
- Use proper semantic HTML button types
- Ensure sufficient color contrast
- Provide keyboard navigation support

```tsx
{
  /* Good: Descriptive text */
}
<Button>Delete User Account</Button>;

{
  /* Good: Icon-only with aria-label via tooltip */
}
<Button iconLeft={faTrash} tooltip="Delete item" />;

{
  /* Good: Proper form submission */
}
<SubmitButton control={control}>Submit Application</SubmitButton>;
```

## Step 8: Common Patterns and Examples

### 8.1 Action Button Groups

```tsx
import { Button, baseIcons } from "@neuron/ui";

const ActionButtonGroup = () => {
  return (
    <div className="action-group">
      <Button variant="primary" iconLeft={baseIcons.floppyDiskSolid}>
        Save
      </Button>
      <Button variant="secondary">Cancel</Button>
      <Button variant="danger" iconLeft={baseIcons.trashCanSolid}>
        Delete
      </Button>
    </div>
  );
};
```

### 8.2 Form Action Buttons

```tsx
import { Button, SubmitButton, baseIcons } from "@neuron/ui";
import { useForm } from "react-hook-form";

const FormWithActions = () => {
  const { control, handleSubmit, reset } = useForm();

  const onSubmit = async (data) => {
    await api.saveData(data);
  };

  const handleReset = () => {
    reset();
  };

  return (
    <form onSubmit={handleSubmit(onSubmit)}>
      {/* Form fields */}

      <div className="form-actions">
        <SubmitButton control={control} variant="primary" iconLeft={baseIcons.floppyDiskSolid}>
          Save Changes
        </SubmitButton>

        <Button type="button" variant="secondary" onClick={handleReset}>
          Reset Form
        </Button>
      </div>
    </form>
  );
};
```

### 8.3 Conditional Button Rendering

```tsx
import { Button, baseIcons } from "@neuron/ui";

const ConditionalButtons = ({ userRole, isEditing, onEdit, onSave, onCancel }) => {
  return (
    <div className="conditional-buttons">
      {userRole === "admin" && (
        <Button variant="danger" iconLeft={baseIcons.trashCanSolid} fullAccess={["admin"]}>
          Delete
        </Button>
      )}

      {isEditing ? (
        <>
          <Button variant="primary" onClick={onSave}>
            Save
          </Button>
          <Button variant="secondary" onClick={onCancel}>
            Cancel
          </Button>
        </>
      ) : (
        <Button variant="secondary" onClick={onEdit}>
          Edit
        </Button>
      )}
    </div>
  );
};
```

### 8.4 API Integration Pattern

```tsx
import { Button, baseIcons } from "@neuron/ui";
import { useApiCall } from "@neuron/core";

const ApiIntegratedButton = () => {
  const [fetchData, apiState] = useApiCall(api.getData);

  return (
    <Button apiState={apiState} onClick={fetchData} variant="primary" iconLeft={baseIcons.downloadSolid}>
      {apiState.fetchingState === "fetching" ? "Loading..." : "Fetch Data"}
    </Button>
  );
};
```

## Step 9: Common Mistakes to Avoid

### 9.1 Don't Override Button Types Incorrectly

```tsx
{
  /* Wrong: Submit button without form context */
}
<Button type="submit" onClick={handleAction}>
  Submit
</Button>;

{
  /* Right: Use SubmitButton for form submissions */
}
<SubmitButton control={control}>Submit</SubmitButton>;

{
  /* Right: Use button type for non-form actions */
}
<Button type="button" onClick={handleAction}>
  Action
</Button>;
```

### 9.2 Don't Ignore Loading States

```tsx
{
  /* Wrong: No loading feedback */
}
<Button onClick={async () => await longRunningOperation()}>Process</Button>;

{
  /* Right: Provide loading feedback */
}
<Button loading={isLoading} onClick={handleAsyncOperation}>
  {isLoading ? "Processing..." : "Process"}
</Button>;
```

### 9.3 Don't Mix Button Variants Incorrectly

```tsx
{
  /* Wrong: Dangerous action with success variant */
}
<Button variant="success" onClick={deleteUser}>
  Delete User
</Button>;

{
  /* Right: Match variant to action semantic */
}
<Button variant="danger" onClick={deleteUser}>
  Delete User
</Button>;
```

## Summary

The Neuron Button component system provides a comprehensive, accessible, and consistent foundation for user interactions. Key points to remember:

1. **Use appropriate variants** based on action semantics
2. **Integrate with forms** using SubmitButton for submissions
3. **Provide loading feedback** for async operations
4. **Use access control** to manage user permissions
5. **Add meaningful icons** to enhance usability
6. **Include tooltips** for additional context
7. **Follow accessibility guidelines** for inclusive design

### 7.1 Active State

Simulate the `:active` CSS state for buttons that should appear pressed:

```tsx
import { Button } from "@neuron/ui";

const ActiveButtons = () => {
  return (
    <div className="active-buttons">
      <Button active={true}>Active Button</Button>
      <Button active={false}>Normal Button</Button>
    </div>
  );
};
```

### 7.2 Fit Content Width

Use `fitContent` to make buttons width fit their content instead of having a minimum width:

```tsx
import { Button } from "@neuron/ui";

const FitContentButtons = () => {
  return (
    <div className="fit-content-buttons">
      <Button fitContent={true}>A</Button>
      <Button fitContent={false}>Standard Width</Button>
    </div>
  );
};
```

## Step 8: Button Props Reference

### 8.1 Core Button Props

| Prop      | Type                                                                                  | Default     | Description                |
| --------- | ------------------------------------------------------------------------------------- | ----------- | -------------------------- |
| variant   | `"primary" \| "secondary" \| "plain" \| "info" \| "success" \| "warning" \| "danger"` | `"primary"` | Button visual style        |
| size      | `"small" \| "medium" \| "large"`                                                      | `"medium"`  | Button size                |
| type      | `"button" \| "submit"`                                                                | `"button"`  | HTML button type           |
| children  | `ReactNode`                                                                           | -           | Button content/text        |
| className | `string`                                                                              | -           | Additional CSS classes     |
| disabled  | `boolean`                                                                             | `false`     | Disable button interaction |

### 8.2 Icon Props

| Prop      | Type         | Default | Description                                            |
| --------- | ------------ | ------- | ------------------------------------------------------ |
| iconLeft  | `TBaseIcons` | -       | Icon displayed on the left (use for icon-only buttons) |
| iconRight | `TBaseIcons` | -       | Icon displayed on the right                            |

### 8.3 Loading and State Props

| Prop       | Type        | Default | Description                                  |
| ---------- | ----------- | ------- | -------------------------------------------- |
| loading    | `boolean`   | `false` | Manual loading state override                |
| apiState   | `IApiState` | -       | API state for automatic loading detection    |
| active     | `boolean`   | `false` | Simulate :active CSS state                   |
| fitContent | `boolean`   | `false` | Button width fits content (no minimum width) |

### 8.4 Access Control Props

| Prop           | Type       | Default | Description                                  |
| -------------- | ---------- | ------- | -------------------------------------------- |
| readonlyAccess | `string[]` | -       | Roles with readonly access (button disabled) |
| fullAccess     | `string[]` | -       | Roles with full access                       |

### 8.5 Tooltip Props

| Prop    | Type                      | Default | Description                          |
| ------- | ------------------------- | ------- | ------------------------------------ |
| tooltip | `string \| TooltipConfig` | -       | Tooltip text or configuration object |

### 8.6 SubmitButton Additional Props

| Prop    | Type                              | Default | Description                    |
| ------- | --------------------------------- | ------- | ------------------------------ |
| control | `Control<TFieldValues, TContext>` | -       | React Hook Form control object |

## Step 9: Best Practices

### 9.1 When to Use Each Variant

**Primary Buttons:**

- Main call-to-action buttons
- Form submission buttons
- Primary navigation actions

```tsx
<Button variant="primary">Save Changes</Button>
<Button variant="primary">Create Account</Button>
```

**Secondary Buttons:**

- Alternative actions
- Cancel buttons
- Secondary navigation

```tsx
<Button variant="secondary">Cancel</Button>
<Button variant="secondary">View Details</Button>
```

**Status Buttons:**

- Actions with semantic meaning
- State-based operations

```tsx
<Button variant="success">Approve</Button>
<Button variant="warning">Warning Action</Button>
<Button variant="danger">Delete</Button>
```

### 9.2 Icon Usage Guidelines

- Use icons to enhance button meaning, not replace text
- **Always use `iconLeft` for icon-only buttons** for consistency
- Use icon-only buttons sparingly and with tooltips
- **Always use baseIcons** for consistency across the application
- Ensure icons are semantically meaningful

```tsx
{
  /* Good: Icon enhances the action */
}
<Button iconLeft={baseIcons.circlePlusSolid}>Add User</Button>;

{
  /* Good: Icon-only with tooltip */
}
<Button iconLeft={baseIcons.penSolid} tooltip="Edit item" />;

{
  /* Good: Consistent icon-only usage */
}
<Button iconLeft={baseIcons.circleInfoSolid} tooltip="Information" />;

{
  /* Avoid: Icon doesn't match action */
}
<Button iconLeft={baseIcons.trashCanSolid}>Save</Button>;
```

### 9.3 Loading State Best Practices

- Always provide feedback during async operations
- Prefer API state integration for automatic handling
- Remember that loading buttons are automatically unclickable

```tsx
{
  /* Good: Automatic API integration */
}
<Button apiState={saveApiState} onClick={handleSave}>
  Save Changes
</Button>;

{
  /* Good: Manual loading control */
}
<Button loading={isLoading} onClick={handleAsyncAction}>
  Process Data
</Button>;
```

### 9.4 Tooltip Best Practices

- Always provide tooltips for icon-only buttons
- Use clear, concise tooltip text
- Choose appropriate placement to avoid UI conflicts

```tsx
{
  /* Good: Icon-only with descriptive tooltip */
}
<Button iconLeft={baseIcons.trashCanSolid} tooltip="Delete item permanently" />;

{
  /* Good: Helpful context for complex actions */
}
<Button tooltip="Save changes to the database">Save</Button>;
```

### 9.5 Accessibility Considerations

- Always provide meaningful button text or tooltips for icon-only buttons
- Use proper semantic HTML button types
- Ensure sufficient color contrast
- Use appropriate variants for action semantics

```tsx
{
  /* Good: Descriptive text */
}
<Button>Delete User Account</Button>;

{
  /* Good: Icon-only with accessible tooltip */
}
<Button iconLeft={baseIcons.trashCanSolid} tooltip="Delete item" />;

{
  /* Good: Proper form submission */
}
<SubmitButton control={control}>Submit Application</SubmitButton>;
```

## Step 10: Common Mistakes to Avoid

### 10.1 Don't Override Button Types Incorrectly

```tsx
{
  /* Wrong: Submit button without form context */
}
<Button type="submit" onClick={handleAction}>
  Submit
</Button>;

{
  /* Right: Use SubmitButton for form submissions */
}
<SubmitButton control={control}>Submit</SubmitButton>;

{
  /* Right: Use button type for non-form actions */
}
<Button type="button" onClick={handleAction}>
  Action
</Button>;
```

### 10.2 Don't Ignore Loading States

```tsx
{
  /* Wrong: No loading feedback */
}
<Button onClick={async () => await longRunningOperation()}>Process</Button>;

{
  /* Right: Provide loading feedback */
}
<Button loading={isLoading} onClick={handleAsyncOperation}>
  Process Data
</Button>;
```

### 10.3 Don't Mix Button Variants Incorrectly

```tsx
{
  /* Wrong: Dangerous action with success variant */
}
<Button variant="success" onClick={deleteUser}>
  Delete User
</Button>;

{
  /* Right: Match variant to action semantic */
}
<Button variant="danger" onClick={deleteUser}>
  Delete User
</Button>;
```

### 10.4 Don't Use iconRight for Icon-Only Buttons

```tsx
{
  /* Wrong: Inconsistent icon-only usage */
}
<Button iconRight={baseIcons.circleInfoSolid} />;

{
  /* Right: Always use iconLeft for icon-only buttons */
}
<Button iconLeft={baseIcons.circleInfoSolid} tooltip="Information" />;
```

## Key Takeaways

The Neuron Button component system provides a comprehensive, accessible, and consistent foundation for user interactions. Key points to remember:

1. **Use appropriate variants** based on action semantics
2. **Always use `iconLeft` for icon-only buttons** for consistency
3. **Always use baseIcons** from `@neuron/ui` for icon consistency
4. **Loading states are automatic** with API integration and prevent clicking
5. **Manual loading override** is available when needed
6. **Tooltips are essential** for icon-only buttons
7. **Access control** manages user permissions automatically
8. **Disabled and loading states** provide proper user feedback
9. **Follow accessibility guidelines** for inclusive design

By following these guidelines, you'll create consistent, accessible, and user-friendly button interfaces across your Neuron applications.

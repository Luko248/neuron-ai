---
agent: agent
---

# Neuron SystemButton Component Guidelines

## For the AI Assistant

This document provides guidelines for the Neuron SystemButton component. **IMPORTANT**: SystemButton is primarily a helper component used internally by other Neuron components and should rarely be used directly in application code.

## Sync Metadata

- **Component Version:** v3.0.3
- **Component Source:** `packages/neuron/ui/src/lib/buttons/systemButton/SystemButton.tsx`
- **Guideline Command:** `/neuron-ui-systembutton`
- **Related Skill:** `neuron-ui-buttons`

## Introduction

The SystemButton component is a specialized helper button designed for internal use within Neuron UI components. It provides a consistent, minimal button style specifically for system-level interactions like toggles, triggers, and toolbar actions.

### What is the SystemButton Component?

SystemButton is a lightweight, icon-only button component that serves as a building block for other Neuron components. It provides standardized system-level button functionality with:

- **Icon-only design** - Minimal visual footprint with icon-based communication
- **Tooltip integration** - Built-in accessibility through hover tooltips
- **Size variants** - Small, medium, and large sizes for different contexts
- **State management** - Support for disabled and active states
- **System styling** - Consistent appearance across all Neuron components
- **Accessibility features** - Proper ARIA labeling and keyboard navigation

### Key Features

- **Three Size Variants**: Small, medium (default), and large
- **Icon Integration**: Uses baseIcons system for consistent iconography
- **Tooltip Support**: Built-in tooltip functionality for accessibility
- **State Management**: Disabled and active state support
- **System Styling**: Minimal, consistent appearance
- **Accessibility Compliance**: Proper ARIA attributes and keyboard support

## ⚠️ CRITICAL: Usage Restrictions

**SystemButton is a HELPER COMPONENT - Use only in these specific cases:**

### ✅ WHEN TO USE SystemButton:

1. **Figma Design Explicitly Shows SystemButton**: Only when the design specifically calls for a system-style button
2. **Building Custom Components**: When creating custom components that need system-level button functionality
3. **Matching Existing Patterns**: When you need to match the exact styling of buttons used in QuickActions, FilterHeader, etc.

### ❌ WHEN NOT TO USE SystemButton:

1. **Standard User Actions**: Use regular `Button` component instead
2. **Form Submissions**: Use `Button` with appropriate variants
3. **Navigation**: Use `Link` or `Button` components
4. **Primary Actions**: Use `Button` with primary variant
5. **General UI Interactions**: Use standard `Button` component

### 🔍 SystemButton is Used Internally By:

- **QuickActions**: Trigger button and action buttons
- **FilterHeader**: Collapse/expand toggle
- **SectionHeader**: Collapsible section toggle
- **RichEditor**: Formatting toolbar buttons

## Step 1: Basic SystemButton Implementation (Rare Use Case)

### 1.1 Import the SystemButton Component

```tsx
import { SystemButton, baseIcons, IconSize } from "@neuron/ui";
```

### 1.2 Basic SystemButton Usage

**Only use when explicitly required by design:**

```tsx
import { SystemButton, baseIcons } from "@neuron/ui";

const SystemButtonExample = () => {
  return (
    <SystemButton
      icon={{ iconDef: baseIcons.gearSolid }}
      tooltip="System Settings"
      onClick={() => console.log("System action triggered")}
    />
  );
};
```

### 1.3 SystemButton with Different Sizes

```tsx
import { SystemButton, baseIcons } from "@neuron/ui";

const SystemButtonSizes = () => {
  return (
    <div className="system-button-sizes">
      {/* Small size - for compact interfaces */}
      <SystemButton icon={{ iconDef: baseIcons.chevronDownSolid }} size="small" tooltip="Expand options" />

      {/* Medium size - default */}
      <SystemButton icon={{ iconDef: baseIcons.elipsisVerticalSolid }} size="medium" tooltip="More actions" />

      {/* Large size - for prominent system actions */}
      <SystemButton icon={{ iconDef: baseIcons.circleXmarkRegular }} size="large" tooltip="Close panel" />
    </div>
  );
};
```

## Step 2: SystemButton States and Variants

### 2.1 Disabled SystemButton

```tsx
import { SystemButton, baseIcons } from "@neuron/ui";

const DisabledSystemButton = () => {
  return (
    <SystemButton
      icon={{ iconDef: baseIcons.trashCanSolid }}
      tooltip="Delete (unavailable)"
      disabled={true}
      onClick={() => console.log("This won't fire")}
    />
  );
};
```

### 2.2 Active SystemButton

```tsx
import { SystemButton, baseIcons } from "@neuron/ui";
import { useState } from "react";

const ActiveSystemButton = () => {
  const [isActive, setIsActive] = useState(false);

  return (
    <SystemButton
      icon={{ iconDef: baseIcons.eyeSolid }}
      tooltip={isActive ? "Hide details" : "Show details"}
      active={isActive}
      onClick={() => setIsActive(!isActive)}
    />
  );
};
```

## Step 3: SystemButton Props Reference

### 3.1 Core SystemButton Props

| Prop      | Type                                   | Default    | Required | Description                          |
| --------- | -------------------------------------- | ---------- | -------- | ------------------------------------ |
| icon      | `IconProps`                            | -          | ✅       | Icon to display (must use baseIcons) |
| size      | `"small" \| "medium" \| "large"`       | `"medium"` | ❌       | Size of the button                   |
| tooltip   | `string`                               | -          | ❌       | Tooltip text for accessibility       |
| onClick   | `MouseEventHandler<HTMLButtonElement>` | -          | ❌       | Click event handler                  |
| disabled  | `boolean`                              | `false`    | ❌       | Disable the button                   |
| active    | `boolean`                              | `false`    | ❌       | Active state styling                 |
| className | `string`                               | -          | ❌       | Additional CSS classes               |
| testId    | `string`                               | -          | ❌       | Custom test ID                       |

## Step 4: Common Use Cases (When Appropriate)

### 4.1 Toggle Button Pattern

```tsx
import { SystemButton, baseIcons } from "@neuron/ui";
import { useState } from "react";

const ToggleSystemButton = () => {
  const [isExpanded, setIsExpanded] = useState(false);

  return (
    <SystemButton
      icon={{
        iconDef: isExpanded ? baseIcons.chevronUpSolid : baseIcons.chevronDownSolid,
      }}
      tooltip={isExpanded ? "Collapse" : "Expand"}
      active={isExpanded}
      onClick={() => setIsExpanded(!isExpanded)}
      size="small"
    />
  );
};
```

### 4.2 Toolbar Button Pattern

```tsx
import { SystemButton, baseIcons } from "@neuron/ui";

const ToolbarSystemButtons = () => {
  return (
    <div className="toolbar">
      <SystemButton
        icon={{ iconDef: baseIcons.floppyDiskSolid }}
        tooltip="Save"
        size="small"
        onClick={() => console.log("Save action")}
      />

      <SystemButton
        icon={{ iconDef: baseIcons.copyRegular }}
        tooltip="Copy"
        size="small"
        onClick={() => console.log("Copy action")}
      />

      <SystemButton
        icon={{ iconDef: baseIcons.trashCanSolid }}
        tooltip="Delete"
        size="small"
        onClick={() => console.log("Delete action")}
      />
    </div>
  );
};
```

## Step 5: Best Practices

### 5.1 Prefer Standard Button Component

```tsx
// ❌ WRONG: Using SystemButton for standard user actions
<SystemButton
  icon={{ iconDef: baseIcons.circlePlusSolid }}
  tooltip="Add New Item"
  onClick={addItem}
/>

// ✅ CORRECT: Use Button component for user actions
<Button
  variant="primary"
  iconLeft={baseIcons.circlePlusSolid}
  onClick={addItem}
>
  Add New Item
</Button>
```

### 5.2 Always Provide Tooltips

```tsx
// ❌ WRONG: No tooltip for icon-only button
<SystemButton
  icon={{ iconDef: baseIcons.gearSolid }}
  onClick={openSettings}
/>

// ✅ CORRECT: Always include tooltip for accessibility
<SystemButton
  icon={{ iconDef: baseIcons.gearSolid }}
  tooltip="Open Settings"
  onClick={openSettings}
/>
```

### 5.3 Use Appropriate Sizes

```tsx
// ✅ CORRECT: Size selection based on context
const ContextualSizing = () => {
  return (
    <div>
      {/* Small for compact toolbars */}
      <div className="compact-toolbar">
        <SystemButton icon={{ iconDef: baseIcons.penSolid }} size="small" tooltip="Edit" />
      </div>

      {/* Medium for standard interfaces */}
      <div className="standard-interface">
        <SystemButton icon={{ iconDef: baseIcons.elipsisVerticalSolid }} size="medium" tooltip="More options" />
      </div>

      {/* Large for prominent system controls */}
      <div className="prominent-controls">
        <SystemButton icon={{ iconDef: baseIcons.circleXmarkRegular }} size="large" tooltip="Close" />
      </div>
    </div>
  );
};
```

## Step 6: Integration with Existing Components

### 6.1 Understanding Internal Usage

SystemButton is used internally by these components:

```tsx
// QuickActions uses SystemButton for trigger and actions
<QuickActions>
  {/* Internally uses SystemButton for the trigger */}
  <QuickActionsTrigger />
  <QuickActionsPopover>
    {/* Internally uses SystemButton for each action */}
  </QuickActionsPopover>
</QuickActions>

// FilterHeader uses SystemButton for collapse toggle
<FilterHeader
  isCollapsible={true}
  // Internally uses SystemButton for expand/collapse
/>

// SectionHeader uses SystemButton for collapsible sections
<SectionHeader
  collapsible={true}
  // Internally uses SystemButton for toggle
/>
```

### 6.2 When Building Custom Components

If you need to create a custom component that requires system-level buttons:

```tsx
import { SystemButton, baseIcons } from "@neuron/ui";

const CustomToolbar = ({ onSave, onReset, onClose }) => {
  return (
    <div className="custom-toolbar">
      <div className="toolbar-actions">
        <SystemButton
          icon={{ iconDef: baseIcons.floppyDiskSolid }}
          tooltip="Save Changes"
          size="small"
          onClick={onSave}
        />

        <SystemButton
          icon={{ iconDef: baseIcons.arrowsRotateSolid }}
          tooltip="Reset Form"
          size="small"
          onClick={onReset}
        />
      </div>

      <SystemButton
        icon={{ iconDef: baseIcons.circleXmarkRegular }}
        tooltip="Close Toolbar"
        size="small"
        onClick={onClose}
      />
    </div>
  );
};
```

## Step 7: Common Mistakes to Avoid

### 7.1 Don't Use for Primary User Actions

```tsx
{
  /* ❌ WRONG: SystemButton for primary actions */
}
<div className="form-actions">
  <SystemButton icon={{ iconDef: baseIcons.floppyDiskSolid }} tooltip="Save Form" onClick={saveForm} />
</div>;

{
  /* ✅ CORRECT: Use Button for primary actions */
}
<div className="form-actions">
  <Button variant="primary" iconLeft={baseIcons.floppyDiskSolid} onClick={saveForm}>
    Save Form
  </Button>
</div>;
```

### 7.2 Don't Forget Accessibility

```tsx
{
  /* ❌ WRONG: Missing tooltip and aria-label */
}
<SystemButton icon={{ iconDef: baseIcons.trashCanSolid }} onClick={deleteItem} />;

{
  /* ✅ CORRECT: Proper accessibility */
}
<SystemButton
  icon={{ iconDef: baseIcons.trashCanSolid }}
  tooltip="Delete Item"
  onClick={deleteItem}
  testId="delete-item-button"
/>;
```

### 7.3 Don't Use Without Clear System Context

```tsx
{
  /* ❌ WRONG: SystemButton in general UI context */
}
<div className="user-profile">
  <h2>User Profile</h2>
  <SystemButton icon={{ iconDef: baseIcons.penSolid }} tooltip="Edit Profile" onClick={editProfile} />
</div>;

{
  /* ✅ CORRECT: Regular Button for user actions */
}
<div className="user-profile">
  <h2>User Profile</h2>
  <Button variant="secondary" size="small" iconLeft={baseIcons.penSolid} onClick={editProfile}>
    Edit Profile
  </Button>
</div>;
```

## Key Takeaways

The Neuron SystemButton component is a specialized helper component with specific use cases:

1. **Primarily Internal Use** - Used by other Neuron components like QuickActions, FilterHeader, SectionHeader
2. **Rare Direct Usage** - Only use directly when Figma design explicitly requires it
3. **Prefer Standard Button** - Use regular Button component for most user interactions
4. **Always Include Tooltips** - Essential for accessibility with icon-only buttons
5. **Appropriate Sizing** - Choose size based on interface context and prominence
6. **System-Level Actions** - Best suited for toggles, triggers, and toolbar actions
7. **Accessibility First** - Ensure proper ARIA attributes and keyboard navigation

**Remember**: In 95% of cases, you should use the standard `Button` component instead of `SystemButton`. Only use `SystemButton` when specifically required by design or when building custom components that need system-level button functionality.

## Additional Resources

For more detailed examples and standard button usage, refer to:

- Button Component Guidelines (`/neuron-ui-button`)
- QuickActions Component Guidelines (`/neuron-ui-quickactions`)
- Neuron UI Documentation (`README-AI.md`)

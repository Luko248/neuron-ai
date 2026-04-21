---
agent: agent
---

# AI-Assisted Neuron ToggleButton Component Integration Guide

## For the AI Assistant

Your task is to integrate and configure the Neuron ToggleButton component in a React application. This guide provides comprehensive instructions for implementing ToggleButton, which enables binary state selection with visual feedback across all Neuron applications.

## Sync Metadata

- **Component Version:** v3.1.2
- **Component Source:** `packages/neuron/ui/src/lib/buttons/toggleButton/ToggleButton.tsx`
- **Guideline Command:** `/neuron-ui-togglebutton`
- **Related Skill:** `neuron-ui-buttons`

## Introduction

The ToggleButton is a specialized interactive component that provides users with a way to switch between two states (on/off, enabled/disabled, etc.) with immediate visual feedback.

### What is the ToggleButton?

The ToggleButton component extends the standard button functionality to provide binary state switching with support for:

- Two-state toggle with visual feedback (checked/unchecked)
- Custom labels for both states (onLabel/offLabel)
- Icon support for both states (onIcon/offIcon)
- Multiple variants (primary, secondary, plain, info, success, warning, danger)
- Different sizes (small, medium, large)
- Access control integration for permission-based UI
- Built-in tooltip functionality
- Disabled state handling
- Custom children content with notification badges
- Fit content option for compact layouts

### Key Features

- **Binary State Management**: Toggle between two states with automatic visual feedback
- **Dual State Labels**: Separate labels for on and off states
- **Dual State Icons**: Different icons for each state
- **Consistent Styling**: Inherits Button variants and sizes
- **Access Control**: Built-in readonly and full access permission handling
- **Tooltip Support**: Built-in tooltip functionality with placement options
- **Disabled State**: Proper disabled state handling with visual feedback
- **Custom Content**: Support for custom children with notification badges
- **TypeScript Support**: Full type safety with comprehensive prop interfaces
- **Accessibility**: Built-in accessibility features and ARIA support

**Storybook Reference:** You can refer to the Storybook documentation for visual examples and interactive demos of the ToggleButton component.

## Step 1: Basic ToggleButton Implementation

### 1.1 Import the ToggleButton Component

```tsx
import { ToggleButton } from "@neuron/ui";
```

### 1.2 Basic Usage

Here's a simple implementation of the ToggleButton component:

```tsx
import { ToggleButton } from "@neuron/ui";

const MyComponent = () => {
  const handleChange = (e) => {
    console.info("Toggle state:", e.value);
  };

  return <ToggleButton checked={false} onLabel="On" offLabel="Off" onChange={handleChange} />;
};
```

### 1.3 ToggleButton Variants

The ToggleButton component supports multiple variants for different use cases:

```tsx
import { ToggleButton } from "@neuron/ui";

const ToggleButtonVariants = () => {
  return (
    <div className="toggle-button-variants">
      {/* Primary toggle - main actions */}
      <ToggleButton variant="primary" onLabel="On" offLabel="Off" checked={false} />

      {/* Secondary toggle - alternative actions */}
      <ToggleButton variant="secondary" onLabel="On" offLabel="Off" checked={false} />

      {/* Plain toggle - minimal styling */}
      <ToggleButton variant="plain" onLabel="On" offLabel="Off" checked={false} />

      {/* Status-based toggles */}
      <ToggleButton variant="info" onLabel="On" offLabel="Off" checked={false} />
      <ToggleButton variant="success" onLabel="On" offLabel="Off" checked={false} />
      <ToggleButton variant="warning" onLabel="On" offLabel="Off" checked={false} />
      <ToggleButton variant="danger" onLabel="On" offLabel="Off" checked={false} />
    </div>
  );
};
```

## Step 2: ToggleButton Sizes and Icon Integration

### 2.1 ToggleButton Sizes

Choose the appropriate size based on your UI hierarchy:

```tsx
import { ToggleButton } from "@neuron/ui";

const ToggleButtonSizes = () => {
  return (
    <div className="toggle-button-sizes">
      <ToggleButton size="small" onLabel="Small" offLabel="Small" checked={false} />
      <ToggleButton size="medium" onLabel="Medium" offLabel="Medium" checked={false} /> {/* Default */}
      <ToggleButton size="large" onLabel="Large" offLabel="Large" checked={false} />
    </div>
  );
};
```

### 2.2 Icon Integration

Add icons to enhance toggle button functionality and visual appeal using baseIcons:

```tsx
import { ToggleButton, baseIcons } from "@neuron/ui";

const IconToggleButtons = () => {
  return (
    <div className="icon-toggle-buttons">
      {/* Different icons for each state */}
      <ToggleButton
        onIcon={{ iconDef: baseIcons.circleCheckRegular }}
        offIcon={{ iconDef: baseIcons.circleXmarkRegular }}
        onLabel="Active"
        offLabel="Inactive"
        checked={false}
      />

      {/* Icon only toggle - no labels */}
      <ToggleButton
        onIcon={{ iconDef: baseIcons.eyeRegular }}
        offIcon={{ iconDef: baseIcons.eyeSlashRegular }}
        checked={false}
      />
    </div>
  );
};
```

### 2.3 Icon-Only ToggleButtons

For icon-only toggle buttons, omit the labels:

```tsx
import { ToggleButton, baseIcons } from "@neuron/ui";

const IconOnlyToggleButtons = () => {
  return (
    <div className="icon-only-toggle-buttons">
      {/* Standard icon-only toggle */}
      <ToggleButton
        onIcon={{ iconDef: baseIcons.circleCheckRegular }}
        offIcon={{ iconDef: baseIcons.circleXmarkRegular }}
        checked={false}
      />

      {/* Different variants */}
      <ToggleButton
        variant="secondary"
        onIcon={{ iconDef: baseIcons.lockRegular }}
        offIcon={{ iconDef: baseIcons.lockOpenRegular }}
        checked={false}
      />

      {/* Different sizes */}
      <ToggleButton
        size="small"
        onIcon={{ iconDef: baseIcons.bellRegular }}
        offIcon={{ iconDef: baseIcons.bellSlashRegular }}
        checked={false}
      />
    </div>
  );
};
```

## Step 3: Controlled and Uncontrolled ToggleButtons

### 3.1 Uncontrolled ToggleButton

The component manages its own state internally:

```tsx
import { ToggleButton, baseIcons } from "@neuron/ui";

const UncontrolledToggle = () => {
  const handleChange = (e) => {
    console.info("New state:", e.value);
  };

  return <ToggleButton checked={false} onLabel="Enable" offLabel="Disable" onChange={handleChange} />;
};
```

### 3.2 Controlled ToggleButton

Control the state externally for more complex interactions:

```tsx
import { ToggleButton, baseIcons } from "@neuron/ui";
import { useState } from "react";

const ControlledToggle = () => {
  const [isEnabled, setIsEnabled] = useState(false);

  const handleChange = (e) => {
    setIsEnabled(e.value);
    console.info("Feature enabled:", e.value);
  };

  return (
    <div>
      <ToggleButton checked={isEnabled} onLabel="Enabled" offLabel="Disabled" onChange={handleChange} />
      <p>Status: {isEnabled ? "On" : "Off"}</p>
    </div>
  );
};
```

### 3.3 Disabled State

Use the `disabled` prop to make toggle buttons unclickable with visual feedback:

```tsx
import { ToggleButton, baseIcons } from "@neuron/ui";

const DisabledToggleButtons = () => {
  return (
    <div className="disabled-toggle-buttons">
      <ToggleButton disabled onLabel="On" offLabel="Off" checked={false} />
      <ToggleButton disabled checked={true} onLabel="On" offLabel="Off" />
    </div>
  );
};
```

## Step 4: Tooltip Integration

### 4.1 Basic Tooltip Usage

Add helpful tooltips to provide additional context or information:

```tsx
import { ToggleButton, baseIcons } from "@neuron/ui";

const TooltipToggleButtons = () => {
  return (
    <div className="tooltip-toggle-buttons">
      {/* Simple string tooltip */}
      <ToggleButton
        onLabel="Notifications"
        offLabel="Notifications"
        tooltipText="Toggle notification settings"
        checked={false}
      />

      {/* Tooltip for icon-only toggle */}
      <ToggleButton
        onIcon={{ iconDef: baseIcons.bellRegular }}
        offIcon={{ iconDef: baseIcons.bellSlashRegular }}
        tooltipText="Toggle notifications"
        checked={false}
      />
    </div>
  );
};
```

### 4.2 Advanced Tooltip Configuration

Use tooltip placement options for more control:

```tsx
import { ToggleButton, baseIcons } from "@neuron/ui";

const AdvancedTooltipToggles = () => {
  return (
    <div className="advanced-tooltip-toggles">
      <ToggleButton
        tooltipText="Enable dark mode"
        tooltipPlacement="top"
        onIcon={{ iconDef: baseIcons.moonRegular }}
        offIcon={{ iconDef: baseIcons.sunRegular }}
        checked={false}
      />

      <ToggleButton
        tooltipText="Toggle visibility"
        tooltipPlacement="right"
        onIcon={{ iconDef: baseIcons.eyeRegular }}
        offIcon={{ iconDef: baseIcons.eyeSlashRegular }}
        checked={false}
      />
    </div>
  );
};
```

**Tooltip Placement Options**: `top`, `bottom`, `left`, `right`

## Step 5: Access Control Integration

### 5.1 Permission-Based ToggleButton Visibility

Control toggle button accessibility based on user permissions:

```tsx
import { ToggleButton } from "@neuron/ui";

const AccessControlToggleButtons = () => {
  return (
    <div className="access-control-toggle-buttons">
      {/* ToggleButton with readonly access - shows but disabled */}
      <ToggleButton readonlyAccess={["viewer"]} onLabel="Edit Mode" offLabel="View Mode" checked={false} />

      {/* ToggleButton with full access control */}
      <ToggleButton
        fullAccess={["admin", "editor"]}
        readonlyAccess={["viewer"]}
        variant="danger"
        onLabel="Delete Enabled"
        offLabel="Delete Disabled"
        checked={false}
      />
    </div>
  );
};
```

### 5.2 Access Control Behavior

- **fullAccess**: Array of roles that have full access to the toggle button
- **readonlyAccess**: Array of roles that can see the toggle button but it's disabled
- Users without matching roles won't see the toggle button at all

## Step 6: Custom Children Content

### 6.1 Custom Content with Notification Badges

ToggleButton supports custom children content including notification badges:

```tsx
import { ToggleButton, NotificationBadge, baseIcons } from "@neuron/ui";

const CustomChildrenToggle = () => {
  return (
    <ToggleButton
      onIcon={{ iconDef: baseIcons.bellRegular }}
      offIcon={{ iconDef: baseIcons.bellSlashRegular }}
      checked={false}
    >
      <>
        Notifications
        <NotificationBadge count={5} />
      </>
    </ToggleButton>
  );
};
```

### 6.2 Fit Content Width

Use `fitContent` to make toggle buttons width fit their content:

```tsx
import { ToggleButton } from "@neuron/ui";

const FitContentToggles = () => {
  return (
    <div className="fit-content-toggles">
      <ToggleButton fitContent={true} onLabel="A" offLabel="B" checked={false} />
      <ToggleButton fitContent={false} onLabel="Standard Width" offLabel="Standard Width" checked={false} />
    </div>
  );
};
```

## Step 7: ToggleButton Props Reference

### 7.1 Core ToggleButton Props

| Prop      | Type                                                                                  | Default       | Description                    |
| --------- | ------------------------------------------------------------------------------------- | ------------- | ------------------------------ |
| variant   | `"primary" \| "secondary" \| "plain" \| "info" \| "success" \| "warning" \| "danger"` | `"secondary"` | Toggle button visual style     |
| size      | `"small" \| "medium" \| "large"`                                                      | `"medium"`    | Toggle button size             |
| checked   | `boolean`                                                                             | `false`       | Current checked state          |
| onLabel   | `string`                                                                              | -             | Label text for checked state   |
| offLabel  | `string`                                                                              | -             | Label text for unchecked state |
| className | `string`                                                                              | -             | Additional CSS classes         |
| disabled  | `boolean`                                                                             | `false`       | Disable toggle interaction     |
| children  | `ReactNode`                                                                           | -             | Custom content inside toggle   |

### 7.2 Icon Props

| Prop    | Type        | Default | Description                               |
| ------- | ----------- | ------- | ----------------------------------------- |
| onIcon  | `IconProps` | -       | Icon displayed when checked (on state)    |
| offIcon | `IconProps` | -       | Icon displayed when unchecked (off state) |

### 7.3 State and Callback Props

| Prop       | Type                                   | Default | Description                      |
| ---------- | -------------------------------------- | ------- | -------------------------------- |
| onChange   | `(e: ToggleButtonChangeEvent) => void` | -       | Callback when state changes      |
| fitContent | `boolean`                              | `false` | Toggle button width fits content |

### 7.4 Access Control Props

| Prop           | Type       | Default | Description                |
| -------------- | ---------- | ------- | -------------------------- |
| readonlyAccess | `string[]` | -       | Roles with readonly access |
| fullAccess     | `string[]` | -       | Roles with full access     |

### 7.5 Tooltip Props

| Prop             | Type                                     | Default   | Description                 |
| ---------------- | ---------------------------------------- | --------- | --------------------------- |
| tooltipText      | `string`                                 | -         | Tooltip text                |
| tooltipPlacement | `"top" \| "bottom" \| "left" \| "right"` | `"right"` | Tooltip placement direction |

### 7.6 Testing Props

| Prop   | Type     | Default | Description             |
| ------ | -------- | ------- | ----------------------- |
| testId | `string` | -       | Custom test ID override |

## Step 8: Best Practices

### 8.1 When to Use ToggleButton

**Use ToggleButton for:**

- Binary settings that need immediate feedback
- Feature enable/disable controls
- Visibility toggles (show/hide)
- Mode switches (edit/view, light/dark)
- Status toggles (active/inactive)

```tsx
<ToggleButton
  onLabel="Dark Mode"
  offLabel="Light Mode"
  onIcon={{ iconDef: baseIcons.moonRegular }}
  offIcon={{ iconDef: baseIcons.sunRegular }}
  checked={isDarkMode}
  onChange={handleThemeChange}
/>

<ToggleButton
  onLabel="Edit Mode"
  offLabel="View Mode"
  checked={isEditMode}
  onChange={handleModeChange}
/>
```

**Don't use ToggleButton for:**

- Actions that require confirmation
- Navigation or routing actions
- Form submissions
- Multiple choice selections (use radio buttons or dropdown)

### 8.2 Label and Icon Guidelines

- Provide clear, descriptive labels for both states
- Use icons that clearly represent each state
- Keep label text concise (1-3 words)
- Ensure icons are semantically meaningful

```tsx
{
  /* Good: Clear state labels */
}
<ToggleButton onLabel="Enabled" offLabel="Disabled" checked={isEnabled} />;

{
  /* Good: Meaningful icons */
}
<ToggleButton
  onIcon={{ iconDef: baseIcons.lockRegular }}
  offIcon={{ iconDef: baseIcons.lockOpenRegular }}
  onLabel="Locked"
  offLabel="Unlocked"
  checked={isLocked}
/>;

{
  /* Avoid: Confusing icons */
}
<ToggleButton
  onIcon={{ iconDef: baseIcons.trashCanSolid }}
  offIcon={{ iconDef: baseIcons.checkSolid }}
  onLabel="Delete"
  offLabel="Save"
  checked={false}
/>;
```

### 8.3 State Management Best Practices

- Use controlled state for complex interactions
- Provide immediate visual feedback on state change
- Persist state when appropriate
- Clear communication of current state

```tsx
{
  /* Good: Controlled state with persistence */
}
const [notifications, setNotifications] = useState(() => {
  return localStorage.getItem("notificationsEnabled") === "true";
});

const handleToggle = (e) => {
  setNotifications(e.value);
  localStorage.setItem("notificationsEnabled", String(e.value));
};

<ToggleButton
  checked={notifications}
  onChange={handleToggle}
  onLabel="Notifications On"
  offLabel="Notifications Off"
/>;
```

### 8.4 Tooltip Best Practices

- Always provide tooltips for icon-only toggle buttons
- Use clear, concise tooltip text
- Describe what will happen when toggled

```tsx
{
  /* Good: Icon-only with descriptive tooltip */
}
<ToggleButton
  onIcon={{ iconDef: baseIcons.eyeRegular }}
  offIcon={{ iconDef: baseIcons.eyeSlashRegular }}
  tooltipText="Toggle content visibility"
  checked={isVisible}
/>;
```

### 8.5 Accessibility Considerations

- Always provide meaningful labels or tooltips
- Ensure sufficient color contrast
- Maintain keyboard navigation support
- Provide clear visual feedback for state changes

```tsx
{
  /* Good: Accessible with clear labels */
}
<ToggleButton
  onLabel="Enable Notifications"
  offLabel="Disable Notifications"
  checked={notificationsEnabled}
  onChange={handleNotificationToggle}
/>;
```

## Step 9: Common Patterns and Examples

### 9.1 Settings Panel with ToggleButtons

```tsx
import { ToggleButton, baseIcons } from "@neuron/ui";
import { useState } from "react";

const SettingsPanel = () => {
  const [darkMode, setDarkMode] = useState(false);
  const [notifications, setNotifications] = useState(true);
  const [autoSave, setAutoSave] = useState(true);

  return (
    <div className="settings-panel d-grid form-gap">
      <div className="d-flex justify-content-between align-items-center">
        <label>Dark Mode</label>
        <ToggleButton
          checked={darkMode}
          onChange={(e) => setDarkMode(e.value)}
          onIcon={{ iconDef: baseIcons.moonRegular }}
          offIcon={{ iconDef: baseIcons.sunRegular }}
          onLabel="On"
          offLabel="Off"
        />
      </div>

      <div className="d-flex justify-content-between align-items-center">
        <label>Notifications</label>
        <ToggleButton
          checked={notifications}
          onChange={(e) => setNotifications(e.value)}
          onIcon={{ iconDef: baseIcons.bellRegular }}
          offIcon={{ iconDef: baseIcons.bellSlashRegular }}
          onLabel="On"
          offLabel="Off"
        />
      </div>

      <div className="d-flex justify-content-between align-items-center">
        <label>Auto Save</label>
        <ToggleButton checked={autoSave} onChange={(e) => setAutoSave(e.value)} onLabel="On" offLabel="Off" />
      </div>
    </div>
  );
};
```

### 9.2 Feature Toggle with Confirmation

```tsx
import { ToggleButton, baseIcons } from "@neuron/ui";
import { useState } from "react";

const FeatureToggleWithConfirmation = () => {
  const [featureEnabled, setFeatureEnabled] = useState(false);

  const handleToggle = (e) => {
    if (!e.value) {
      // Disabling - ask for confirmation
      if (window.confirm("Are you sure you want to disable this feature?")) {
        setFeatureEnabled(false);
      }
    } else {
      // Enabling - no confirmation needed
      setFeatureEnabled(true);
    }
  };

  return (
    <ToggleButton
      checked={featureEnabled}
      onChange={handleToggle}
      onLabel="Feature Enabled"
      offLabel="Feature Disabled"
      variant={featureEnabled ? "success" : "secondary"}
    />
  );
};
```

### 9.3 Visibility Toggle Group

```tsx
import { ToggleButton, baseIcons } from "@neuron/ui";
import { useState } from "react";

const VisibilityToggleGroup = () => {
  const [showPersonal, setShowPersonal] = useState(true);
  const [showContact, setShowContact] = useState(true);
  const [showFinancial, setShowFinancial] = useState(false);

  return (
    <div className="visibility-toggles d-flex gap-8">
      <ToggleButton
        size="small"
        checked={showPersonal}
        onChange={(e) => setShowPersonal(e.value)}
        onIcon={{ iconDef: baseIcons.eyeRegular }}
        offIcon={{ iconDef: baseIcons.eyeSlashRegular }}
        tooltipText="Toggle personal information visibility"
      />
      <ToggleButton
        size="small"
        checked={showContact}
        onChange={(e) => setShowContact(e.value)}
        onIcon={{ iconDef: baseIcons.eyeRegular }}
        offIcon={{ iconDef: baseIcons.eyeSlashRegular }}
        tooltipText="Toggle contact information visibility"
      />
      <ToggleButton
        size="small"
        checked={showFinancial}
        onChange={(e) => setShowFinancial(e.value)}
        onIcon={{ iconDef: baseIcons.eyeRegular }}
        offIcon={{ iconDef: baseIcons.eyeSlashRegular }}
        tooltipText="Toggle financial information visibility"
      />
    </div>
  );
};
```

## Step 10: Common Mistakes to Avoid

### 10.1 Don't Use ToggleButton for Actions

```tsx
{
  /* Wrong: Using toggle for action that requires confirmation */
}
<ToggleButton onLabel="Delete" offLabel="Keep" checked={false} onChange={handleDelete} />;

{
  /* Right: Use Button for actions */
}
<Button variant="danger" onClick={handleDelete}>
  Delete
</Button>;
```

### 10.2 Don't Ignore State Management

```tsx
{
  /* Wrong: No state management */
}
<ToggleButton onLabel="On" offLabel="Off" onChange={(e) => console.info(e.value)} />;

{
  /* Right: Proper state management */
}
const [enabled, setEnabled] = useState(false);
<ToggleButton checked={enabled} onChange={(e) => setEnabled(e.value)} onLabel="On" offLabel="Off" />;
```

### 10.3 Don't Mix Toggle Semantics Incorrectly

```tsx
{
  /* Wrong: Success variant for potentially dangerous toggle */
}
<ToggleButton variant="success" onLabel="Delete Mode On" offLabel="Delete Mode Off" checked={deleteMode} />;

{
  /* Right: Match variant to action semantic */
}
<ToggleButton variant="danger" onLabel="Delete Mode On" offLabel="Delete Mode Off" checked={deleteMode} />;
```

### 10.4 Don't Use Unclear Labels

```tsx
{
  /* Wrong: Ambiguous labels */
}
<ToggleButton onLabel="Yes" offLabel="No" checked={something} />;

{
  /* Right: Clear, descriptive labels */
}
<ToggleButton onLabel="Auto-save Enabled" offLabel="Auto-save Disabled" checked={autoSaveEnabled} />;
```

## Key Takeaways

The Neuron ToggleButton component provides a comprehensive, accessible solution for binary state management. Key points to remember:

1. **Use appropriate variants** based on toggle semantics
2. **Provide clear labels** for both states
3. **Use meaningful icons** that represent each state
4. **Always use baseIcons** from `@neuron/ui` for consistency
5. **Manage state properly** with controlled or uncontrolled patterns
6. **Tooltips are essential** for icon-only toggle buttons
7. **Access control** manages user permissions automatically
8. **Disabled state** provides proper user feedback
9. **Follow accessibility guidelines** for inclusive design
10. **Don't use for actions** - use Button component instead

By following these guidelines, you'll create consistent, accessible, and user-friendly toggle button interfaces across your Neuron applications.

---
agent: agent
---

# AI-Assisted Neuron Tooltip Component Integration Guide

## For the AI Assistant

Your task is to integrate and configure the Neuron Tooltip component in a React application. This guide provides comprehensive instructions for implementing the Tooltip component, which serves as a contextual information overlay system across all Neuron applications.

## Sync Metadata

- **Component Version:** v5.0.6
- **Component Source:** `packages/neuron/ui/src/lib/popups/tooltip/Tooltip.tsx`
- **Guideline Command:** `/neuron-ui-tooltip`
- **Related Skill:** `neuron-ui-popups`

## Introduction

The Tooltip system is a core part of the Neuron UI framework, designed to provide contextual information and help text in a consistent, accessible manner across all Neuron applications.

### What is the Tooltip System?

The Tooltip component provides standardized contextual information overlays for your application with support for:

- Multiple placement options (top, bottom, left, right, auto)
- Light and dark variants for different UI contexts
- Custom content support (text, ReactNode, functions)
- List item rendering for structured information
- Automatic and manual trigger events
- Custom children or default info icon
- Internationalization support with TFuncKey
- Accessibility features and ARIA support
- Custom width control and responsive behavior

### Key Features

- **Flexible Content**: Support for text, ReactNode, functions, and i18n keys
- **Multiple Placements**: Top, bottom, left, right, and auto positioning
- **Variant Support**: Light and dark themes for different UI contexts
- **Event Control**: Hover, focus, click, and combined trigger events
- **Custom Triggers**: Use custom children or default info icon
- **List Support**: Built-in list rendering for structured information
- **Accessibility**: Full ARIA support and keyboard navigation
- **Responsive**: Auto-positioning and custom width controls
- **TypeScript Support**: Full type safety with comprehensive interfaces
- **Test Integration**: Built-in test ID support for automated testing

**Storybook Reference:** You can refer to the Storybook documentation for visual examples and interactive demos of the Tooltip component.

## Step 1: Basic Tooltip Implementation

### 1.1 Import the Tooltip Component

```tsx
import { Tooltip } from "@neuron/ui";
```

### 1.2 Basic Usage with Default Icon

Here's a simple implementation of the Tooltip component with the default info icon:

```tsx
import { Tooltip } from "@neuron/ui";

const MyComponent = () => {
  return (
    <div className="info-section">
      <h3>Important Information</h3>
      <Tooltip tooltipContent="This provides additional context about the information displayed." />
    </div>
  );
};
```

### 1.3 Tooltip with Custom Children

Use custom children to trigger the tooltip on any element:

```tsx
import { Tooltip, Button, baseIcons } from "@neuron/ui";

const CustomTriggerExample = () => {
  return (
    <div className="custom-triggers">
      {/* Tooltip on button */}
      <Tooltip tooltipContent="This button performs an important action">
        <Button iconLeft={baseIcons.circlePlusSolid}>Add Item</Button>
      </Tooltip>

      {/* Tooltip on text */}
      <Tooltip tooltipContent="Click for more details">
        <span className="clickable-text">Hover me</span>
      </Tooltip>

      {/* Tooltip on icon */}
      <Tooltip tooltipContent="Status information">
        <Icon iconDef={baseIcons.circleCheckSolid} />
      </Tooltip>
    </div>
  );
};
```

## Step 2: Tooltip Placement and Positioning

### 2.1 Placement Options

Choose the appropriate placement based on your UI layout:

```tsx
import { Tooltip } from "@neuron/ui";

const PlacementExamples = () => {
  return (
    <div className="placement-examples">
      {/* Top placement (default) */}
      <Tooltip placement="top" tooltipContent="Tooltip appears above the trigger" />

      {/* Bottom placement */}
      <Tooltip placement="bottom" tooltipContent="Tooltip appears below the trigger" />

      {/* Left placement */}
      <Tooltip placement="left" tooltipContent="Tooltip appears to the left" />

      {/* Right placement */}
      <Tooltip placement="right" tooltipContent="Tooltip appears to the right" />

      {/* Auto placement - follows mouse */}
      <Tooltip placement="auto" tooltipContent="Tooltip follows mouse cursor" />
    </div>
  );
};
```

### 2.2 Smart Placement for UI Elements

Use appropriate placement based on element position in the UI:

```tsx
import { Tooltip, Button } from "@neuron/ui";

const SmartPlacementExample = () => {
  return (
    <div className="layout-example">
      {/* Header elements - use bottom placement */}
      <header className="app-header">
        <Tooltip placement="bottom" tooltipContent="Main navigation menu">
          <Button>Menu</Button>
        </Tooltip>
      </header>

      {/* Sidebar elements - use right placement */}
      <aside className="sidebar">
        <Tooltip placement="right" tooltipContent="User profile settings">
          <Button>Profile</Button>
        </Tooltip>
      </aside>

      {/* Footer elements - use top placement */}
      <footer className="app-footer">
        <Tooltip placement="top" tooltipContent="Application version info">
          <span>v1.0.0</span>
        </Tooltip>
      </footer>
    </div>
  );
};
```

## Step 3: Tooltip Variants and Styling

### 3.1 Light and Dark Variants

Choose the appropriate variant based on your UI theme:

```tsx
import { Tooltip } from "@neuron/ui";

const VariantExamples = () => {
  return (
    <div className="variant-examples">
      {/* Dark variant (default) - for light backgrounds */}
      <div className="light-section">
        <Tooltip variant="dark" tooltipContent="Dark tooltip on light background" />
      </div>

      {/* Light variant - for dark backgrounds */}
      <div className="dark-section">
        <Tooltip variant="light" tooltipContent="Light tooltip on dark background" />
      </div>
    </div>
  );
};
```

### 3.2 Custom Width Control

Control tooltip width for different content types:

```tsx
import { Tooltip } from "@neuron/ui";

const WidthControlExamples = () => {
  return (
    <div className="width-examples">
      {/* Default width (400px) */}
      <Tooltip tooltipContent="Standard width tooltip with default 400px maximum width" />

      {/* Custom width */}
      <Tooltip maxWidth="200px" tooltipContent="Narrow tooltip with custom width" />

      {/* Unlimited width */}
      <Tooltip maxWidth="unset" tooltipContent="This tooltip can expand to any width needed for the content" />

      {/* Responsive width */}
      <Tooltip maxWidth="50vw" tooltipContent="Responsive tooltip that adapts to viewport width" />
    </div>
  );
};
```

## Step 4: Advanced Content Types

### 4.1 List Items Support

Display structured information using list items:

```tsx
import { Tooltip } from "@neuron/ui";

const ListTooltipExample = () => {
  const featureList = [
    "Real-time data synchronization",
    "Advanced filtering options",
    "Export to multiple formats",
    "Collaborative editing",
  ];

  return (
    <div className="feature-info">
      <h3>Premium Features</h3>
      <Tooltip
        tooltipContent="Available premium features:"
        listItems={featureList}
        placement="right"
        maxWidth="300px"
      />
    </div>
  );
};
```

### 4.2 Custom ReactNode Content

Use complex content with JSX elements:

```tsx
import { Tooltip, Button } from "@neuron/ui";

const CustomContentExample = () => {
  const customContent = (
    <div className="custom-tooltip-content">
      <h4 className="m-0 mb-8">Action Details</h4>
      <p className="m-0 mb-12">
        This action will permanently delete the selected items. This operation cannot be undone.
      </p>
      <div className="d-flex gap-8">
        <Button size="small" variant="danger">
          Confirm
        </Button>
        <Button size="small" variant="secondary">
          Cancel
        </Button>
      </div>
    </div>
  );

  return (
    <Tooltip tooltipContent={customContent} variant="light" maxWidth="350px" placement="top">
      <Button variant="danger">Delete Items</Button>
    </Tooltip>
  );
};
```

### 4.3 Function-Based Content

Use functions for dynamic content generation:

```tsx
import { Tooltip } from "@neuron/ui";

const DynamicContentExample = () => {
  const getDynamicContent = () => {
    const currentTime = new Date().toLocaleTimeString();
    return `Last updated: ${currentTime}`;
  };

  return (
    <div className="status-info">
      <span>System Status</span>
      <Tooltip tooltipContent={getDynamicContent} placement="right" event="hover" />
    </div>
  );
};
```

## Step 5: Event Handling and Triggers

### 5.1 Event Types

Control when tooltips appear using different event types:

```tsx
import { Tooltip, Input } from "@neuron/ui";

const EventExamples = () => {
  return (
    <div className="event-examples">
      {/* Hover only */}
      <Tooltip event="hover" tooltipContent="Appears on hover only" />

      {/* Focus only */}
      <Tooltip event="focus" tooltipContent="Appears on focus only">
        <Input placeholder="Focus me for tooltip" />
      </Tooltip>

      {/* Both hover and focus (default) */}
      <Tooltip event="both" tooltipContent="Appears on hover and focus">
        <button>Interactive element</button>
      </Tooltip>
    </div>
  );
};
```

### 5.2 Delay Control

Add delays for better user experience:

```tsx
import { Tooltip } from "@neuron/ui";

const DelayExamples = () => {
  return (
    <div className="delay-examples">
      {/* No delay (immediate) */}
      <Tooltip tooltipContent="Immediate tooltip" />

      {/* Short delay */}
      <Tooltip delay={500} tooltipContent="Appears after 500ms delay" />

      {/* Longer delay for less intrusive tooltips */}
      <Tooltip delay={1000} tooltipContent="Appears after 1 second delay" />
    </div>
  );
};
```

## Step 6: Internationalization Support

### 6.1 Using Translation Keys

Integrate with the i18n system using TFuncKey:

```tsx
import { Tooltip } from "@neuron/ui";
import { useTranslation } from "react-i18next";

const I18nTooltipExample = () => {
  const { t } = useTranslation();

  return (
    <div className="i18n-examples">
      {/* Direct translation key */}
      <Tooltip tooltipContent="common.buttons.save.tooltip" />

      {/* Using t() function */}
      <Tooltip tooltipContent={t("user.profile.editTooltip")} />

      {/* Complex content with translations */}
      <Tooltip
        tooltipContent={
          <div>
            <h4>{t("help.title")}</h4>
            <p>{t("help.description")}</p>
          </div>
        }
      />
    </div>
  );
};
```

## Step 7: Accessibility and Disabled States

### 7.1 Disabled Tooltips

Control tooltip visibility based on conditions:

```tsx
import { Tooltip, Button } from "@neuron/ui";

const DisabledTooltipExample = ({ userHasPermission }) => {
  return (
    <div className="disabled-examples">
      {/* Conditionally disabled */}
      <Tooltip disabled={!userHasPermission} tooltipContent="You need admin permissions for this action">
        <Button disabled={!userHasPermission}>Admin Action</Button>
      </Tooltip>

      {/* Hidden tooltip */}
      <Tooltip hide={true} tooltipContent="This tooltip is hidden" />
    </div>
  );
};
```

### 7.2 Accessibility Best Practices

Ensure tooltips are accessible to all users:

```tsx
import { Tooltip, Button } from "@neuron/ui";

const AccessibleTooltipExample = () => {
  return (
    <div className="accessible-examples">
      {/* Descriptive tooltip for icon-only buttons */}
      <Tooltip tooltipContent="Delete this item permanently">
        <Button iconLeft={baseIcons.trashCanSolid} />
      </Tooltip>

      {/* Helpful context for complex actions */}
      <Tooltip tooltipContent="This will send an email notification to all team members" placement="top">
        <Button>Notify Team</Button>
      </Tooltip>

      {/* Form field help */}
      <div className="form-field">
        <label htmlFor="password">Password</label>
        <Input id="password" type="password" />
        <Tooltip
          tooltipContent="Password must be at least 8 characters with uppercase, lowercase, and numbers"
          placement="right"
        />
      </div>
    </div>
  );
};
```

## Step 8: Integration with Other Components

### 8.1 Form Field Integration

Enhance form fields with helpful tooltips:

```tsx
import { Tooltip, Input, Select, CheckBox } from "@neuron/ui";

const FormTooltipExample = () => {
  return (
    <form className="form-with-tooltips">
      {/* Input with help tooltip */}
      <div className="form-field">
        <Input labelText="Email Address" placeholder="Enter your email" />
        <Tooltip
          tooltipContent="We'll use this email for account notifications and password recovery"
          placement="right"
        />
      </div>

      {/* Select with options explanation */}
      <div className="form-field">
        <Select labelText="Account Type" options={accountTypeOptions} />
        <Tooltip
          tooltipContent="Choose the account type that best matches your organization size"
          listItems={[
            "Personal: Individual users",
            "Team: Small teams (2-10 users)",
            "Enterprise: Large organizations (10+ users)",
          ]}
          placement="right"
          maxWidth="300px"
        />
      </div>

      {/* Checkbox with legal information */}
      <div className="form-field">
        <CheckBox labelText="I agree to the terms and conditions" />
        <Tooltip tooltipContent="Click to read our full terms and conditions and privacy policy" placement="top" />
      </div>
    </form>
  );
};
```

### 8.2 Button Integration

Enhance buttons with contextual information:

```tsx
import { Tooltip, Button, baseIcons } from "@neuron/ui";

const ButtonTooltipExample = () => {
  return (
    <div className="button-tooltips">
      {/* Action explanation */}
      <Tooltip tooltipContent="Save all changes to the database">
        <Button variant="primary" iconLeft={baseIcons.floppyDiskSolid}>
          Save Changes
        </Button>
      </Tooltip>

      {/* Keyboard shortcut information */}
      <Tooltip tooltipContent="Export data to CSV file (Ctrl+E)" placement="bottom">
        <Button iconLeft={baseIcons.downloadSolid}>Export</Button>
      </Tooltip>

      {/* Status information */}
      <Tooltip tooltipContent="This action requires admin privileges" variant="light">
        <Button variant="danger" disabled>
          Delete All
        </Button>
      </Tooltip>
    </div>
  );
};
```

## Step 9: Tooltip Props Reference

### 9.1 Core Tooltip Props

| Prop           | Type                                               | Default  | Description                                             |
| -------------- | -------------------------------------------------- | -------- | ------------------------------------------------------- |
| tooltipContent | `ReactNode \| (() => ReactNode) \| TFuncKey`       | -        | Content of the tooltip                                  |
| placement      | `"top" \| "bottom" \| "right" \| "left" \| "auto"` | `"top"`  | Tooltip position relative to trigger                    |
| variant        | `"light" \| "dark"`                                | `"dark"` | Tooltip visual theme                                    |
| children       | `ReactNode`                                        | -        | Custom trigger element (uses info icon if not provided) |
| className      | `string`                                           | -        | Additional CSS classes for trigger                      |

### 9.2 Content and Display Props

| Prop      | Type                        | Default   | Description                              |
| --------- | --------------------------- | --------- | ---------------------------------------- |
| listItems | `string[]`                  | -         | Array of strings to render as list items |
| maxWidth  | `CSSProperties["maxWidth"]` | `"400px"` | Maximum width of tooltip                 |
| hide      | `boolean`                   | `false`   | Hide the tooltip completely              |
| disabled  | `boolean`                   | `false`   | Disable tooltip functionality            |

### 9.3 Event and Timing Props

| Prop  | Type                           | Default  | Description                                  |
| ----- | ------------------------------ | -------- | -------------------------------------------- |
| event | `"hover" \| "focus" \| "both"` | `"both"` | Events that trigger tooltip                  |
| delay | `number`                       | -        | Delay in milliseconds before showing tooltip |

### 9.4 Testing and Accessibility Props

| Prop   | Type                | Default | Description                                 |
| ------ | ------------------- | ------- | ------------------------------------------- |
| testId | `string`            | -       | Custom test ID for automated testing        |
| target | `string \| Element` | -       | Target element for tooltip (advanced usage) |

## Step 10: Best Practices

### 10.1 When to Use Tooltips

**Good Use Cases:**

- Icon-only buttons that need explanation
- Form fields requiring additional context
- Complex actions that need clarification
- Keyboard shortcuts and help information
- Status indicators and system information

```tsx
{
  /* Good: Icon button with explanation */
}
<Tooltip tooltipContent="Delete this item permanently">
  <Button iconLeft={baseIcons.trashCanSolid} />
</Tooltip>;

{
  /* Good: Form field help */
}
<Tooltip tooltipContent="Password must contain at least 8 characters">
  <Input type="password" labelText="Password" />
</Tooltip>;
```

**Avoid Using Tooltips For:**

- Essential information that should always be visible
- Long text that would be better as regular content
- Interactive elements within tooltips (except simple buttons)
- Mobile-first interfaces where hover isn't available

### 10.2 Content Guidelines

**Keep Content Concise:**

```tsx
{
  /* Good: Clear and concise */
}
<Tooltip tooltipContent="Save changes to database">
  <Button>Save</Button>
</Tooltip>;

{
  /* Avoid: Too verbose */
}
<Tooltip tooltipContent="This button will save all of your current changes to the database and update the user interface to reflect the new state">
  <Button>Save</Button>
</Tooltip>;
```

**Use Appropriate Content Types:**

```tsx
{
  /* Good: Simple text for basic info */
}
<Tooltip tooltipContent="Edit user profile" />;

{
  /* Good: List for structured info */
}
<Tooltip tooltipContent="Available actions:" listItems={["Edit", "Delete", "Share", "Export"]} />;

{
  /* Good: Custom content for complex info */
}
<Tooltip
  tooltipContent={
    <div>
      <h4>Keyboard Shortcuts</h4>
      <p>Ctrl+S: Save</p>
      <p>Ctrl+Z: Undo</p>
    </div>
  }
/>;
```

### 10.3 Placement Guidelines

**Consider UI Layout:**

```tsx
{
  /* Header elements - use bottom */
}
<Tooltip placement="bottom" tooltipContent="Main menu">
  <Button>Menu</Button>
</Tooltip>;

{
  /* Sidebar elements - use right */
}
<Tooltip placement="right" tooltipContent="User settings">
  <Button>Settings</Button>
</Tooltip>;

{
  /* Near screen edges - use auto */
}
<Tooltip placement="auto" tooltipContent="Adaptive positioning">
  <Button>Edge Button</Button>
</Tooltip>;
```

### 10.4 Accessibility Considerations

- Always provide meaningful tooltip content
- Use appropriate event triggers for the context
- Ensure tooltips don't interfere with keyboard navigation
- Test with screen readers and keyboard-only navigation
- Consider mobile users who can't hover

```tsx
{
  /* Good: Accessible icon button */
}
<Tooltip tooltipContent="Delete item" event="both">
  <Button iconLeft={baseIcons.trashCanSolid} />
</Tooltip>;

{
  /* Good: Form field help */
}
<Tooltip tooltipContent="Enter your full legal name as it appears on official documents" placement="right">
  <Input labelText="Full Name" />
</Tooltip>;
```

## Step 11: Common Patterns and Examples

### 11.1 Help System Integration

```tsx
import { Tooltip, Panel, Input } from "@neuron/ui";

const HelpSystemExample = () => {
  return (
    <Panel title="User Registration">
      <div className="form-with-help">
        <div className="form-field">
          <Input labelText="Username" />
          <Tooltip tooltipContent="Username must be 3-20 characters, letters and numbers only" placement="right" />
        </div>

        <div className="form-field">
          <Input type="email" labelText="Email" />
          <Tooltip tooltipContent="We'll send a verification email to this address" placement="right" />
        </div>

        <div className="form-field">
          <Input type="password" labelText="Password" />
          <Tooltip
            tooltipContent="Password requirements:"
            listItems={[
              "At least 8 characters",
              "One uppercase letter",
              "One lowercase letter",
              "One number",
              "One special character",
            ]}
            placement="right"
            maxWidth="250px"
          />
        </div>
      </div>
    </Panel>
  );
};
```

### 11.2 Status and Information Display

```tsx
import { Tooltip, Badge, Icon } from "@neuron/ui";

const StatusTooltipExample = () => {
  return (
    <div className="status-display">
      {/* System status */}
      <div className="status-item">
        <span>Database</span>
        <Tooltip tooltipContent="Database connection is healthy, last check: 2 minutes ago">
          <Badge variant="success">Online</Badge>
        </Tooltip>
      </div>

      {/* User status */}
      <div className="status-item">
        <span>User Status</span>
        <Tooltip tooltipContent="User account is active with premium subscription" placement="left">
          <Icon iconDef={baseIcons.circleCheckSolid} className="text-success" />
        </Tooltip>
      </div>

      {/* Process status */}
      <div className="status-item">
        <span>Background Process</span>
        <Tooltip tooltipContent="Data synchronization in progress, estimated completion: 5 minutes" variant="light">
          <Badge variant="warning">Running</Badge>
        </Tooltip>
      </div>
    </div>
  );
};
```

### 11.3 Navigation Enhancement

```tsx
import { Tooltip, Button, baseIcons } from "@neuron/ui";

const NavigationTooltipExample = () => {
  return (
    <nav className="app-navigation">
      <div className="nav-section">
        <Tooltip tooltipContent="Go to dashboard overview">
          <Button iconLeft={baseIcons.houseSolid}>Dashboard</Button>
        </Tooltip>

        <Tooltip tooltipContent="Manage user accounts and permissions">
          <Button iconLeft={baseIcons.usersSolid}>Users</Button>
        </Tooltip>

        <Tooltip tooltipContent="View and analyze system reports" placement="right">
          <Button iconLeft={baseIcons.chartBarSolid}>Reports</Button>
        </Tooltip>
      </div>

      {/* Icon-only navigation */}
      <div className="icon-nav">
        <Tooltip tooltipContent="Application settings" placement="bottom">
          <Button iconLeft={baseIcons.cogSolid} />
        </Tooltip>

        <Tooltip tooltipContent="User profile and account" placement="bottom">
          <Button iconLeft={baseIcons.userSolid} />
        </Tooltip>

        <Tooltip tooltipContent="Sign out of application" placement="bottom">
          <Button iconLeft={baseIcons.signOutSolid} />
        </Tooltip>
      </div>
    </nav>
  );
};
```

## Step 12: Common Mistakes to Avoid

### 12.1 Don't Overuse Tooltips

```tsx
{
  /* Wrong: Tooltip on obvious elements */
}
<Tooltip tooltipContent="This is a save button">
  <Button>Save</Button>
</Tooltip>;

{
  /* Right: Tooltip only when needed */
}
<Button>Save</Button>;

{
  /* Right: Tooltip for icon-only buttons */
}
<Tooltip tooltipContent="Save changes">
  <Button iconLeft={baseIcons.floppyDiskSolid} />
</Tooltip>;
```

### 12.2 Don't Use Tooltips for Essential Information

```tsx
{
  /* Wrong: Critical info in tooltip */
}
<Tooltip tooltipContent="This action cannot be undone!">
  <Button variant="danger">Delete</Button>
</Tooltip>;

{
  /* Right: Essential info visible */
}
<div>
  <p className="text-danger">Warning: This action cannot be undone!</p>
  <Button variant="danger">Delete</Button>
</div>;
```

### 12.3 Don't Make Tooltips Too Long

```tsx
{
  /* Wrong: Too much content */
}
<Tooltip tooltipContent="This is a very long tooltip with lots of information that would be better displayed as regular content on the page rather than hidden in a tooltip that users might not discover">
  <Button>Action</Button>
</Tooltip>;

{
  /* Right: Concise and helpful */
}
<Tooltip tooltipContent="Permanently delete selected items">
  <Button variant="danger">Delete</Button>
</Tooltip>;
```

### 12.4 Don't Ignore Mobile Users

```tsx
{
  /* Consider: Mobile users can't hover */
}
<Tooltip
  event="both" // Supports both hover and focus
  tooltipContent="Help information"
>
  <Button>Action</Button>
</Tooltip>;

{
  /* Better: Provide alternative for mobile */
}
<div className="action-with-help">
  <Button>Action</Button>
  <Button
    iconLeft={baseIcons.circleInfoSolid}
    variant="plain"
    className="d-md-none" // Show only on mobile
  >
    Help
  </Button>
  <Tooltip
    tooltipContent="Help information"
    className="d-none d-md-inline" // Hide on mobile
  />
</div>;
```

## Summary

The Neuron Tooltip component provides a comprehensive, accessible, and flexible system for contextual information display. Key points to remember:

1. **Use appropriate placement** based on UI layout and element position
2. **Choose the right variant** (light/dark) for your theme context
3. **Keep content concise** and meaningful
4. **Always provide tooltips** for icon-only buttons
5. **Consider accessibility** and keyboard navigation
6. **Test on mobile devices** where hover isn't available
7. **Use structured content** (lists, custom JSX) when appropriate
8. **Integrate with i18n** for multilingual applications
9. **Control timing and events** for optimal user experience
10. **Follow accessibility guidelines** for inclusive design

### Key Features Summary:

- **Flexible Content**: Text, ReactNode, functions, and i18n keys
- **Smart Positioning**: Auto-placement and responsive behavior
- **Theme Support**: Light and dark variants
- **Event Control**: Hover, focus, and combined triggers
- **Accessibility**: Full ARIA support and keyboard navigation
- **Integration**: Works seamlessly with all Neuron UI components
- **Testing**: Built-in test ID support
- **Performance**: Optimized rendering and event handling

By following these guidelines, you'll create consistent, accessible, and user-friendly tooltip interfaces that enhance the user experience across your Neuron applications.

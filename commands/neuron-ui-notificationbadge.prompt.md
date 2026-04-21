---
agent: agent
---

# AI-Assisted Neuron NotificationBadge Component Integration Guide

## For the AI Assistant

Your task is to integrate and configure the Neuron NotificationBadge components in a React application. This guide provides essential instructions for implementing NotificationBadge components, which serve as visual indicators for notifications and status updates across all Neuron applications.

## Sync Metadata

- **Component Version:** v4.0.3
- **Component Source:** `packages/neuron/ui/src/lib/misc/notificationBadge/NotificationBadge.tsx`
- **Guideline Command:** `/neuron-ui-notificationbadge`
- **Related Skill:** `neuron-ui-misc`

## Introduction

The NotificationBadge component provides standardized notification rendering for your application with support for:

- **Count-based badges** - Display numerical notification counts with automatic formatting
- **Icon badges** - Icon-only indicators for status or presence
- **Multiple variants** - Semantic color variants (danger, warning, success, info, brand)
- **Tooltip integration** - Built-in tooltip support for additional context
- **Smart display logic** - Component is not rendered if count ≤ 0
- **Accessibility features** - Proper ARIA support and semantic meaning

### Key Features

- **Seven Badge Variants**: danger, danger-negative, warning, success, info, brand, brand-negative
- **Flexible Content**: Support for counts and icons
- **Smart Count Formatting**: Automatic "99+" formatting for large numbers (>99)
- **Tooltip Integration**: Built-in tooltip support for enhanced UX
- **Auto-hiding Logic**: Component not rendered when count ≤ 0
- **TypeScript Support**: Full type safety with comprehensive props typing

**Storybook Reference:** You can refer to the Storybook documentation for visual examples and interactive demos of all available badge configurations.

## Step 1: Basic NotificationBadge Implementation

### 1.1 Import the NotificationBadge Component

```tsx
import { NotificationBadge, baseIcons } from "@neuron/ui";
```

### 1.2 Basic Badge Usage

Here are the fundamental ways to use the NotificationBadge component:

```tsx
import { NotificationBadge } from "@neuron/ui";

const BasicBadges = () => {
  return (
    <div className="badge-examples">
      {/* Simple count badge */}
      <NotificationBadge count={5} />

      {/* Badge with custom variant */}
      <NotificationBadge count={12} variant="success" />

      {/* Badge with tooltip */}
      <NotificationBadge count={3} variant="warning" tooltip="You have 3 pending notifications" />

      {/* Count ≤ 0 - badge is not rendered */}
      <NotificationBadge count={0} variant="info" />
      <NotificationBadge count={-5} variant="warning" />
    </div>
  );
};
```

### 1.3 Badge Variants

Use different variants based on the type of notification or status:

```tsx
import { NotificationBadge } from "@neuron/ui";

const BadgeVariants = () => {
  return (
    <div className="badge-variants">
      {/* Danger - errors, critical alerts */}
      <NotificationBadge count={5} variant="danger" />

      {/* Warning - warnings, attention needed */}
      <NotificationBadge count={3} variant="warning" />

      {/* Success - completed actions, positive status */}
      <NotificationBadge count={2} variant="success" />

      {/* Info - general information */}
      <NotificationBadge count={1} variant="info" />

      {/* Brand - brand-related notifications */}
      <NotificationBadge count={7} variant="brand" />

      {/* Negative variants for dark backgrounds */}
      <NotificationBadge count={4} variant="danger-negative" />
      <NotificationBadge count={6} variant="brand-negative" />
    </div>
  );
};
```

**When to use each variant:**

- **danger**: Error notifications, critical alerts, failed operations
- **warning**: Warning messages, attention-required items, pending actions
- **success**: Completed tasks, successful operations, positive confirmations
- **info**: General information, neutral notifications, tips
- **brand**: Brand-specific notifications, promotional content
- **danger-negative/brand-negative**: Use on dark backgrounds or inverse color schemes

## Step 2: Count Formatting and Display Logic

### 2.1 Automatic Count Formatting

The component automatically formats counts for optimal display:

```tsx
import { NotificationBadge } from "@neuron/ui";

const CountFormatting = () => {
  return (
    <div className="count-examples">
      {/* Small numbers - display as-is */}
      <NotificationBadge count={1} variant="danger" />
      <NotificationBadge count={9} variant="warning" />
      <NotificationBadge count={25} variant="success" />
      <NotificationBadge count={99} variant="info" />

      {/* Large numbers - automatic "99+" formatting */}
      <NotificationBadge count={100} variant="brand" />
      <NotificationBadge count={150} variant="danger" />
      <NotificationBadge count={999} variant="info" />
    </div>
  );
};
```

### 2.2 Auto-hiding for Empty States

**Important**: Component is not rendered when count ≤ 0:

```tsx
import { NotificationBadge } from "@neuron/ui";

const AutoHidingBehavior = () => {
  return (
    <div className="auto-hiding-examples">
      {/* These will render */}
      <NotificationBadge count={1} variant="danger" />
      <NotificationBadge count={5} variant="info" />

      {/* These will NOT render (auto-hidden) */}
      <NotificationBadge count={0} variant="warning" />
      <NotificationBadge count={-5} variant="danger" />
    </div>
  );
};
```

## Step 3: Icon Badges

### 3.1 Icon-Only Badges

Use icons to indicate status or presence without counts:

```tsx
import { NotificationBadge, baseIcons } from "@neuron/ui";

const IconBadges = () => {
  return (
    <div className="icon-badges">
      {/* Status indicators */}
      <NotificationBadge
        icon={baseIcons.bellNotificationSolid}
        variant="danger"
        tooltip="New notifications available"
      />

      {/* Warning indicator */}
      <NotificationBadge icon={baseIcons.circleExclamationSolid} variant="warning" tooltip="Action required" />

      {/* Success indicator */}
      <NotificationBadge icon={baseIcons.circleCheckSolid} variant="success" tooltip="All tasks completed" />

      {/* Info indicator */}
      <NotificationBadge icon={baseIcons.circleInfoSolid} variant="info" tooltip="Information available" />
    </div>
  );
};
```

### 3.2 Combined Icon and Count Badges

Combine icons with counts for enhanced visual communication:

```tsx
import { NotificationBadge, baseIcons } from "@neuron/ui";

const CombinedBadges = () => {
  return (
    <div className="combined-badges">
      {/* Notification bell with count */}
      <NotificationBadge
        icon={baseIcons.bellNotificationSolid}
        count={5}
        variant="danger"
        tooltip="5 unread notifications"
      />

      {/* Message icon with count */}
      <NotificationBadge icon={baseIcons.messagesSolid} count={12} variant="info" tooltip="12 unread messages" />

      {/* Task completion with count */}
      <NotificationBadge icon={baseIcons.taskListSolid} count={3} variant="warning" tooltip="3 pending tasks" />

      {/* Email notifications */}
      <NotificationBadge icon={baseIcons.envelopeSolid} count={25} variant="brand" tooltip="25 unread emails" />
    </div>
  );
};
```

## Step 4: Tooltip Integration

### 4.1 Descriptive Tooltips

Provide context and additional information through tooltips:

```tsx
import { NotificationBadge, baseIcons } from "@neuron/ui";

const TooltipBadges = () => {
  return (
    <div className="tooltip-badges">
      {/* Detailed count information */}
      <NotificationBadge count={5} variant="danger" tooltip="5 critical errors require immediate attention" />

      {/* Action-oriented tooltips */}
      <NotificationBadge count={12} variant="warning" tooltip="12 pending approvals - Click to review" />

      {/* Status information */}
      <NotificationBadge icon={baseIcons.circleCheckSolid} variant="success" tooltip="All systems operational" />

      {/* Time-sensitive information */}
      <NotificationBadge count={3} variant="info" tooltip="3 tasks due today" />
    </div>
  );
};
```

## Step 5: NotificationBadge Props Reference

### 5.1 Core Badge Props

| Prop        | Type                           | Default    | Description                                      |
| ----------- | ------------------------------ | ---------- | ------------------------------------------------ |
| `count`     | `number`                       | -          | Numerical count to display (not rendered if ≤ 0) |
| `variant`   | `TBadgeVariants`               | `"danger"` | Visual variant of the badge                      |
| `icon`      | `TBaseIcons \| IconDefinition` | -          | Icon to display in badge                         |
| `tooltip`   | `string`                       | -          | Tooltip text for additional context              |
| `className` | `string`                       | -          | Additional CSS classes                           |

### 5.2 Badge Variants

| Variant           | Use Case                                | Color Scheme                |
| ----------------- | --------------------------------------- | --------------------------- |
| `danger`          | Errors, critical alerts                 | Red background              |
| `danger-negative` | Errors on dark backgrounds              | Red with light text         |
| `warning`         | Warnings, attention needed              | Orange/yellow background    |
| `success`         | Completed tasks, positive status        | Green background            |
| `info`            | General information                     | Blue background             |
| `brand`           | Brand notifications                     | Brand color background      |
| `brand-negative`  | Brand notifications on dark backgrounds | Brand color with light text |

## Step 6: Best Practices

### 6.1 Badge Variant Selection

**Choose appropriate variants based on notification type:**

```tsx
{/* ✅ CORRECT: Semantic variant usage */}
<NotificationBadge count={5} variant="danger" />    {/* Critical errors */}
<NotificationBadge count={3} variant="warning" />   {/* Warnings */}
<NotificationBadge count={2} variant="success" />   {/* Completed items */}
<NotificationBadge count={1} variant="info" />      {/* General info */}
<NotificationBadge count={5} variant="success" />   {/* Successful operations */}
<NotificationBadge count={12} variant="brand" />    {/* Brand notifications */}

{/* ❌ WRONG: Mismatched semantic meaning */}
<NotificationBadge count={5} variant="success" tooltip="5 critical errors" />
<NotificationBadge count={3} variant="danger" tooltip="3 tasks completed" />
```

### 6.2 Tooltip Guidelines

**Provide meaningful, actionable tooltip content:**

```tsx
{/* ✅ CORRECT: Descriptive and actionable */}
<NotificationBadge
  count={5}
  variant="danger"
  tooltip="5 critical errors require immediate attention"
/>

{/* ❌ WRONG: Vague or redundant tooltips */}
<NotificationBadge count={5} tooltip="5 items" />
<NotificationBadge count={3} tooltip="Notifications" />
```

### 6.3 Icon Usage Guidelines

**Use appropriate icons for different badge purposes:**

```tsx
{/* ✅ CORRECT: Semantic icon usage */}
<NotificationBadge icon={baseIcons.bellNotificationSolid} variant="danger" />
<NotificationBadge icon={baseIcons.circleExclamationSolid} variant="warning" />
<NotificationBadge icon={baseIcons.messagesSolid} variant="info" />

{/* ❌ WRONG: Misleading icons */}
<NotificationBadge icon={baseIcons.circleCheckSolid} variant="danger" />
<NotificationBadge icon={baseIcons.trashCanSolid} variant="success" />
```

## Step 7: Common Use Cases

### 7.1 Navigation Badges

Badges in navigation elements and menus:

```tsx
import { NotificationBadge, baseIcons } from "@neuron/ui";

const NavigationWithBadges = () => {
  return (
    <nav className="main-navigation">
      <div className="nav-item">
        <span>Messages</span>
        <NotificationBadge count={5} variant="info" tooltip="5 unread messages" />
      </div>

      <div className="nav-item">
        <span>Notifications</span>
        <NotificationBadge count={12} variant="danger" tooltip="12 urgent notifications" />
      </div>
    </nav>
  );
};
```

### 7.2 Status Indicators

Badges for system and application status:

```tsx
import { NotificationBadge, baseIcons } from "@neuron/ui";

const StatusIndicators = () => {
  return (
    <div className="status-panel">
      <div className="status-item">
        <span>System Health</span>
        <NotificationBadge icon={baseIcons.circleCheckSolid} variant="success" tooltip="All systems operational" />
      </div>

      <div className="status-item">
        <span>Active Users</span>
        <NotificationBadge count={1234} variant="brand" tooltip="1234 users currently online" />
      </div>
    </div>
  );
};
```

## Step 8: Common Mistakes to Avoid

### 8.1 Don't Use FontAwesome Icons Directly

**Always use baseIcons instead of direct FontAwesome imports:**

```tsx
{
  /* ❌ WRONG: Direct FontAwesome usage */
}
import { faBell } from "@fortawesome/pro-solid-svg-icons";
<NotificationBadge icon={faBell} count={5} />;

{
  /* ✅ CORRECT: Use baseIcons */
}
import { baseIcons } from "@neuron/ui";
<NotificationBadge icon={baseIcons.bellNotificationSolid} count={5} />;
```

### 8.2 Don't Use Inappropriate Variants

**Choose variants that match the semantic meaning of your content:**

```tsx
{/* ❌ WRONG: Variant doesn't match tooltip content */}
<NotificationBadge count={5} variant="success" tooltip="5 critical errors" />
<NotificationBadge count={3} variant="danger" tooltip="3 tasks completed successfully" />

{/* ✅ CORRECT: Variant matches content meaning */}
<NotificationBadge count={5} variant="danger" tooltip="5 critical errors" />
<NotificationBadge count={3} variant="success" tooltip="3 tasks completed successfully" />
<NotificationBadge count={10} variant="success" tooltip="10 successful operations" />
```

### 8.3 Don't Ignore Accessibility

**Always provide meaningful tooltips for important badges:**

```tsx
{
  /* ❌ WRONG: Missing context */
}
<NotificationBadge count={5} variant="danger" />;

{
  /* ✅ CORRECT: Descriptive tooltip */
}
<NotificationBadge count={5} variant="danger" tooltip="5 critical errors require immediate attention" />;
```

### 8.4 Don't Expect Rendering for Zero Counts

**Remember that badges with count ≤ 0 are not rendered:**

```tsx
{/* ❌ WRONG: Expecting badge to show */}
<NotificationBadge count={0} variant="info" /> {/* This will not render */}

{/* ✅ CORRECT: Use icon-only for status indicators */}
<NotificationBadge
  icon={baseIcons.circleCheckSolid}
  variant="success"
  tooltip="No pending items"
/>
```

## Key Takeaways

The Neuron NotificationBadge component provides a simple, accessible foundation for notification indicators. Key points to remember:

1. **Use semantic variants** based on notification type (danger, warning, success, info, brand)
2. **Always use baseIcons** instead of direct FontAwesome imports
3. **Provide meaningful tooltips** for context and accessibility
4. **Remember auto-hiding behavior** - badges with count ≤ 0 are not rendered
5. **Use icon-only badges** for status indicators without counts
6. **Automatic count formatting** - displays "99+" for counts over 99
7. **Choose appropriate variants** that match notification severity
8. **Combine icons and counts** when both visual and numerical information are needed
9. **Use negative variants** for dark backgrounds
10. **Test with different count values** to ensure proper display behavior

By following these guidelines, you'll create consistent, accessible, and informative notification indicators across your Neuron applications.

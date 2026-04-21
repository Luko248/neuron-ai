---
agent: agent
---

# AI-Assisted Neuron InnerAction Component Integration Guide

## For the AI Assistant

Your task is to integrate and configure the Neuron InnerAction component in React applications. This guide provides comprehensive instructions for implementing InnerAction components, which serve as specialized link/button elements primarily designed for use within popover components and other container contexts.

## Sync Metadata

- **Component Version:** v3.0.1
- **Component Source:** `packages/neuron/ui/src/lib/helpers/innerAction/InnerAction.tsx`
- **Guideline Command:** `/neuron-ui-inneraction`
- **Related Skill:** `neuron-ui-helpers`

## Introduction

The InnerAction component is a specialized helper component that combines the functionality of a Link component with additional features like notification badges and selection states. It is primarily designed for use within popover components, dropdown menus, and other container contexts where you need interactive action items.

### What is the InnerAction Component?

The InnerAction component provides specialized action functionality with support for:

- **Link/Button hybrid** - Functions as both link and button depending on props
- **Navigation support** - href and to props for external and internal navigation
- **Click handlers** - onClick support for custom actions
- **Icon integration** - Left and right icon support from baseIcons
- **Selection states** - Visual indication of selected/active states
- **Notification badges** - Built-in notification badge with count and tooltip
- **Accessibility** - Full keyboard navigation and screen reader support
- **Test integration** - Built-in test ID support for automated testing

### Key Features

- **Dual Functionality**: Works as both link and button based on provided props
- **Navigation Integration**: Support for both external (href) and internal (to) navigation
- **Selection States**: Visual indication of active/selected items
- **Notification System**: Built-in notification badges with count and tooltip
- **Icon Support**: Left and right icon positioning with baseIcons integration
- **Container Optimized**: Specifically designed for use within popovers and menus
- **Accessibility Compliant**: Full keyboard navigation and screen reader support
- **Framework Integration**: Used internally by MenuUser and QuickActionsDialog components

**Storybook Reference:** You can refer to the Storybook documentation for visual examples and interactive demos of the InnerAction component.

**Important Note:** InnerAction is primarily designed for use within Neuron UI components like Popover, MenuUser, and QuickActionsDialog. While it can be used in custom content, it's optimized for container contexts rather than standalone usage.

## Step 1: Basic InnerAction Implementation

### 1.1 Import the InnerAction Component

```tsx
import { InnerAction } from "@neuron/ui";
```

### 1.2 Basic InnerAction Usage

Here's a simple implementation of the InnerAction component:

```tsx
import { InnerAction } from "@neuron/ui";

const BasicInnerActionExample = () => {
  const handleClick = () => {
    console.log("Action clicked");
  };

  return (
    <div>
      {/* Basic click action */}
      <InnerAction onClick={handleClick}>Basic Action</InnerAction>

      {/* Navigation action */}
      <InnerAction href="https://example.com" target="_blank">
        External Link
      </InnerAction>

      {/* Internal navigation */}
      <InnerAction to="/dashboard">Go to Dashboard</InnerAction>
    </div>
  );
};
```

### 1.3 InnerAction with Icons

Add icons to enhance visual context:

```tsx
import { InnerAction, baseIcons } from "@neuron/ui";

const IconInnerActionExample = () => {
  return (
    <div>
      {/* Left icon */}
      <InnerAction onClick={() => console.log("Edit clicked")} iconLeft={baseIcons.editSolid}>
        Edit Item
      </InnerAction>

      {/* Right icon */}
      <InnerAction to="/settings" iconRight={baseIcons.chevronRightSolid}>
        Settings
      </InnerAction>

      {/* Both icons */}
      <InnerAction href="/download" iconLeft={baseIcons.downloadSolid} iconRight={baseIcons.externalLinkSolid}>
        Download File
      </InnerAction>
    </div>
  );
};
```

## Step 2: InnerAction in Popover Context

### 2.1 Popover Action Menu

Use InnerAction within Popover components for action menus:

```tsx
import { InnerAction, Popover, PopoverRefType, Button, baseIcons } from "@neuron/ui";
import { useRef } from "react";

const PopoverActionMenuExample = () => {
  const popoverRef = useRef<PopoverRefType>(null);

  const handleEdit = () => {
    console.log("Edit action");
    popoverRef.current?.hide();
  };

  const handleDelete = () => {
    console.log("Delete action");
    popoverRef.current?.hide();
  };

  return (
    <div>
      <Button onClick={(e) => popoverRef.current?.toggle(e)}>Item Actions</Button>

      <Popover ref={popoverRef}>
        <InnerAction onClick={handleEdit} iconLeft={baseIcons.editSolid}>
          Edit Item
        </InnerAction>

        <InnerAction onClick={handleDelete} iconLeft={baseIcons.trashSolid}>
          Delete Item
        </InnerAction>
      </Popover>
    </div>
  );
};
```

### 2.2 User Menu with InnerAction

Create user menus similar to MenuUser component:

```tsx
import { InnerAction, Popover, PopoverRefType, Button, baseIcons } from "@neuron/ui";
import { useRef } from "react";

const UserMenuExample = () => {
  const popoverRef = useRef<PopoverRefType>(null);

  const handleLogout = () => {
    console.log("Logout action");
    popoverRef.current?.hide();
  };

  return (
    <div>
      <Button onClick={(e) => popoverRef.current?.toggle(e)} iconLeft={baseIcons.userSolid}>
        John Doe
      </Button>

      <Popover ref={popoverRef} showCloseIcon>
        <InnerAction to="/profile" iconLeft={baseIcons.userSolid}>
          My Profile
        </InnerAction>

        <InnerAction to="/settings" iconLeft={baseIcons.gearSolid}>
          Account Settings
        </InnerAction>

        <InnerAction
          to="/notifications"
          iconLeft={baseIcons.bellSolid}
          notificationCount={3}
          notificationTooltip="3 new notifications"
        >
          Notifications
        </InnerAction>

        <InnerAction onClick={handleLogout} iconLeft={baseIcons.signOutSolid}>
          Sign Out
        </InnerAction>
      </Popover>
    </div>
  );
};
```

## Step 3: Selection States and Notifications

### 3.1 Selection State

Use selection state to indicate active items:

```tsx
import { InnerAction, baseIcons } from "@neuron/ui";
import { useState } from "react";

const SelectionStateExample = () => {
  const [selectedItem, setSelectedItem] = useState<string>("dashboard");

  const menuItems = [
    { id: "dashboard", label: "Dashboard", icon: baseIcons.chartLineSolid },
    { id: "users", label: "Users", icon: baseIcons.usersSolid },
    { id: "settings", label: "Settings", icon: baseIcons.gearSolid },
  ];

  return (
    <div>
      {menuItems.map((item) => (
        <InnerAction
          key={item.id}
          selected={selectedItem === item.id}
          iconLeft={item.icon}
          onClick={() => setSelectedItem(item.id)}
        >
          {item.label}
        </InnerAction>
      ))}
    </div>
  );
};
```

### 3.2 Notification Badges

Add notification badges to indicate counts or alerts:

```tsx
import { InnerAction, baseIcons } from "@neuron/ui";

const NotificationBadgeExample = () => {
  return (
    <div>
      <InnerAction
        to="/messages"
        iconLeft={baseIcons.envelopeSolid}
        notificationCount={5}
        notificationTooltip="5 new messages"
      >
        Messages
      </InnerAction>

      <InnerAction
        to="/alerts"
        iconLeft={baseIcons.bellSolid}
        notificationCount={12}
        notificationTooltip="12 new notifications"
        selected={true}
      >
        Notifications
      </InnerAction>
    </div>
  );
};
```

## Step 4: Props Reference

### 4.1 InnerActionProps Interface

| Prop                  | Type        | Default | Description                             |
| --------------------- | ----------- | ------- | --------------------------------------- |
| `children`            | `ReactNode` | -       | Content to display in the action        |
| `className`           | `string`    | -       | Additional CSS classes                  |
| `selected`            | `boolean`   | `false` | Whether the action is in selected state |
| `notificationCount`   | `number`    | -       | Number to display in notification badge |
| `notificationTooltip` | `string`    | -       | Tooltip text for notification badge     |
| `testId`              | `string`    | -       | Custom test ID for the component        |

**Inherited from Link:**

- `href` - External URL navigation
- `to` - Internal route navigation
- `onClick` - Click handler function
- `disabled` - Disabled state
- `iconLeft` - Left icon from baseIcons
- `iconRight` - Right icon from baseIcons
- `target` - Link target attribute

## Step 5: Best Practices

### 5.1 Usage Guidelines

**Do:**

- Use InnerAction primarily within container components like Popover
- Provide clear, descriptive text for each action
- Use appropriate icons that enhance understanding
- Handle action completion by closing containing popovers
- Use notification badges sparingly and meaningfully

**Don't:**

- Use InnerAction as a replacement for regular Button or Link components in main content
- Overcrowd containers with too many InnerActions
- Use unclear or ambiguous action text
- Forget to close popovers after action completion

### 5.2 Accessibility

The InnerAction component includes built-in accessibility features:

- Proper link/button semantics based on props
- Full keyboard navigation support
- Screen reader compatibility
- Focus management

## Summary

The Neuron InnerAction component provides specialized action functionality optimized for container contexts with support for:

- **Dual Functionality**: Link and button capabilities based on props
- **Container Optimization**: Designed for use within popovers and menus
- **Selection States**: Visual indication of active/selected items
- **Notification System**: Built-in notification badges with tooltips
- **Icon Integration**: Left and right icon support with baseIcons
- **Framework Integration**: Used by core Neuron components like MenuUser and QuickActionsDialog
- **Accessibility**: Full keyboard navigation and screen reader support

Use InnerAction strategically within container components like Popover for action menus, user menus, and quick action dialogs.

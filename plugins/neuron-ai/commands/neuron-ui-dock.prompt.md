---
agent: agent
---

# AI-Assisted Neuron Dock Component Integration Guide

## For the AI Assistant

Your task is to understand and implement the Dock component within Neuron FE applications. This guide provides step-by-step instructions for working with the Dock component, which is a floating action button container typically positioned on the right side of the screen.

## Sync Metadata

- **Component Version:** v3.1.3
- **Component Source:** `packages/neuron/ui/src/lib/helpers/dock/Dock.tsx`
- **Guideline Command:** `/neuron-ui-dock`
- **Related Skill:** `neuron-ui-layout`

## Overview

The Dock component is a helper component that displays a stack of action buttons on the right side of the page. It's designed to provide quick access to important actions without cluttering the main UI. The Dock is integrated with the Layout system but can also be used independently.

Key features:

- Floating action buttons container
- Automatically hidden when empty
- Integrated with Layout component
- Supports notification badges
- Customizable positioning and styling

## Step 1: Understanding Dock Components

The Dock system consists of two main components:

1. **Dock**: The container component that holds the buttons
2. **DockButton**: Individual action buttons that are rendered inside the Dock using React portals

### What is DockButton?

DockButton is a specialized component that creates floating action buttons within the Dock container. It provides:

- **Portal-based rendering** - Buttons are rendered into the Dock container from anywhere in the component tree
- **Automatic cleanup** - Buttons remove themselves when components unmount
- **Notification support** - Built-in notification badge functionality
- **State management** - Active state tracking and visual feedback
- **Layout integration** - Seamless integration with the Layout system
- **baseIcons support** - Uses the Neuron icon system for consistent iconography

**IMPORTANT**: DockButton is strictly used only within the Dock component system and should not be used independently.

## Step 2: Basic Implementation

### 2.1 Using Dock with Layout

When using the Layout component, the Dock is already included by default. The Layout component creates a Dock internally and stores its reference in the LayoutContext. You don't need to add it manually.

```tsx
import { Layout, LayoutProvider } from "@neuron/ui";

const App = () => {
  return (
    <LayoutProvider>
      <Layout>{/* Your content here */}</Layout>
      {/* No need to add Dock here, it's already included in Layout */}
    </LayoutProvider>
  );
};
```

### 2.2 Using Dock Independently

If you need to use Dock outside of the Layout component:

```tsx
import { Dock, LayoutProvider } from "@neuron/ui";

const StandaloneDock = () => {
  return (
    <LayoutProvider>
      <div style={{ position: "relative", height: "100vh" }}>
        {/* Your content here */}
        <Dock />
      </div>
    </LayoutProvider>
  );
};
```

**Important**: The Dock component must always be wrapped in a LayoutProvider.

## Step 3: Adding Buttons to the Dock

You can add buttons to the Dock from anywhere in your application using the DockButton component:

```tsx
import { DockButton, baseIcons } from "@neuron/ui";

const MyComponent = () => {
  const handleClick = () => {
    console.log("Button clicked");
  };

  return (
    <>
      {/* Your component content */}

      {/* Add a button to the Layout dock */}
      <DockButton icon={baseIcons.circlePlusSolid} onClick={handleClick} tooltip="Add new item" title="Add Item" />

      {/* Add a button with notification count */}
      <DockButton
        icon={baseIcons.bellNotificationSolid}
        onClick={handleClick}
        notificationCount={5}
        tooltip="Notifications"
        title="Notifications"
      />
    </>
  );
};
```

### 3.1 DockButton Props Reference

| Prop              | Type                                   | Default | Description                                 |
| ----------------- | -------------------------------------- | ------- | ------------------------------------------- |
| icon              | `TBaseIcons \| IconDefinition`         | -       | Icon to display (use baseIcons)             |
| title             | `string`                               | -       | Button title text                           |
| onClick           | `MouseEventHandler<HTMLButtonElement>` | -       | Click event handler                         |
| tooltip           | `string`                               | -       | Tooltip text for accessibility              |
| active            | `boolean`                              | `false` | Whether button is in active state           |
| disabled          | `boolean`                              | `false` | Whether button is disabled                  |
| loading           | `boolean`                              | `false` | Whether button shows loading state          |
| notificationCount | `number`                               | -       | Number for notification badge               |
| customDockElement | `HTMLDivElement`                       | -       | Custom dock element reference               |
| dockId            | `string`                               | -       | **Deprecated** - Use Layout context instead |
| className         | `string`                               | -       | Additional CSS classes                      |
| testId            | `string`                               | -       | Custom test ID                              |

### 3.2 DockButton Implementation Patterns

```tsx
import { DockButton, baseIcons } from "@neuron/ui";
import { useState } from "react";

const DockButtonExamples = () => {
  const [isFilterActive, setIsFilterActive] = useState(false);
  const [notificationCount, setNotificationCount] = useState(3);

  return (
    <>
      {/* Basic DockButton */}
      <DockButton
        icon={baseIcons.houseSolid}
        title="Home"
        tooltip="Go to home page"
        onClick={() => console.log("Home clicked")}
      />

      {/* Active state DockButton */}
      <DockButton
        icon={baseIcons.filterSolid}
        title="Filter"
        tooltip="Toggle filters"
        active={isFilterActive}
        onClick={() => setIsFilterActive(!isFilterActive)}
      />

      {/* DockButton with notification */}
      <DockButton
        icon={baseIcons.messagesSolid}
        title="Messages"
        tooltip="View messages"
        notificationCount={notificationCount}
        onClick={() => setNotificationCount(0)}
      />

      {/* Disabled DockButton */}
      <DockButton
        icon={baseIcons.gearSolid}
        title="Settings"
        tooltip="Settings (unavailable)"
        disabled={true}
        onClick={() => console.log("Won't fire")}
      />

      {/* Loading DockButton */}
      <DockButton
        icon={baseIcons.arrowsRotateSolid}
        title="Refresh"
        tooltip="Refreshing data..."
        loading={true}
        onClick={() => console.log("Refreshing")}
      />
    </>
  );
};
```

## Step 4: Advanced Usage

### 4.1 Using a Custom Dock

If you need to use a custom Dock separate from the one in Layout, you can create your own Dock instance. However, you'll need to ensure the DockButtons can find it:

```tsx
import { Dock, DockButton, baseIcons } from "@neuron/ui";
import { useRef, useEffect } from "react";

const CustomDockExample = () => {
  const dockRef = useRef(null);

  return (
    <div style={{ position: "relative", height: "100vh" }}>
      {/* Create a custom dock */}
      <Dock ref={dockRef} />

      {/* Target it with buttons using customDockElement */}
      <DockButton customDockElement={dockRef.current} icon={baseIcons.plus} onClick={() => console.log("Clicked")} />
    </div>
  );
};
```

**Note**: The `dockId` prop is deprecated. For newer applications, use the Layout context approach or the `customDockElement` prop.

### 4.2 Using Layout Context (Recommended)

The modern approach uses the Layout context to find the dock element:

```tsx
import { DockButton, useContext, LayoutContext } from "@neuron/ui";

const MyComponent = () => {
  const { dockElement } = useContext(LayoutContext);

  return <DockButton customDockElement={dockElement} icon={baseIcons.plus} onClick={handleClick} />;
};
```

### 4.3 Button States

DockButtons support active states and loading states:

```tsx
// Active button
<DockButton
  icon={baseIcons.filter}
  onClick={handleClick}
  active={isFilterActive}
/>

// Loading button
<DockButton
  icon={baseIcons.refresh}
  onClick={handleRefresh}
  loading={isLoading}
/>
```

### 4.4 Notification Badges

You can display notification counts on DockButtons:

```tsx
<DockButton icon={baseIcons.bellNotificationSolid} onClick={openNotifications} notificationCount={unreadCount} />
```

## Step 5: Cleanup

When using DockButtons in components that may unmount, you don't need to worry about explicit cleanup. DockButtons automatically remove themselves from the Dock when they unmount:

```tsx
import { DockButton, baseIcons } from "@neuron/ui";

const TemporaryComponent = () => {
  // No manual cleanup needed for DockButton
  return <DockButton icon={baseIcons.plus} onClick={() => console.log("Clicked")} tooltip="Add item" />;
};
```

This is because DockButton uses React's component lifecycle to handle its own cleanup when it unmounts.

## Step 6: Common DockButton Use Cases

### 6.1 Navigation Actions

```tsx
import { DockButton, baseIcons } from "@neuron/ui";
import { useNavigate } from "react-router-dom";

const NavigationDockButtons = () => {
  const navigate = useNavigate();

  return (
    <>
      <DockButton
        icon={baseIcons.houseSolid}
        title="Home"
        tooltip="Go to dashboard"
        onClick={() => navigate("/dashboard")}
      />

      <DockButton
        icon={baseIcons.personSolid}
        title="Profile"
        tooltip="View your profile"
        onClick={() => navigate("/profile")}
      />
    </>
  );
};
```

### 6.2 Action Buttons with State

```tsx
import { DockButton, baseIcons } from "@neuron/ui";
import { useState } from "react";

const StatefulDockButtons = () => {
  const [isFavorite, setIsFavorite] = useState(false);
  const [isEditing, setIsEditing] = useState(false);

  return (
    <>
      <DockButton
        icon={isFavorite ? baseIcons.heartSolid : baseIcons.heartRegular}
        title="Favorite"
        tooltip={isFavorite ? "Remove from favorites" : "Add to favorites"}
        active={isFavorite}
        onClick={() => setIsFavorite(!isFavorite)}
      />

      <DockButton
        icon={baseIcons.penSolid}
        title="Edit"
        tooltip={isEditing ? "Stop editing" : "Start editing"}
        active={isEditing}
        onClick={() => setIsEditing(!isEditing)}
      />
    </>
  );
};
```

### 6.3 Notification and Communication

```tsx
import { DockButton, baseIcons } from "@neuron/ui";
import { useNotifications, useMessages } from "./hooks";

const CommunicationDockButtons = () => {
  const { unreadNotifications, markAllAsRead } = useNotifications();
  const { unreadMessages, openMessages } = useMessages();

  return (
    <>
      <DockButton
        icon={baseIcons.bellNotificationSolid}
        title="Notifications"
        tooltip={`${unreadNotifications} unread notifications`}
        notificationCount={unreadNotifications}
        onClick={markAllAsRead}
      />

      <DockButton
        icon={baseIcons.messagesSolid}
        title="Messages"
        tooltip="Open messages"
        notificationCount={unreadMessages}
        onClick={openMessages}
      />
    </>
  );
};
```

### 6.4 Conditional DockButtons

```tsx
import { DockButton, baseIcons } from "@neuron/ui";
import { useAuth, usePermissions } from "./hooks";

const ConditionalDockButtons = () => {
  const { user } = useAuth();
  const { canEdit, canDelete } = usePermissions();

  return (
    <>
      {/* Only show for authenticated users */}
      {user && (
        <DockButton
          icon={baseIcons.gearSolid}
          title="Settings"
          tooltip="User settings"
          onClick={() => console.log("Open settings")}
        />
      )}

      {/* Only show if user has edit permissions */}
      {canEdit && (
        <DockButton
          icon={baseIcons.penSolid}
          title="Edit"
          tooltip="Edit current item"
          onClick={() => console.log("Start editing")}
        />
      )}

      {/* Only show if user has delete permissions */}
      {canDelete && (
        <DockButton
          icon={baseIcons.trashCanSolid}
          title="Delete"
          tooltip="Delete current item"
          onClick={() => console.log("Delete item")}
        />
      )}
    </>
  );
};
```

## Step 7: Styling

The Dock component uses the following CSS classes that you can target for custom styling:

- `.dock`: The main container
- `.dock__btn`: The button container within the dock

## Step 7: DockButton Best Practices

### 7.1 Icon and Accessibility Guidelines

```tsx
// ✅ CORRECT: Use baseIcons with meaningful tooltips
<DockButton
  icon={baseIcons.circlePlusSolid}
  title="Add New Item"
  tooltip="Create a new item in the current context"
  onClick={handleAdd}
/>

// ❌ WRONG: Missing tooltip and title
<DockButton
  icon={baseIcons.circlePlusSolid}
  onClick={handleAdd}
/>
```

### 7.2 State Management

```tsx
// ✅ CORRECT: Proper active state management
const [isFilterActive, setIsFilterActive] = useState(false);

<DockButton
  icon={baseIcons.filterSolid}
  title="Filter"
  tooltip={isFilterActive ? "Hide filters" : "Show filters"}
  active={isFilterActive}
  onClick={() => setIsFilterActive(!isFilterActive)}
/>

// ❌ WRONG: No state feedback
<DockButton
  icon={baseIcons.filterSolid}
  title="Filter"
  onClick={toggleFilter}
/>
```

### 7.3 Notification Count Usage

```tsx
// ✅ CORRECT: Meaningful notification counts
<DockButton
  icon={baseIcons.bellNotificationSolid}
  title="Notifications"
  tooltip={`${unreadCount} unread notifications`}
  notificationCount={unreadCount > 0 ? unreadCount : undefined}
  onClick={openNotifications}
/>

// ❌ WRONG: Always showing zero count
<DockButton
  icon={baseIcons.bellNotificationSolid}
  notificationCount={0}
  onClick={openNotifications}
/>
```

### 7.4 General Best Practices

1. **Use meaningful icons**

   - Choose baseIcons that clearly represent the action
   - Add tooltips to provide additional context

2. **Limit the number of buttons**

   - The Dock should contain only the most important actions
   - Too many buttons can clutter the interface

3. **Handle active states**

   - Use the `active` prop to indicate when a button's action is in effect
   - This provides visual feedback to users

4. **DockButtons clean up automatically**

   - DockButtons automatically remove themselves when unmounted
   - No manual cleanup is required

5. **Use notification badges sparingly**

   - Only show notification counts for important updates
   - Keep the counts accurate and up-to-date

6. **Always provide titles and tooltips**
   - Essential for accessibility and user understanding
   - Tooltips should be descriptive and contextual

## Troubleshooting

### Dock buttons not appearing

1. Ensure the Layout component is mounted before adding DockButtons
2. Verify that the DockButton is correctly accessing the dock element (via LayoutContext or customDockElement)
3. Check that both Layout/Dock and DockButton are wrapped in the same LayoutProvider

### Console warnings

- `DockButton: dockId is deprecated`: Update to use the Layout context approach
- `DockButton: Element not found`: The dock element reference is not available yet

## API Reference

### Dock Props

| Prop      | Type     | Default        | Description                      |
| --------- | -------- | -------------- | -------------------------------- |
| className | `string` | -              | Additional CSS classes           |
| htmlId    | `string` | `"layoutDock"` | HTML ID attribute for the dock   |
| testId    | `string` | -              | Custom test ID for the component |

### DockButton Props

| Prop              | Type                                   | Default | Required | Description                                 |
| ----------------- | -------------------------------------- | ------- | -------- | ------------------------------------------- |
| icon              | `TBaseIcons \| IconDefinition`         | -       | ✅       | Button icon (use baseIcons)                 |
| title             | `string`                               | -       | ❌       | Button title text                           |
| onClick           | `MouseEventHandler<HTMLButtonElement>` | -       | ❌       | Click event handler                         |
| tooltip           | `string`                               | -       | ❌       | Tooltip text for accessibility              |
| active            | `boolean`                              | `false` | ❌       | Whether button is in active state           |
| disabled          | `boolean`                              | `false` | ❌       | Whether button is disabled                  |
| loading           | `boolean`                              | `false` | ❌       | Whether button shows loading state          |
| notificationCount | `number`                               | -       | ❌       | Number for notification badge               |
| customDockElement | `HTMLDivElement`                       | -       | ❌       | Custom dock element reference               |
| dockId            | `string`                               | -       | ❌       | **Deprecated** - Use Layout context instead |
| className         | `string`                               | -       | ❌       | Additional CSS classes                      |
| testId            | `string`                               | -       | ❌       | Custom test ID for the component            |

### DockButton Inherited Props

DockButton extends Button component props and inherits:

| Prop | Type                              | Default    | Description           |
| ---- | --------------------------------- | ---------- | --------------------- |
| type | `"button" \| "submit" \| "reset"` | `"button"` | Button type attribute |

**Note**: DockButton automatically sets `variant="secondary"` and `size="small"` and cannot be overridden.

## DockButton Key Takeaways

The Neuron DockButton component provides floating action button functionality within the Dock system:

1. **Portal-based Rendering** - Buttons render into Dock container from anywhere in the component tree
2. **Automatic Cleanup** - Components remove themselves when unmounted
3. **Layout Integration** - Seamlessly works with the Layout system
4. **baseIcons Required** - Always use baseIcons for consistent iconography
5. **Accessibility First** - Always provide meaningful titles and tooltips
6. **State Management** - Proper active state handling for toggle functionality
7. **Notification Support** - Built-in notification badge functionality
8. **Strict Usage** - Only used within the Dock component system

**Remember**: DockButton is strictly used only within the Dock component system and should not be used independently from the Dock container.

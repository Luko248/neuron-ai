---
agent: agent
---

# AI-Assisted Neuron HorizontalList Component Integration Guide

## For the AI Assistant

Your task is to integrate and configure the Neuron HorizontalList component in React applications. This guide provides comprehensive instructions for implementing HorizontalList components, which serve as horizontal displays of items with support for badges, events, tooltips, icons, and various interaction patterns.

## Sync Metadata

- **Component Version:** v4.2.3
- **Component Source:** `packages/neuron/ui/src/lib/data/horizontalList/HorizontalList.tsx`
- **Guideline Command:** `/neuron-ui-horizontallist`
- **Related Skill:** `neuron-ui-data`

## Introduction

The HorizontalList component is a versatile UI element designed to display a collection of items in a horizontal layout. It serves as an efficient way to present lists of related information, navigation items, tags, or any collection of data that benefits from horizontal presentation with optional notification badges.

### What is the HorizontalList Component?

The HorizontalList component provides horizontal item display with support for:

- **Flexible item types** - Simple strings or complex objects with settings
- **Semantic states** - Support for info, warning, danger, and success states
- **Notification badges** - Count-based or event-based badge display with semantic variants
- **Item customization** - Icons, tooltips, links, highlighting, and semantic styling
- **Interactive modes** - Items can be read-only or interactive (links/click events)
- **Variant styling** - Different visual presentations (default, compact, etc.)
- **Event integration** - NotificationBadge integration for event-driven badges
- **Accessibility** - Built-in accessibility features and test ID support

### Key Features

- **Flexible Item Configuration**: Support for both simple strings and complex item objects
- **Semantic State Support**: Built-in support for info, warning, danger, and success states
- **Badge Integration**: Count-based and event-based notification badges with semantic variants
- **Rich Item Properties**: Icons, tooltips, links, highlighting, and semantic styling support
- **Interactive Modes**: Items can be read-only display or interactive (href links or click handlers)
- **Variant System**: Multiple visual presentations for different contexts
- **Event-Driven Badges**: Integration with event definitions for dynamic badges
- **Accessibility Compliant**: Full keyboard navigation and screen reader support
- **Test Integration**: Built-in test ID support for automated testing

**Storybook Reference:** You can refer to the Storybook documentation for visual examples and interactive demos of the HorizontalList component.

## Step 1: Basic HorizontalList Implementation

### 1.1 Import the HorizontalList Component

```tsx
import { HorizontalList } from "@neuron/ui";
```

### 1.2 Basic HorizontalList Usage

Here's a simple implementation with string items:

```tsx
import { HorizontalList } from "@neuron/ui";

const BasicHorizontalListExample = () => {
  const simpleItems = ["Item 1", "Item 2", "Item 3", "Item 4"];

  return (
    <div>
      <h3>Basic Horizontal List</h3>
      <HorizontalList items={simpleItems} />
    </div>
  );
};
```

### 1.3 HorizontalList with Badge

Add notification badges to indicate counts or status:

```tsx
import { HorizontalList } from "@neuron/ui";

const BadgeHorizontalListExample = () => {
  const items = ["Notifications", "Messages", "Updates"];

  return (
    <div>
      <h3>List with Badge</h3>
      <HorizontalList items={items} badge={5} />
    </div>
  );
};
```

### 1.4 HorizontalList with Variants

Use different variants for various visual presentations:

```tsx
import { HorizontalList } from "@neuron/ui";

const VariantHorizontalListExample = () => {
  const items = ["Home", "Products", "Services", "Contact"];

  return (
    <div>
      {/* Default variant */}
      <div>
        <h4>Default Variant</h4>
        <HorizontalList items={items} variant="default" />
      </div>

      {/* Compact variant */}
      <div>
        <h4>Compact Variant</h4>
        <HorizontalList items={items} variant="compact" />
      </div>
    </div>
  );
};
```

## Step 2: Semantic States and Interactive Modes

### 2.1 Semantic State Variants

HorizontalList supports semantic states for both the component variant and badge notifications:

```tsx
import { HorizontalList } from "@neuron/ui";

const SemanticStatesExample = () => {
  const items = ["System Status", "Alerts", "Notifications"];

  // Event definitions with semantic variants
  const semanticEvents = {
    info: {
      count: 3,
      variant: "info" as const,
      tooltip: "3 informational messages",
    },
    warning: {
      count: 2,
      variant: "warning" as const,
      tooltip: "2 warning alerts",
    },
    danger: {
      count: 1,
      variant: "danger" as const,
      tooltip: "1 critical error",
    },
    success: {
      count: 5,
      variant: "success" as const,
      tooltip: "5 successful operations",
    },
  };

  return (
    <div>
      {/* Info state */}
      <div>
        <h4>Info State</h4>
        <HorizontalList items={items} event="info" eventsDefinition={semanticEvents} />
      </div>

      {/* Warning state */}
      <div>
        <h4>Warning State</h4>
        <HorizontalList items={items} event="warning" eventsDefinition={semanticEvents} />
      </div>

      {/* Danger state */}
      <div>
        <h4>Danger State</h4>
        <HorizontalList items={items} event="danger" eventsDefinition={semanticEvents} />
      </div>

      {/* Success state */}
      <div>
        <h4>Success State</h4>
        <HorizontalList items={items} event="success" eventsDefinition={semanticEvents} />
      </div>
    </div>
  );
};
```

### 2.2 Interactive vs Read-Only Items

Items can be configured as read-only display elements or interactive elements with links or click handlers:

```tsx
import { HorizontalList, baseIcons } from "@neuron/ui";

const InteractiveModeExample = () => {
  const handleItemClick = (itemText: string) => {
    console.log(`Clicked on: ${itemText}`);
    // Perform custom action
  };

  const mixedItems = [
    // Read-only items (display only)
    {
      text: "Read-Only Status",
      tooltip: "This item is for display only",
      icon: baseIcons.infoCircleSolid,
      highlighted: false,
    },

    // Interactive item with href (navigation)
    {
      text: "External Link",
      tooltip: "Navigate to external page",
      icon: baseIcons.linkSolid,
      href: "https://example.com",
      highlighted: false,
    },

    // Interactive item with click handler (custom action)
    {
      text: "Action Item",
      tooltip: "Click to perform action",
      icon: baseIcons.mouseSolid,
      onClick: () => handleItemClick("Action Item"),
      highlighted: true,
    },

    // Simple read-only string
    "Simple Display Item",
  ];

  return (
    <div>
      <h3>Interactive vs Read-Only Items</h3>
      <HorizontalList items={mixedItems} />
      <div>
        <p>
          <strong>Item Types:</strong>
        </p>
        <ul>
          <li>
            <strong>Read-Only:</strong> Display information without interaction
          </li>
          <li>
            <strong>Link (href):</strong> Navigate to URLs or routes
          </li>
          <li>
            <strong>Click Handler:</strong> Execute custom JavaScript functions
          </li>
        </ul>
      </div>
    </div>
  );
};
```

## Step 3: Advanced Item Configuration

### 3.1 Complex Item Objects

Use complex item objects for rich functionality:

```tsx
import { HorizontalList, baseIcons } from "@neuron/ui";

const ComplexItemsExample = () => {
  const advancedItems = [
    {
      text: "Dashboard",
      tooltip: "Go to main dashboard",
      icon: baseIcons.chartLineSolid,
      href: "/dashboard",
      highlighted: true,
    },
    {
      text: "Profile",
      tooltip: "View your profile",
      icon: baseIcons.userSolid,
      href: "/profile",
      highlighted: false,
    },
    {
      text: "Settings",
      tooltip: "Application settings",
      icon: baseIcons.gearSolid,
      href: "/settings",
      highlighted: false,
    },
    {
      text: "Help",
      tooltip: "Get help and support",
      icon: baseIcons.questionCircleSolid,
      href: "/help",
      highlighted: false,
    },
  ];

  return (
    <div>
      <h3>Advanced Items with Icons and Links</h3>
      <HorizontalList items={advancedItems} />
    </div>
  );
};
```

### 2.2 Mixed Item Types

Combine simple strings and complex objects:

```tsx
import { HorizontalList, baseIcons } from "@neuron/ui";

const MixedItemsExample = () => {
  const mixedItems = [
    "Simple Item",
    {
      text: "Complex Item",
      tooltip: "This item has additional properties",
      icon: baseIcons.starSolid,
      highlighted: true,
    },
    "Another Simple Item",
    {
      text: "Link Item",
      href: "https://example.com",
      tooltip: "External link",
    },
  ];

  return (
    <div>
      <h3>Mixed Item Types</h3>
      <HorizontalList items={mixedItems} />
    </div>
  );
};
```

### 2.3 Highlighted Items

Use highlighting to emphasize important items:

```tsx
import { HorizontalList } from "@neuron/ui";

const HighlightedItemsExample = () => {
  const itemsWithHighlights = [
    { text: "Regular Item", highlighted: false },
    { text: "Important Item", highlighted: true, tooltip: "This item is highlighted" },
    { text: "Another Regular Item", highlighted: false },
    { text: "Featured Item", highlighted: true, tooltip: "Featured content" },
  ];

  return (
    <div>
      <h3>Items with Highlighting</h3>
      <HorizontalList items={itemsWithHighlights} />
    </div>
  );
};
```

## Step 3: Event-Based Badges

### 3.1 Event-Driven Notification Badges

Use event-based badges for dynamic notifications:

```tsx
import { HorizontalList } from "@neuron/ui";

const EventBadgeExample = () => {
  const items = ["Inbox", "Sent", "Drafts", "Archive"];

  // Event definitions for different notification types
  const eventsDefinition = {
    newMessage: {
      count: 3,
      variant: "danger" as const,
      tooltip: "You have new messages",
    },
    draft: {
      count: 1,
      variant: "warning" as const,
      tooltip: "You have unsaved drafts",
    },
  };

  return (
    <div>
      <h3>List with Event-Based Badge</h3>
      <HorizontalList items={items} event="newMessage" eventsDefinition={eventsDefinition} />
    </div>
  );
};
```

### 3.2 Dynamic Event Handling

Handle dynamic events based on application state:

```tsx
import { HorizontalList } from "@neuron/ui";
import { useState, useEffect } from "react";

const DynamicEventExample = () => {
  const [currentEvent, setCurrentEvent] = useState<string | undefined>("notifications");
  const [eventCounts, setEventCounts] = useState({
    notifications: 5,
    messages: 2,
    alerts: 0,
  });

  const items = ["Dashboard", "Notifications", "Messages", "Alerts"];

  const eventsDefinition = {
    notifications: {
      count: eventCounts.notifications,
      variant: "info" as const,
      tooltip: `${eventCounts.notifications} new notifications`,
    },
    messages: {
      count: eventCounts.messages,
      variant: "success" as const,
      tooltip: `${eventCounts.messages} new messages`,
    },
    alerts: {
      count: eventCounts.alerts,
      variant: "danger" as const,
      tooltip: `${eventCounts.alerts} alerts`,
    },
  };

  // Simulate dynamic event updates
  useEffect(() => {
    const interval = setInterval(() => {
      setEventCounts((prev) => ({
        notifications: Math.floor(Math.random() * 10),
        messages: Math.floor(Math.random() * 5),
        alerts: Math.floor(Math.random() * 3),
      }));
    }, 5000);

    return () => clearInterval(interval);
  }, []);

  return (
    <div>
      <h3>Dynamic Event-Based Badges</h3>
      <div>
        <button onClick={() => setCurrentEvent("notifications")}>Show Notifications</button>
        <button onClick={() => setCurrentEvent("messages")}>Show Messages</button>
        <button onClick={() => setCurrentEvent("alerts")}>Show Alerts</button>
        <button onClick={() => setCurrentEvent(undefined)}>Hide Badge</button>
      </div>
      <HorizontalList items={items} event={currentEvent} eventsDefinition={eventsDefinition} />
    </div>
  );
};
```

## Step 4: Common Use Cases and Patterns

### 4.1 Navigation Menu

Create horizontal navigation menus:

```tsx
import { HorizontalList, baseIcons } from "@neuron/ui";

const NavigationMenuExample = () => {
  const navigationItems = [
    {
      text: "Home",
      icon: baseIcons.homeSolid,
      href: "/",
      highlighted: true, // Current page
      tooltip: "Go to homepage",
    },
    {
      text: "Products",
      icon: baseIcons.boxSolid,
      href: "/products",
      tooltip: "Browse our products",
    },
    {
      text: "Services",
      icon: baseIcons.handshakeSolid,
      href: "/services",
      tooltip: "Our services",
    },
    {
      text: "About",
      icon: baseIcons.infoCircleSolid,
      href: "/about",
      tooltip: "Learn about us",
    },
    {
      text: "Contact",
      icon: baseIcons.envelopeSolid,
      href: "/contact",
      tooltip: "Get in touch",
    },
  ];

  return (
    <div>
      <h3>Navigation Menu</h3>
      <HorizontalList items={navigationItems} variant="default" />
    </div>
  );
};
```

### 4.2 Tag Display

Display tags or categories horizontally:

```tsx
import { HorizontalList } from "@neuron/ui";

const TagDisplayExample = () => {
  const tags = [
    { text: "React", highlighted: true, tooltip: "React framework" },
    { text: "TypeScript", tooltip: "TypeScript language" },
    { text: "UI Components", tooltip: "User interface components" },
    { text: "Design System", tooltip: "Consistent design patterns" },
    "Documentation",
    "Best Practices",
  ];

  return (
    <div>
      <h3>Technology Tags</h3>
      <HorizontalList items={tags} variant="compact" />
    </div>
  );
};
```

### 4.3 Status Indicators

Show status information with badges:

```tsx
import { HorizontalList } from "@neuron/ui";

const StatusIndicatorsExample = () => {
  const statusItems = ["Active", "Pending", "Completed", "Failed"];

  const statusEvents = {
    active: { count: 12, variant: "success" as const, tooltip: "12 active items" },
    pending: { count: 5, variant: "warning" as const, tooltip: "5 pending items" },
    completed: { count: 28, variant: "info" as const, tooltip: "28 completed items" },
    failed: { count: 2, variant: "danger" as const, tooltip: "2 failed items" },
  };

  return (
    <div>
      <h3>System Status</h3>
      <div>
        <HorizontalList items={statusItems} event="active" eventsDefinition={statusEvents} />
      </div>
      <div>
        <HorizontalList items={statusItems} event="pending" eventsDefinition={statusEvents} />
      </div>
      <div>
        <HorizontalList items={statusItems} event="failed" eventsDefinition={statusEvents} />
      </div>
    </div>
  );
};
```

### 4.4 Dashboard Widgets

Create dashboard-style horizontal lists:

```tsx
import { HorizontalList, baseIcons } from "@neuron/ui";

const DashboardWidgetsExample = () => {
  const dashboardItems = [
    {
      text: "Analytics",
      icon: baseIcons.chartBarSolid,
      tooltip: "View analytics dashboard",
      href: "/analytics",
      highlighted: false,
    },
    {
      text: "Reports",
      icon: baseIcons.fileSolid,
      tooltip: "Generate reports",
      href: "/reports",
      highlighted: false,
    },
    {
      text: "Users",
      icon: baseIcons.usersSolid,
      tooltip: "Manage users",
      href: "/users",
      highlighted: false,
    },
    {
      text: "Settings",
      icon: baseIcons.gearSolid,
      tooltip: "System settings",
      href: "/settings",
      highlighted: false,
    },
  ];

  const notificationEvents = {
    newUsers: {
      count: 7,
      variant: "info" as const,
      tooltip: "7 new users this week",
    },
  };

  return (
    <div>
      <h3>Dashboard Navigation</h3>
      <HorizontalList items={dashboardItems} event="newUsers" eventsDefinition={notificationEvents} variant="default" />
    </div>
  );
};
```

### 4.5 Filter Options

Display filter options horizontally:

```tsx
import { HorizontalList } from "@neuron/ui";
import { useState } from "react";

const FilterOptionsExample = () => {
  const [activeFilter, setActiveFilter] = useState("all");

  const filterItems = [
    {
      text: "All Items",
      highlighted: activeFilter === "all",
      tooltip: "Show all items",
    },
    {
      text: "Active",
      highlighted: activeFilter === "active",
      tooltip: "Show active items only",
    },
    {
      text: "Completed",
      highlighted: activeFilter === "completed",
      tooltip: "Show completed items only",
    },
    {
      text: "Archived",
      highlighted: activeFilter === "archived",
      tooltip: "Show archived items only",
    },
  ];

  const filterCounts = {
    all: { count: 45, variant: "info" as const },
    active: { count: 12, variant: "success" as const },
    completed: { count: 28, variant: "info" as const },
    archived: { count: 5, variant: "secondary" as const },
  };

  return (
    <div>
      <h3>Filter Options</h3>
      <div>
        <button onClick={() => setActiveFilter("all")}>All</button>
        <button onClick={() => setActiveFilter("active")}>Active</button>
        <button onClick={() => setActiveFilter("completed")}>Completed</button>
        <button onClick={() => setActiveFilter("archived")}>Archived</button>
      </div>
      <HorizontalList items={filterItems} event={activeFilter} eventsDefinition={filterCounts} variant="compact" />
    </div>
  );
};
```

## Step 5: Empty States and Error Handling

### 5.1 Empty List Handling

Handle empty lists gracefully:

```tsx
import { HorizontalList } from "@neuron/ui";

const EmptyListExample = () => {
  const emptyItems: string[] = [];
  const hasItems = emptyItems.length > 0;

  return (
    <div>
      <h3>Empty List Handling</h3>
      {hasItems ? (
        <HorizontalList items={emptyItems} />
      ) : (
        <div>
          <p>No items to display</p>
          <button>Add First Item</button>
        </div>
      )}
    </div>
  );
};
```

### 5.2 Conditional Rendering

Conditionally render lists based on data availability:

```tsx
import { HorizontalList } from "@neuron/ui";
import { useState, useEffect } from "react";

const ConditionalRenderingExample = () => {
  const [items, setItems] = useState<string[]>([]);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState<string | null>(null);

  useEffect(() => {
    // Simulate data loading
    const loadData = async () => {
      try {
        setLoading(true);
        // Simulate API call
        await new Promise((resolve) => setTimeout(resolve, 2000));
        setItems(["Item 1", "Item 2", "Item 3"]);
      } catch (err) {
        setError("Failed to load items");
      } finally {
        setLoading(false);
      }
    };

    loadData();
  }, []);

  if (loading) {
    return <div>Loading items...</div>;
  }

  if (error) {
    return <div>Error: {error}</div>;
  }

  return (
    <div>
      <h3>Conditionally Rendered List</h3>
      <HorizontalList items={items} badge={items.length} />
    </div>
  );
};
```

## Step 6: Accessibility and Best Practices

### 6.1 Accessibility Features

The HorizontalList component includes built-in accessibility features:

- **Semantic Structure**: Proper list semantics for screen readers
- **Keyboard Navigation**: Full keyboard support for interactive items
- **ARIA Labels**: Appropriate labeling for assistive technologies
- **Tooltip Integration**: Accessible tooltip implementation
- **Test Integration**: Built-in test ID support for automated testing

```tsx
import { HorizontalList } from "@neuron/ui";

const AccessibilityExample = () => {
  const accessibleItems = [
    {
      text: "Accessible Item 1",
      tooltip: "This item follows accessibility best practices",
      href: "/item1",
    },
    {
      text: "Accessible Item 2",
      tooltip: "Screen reader friendly item",
      href: "/item2",
    },
  ];

  return (
    <div>
      <h3>Accessible Horizontal List</h3>
      <HorizontalList items={accessibleItems} testId="accessible-horizontal-list" />
    </div>
  );
};
```

### 6.2 Best Practices

**Do:**

- Use meaningful item text that clearly describes the content or action
- Choose appropriate semantic states (info, warning, danger, success) for badges
- Clearly distinguish between read-only and interactive items
- Use href for navigation and onClick for custom actions
- Provide tooltips for items that need additional context
- Use highlighting to indicate current or important items
- Choose appropriate variants based on the context and available space
- Include test IDs for automated testing
- Handle empty states gracefully

**Don't:**

- Overcrowd the list with too many items (consider pagination or grouping)
- Mix href and onClick on the same item (use one interaction method)
- Use overly long text that might cause layout issues
- Rely solely on color to convey meaning (use icons and text)
- Mix different interaction patterns inconsistently
- Use semantic states inappropriately (e.g., danger for non-critical items)
- Ignore loading and error states

### 6.3 Performance Considerations

- HorizontalList components are optimized for rendering multiple items efficiently
- Badge rendering is optimized and only rendered when needed
- Icon rendering is efficient through the baseIcons system
- Consider virtualization for very large lists (though HorizontalList is typically used for smaller sets)

## Step 7: Integration Patterns

### 7.1 Integration with Layout Components

Use HorizontalList within layout components:

```tsx
import { HorizontalList, Layout } from "@neuron/ui";

const LayoutIntegrationExample = () => {
  const navigationItems = ["Home", "Products", "Services", "Contact"];

  return (
    <Layout>
      <Layout.Header>
        <div>
          <h1>Application Title</h1>
          <HorizontalList items={navigationItems} variant="compact" />
        </div>
      </Layout.Header>
      <Layout.Content>
        <div>Main content area</div>
      </Layout.Content>
    </Layout>
  );
};
```

### 7.2 Integration with Forms

Use HorizontalList for form-related selections:

```tsx
import { HorizontalList } from "@neuron/ui";
import { useState } from "react";

const FormIntegrationExample = () => {
  const [selectedCategory, setSelectedCategory] = useState("general");

  const categoryItems = [
    {
      text: "General",
      highlighted: selectedCategory === "general",
      tooltip: "General inquiries",
    },
    {
      text: "Support",
      highlighted: selectedCategory === "support",
      tooltip: "Technical support",
    },
    {
      text: "Sales",
      highlighted: selectedCategory === "sales",
      tooltip: "Sales inquiries",
    },
  ];

  return (
    <div>
      <h3>Contact Form</h3>
      <div>
        <label>Category:</label>
        <HorizontalList items={categoryItems} variant="compact" />
      </div>
      <div>
        <label>Message:</label>
        <textarea placeholder="Enter your message..."></textarea>
      </div>
    </div>
  );
};
```

## Summary

The Neuron HorizontalList component provides versatile horizontal item display with support for:

- **Flexible Item Types**: Simple strings and complex objects with rich properties
- **Semantic States**: Built-in support for info, warning, danger, and success states
- **Interactive Modes**: Items can be read-only display or interactive (href links or click handlers)
- **Badge Integration**: Count-based and event-driven notification badges with semantic variants
- **Visual Variants**: Multiple presentation styles for different contexts
- **Event System**: Dynamic badge updates based on application events
- **Rich Item Properties**: Icons, highlighting, tooltips, and navigation support
- **Accessibility**: Full keyboard navigation and screen reader support
- **Test Integration**: Built-in test ID support for automated testing

Use HorizontalList strategically for navigation menus, tag displays, status indicators, dashboard widgets, and any scenario requiring horizontal presentation of related items. The component's flexibility makes it suitable for a wide range of data presentation needs in your Neuron applications.

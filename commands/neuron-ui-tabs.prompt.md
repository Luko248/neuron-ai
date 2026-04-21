---
agent: agent
---

# AI-Assisted Neuron Tabs Component Integration Guide

## For the AI Assistant

Your task is to integrate and configure the Neuron Tabs components in a React application. This guide provides comprehensive instructions for implementing the Tabs component, which serves as a navigation system for organizing content into switchable sections across all Neuron applications.

## Sync Metadata

- **Component Version:** v4.2.2
- **Component Source:** `packages/neuron/ui/src/lib/menus/tabs/Tabs.tsx`
- **Guideline Command:** `/neuron-ui-tabs`
- **Related Skill:** `neuron-ui-menu`

## Introduction

The Tabs system is a core navigation component of the Neuron UI framework, designed to create organized, accessible, and interactive content sections across all Neuron applications.

### What is the Tabs System?

The Tabs component provides a standardized tabbed interface for your application with support for:

- Multiple variants (block, inline)
- Dynamic tab management (adding/removing tabs)
- Routing integration (path-based and query-based)
- Hidden tabs functionality
- Highlighted tabs for special actions
- Icon and notification support
- Controlled and uncontrolled modes
- Default tab selection
- Disabled state handling
- Content lazy loading

### Key Features

- **Consistent Navigation**: Standardized tabbed interface across applications
- **Dynamic Tab Management**: Add and remove tabs programmatically
- **Routing Integration**: Built-in support for URL-based tab navigation
- **Flexible Content**: Support for React components, functions, and static content
- **Access Control**: Built-in permission handling for tab visibility
- **Icon Integration**: Support for baseIcons from the Neuron icon system
- **Notification Support**: Built-in notification badges and tags
- **Responsive Design**: Adaptive layout for different screen sizes
- **TypeScript Support**: Full type safety with comprehensive interfaces
- **Accessibility**: Built-in accessibility features and ARIA support

**Storybook Reference:** You can refer to the Storybook documentation for visual examples and interactive demos of the Tabs component.

## Step 1: Basic Tabs Implementation

### 1.1 Import the Tabs Component

```tsx
import { Tabs } from "@neuron/ui";
```

### 1.2 Basic Usage

Here's a simple implementation of the Tabs component:

```tsx
import { Tabs } from "@neuron/ui";

const MyComponent = () => {
  const tabs = [
    {
      tabKey: "tab1",
      title: "First Tab",
      content: <div>Content for first tab</div>,
    },
    {
      tabKey: "tab2",
      title: "Second Tab",
      content: <div>Content for second tab</div>,
    },
    {
      tabKey: "tab3",
      title: "Third Tab",
      content: <div>Content for third tab</div>,
    },
  ];

  return <Tabs tabs={tabs} />;
};
```

### 1.3 Tab Variants

The Tabs component supports multiple variants for different use cases:

```tsx
import { Tabs } from "@neuron/ui";

const TabVariants = () => {
  const tabs = [
    {
      tabKey: "overview",
      title: "Overview",
      content: <div>Overview content</div>,
    },
    {
      tabKey: "details",
      title: "Details",
      content: <div>Details content</div>,
    },
  ];

  return (
    <div className="tab-variants">
      {/* Block variant - default, full-width tabs */}
      <Tabs variant="block" tabs={tabs} />

      {/* Inline variant - compact, inline tabs */}
      <Tabs variant="inline" tabs={tabs} />
    </div>
  );
};
```

### 1.4 Tab Nesting Rules

**⚠️ CRITICAL: Tab Nesting Hierarchy**

The Tabs component has strict nesting rules that must be followed:

- **Block tabs can contain inline tabs** (block → inline nesting allowed)
- **Inline tabs CANNOT contain block tabs** (inline → block nesting forbidden)
- This hierarchy ensures proper visual hierarchy and prevents layout conflicts

```tsx
import { Tabs } from "@neuron/ui";

const NestedTabsExample = () => {
  // Inline tabs for nested content
  const nestedInlineTabs = [
    {
      tabKey: "basic-info",
      title: "Basic Info",
      content: <div>Basic information form</div>,
    },
    {
      tabKey: "advanced-info",
      title: "Advanced",
      content: <div>Advanced settings form</div>,
    },
  ];

  // Main block tabs
  const mainTabs = [
    {
      tabKey: "dashboard",
      title: "Dashboard",
      content: <div>Dashboard overview content</div>,
    },
    {
      tabKey: "settings",
      title: "Settings",
      content: (
        <div>
          <h3>Settings Configuration</h3>
          {/* ✅ CORRECT: Block tabs containing inline tabs */}
          <Tabs variant="inline" tabs={nestedInlineTabs} />
        </div>
      ),
    },
    {
      tabKey: "reports",
      title: "Reports",
      content: <div>Reports content</div>,
    },
  ];

  return <Tabs variant="block" tabs={mainTabs} />;
};
```

**Nesting Best Practices:**

```tsx
{
  /* ✅ CORRECT: Block → Inline nesting */
}
<Tabs
  variant="block"
  tabs={[
    {
      tabKey: "main",
      title: "Main Section",
      content: (
        <div>
          <h2>Main Content</h2>
          <Tabs variant="inline" tabs={subTabs} />
        </div>
      ),
    },
  ]}
/>;

{
  /* ❌ INCORRECT: Inline → Block nesting */
}
<Tabs
  variant="inline"
  tabs={[
    {
      tabKey: "sub",
      title: "Sub Section",
      content: (
        <div>
          {/* This will cause layout issues */}
          <Tabs variant="block" tabs={mainTabs} />
        </div>
      ),
    },
  ]}
/>;

{
  /* ✅ CORRECT: Multiple levels of proper nesting */
}
<Tabs
  variant="block"
  tabs={[
    {
      tabKey: "level1",
      title: "Level 1",
      content: (
        <div>
          <Tabs
            variant="inline"
            tabs={[
              {
                tabKey: "level2",
                title: "Level 2",
                content: <div>Nested content</div>,
              },
            ]}
          />
        </div>
      ),
    },
  ]}
/>;
```

## Step 2: Advanced Tab Configuration

### 2.1 Tabs with Icons and Notifications

Enhance tabs with icons, notifications, and tags:

```tsx
import { Tabs, baseIcons } from "@neuron/ui";

const AdvancedTabs = () => {
  const tabs = [
    {
      tabKey: "dashboard",
      title: "Dashboard",
      icon: baseIcons.chartBarSolid,
      content: <div>Dashboard content</div>,
    },
    {
      tabKey: "messages",
      title: "Messages",
      icon: baseIcons.envelopeSolid,
      notificationBadge: {
        value: 5,
        variant: "danger",
      },
      content: <div>Messages content</div>,
    },
    {
      tabKey: "settings",
      title: "Settings",
      icon: baseIcons.cogSolid,
      tag: {
        text: "New",
        variant: "success",
      },
      content: <div>Settings content</div>,
    },
  ];

  return <Tabs variant="block" tabs={tabs} />;
};
```

### 2.2 Tab Descriptions and Tooltips

Add descriptions and tooltips for better user experience:

```tsx
import { Tabs } from "@neuron/ui";

const DescriptiveTabs = () => {
  const tabs = [
    {
      tabKey: "profile",
      title: "User Profile",
      description: "Manage your personal information",
      tooltip: "Update your profile settings and preferences",
      content: <div>Profile content</div>,
    },
    {
      tabKey: "security",
      title: "Security",
      description: "Password and authentication settings",
      tooltip: "Configure security options and two-factor authentication",
      content: <div>Security content</div>,
    },
  ];

  return <Tabs variant="block" tabs={tabs} />;
};
```

### 2.3 Hidden and Highlighted Tabs

Control tab visibility and highlighting:

```tsx
import { Tabs } from "@neuron/ui";

const ConditionalTabs = () => {
  const tabs = [
    {
      tabKey: "public",
      title: "Public",
      content: <div>Public content</div>,
    },
    {
      tabKey: "private",
      title: "Private",
      isHidden: true, // Hidden from view
      content: <div>Private content</div>,
    },
    {
      tabKey: "admin",
      title: "Admin",
      isHighlighted: true, // Appears in controls area
      content: <div>Admin content</div>,
    },
  ];

  return <Tabs variant="block" tabs={tabs} />;
};
```

## Step 3: Dynamic Tab Management

### 3.1 Adding New Tabs

Enable users to add new tabs dynamically:

```tsx
import { Tabs } from "@neuron/ui";
import { useState } from "react";

const AddableTabs = () => {
  const [tabs, setTabs] = useState([
    {
      tabKey: "existing1",
      title: "Existing Tab 1",
      content: <div>Content 1</div>,
    },
    {
      tabKey: "existing2",
      title: "Existing Tab 2",
      content: <div>Content 2</div>,
    },
  ]);

  const handleTabAdd = (newTabKey: string) => {
    console.info(`New tab added: ${newTabKey}`);
    // Additional logic for new tab creation
  };

  return (
    <Tabs
      variant="block"
      tabs={tabs}
      addableTab={{
        tab: {
          tabKey: "new-tab",
          title: "New Tab",
          description: "Dynamically created tab",
          content: <div>New tab content</div>,
        },
        onClick: handleTabAdd,
      }}
      newTabIsActive={true} // New tab becomes active immediately
    />
  );
};
```

### 3.2 Removing Tabs

Implement tab removal functionality:

```tsx
import { Tabs, Button } from "@neuron/ui";
import { useState } from "react";

const RemovableTabs = () => {
  const [tabs, setTabs] = useState([
    {
      tabKey: "tab1",
      title: "Tab 1",
      content: <div>Content 1</div>,
    },
    {
      tabKey: "tab2",
      title: "Tab 2",
      content: <div>Content 2</div>,
    },
    {
      tabKey: "tab3",
      title: "Tab 3",
      content: <div>Content 3</div>,
    },
  ]);

  const removeTab = (tabKey: string) => {
    setTabs((prevTabs) => prevTabs.filter((tab) => tab.tabKey !== tabKey));
  };

  return (
    <div>
      <div className="tab-controls">
        <Button onClick={() => removeTab("tab2")}>Remove Tab 2</Button>
      </div>

      <Tabs variant="block" tabs={tabs} />
    </div>
  );
};
```

## Step 4: Routing Integration

### 4.1 Path-Based Routing

Integrate tabs with URL path routing:

```tsx
import { Tabs } from "@neuron/ui";

const PathRoutedTabs = () => {
  const tabs = [
    {
      tabKey: "overview",
      title: "Overview",
      content: <div>Overview content</div>,
    },
    {
      tabKey: "analytics",
      title: "Analytics",
      content: <div>Analytics content</div>,
    },
    {
      tabKey: "settings",
      title: "Settings",
      content: <div>Settings content</div>,
    },
  ];

  return (
    <Tabs
      variant="block"
      tabs={tabs}
      routing={{
        routingType: "path",
      }}
    />
  );
};
```

### 4.2 Query Parameter Routing

Use query parameters for tab navigation:

```tsx
import { Tabs } from "@neuron/ui";

const QueryRoutedTabs = () => {
  const tabs = [
    {
      tabKey: "dashboard",
      title: "Dashboard",
      content: <div>Dashboard content</div>,
    },
    {
      tabKey: "reports",
      title: "Reports",
      content: <div>Reports content</div>,
    },
    {
      tabKey: "users",
      title: "Users",
      content: <div>Users content</div>,
    },
  ];

  return (
    <Tabs
      variant="block"
      tabs={tabs}
      routing={{
        routingType: "query",
        queryParam: "tab-id", // URL will be ?tab-id=dashboard
      }}
    />
  );
};
```

### 4.3 Custom Routing Configuration

Advanced routing with custom parameters:

```tsx
import { Tabs } from "@neuron/ui";

const CustomRoutedTabs = () => {
  const tabs = [
    {
      tabKey: "profile",
      title: "Profile",
      content: <div>Profile content</div>,
    },
    {
      tabKey: "preferences",
      title: "Preferences",
      content: <div>Preferences content</div>,
    },
  ];

  return (
    <Tabs
      variant="block"
      tabs={tabs}
      routing={{
        routingType: "query",
        queryParam: "section", // Custom query parameter name
      }}
      onTabChange={(tabKey) => {
        console.info(`Tab changed to: ${tabKey}`);
        // Custom routing logic
      }}
    />
  );
};
```

## Step 5: Controlled Tabs

### 5.1 Controlled Tab State

Control tab state externally:

```tsx
import { Tabs, Button } from "@neuron/ui";
import { useState } from "react";

const ControlledTabs = () => {
  const [activeTab, setActiveTab] = useState("tab1");

  const tabs = [
    {
      tabKey: "tab1",
      title: "Tab 1",
      content: <div>Content 1</div>,
    },
    {
      tabKey: "tab2",
      title: "Tab 2",
      content: <div>Content 2</div>,
    },
    {
      tabKey: "tab3",
      title: "Tab 3",
      content: <div>Content 3</div>,
    },
  ];

  const handleTabChange = (tabKey: string) => {
    setActiveTab(tabKey);
    console.info(`Active tab changed to: ${tabKey}`);
  };

  return (
    <div>
      <div className="external-controls">
        <Button onClick={() => setActiveTab("tab1")}>Go to Tab 1</Button>
        <Button onClick={() => setActiveTab("tab2")}>Go to Tab 2</Button>
        <Button onClick={() => setActiveTab("tab3")}>Go to Tab 3</Button>
      </div>

      <Tabs variant="block" tabs={tabs} activeTabKey={activeTab} onTabChange={handleTabChange} />
    </div>
  );
};
```

### 5.2 Default Tab Selection

Set a default active tab:

```tsx
import { Tabs } from "@neuron/ui";

const DefaultTabExample = () => {
  const tabs = [
    {
      tabKey: "first",
      title: "First Tab",
      content: <div>First content</div>,
    },
    {
      tabKey: "second",
      title: "Second Tab",
      content: <div>Second content</div>,
    },
    {
      tabKey: "third",
      title: "Third Tab",
      content: <div>Third content</div>,
    },
  ];

  return (
    <Tabs
      variant="block"
      tabs={tabs}
      defaultTab="second" // Second tab will be active by default
    />
  );
};
```

## Step 6: Dynamic Content and Lazy Loading

### 6.1 Function-Based Content

Use functions for dynamic content generation:

```tsx
import { Tabs } from "@neuron/ui";

const DynamicContentTabs = () => {
  const tabs = [
    {
      tabKey: "user-info",
      title: "User Info",
      content: (tabInfo) => (
        <div>
          <h3>Dynamic Content</h3>
          <p>Active tab: {tabInfo.tabKey}</p>
          <p>Is active: {tabInfo.isActive ? "Yes" : "No"}</p>
          <p>Loaded at: {new Date().toLocaleTimeString()}</p>
        </div>
      ),
    },
    {
      tabKey: "statistics",
      title: "Statistics",
      content: (tabInfo) => (
        <div>
          <h3>Statistics Dashboard</h3>
          <p>Tab key: {tabInfo.tabKey}</p>
          {/* Dynamic data loading based on tab info */}
        </div>
      ),
    },
  ];

  return <Tabs variant="block" tabs={tabs} />;
};
```

### 6.2 Lazy Loading Content

Implement lazy loading for tab content:

```tsx
import { Tabs } from "@neuron/ui";
import { useState, useEffect } from "react";

const LazyLoadingTabs = () => {
  const LazyContent = ({ tabKey }: { tabKey: string }) => {
    const [data, setData] = useState(null);
    const [loading, setLoading] = useState(true);

    useEffect(() => {
      const loadData = async () => {
        setLoading(true);
        try {
          // Simulate API call
          await new Promise((resolve) => setTimeout(resolve, 1000));
          setData(`Loaded data for ${tabKey}`);
        } finally {
          setLoading(false);
        }
      };

      loadData();
    }, [tabKey]);

    if (loading) {
      return <div>Loading content for {tabKey}...</div>;
    }

    return <div>{data}</div>;
  };

  const tabs = [
    {
      tabKey: "reports",
      title: "Reports",
      content: () => <LazyContent tabKey="reports" />,
    },
    {
      tabKey: "analytics",
      title: "Analytics",
      content: () => <LazyContent tabKey="analytics" />,
    },
  ];

  return <Tabs variant="block" tabs={tabs} />;
};
```

## Step 7: Disabled States and Access Control

### 7.1 Disabled Tabs

Control tab accessibility:

```tsx
import { Tabs } from "@neuron/ui";

const DisabledTabs = () => {
  const tabs = [
    {
      tabKey: "available",
      title: "Available",
      content: <div>Available content</div>,
    },
    {
      tabKey: "disabled-tab",
      title: "Disabled Tab",
      disabled: true, // Individual tab disabled
      content: <div>Disabled content</div>,
    },
    {
      tabKey: "another",
      title: "Another Tab",
      content: <div>Another content</div>,
    },
  ];

  return (
    <div>
      {/* All tabs disabled */}
      <Tabs variant="block" tabs={tabs} disabled={true} />

      {/* Individual tab disabled */}
      <Tabs variant="block" tabs={tabs} />
    </div>
  );
};
```

### 7.2 Conditional Tab Rendering

Show/hide tabs based on conditions:

```tsx
import { Tabs } from "@neuron/ui";

const ConditionalTabsExample = ({ userRole }: { userRole: string }) => {
  const baseTabs = [
    {
      tabKey: "dashboard",
      title: "Dashboard",
      content: <div>Dashboard content</div>,
    },
    {
      tabKey: "profile",
      title: "Profile",
      content: <div>Profile content</div>,
    },
  ];

  const adminTabs = [
    {
      tabKey: "admin",
      title: "Admin Panel",
      isHidden: userRole !== "admin", // Hide for non-admin users
      content: <div>Admin content</div>,
    },
    {
      tabKey: "users",
      title: "User Management",
      isHidden: userRole !== "admin",
      content: <div>User management content</div>,
    },
  ];

  const allTabs = [...baseTabs, ...adminTabs];

  return <Tabs variant="block" tabs={allTabs} />;
};
```

## Step 8: Tab Props Reference

### 8.1 Core Tabs Props

| Prop      | Type                  | Default   | Description              |
| --------- | --------------------- | --------- | ------------------------ |
| tabs      | `ITab[]`              | -         | Array of tab definitions |
| variant   | `"block" \| "inline"` | `"block"` | Tab display style        |
| className | `string`              | -         | Additional CSS classes   |
| disabled  | `boolean`             | `false`   | Disable all tabs         |
| testId    | `string`              | -         | Test identifier          |

### 8.2 Tab State Props

| Prop         | Type                       | Default | Description               |
| ------------ | -------------------------- | ------- | ------------------------- |
| defaultTab   | `string`                   | -       | Default active tab key    |
| activeTabKey | `string`                   | -       | Controlled active tab key |
| onTabChange  | `(tabKey: string) => void` | -       | Tab change callback       |

### 8.3 Dynamic Tab Props

| Prop             | Type          | Default | Description                       |
| ---------------- | ------------- | ------- | --------------------------------- |
| addableTab       | `IAddableTab` | -       | Configuration for adding new tabs |
| disableAddButton | `boolean`     | `false` | Disable the add tab button        |
| newTabIsActive   | `boolean`     | `true`  | New tab becomes active when added |

### 8.4 Routing Props

| Prop    | Type          | Default | Description                  |
| ------- | ------------- | ------- | ---------------------------- |
| routing | `ITabRouting` | -       | Routing configuration object |

### 8.5 Individual Tab Props (ITab)

| Prop              | Type                                                    | Default | Description                      |
| ----------------- | ------------------------------------------------------- | ------- | -------------------------------- |
| tabKey            | `string`                                                | -       | Unique identifier for the tab    |
| title             | `string`                                                | -       | Tab display title                |
| description       | `string`                                                | -       | Optional tab description         |
| content           | `ReactNode \| ((tabInfo: TActiveTabInfo) => ReactNode)` | -       | Tab content                      |
| icon              | `TBaseIcons`                                            | -       | Tab icon                         |
| disabled          | `boolean`                                               | `false` | Individual tab disabled state    |
| isHidden          | `boolean`                                               | `false` | Hide tab from view               |
| isHighlighted     | `boolean`                                               | `false` | Show tab in controls area        |
| tooltip           | `string \| TooltipConfig`                               | -       | Tab tooltip                      |
| notificationBadge | `NotificationBadgeConfig`                               | -       | Notification badge configuration |
| tag               | `TagConfig`                                             | -       | Tag configuration                |

### 8.6 Routing Configuration (ITabRouting)

| Prop        | Type                | Default | Description                              |
| ----------- | ------------------- | ------- | ---------------------------------------- |
| routingType | `"path" \| "query"` | -       | Type of routing to use                   |
| queryParam  | `string`            | -       | Query parameter name (for query routing) |

## Step 9: Best Practices

### 9.1 When to Use Each Variant

**Block Variant:**

- Main navigation sections
- Full-width content areas
- Primary tab interfaces

```tsx
<Tabs variant="block" tabs={mainTabs} />
```

**Inline Variant:**

- Secondary navigation
- Compact interfaces
- Embedded tab sections
- Nested within block tabs

```tsx
<Tabs variant="inline" tabs={secondaryTabs} />
```

**⚠️ CRITICAL: Nesting Rules**

- **Block tabs can contain inline tabs** ✅
- **Inline tabs CANNOT contain block tabs** ❌
- Always follow the hierarchy: Block → Inline

```tsx
{
  /* ✅ CORRECT: Block containing inline */
}
<Tabs
  variant="block"
  tabs={[
    {
      tabKey: "main",
      title: "Main",
      content: (
        <div>
          <Tabs variant="inline" tabs={subTabs} />
        </div>
      ),
    },
  ]}
/>;

{
  /* ❌ INCORRECT: Inline containing block */
}
<Tabs
  variant="inline"
  tabs={[
    {
      tabKey: "sub",
      title: "Sub",
      content: <Tabs variant="block" tabs={mainTabs} />, // Will cause layout issues
    },
  ]}
/>;
```

### 9.2 Tab Organization Guidelines

- Keep tab titles concise and descriptive
- Use icons to enhance meaning, not replace text
- Group related functionality in tabs
- Limit the number of visible tabs (5-7 maximum)
- Use hidden tabs for conditional content

```tsx
{
  /* Good: Clear, descriptive titles */
}
const tabs = [
  { tabKey: "overview", title: "Overview", icon: baseIcons.chartBarSolid },
  { tabKey: "details", title: "Details", icon: baseIcons.listSolid },
  { tabKey: "settings", title: "Settings", icon: baseIcons.cogSolid },
];

{
  /* Avoid: Too many visible tabs */
}
const tooManyTabs = [
  // 10+ tabs make navigation difficult
];
```

### 9.3 Content Loading Best Practices

- Use lazy loading for heavy content
- Provide loading indicators
- Cache content when appropriate
- Handle errors gracefully

```tsx
{
  /* Good: Lazy loading with error handling */
}
const LazyTabContent = ({ tabKey }) => {
  const [data, setData] = useState(null);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState(null);

  useEffect(() => {
    loadTabData(tabKey)
      .then(setData)
      .catch(setError)
      .finally(() => setLoading(false));
  }, [tabKey]);

  if (loading) return <div>Loading...</div>;
  if (error) return <div>Error loading content</div>;
  return <div>{data}</div>;
};
```

### 9.4 Routing Best Practices

- Use path routing for main navigation
- Use query routing for secondary navigation
- Provide meaningful URLs
- Handle invalid tab keys gracefully

```tsx
{
  /* Good: Meaningful routing */
}
<Tabs
  tabs={tabs}
  routing={{
    routingType: "path", // URLs like /dashboard/overview
  }}
/>;

{
  /* Good: Descriptive query parameters */
}
<Tabs
  tabs={tabs}
  routing={{
    routingType: "query",
    queryParam: "section", // URLs like ?section=settings
  }}
/>;
```

### 9.5 Accessibility Considerations

- Provide meaningful tab titles
- Use proper ARIA attributes
- Ensure keyboard navigation works
- Support screen readers

```tsx
{
  /* Good: Accessible tab configuration */
}
const accessibleTabs = [
  {
    tabKey: "profile",
    title: "User Profile",
    description: "Manage your personal information",
    tooltip: "Update profile settings and preferences",
  },
];
```

## Step 10: Common Patterns and Examples

### 10.1 Dashboard Tabs

```tsx
import { Tabs, baseIcons } from "@neuron/ui";

const DashboardTabs = () => {
  const tabs = [
    {
      tabKey: "overview",
      title: "Overview",
      icon: baseIcons.chartBarSolid,
      content: <div>Dashboard overview</div>,
    },
    {
      tabKey: "analytics",
      title: "Analytics",
      icon: baseIcons.chartLineSolid,
      content: <div>Analytics dashboard</div>,
    },
    {
      tabKey: "reports",
      title: "Reports",
      icon: baseIcons.documentSolid,
      notificationBadge: {
        value: 3,
        variant: "info",
      },
      content: <div>Reports section</div>,
    },
  ];

  return (
    <Tabs
      variant="block"
      tabs={tabs}
      routing={{
        routingType: "path",
      }}
    />
  );
};
```

### 10.2 Settings Tabs with Conditional Access

```tsx
import { Tabs, baseIcons } from "@neuron/ui";

const SettingsTabs = ({ userRole }: { userRole: string }) => {
  const tabs = [
    {
      tabKey: "profile",
      title: "Profile",
      icon: baseIcons.userSolid,
      content: <div>Profile settings</div>,
    },
    {
      tabKey: "security",
      title: "Security",
      icon: baseIcons.shieldSolid,
      content: <div>Security settings</div>,
    },
    {
      tabKey: "admin",
      title: "Admin",
      icon: baseIcons.cogSolid,
      isHidden: userRole !== "admin",
      isHighlighted: true,
      content: <div>Admin settings</div>,
    },
  ];

  return (
    <Tabs
      variant="block"
      tabs={tabs}
      routing={{
        routingType: "query",
        queryParam: "settings-tab",
      }}
    />
  );
};
```

### 10.3 Dynamic Project Tabs

```tsx
import { Tabs, baseIcons } from "@neuron/ui";
import { useState } from "react";

const ProjectTabs = () => {
  const [tabs, setTabs] = useState([
    {
      tabKey: "project-1",
      title: "Project Alpha",
      content: <div>Project Alpha content</div>,
    },
  ]);

  const handleAddProject = (newTabKey: string) => {
    const projectNumber = tabs.length + 1;
    const newTab = {
      tabKey: newTabKey,
      title: `Project ${projectNumber}`,
      content: <div>New project content</div>,
    };

    setTabs((prevTabs) => [...prevTabs, newTab]);
  };

  return (
    <Tabs
      variant="block"
      tabs={tabs}
      addableTab={{
        tab: {
          tabKey: "new-project",
          title: "New Project",
          content: <div>New project workspace</div>,
        },
        onClick: handleAddProject,
      }}
      newTabIsActive={true}
    />
  );
};
```

## Step 11: Common Mistakes to Avoid

### 11.1 Don't Forget Tab Keys

```tsx
{
  /* Wrong: Missing or duplicate tab keys */
}
const badTabs = [
  { title: "Tab 1", content: <div>Content</div> }, // Missing tabKey
  { tabKey: "tab1", title: "Tab 1", content: <div>Content 1</div> },
  { tabKey: "tab1", title: "Tab 2", content: <div>Content 2</div> }, // Duplicate tabKey
];

{
  /* Right: Unique tab keys for all tabs */
}
const goodTabs = [
  { tabKey: "tab1", title: "Tab 1", content: <div>Content 1</div> },
  { tabKey: "tab2", title: "Tab 2", content: <div>Content 2</div> },
];
```

### 11.2 Don't Mix Controlled and Uncontrolled

```tsx
{/* Wrong: Mixing controlled and uncontrolled */}
<Tabs
  tabs={tabs}
  activeTabKey={activeTab} // Controlled
  defaultTab="tab1" // Uncontrolled - will be ignored
/>

{/* Right: Use either controlled or uncontrolled */}
<Tabs tabs={tabs} activeTabKey={activeTab} onTabChange={setActiveTab} />
// OR
<Tabs tabs={tabs} defaultTab="tab1" />
```

### 11.3 Don't Ignore Loading States

```tsx
{
  /* Wrong: No loading feedback for dynamic content */
}
const BadTabContent = ({ data }) => {
  return <div>{data}</div>; // What if data is still loading?
};

{
  /* Right: Handle loading states */
}
const GoodTabContent = ({ data, loading }) => {
  if (loading) return <div>Loading...</div>;
  return <div>{data}</div>;
};
```

### 11.4 Don't Overuse Hidden Tabs

```tsx
{
  /* Wrong: Too many hidden tabs */
}
const tabs = [
  { tabKey: "visible", title: "Visible", content: <div>Content</div> },
  { tabKey: "hidden1", title: "Hidden 1", isHidden: true, content: <div>Content</div> },
  { tabKey: "hidden2", title: "Hidden 2", isHidden: true, content: <div>Content</div> },
  // ... many more hidden tabs
];

{
  /* Right: Use hidden tabs sparingly for conditional content */
}
const tabs = [
  { tabKey: "public", title: "Public", content: <div>Content</div> },
  { tabKey: "admin", title: "Admin", isHidden: !isAdmin, content: <div>Content</div> },
];
```

## Key Takeaways

The Neuron Tabs component system provides a comprehensive, accessible, and flexible foundation for organizing content. Key points to remember:

1. **Use appropriate variants** based on interface requirements (block vs inline)
2. **Provide unique tab keys** for all tabs to ensure proper functionality
3. **Handle loading states** for dynamic content appropriately
4. **Use routing integration** for better user experience and bookmarkable URLs
5. **Implement access control** through hidden tabs for role-based content
6. **Follow accessibility guidelines** with proper titles, descriptions, and tooltips
7. **Use controlled mode** when you need external tab state management
8. **Implement lazy loading** for performance with heavy content
9. **Provide meaningful feedback** during tab operations and content loading
10. **Keep tab organization simple** and intuitive for users

By following these guidelines, you'll create consistent, accessible, and user-friendly tabbed interfaces across your Neuron applications.

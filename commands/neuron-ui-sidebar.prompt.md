---
agent: agent
---

# AI-Assisted Neuron Sidebar Component Integration Guide

## For the AI Assistant

Your task is to integrate and configure the Neuron Sidebar components in a React application. This guide provides comprehensive instructions for implementing the Sidebar component, which serves as a persistent side panel for displaying important information, navigation, or contextual content across all Neuron applications.

## Sync Metadata

- **Component Version:** v3.1.3
- **Component Source:** `packages/neuron/ui/src/lib/panels/sidebar/Sidebar.tsx`
- **Guideline Command:** `/neuron-ui-sidebar`
- **Related Skill:** `neuron-ui-panels`

## Introduction

The Sidebar system is a core layout component of the Neuron UI framework, designed to create persistent, accessible, and responsive side panels that adapt to different screen sizes across all Neuron applications.

### What is the Sidebar System?

The Sidebar component provides a standardized side panel interface for your application with support for:

- Responsive design (desktop sidebar, mobile sidesheet)
- Controlled and uncontrolled open/close states
- Persistent state across page reloads
- Custom content and structured content modes
- Header, footer, and scrollable content areas
- Action buttons and tags integration
- Icon and tooltip support
- Layout portal integration
- Session storage persistence
- Dock button integration for toggle functionality

### Key Features

- **Responsive Behavior**: Automatically switches between desktop sidebar and mobile sidesheet based on screen resolution (1024px breakpoint)
- **Persistent State**: Maintains open/close state across page reloads using session storage
- **Layout Integration**: Seamless integration with Layout component using LayoutPortal
- **Flexible Content**: Support for structured content with title/description or completely custom content
- **Action Integration**: Built-in support for action buttons and tags
- **Controlled/Uncontrolled**: Support for both controlled and uncontrolled component patterns
- **Dock Integration**: Automatic dock button for toggle functionality
- **TypeScript Support**: Full type safety with comprehensive prop interfaces
- **Accessibility**: Built-in accessibility features and ARIA support

**Storybook Reference:** You can refer to the Storybook documentation for visual examples and interactive demos of the Sidebar component.

## Step 1: Basic Sidebar Implementation

### 1.1 Import the Sidebar Component

```tsx
import { Sidebar, LayoutPortal } from "@neuron/ui";
```

### 1.2 Basic Usage with Layout Integration

**⚠️ CRITICAL: Sidebar must be used with LayoutPortal for proper positioning**

```tsx
import { Sidebar, LayoutPortal, baseIcons } from "@neuron/ui";

const MyComponent = () => {
  return (
    <>
      {/* Sidebar must be placed in Layout using LayoutPortal */}
      <LayoutPortal position="right">
        <Sidebar title="My Sidebar" description="This is a basic sidebar" icon={baseIcons.placeholder} open={true}>
          <div>Sidebar content goes here</div>
        </Sidebar>
      </LayoutPortal>

      {/* Your main page content */}
      <div>
        <h1>Main Page Content</h1>
        <p>The sidebar will appear on the right side</p>
      </div>
    </>
  );
};
```

### 1.3 Layout Portal Positioning

The Sidebar can be positioned on either side of the layout:

```tsx
import { Sidebar, LayoutPortal, baseIcons } from "@neuron/ui";

const SidebarPositioning = () => {
  return (
    <>
      {/* Right-side sidebar (most common) */}
      <LayoutPortal position="right">
        <Sidebar title="Right Sidebar" icon={baseIcons.cogSolid} open={true}>
          <div>Right sidebar content</div>
        </Sidebar>
      </LayoutPortal>

      {/* Left-side sidebar (alternative) */}
      <LayoutPortal position="left">
        <Sidebar title="Left Sidebar" icon={baseIcons.menuSolid} open={false}>
          <div>Left sidebar content</div>
        </Sidebar>
      </LayoutPortal>
    </>
  );
};
```

## Step 2: Responsive Behavior

### 2.1 Automatic Desktop/Mobile Switching

The Sidebar automatically adapts to screen size:

- **Desktop (>1024px)**: Shows as a traditional sidebar
- **Mobile/Tablet (≤1024px)**: Shows as a sidesheet overlay

```tsx
import { Sidebar, LayoutPortal, baseIcons } from "@neuron/ui";

const ResponsiveSidebar = () => {
  return (
    <LayoutPortal position="right">
      <Sidebar
        title="Responsive Sidebar"
        description="Adapts to screen size automatically"
        icon={baseIcons.deviceMobileSolid}
        open={true}
      >
        <div>
          <p>On desktop: I'm a sidebar</p>
          <p>On mobile: I'm a sidesheet</p>
        </div>
      </Sidebar>
    </LayoutPortal>
  );
};
```

### 2.2 Understanding the Breakpoint

The component uses a 1024px breakpoint for responsive behavior:

```tsx
// Internal logic (for reference only)
const desktopResolution = 1024;
const isDesktop = window.innerWidth > desktopResolution;

// Desktop: Traditional sidebar behavior
// Mobile: Sidesheet overlay behavior
```

## Step 3: State Management

### 3.1 Uncontrolled Sidebar (Default)

The sidebar manages its own state and persists it across page reloads:

```tsx
import { Sidebar, LayoutPortal, baseIcons } from "@neuron/ui";

const UncontrolledSidebar = () => {
  return (
    <LayoutPortal position="right">
      <Sidebar
        title="Uncontrolled Sidebar"
        description="Manages its own state"
        icon={baseIcons.cogSolid}
        open={true} // Initial state only
      >
        <div>
          <p>My state is automatically persisted</p>
          <p>I'll remember if I was open or closed</p>
        </div>
      </Sidebar>
    </LayoutPortal>
  );
};
```

### 3.2 Controlled Sidebar

Control the sidebar state externally:

```tsx
import { Sidebar, LayoutPortal, Button, baseIcons } from "@neuron/ui";
import { useState } from "react";

const ControlledSidebar = () => {
  const [isOpen, setIsOpen] = useState(false);

  return (
    <>
      <div>
        <Button onClick={() => setIsOpen(!isOpen)}>{isOpen ? "Close Sidebar" : "Open Sidebar"}</Button>
      </div>

      <LayoutPortal position="right">
        <Sidebar
          title="Controlled Sidebar"
          description="Controlled from external state"
          icon={baseIcons.cogSolid}
          open={isOpen}
          setOpen={setIsOpen}
        >
          <div>
            <p>My state is controlled externally</p>
            <p>Current state: {isOpen ? "Open" : "Closed"}</p>
          </div>
        </Sidebar>
      </LayoutPortal>
    </>
  );
};
```

### 3.3 State Persistence

The sidebar automatically persists its state using session storage:

```tsx
// Automatic behavior - no code needed
// State is saved as: sessionStorage.setItem("sidebarOpen", "true/false")
// State is restored on component mount
```

## Step 4: Content Configuration

### 4.1 Structured Content Mode

Use the standard structured content with title, description, and content:

```tsx
import { Sidebar, LayoutPortal, baseIcons } from "@neuron/ui";

const StructuredSidebar = () => {
  return (
    <LayoutPortal position="right">
      <Sidebar
        title="User Settings"
        description="Manage your account preferences"
        tooltip="Click to configure your settings"
        icon={baseIcons.userSolid}
        open={true}
        tags={[
          { text: "New", variant: "success" },
          { text: "Beta", variant: "warning" },
        ]}
      >
        <div className="d-grid gap-16">
          <div>Profile Settings</div>
          <div>Privacy Settings</div>
          <div>Notification Settings</div>
        </div>
      </Sidebar>
    </LayoutPortal>
  );
};
```

### 4.2 Custom Content Mode

Use completely custom content structure:

```tsx
import { Sidebar, SidebarContent, LayoutPortal, baseIcons } from "@neuron/ui";

const CustomContentSidebar = () => {
  return (
    <LayoutPortal position="right">
      <Sidebar
        icon={baseIcons.layerGroupSolid}
        open={true}
        alowCustomContent={true} // Enable custom content mode
      >
        <SidebarContent
          title="Section 1"
          description="First custom section"
          tooltip="First section tooltip"
          tags={[{ text: "Active", variant: "success" }]}
          headerContent={<div>Custom header 1</div>}
        >
          <div>Custom content for section 1</div>
        </SidebarContent>

        <SidebarContent
          title="Section 2"
          description="Second custom section"
          tooltip="Second section tooltip"
          tags={[{ text: "Draft", variant: "warning" }]}
          headerContent={<div>Custom header 2</div>}
        >
          <div>Custom content for section 2</div>
        </SidebarContent>
      </Sidebar>
    </LayoutPortal>
  );
};
```

### 4.3 Header and Footer Content

Add custom header and footer content:

```tsx
import { Sidebar, LayoutPortal, Button, baseIcons } from "@neuron/ui";

const HeaderFooterSidebar = () => {
  return (
    <LayoutPortal position="right">
      <Sidebar
        title="Project Tools"
        description="Development utilities"
        icon={baseIcons.toolboxSolid}
        open={true}
        headerContent={
          <div className="d-flex justify-content-between">
            <span>Last updated: Today</span>
            <Button size="small" variant="plain">
              Refresh
            </Button>
          </div>
        }
        footerContent={
          <div className="d-grid gap-8">
            <Button variant="primary" size="small">
              Save Changes
            </Button>
            <Button variant="secondary" size="small">
              Reset
            </Button>
          </div>
        }
      >
        <div className="d-grid gap-16">
          <div>Tool 1</div>
          <div>Tool 2</div>
          <div>Tool 3</div>
        </div>
      </Sidebar>
    </LayoutPortal>
  );
};
```

## Step 5: Action Buttons Integration

### 5.1 Basic Action Buttons

Add action buttons to the sidebar:

```tsx
import { Sidebar, LayoutPortal, baseIcons } from "@neuron/ui";

const ActionButtonsSidebar = () => {
  const sidebarButtons = [
    {
      children: "Save",
      onClick: () => console.info("Save clicked"),
      variant: "primary" as const,
    },
    {
      children: "Export",
      onClick: () => console.info("Export clicked"),
      variant: "secondary" as const,
    },
    {
      children: "Delete",
      onClick: () => console.info("Delete clicked"),
      variant: "danger" as const,
      disabled: false,
    },
  ];

  return (
    <LayoutPortal position="right">
      <Sidebar
        title="Document Actions"
        description="Available document operations"
        icon={baseIcons.documentSolid}
        open={true}
        buttons={sidebarButtons}
      >
        <div>
          <p>Document content and information</p>
          <p>Use the buttons above to perform actions</p>
        </div>
      </Sidebar>
    </LayoutPortal>
  );
};
```

### 5.2 Dynamic Action Buttons

Generate action buttons dynamically:

```tsx
import { Sidebar, LayoutPortal, baseIcons } from "@neuron/ui";
import { ButtonProps } from "@neuron/ui";

const DynamicActionsSidebar = () => {
  const createActionButtons = (): ButtonProps[] => {
    const actions = ["Create", "Edit", "Delete", "Share"];

    return actions.map((action, index) => ({
      children: action,
      onClick: () => console.info(`${action} clicked`),
      variant: index === 0 ? ("primary" as const) : ("secondary" as const),
      disabled: action === "Delete", // Conditionally disable
    }));
  };

  return (
    <LayoutPortal position="right">
      <Sidebar
        title="Dynamic Actions"
        description="Dynamically generated actions"
        icon={baseIcons.boltSolid}
        open={true}
        buttons={createActionButtons()}
      >
        <div>
          <p>Content with dynamic actions</p>
        </div>
      </Sidebar>
    </LayoutPortal>
  );
};
```

## Step 6: Tags and Metadata

### 6.1 Status Tags

Add status tags to provide context:

```tsx
import { Sidebar, LayoutPortal, baseIcons } from "@neuron/ui";

const TaggedSidebar = () => {
  return (
    <LayoutPortal position="right">
      <Sidebar
        title="Project Status"
        description="Current project information"
        icon={baseIcons.projectDiagramSolid}
        open={true}
        tags={[
          { text: "Active", variant: "success" },
          { text: "High Priority", variant: "danger" },
          { text: "Due Soon", variant: "warning" },
        ]}
      >
        <div className="d-grid gap-16">
          <div>Project details...</div>
          <div>Team members...</div>
          <div>Recent activity...</div>
        </div>
      </Sidebar>
    </LayoutPortal>
  );
};
```

### 6.2 Conditional Tags

Show tags based on conditions:

```tsx
import { Sidebar, LayoutPortal, baseIcons } from "@neuron/ui";

const ConditionalTagsSidebar = ({ projectStatus, isUrgent, hasUpdates }) => {
  const getTags = () => {
    const tags = [];

    if (projectStatus === "active") {
      tags.push({ text: "Active", variant: "success" as const });
    }

    if (isUrgent) {
      tags.push({ text: "Urgent", variant: "danger" as const });
    }

    if (hasUpdates) {
      tags.push({ text: "Updated", variant: "info" as const });
    }

    return tags;
  };

  return (
    <LayoutPortal position="right">
      <Sidebar
        title="Project Info"
        description="Dynamic project status"
        icon={baseIcons.infoCircleSolid}
        open={true}
        tags={getTags()}
      >
        <div>Project content based on status</div>
      </Sidebar>
    </LayoutPortal>
  );
};
```

## Step 7: Real-World Implementation Patterns

### 7.1 Application-Level Sidebar Setup

How to implement sidebar in your application (based on the starter example):

```tsx
// YourPage.tsx
import { Helmet, Spinner, LayoutPortal } from "@neuron/ui";
import { Suspense, lazy } from "react";
import { useTranslation } from "react-i18next";

const YourSidebarComponent = lazy(() => import("features/sidebar/YourSidebar"));

const YourPage = () => {
  const { t } = useTranslation("translation");

  return (
    <Helmet title={t("yourPage.title")}>
      <Suspense fallback={<Spinner show />}>
        {/* Main page content */}
        <div>
          <h1>Your Page Content</h1>
        </div>

        {/* Sidebar in layout portal */}
        <LayoutPortal position="right">
          <YourSidebarComponent />
        </LayoutPortal>
      </Suspense>
    </Helmet>
  );
};

export default YourPage;
```

### 7.2 Sidebar Helper Component

Create reusable sidebar components:

```tsx
// YourSidebar.tsx
import { Sidebar, Tile, baseIcons, ButtonProps } from "@neuron/ui";
import { useTranslation } from "react-i18next";

export const YourSidebar = () => {
  const { t } = useTranslation("translation");

  const createSidebarButtons = (): ButtonProps[] => {
    return [
      {
        children: t("sidebar.action1"),
        onClick: () => console.info("Action 1 clicked"),
        variant: "primary",
      },
      {
        children: t("sidebar.action2"),
        onClick: () => console.info("Action 2 clicked"),
        variant: "secondary",
      },
    ];
  };

  const createSidebarTiles = (): JSX.Element[] => {
    const tiles: JSX.Element[] = [];
    for (let i = 1; i <= 4; i++) {
      tiles.push(
        <Tile key={`tile-${i}`} to="#" variant="outline">
          {t("sidebar.tile", { number: i })}
        </Tile>,
      );
    }
    return tiles;
  };

  return (
    <Sidebar
      title={t("sidebar.title")}
      description={t("sidebar.description")}
      tooltip={t("sidebar.tooltip")}
      icon={baseIcons.placeholder}
      open={true}
      buttons={createSidebarButtons()}
    >
      <div className="d-grid gap-16">{createSidebarTiles()}</div>
    </Sidebar>
  );
};
```

### 7.3 Context-Aware Sidebar

Create sidebars that respond to application context:

```tsx
import { Sidebar, LayoutPortal, baseIcons } from "@neuron/ui";
import { useContext } from "react";

const ContextAwareSidebar = () => {
  const { user, currentProject, permissions } = useContext(AppContext);

  const getSidebarContent = () => {
    if (!user) {
      return <div>Please log in to see sidebar content</div>;
    }

    if (currentProject) {
      return (
        <div className="d-grid gap-16">
          <div>Project: {currentProject.name}</div>
          <div>Status: {currentProject.status}</div>
          <div>Team: {currentProject.teamSize} members</div>
        </div>
      );
    }

    return <div>Select a project to see details</div>;
  };

  const getSidebarButtons = () => {
    if (!permissions.canEdit) return [];

    return [
      {
        children: "Edit Project",
        onClick: () => console.info("Edit project"),
        variant: "primary" as const,
      },
      {
        children: "Share",
        onClick: () => console.info("Share project"),
        variant: "secondary" as const,
      },
    ];
  };

  return (
    <LayoutPortal position="right">
      <Sidebar
        title="Project Details"
        description="Current project information"
        icon={baseIcons.folderSolid}
        open={true}
        buttons={getSidebarButtons()}
        tags={currentProject ? [{ text: currentProject.status, variant: "info" }] : []}
      >
        {getSidebarContent()}
      </Sidebar>
    </LayoutPortal>
  );
};
```

## Step 8: Sidebar Props Reference

### 8.1 Core Sidebar Props

| Prop        | Type                           | Default | Description             |
| ----------- | ------------------------------ | ------- | ----------------------- |
| title       | `string`                       | -       | Sidebar title           |
| description | `string`                       | -       | Sidebar description     |
| tooltip     | `string \| TooltipConfig`      | -       | Sidebar tooltip         |
| icon        | `IconDefinition \| TBaseIcons` | -       | Sidebar icon            |
| open        | `boolean`                      | -       | Controlled open state   |
| setOpen     | `(value: boolean) => void`     | -       | Controlled state setter |
| className   | `string`                       | -       | Additional CSS classes  |
| testId      | `string`                       | -       | Test identifier         |

### 8.2 Content Props

| Prop              | Type        | Default | Description                |
| ----------------- | ----------- | ------- | -------------------------- |
| children          | `ReactNode` | -       | Main sidebar content       |
| headerContent     | `ReactNode` | -       | Custom header content      |
| footerContent     | `ReactNode` | -       | Custom footer content      |
| alowCustomContent | `boolean`   | `false` | Enable custom content mode |

### 8.3 Action Props

| Prop    | Type            | Default | Description          |
| ------- | --------------- | ------- | -------------------- |
| buttons | `ButtonProps[]` | -       | Action buttons array |
| tags    | `TagProps[]`    | -       | Status tags array    |

### 8.4 SidebarContent Props (Custom Content Mode)

| Prop          | Type                      | Default | Description            |
| ------------- | ------------------------- | ------- | ---------------------- |
| title         | `string`                  | -       | Section title          |
| description   | `string`                  | -       | Section description    |
| tooltip       | `string \| TooltipConfig` | -       | Section tooltip        |
| tags          | `TagProps[]`              | -       | Section tags           |
| headerContent | `ReactNode`               | -       | Section header content |
| children      | `ReactNode`               | -       | Section content        |

## Step 9: Best Practices

### 9.1 When to Use Sidebar

**Use Sidebar for:**

- Persistent contextual information
- Secondary navigation
- Tool panels and utilities
- Project/document metadata
- Quick actions and shortcuts

```tsx
{
  /* Good: Contextual project information */
}
<Sidebar title="Project Info" icon={baseIcons.projectDiagramSolid}>
  <div>Project details that user needs to reference</div>
</Sidebar>;

{
  /* Good: Tool panel */
}
<Sidebar title="Design Tools" icon={baseIcons.toolboxSolid}>
  <div>Design utilities and quick actions</div>
</Sidebar>;
```

**Don't use Sidebar for:**

- Primary navigation (use Menu component)
- Modal dialogs (use Modal component)
- Temporary notifications (use Toast component)

### 9.2 Layout Integration Best Practices

- **Always use LayoutPortal** for proper positioning
- **Prefer right-side positioning** for most use cases
- **Consider mobile experience** - sidebar becomes sidesheet

```tsx
{
  /* ✅ CORRECT: Using LayoutPortal */
}
<LayoutPortal position="right">
  <Sidebar title="Sidebar">Content</Sidebar>
</LayoutPortal>;

{
  /* ❌ INCORRECT: Direct placement */
}
<div>
  <Sidebar title="Sidebar">Content</Sidebar>
</div>;
```

### 9.3 State Management Best Practices

- **Use uncontrolled mode** for simple persistent sidebars
- **Use controlled mode** when sidebar state affects other components
- **Let the component handle persistence** automatically

```tsx
{
  /* Good: Uncontrolled for simple cases */
}
<Sidebar title="Info" open={true}>
  Content
</Sidebar>;

{
  /* Good: Controlled when state is shared */
}
const [sidebarOpen, setSidebarOpen] = useState(false);
<Sidebar title="Info" open={sidebarOpen} setOpen={setSidebarOpen}>
  Content
</Sidebar>;
```

### 9.4 Content Organization

- **Keep content scannable** with clear hierarchy
- **Use tags for status information**
- **Group related actions** in buttons
- **Provide meaningful titles and descriptions**

```tsx
{
  /* Good: Well-organized content */
}
<Sidebar
  title="Document Properties"
  description="Metadata and actions for current document"
  tags={[{ text: "Modified", variant: "warning" }]}
  buttons={[
    { children: "Save", onClick: handleSave, variant: "primary" },
    { children: "Export", onClick: handleExport, variant: "secondary" },
  ]}
>
  <div className="d-grid gap-16">
    <div>Created: {document.createdAt}</div>
    <div>Modified: {document.modifiedAt}</div>
    <div>Size: {document.size}</div>
  </div>
</Sidebar>;
```

### 9.5 Responsive Considerations

- **Design for both desktop and mobile** experiences
- **Consider touch interactions** for mobile sidesheet
- **Test at the 1024px breakpoint**

```tsx
{
  /* Content should work well in both sidebar and sidesheet modes */
}
<Sidebar title="Responsive Content">
  <div className="d-grid gap-16">
    {/* Touch-friendly spacing for mobile */}
    <Button size="medium">Touch-friendly button</Button>
    <div>Content that works on both desktop and mobile</div>
  </div>
</Sidebar>;
```

### 9.6 Accessibility Considerations

- **Provide meaningful titles and descriptions**
- **Use proper semantic HTML** in content
- **Ensure keyboard navigation** works properly
- **Add tooltips** for icon-only elements

```tsx
{
  /* Good: Accessible sidebar */
}
<Sidebar
  title="Accessible Sidebar"
  description="Clear description of sidebar purpose"
  tooltip="Additional context for screen readers"
  icon={baseIcons.accessibilitySolid}
>
  <div>
    <h3>Section Title</h3>
    <p>Descriptive content</p>
  </div>
</Sidebar>;
```

## Step 10: Common Patterns and Examples

### 10.1 Document Properties Sidebar

```tsx
import { Sidebar, LayoutPortal, baseIcons } from "@neuron/ui";

const DocumentPropertiesSidebar = ({ document }) => {
  return (
    <LayoutPortal position="right">
      <Sidebar
        title="Document Properties"
        description="Metadata and actions"
        icon={baseIcons.documentSolid}
        open={true}
        tags={[
          { text: document.status, variant: "info" },
          { text: `${document.wordCount} words`, variant: "secondary" },
        ]}
        buttons={[
          {
            children: "Save",
            onClick: () => console.info("Save document"),
            variant: "primary",
          },
          {
            children: "Export PDF",
            onClick: () => console.info("Export PDF"),
            variant: "secondary",
          },
        ]}
      >
        <div className="d-grid gap-16">
          <div>
            <strong>Created:</strong> {document.createdAt}
          </div>
          <div>
            <strong>Modified:</strong> {document.modifiedAt}
          </div>
          <div>
            <strong>Author:</strong> {document.author}
          </div>
          <div>
            <strong>Version:</strong> {document.version}
          </div>
        </div>
      </Sidebar>
    </LayoutPortal>
  );
};
```

### 10.2 Project Dashboard Sidebar

```tsx
import { Sidebar, LayoutPortal, Tile, baseIcons } from "@neuron/ui";

const ProjectDashboardSidebar = ({ project, team }) => {
  return (
    <LayoutPortal position="right">
      <Sidebar
        title={project.name}
        description="Project overview and quick actions"
        icon={baseIcons.projectDiagramSolid}
        open={true}
        tags={[
          { text: project.status, variant: "success" },
          { text: `${team.length} members`, variant: "info" },
        ]}
        headerContent={<div>Last updated: {project.lastUpdated}</div>}
        buttons={[
          {
            children: "New Task",
            onClick: () => console.info("Create new task"),
            variant: "primary",
          },
        ]}
      >
        <div className="d-grid gap-16">
          <div>
            <h4>Recent Activity</h4>
            {project.recentActivity.map((activity, index) => (
              <div key={index}>{activity}</div>
            ))}
          </div>

          <div>
            <h4>Quick Links</h4>
            <div className="d-grid gap-8">
              <Tile to="/tasks" variant="outline">
                Tasks
              </Tile>
              <Tile to="/files" variant="outline">
                Files
              </Tile>
              <Tile to="/team" variant="outline">
                Team
              </Tile>
            </div>
          </div>
        </div>
      </Sidebar>
    </LayoutPortal>
  );
};
```

### 10.3 Settings Panel Sidebar

```tsx
import { Sidebar, SidebarContent, LayoutPortal, baseIcons } from "@neuron/ui";

const SettingsPanelSidebar = () => {
  return (
    <LayoutPortal position="right">
      <Sidebar icon={baseIcons.cogSolid} open={true} alowCustomContent={true}>
        <SidebarContent
          title="User Preferences"
          description="Personal settings and preferences"
          tags={[{ text: "Updated", variant: "success" }]}
        >
          <div className="d-grid gap-12">
            <div>Theme: Dark</div>
            <div>Language: English</div>
            <div>Notifications: Enabled</div>
          </div>
        </SidebarContent>

        <SidebarContent
          title="Account Settings"
          description="Account and security settings"
          tags={[{ text: "Secure", variant: "info" }]}
        >
          <div className="d-grid gap-12">
            <div>Two-factor: Enabled</div>
            <div>Last login: Today</div>
            <div>Sessions: 3 active</div>
          </div>
        </SidebarContent>
      </Sidebar>
    </LayoutPortal>
  );
};
```

## Step 11: Common Mistakes to Avoid

### 11.1 Don't Forget LayoutPortal

```tsx
{
  /* ❌ INCORRECT: Direct placement */
}
<div>
  <Sidebar title="Sidebar">Content</Sidebar>
  <div>Main content</div>
</div>;

{
  /* ✅ CORRECT: Using LayoutPortal */
}
<>
  <LayoutPortal position="right">
    <Sidebar title="Sidebar">Content</Sidebar>
  </LayoutPortal>
  <div>Main content</div>
</>;
```

### 11.2 Don't Mix Controlled and Uncontrolled

```tsx
{
  /* ❌ INCORRECT: Mixing controlled and uncontrolled */
}
<Sidebar
  title="Sidebar"
  open={isOpen} // Controlled
  // Missing setOpen - will not work properly
>
  Content
</Sidebar>;

{
  /* ✅ CORRECT: Fully controlled */
}
<Sidebar title="Sidebar" open={isOpen} setOpen={setIsOpen}>
  Content
</Sidebar>;

{
  /* ✅ CORRECT: Uncontrolled */
}
<Sidebar
  title="Sidebar"
  open={true} // Initial state only
>
  Content
</Sidebar>;
```

### 11.3 Don't Ignore Mobile Experience

```tsx
{
  /* ❌ INCORRECT: Desktop-only thinking */
}
<Sidebar title="Sidebar">
  <div style={{ width: "300px" }}>Fixed width content that might not work on mobile</div>
</Sidebar>;

{
  /* ✅ CORRECT: Responsive content */
}
<Sidebar title="Sidebar">
  <div className="d-grid gap-16">Flexible content that works on both desktop and mobile</div>
</Sidebar>;
```

### 11.4 Don't Overload with Actions

```tsx
{
  /* ❌ INCORRECT: Too many buttons */
}
<Sidebar
  title="Sidebar"
  buttons={[
    { children: "Action 1", onClick: () => {} },
    { children: "Action 2", onClick: () => {} },
    { children: "Action 3", onClick: () => {} },
    { children: "Action 4", onClick: () => {} },
    { children: "Action 5", onClick: () => {} },
    // Too many actions
  ]}
>
  Content
</Sidebar>;

{
  /* ✅ CORRECT: Focused actions */
}
<Sidebar
  title="Sidebar"
  buttons={[
    { children: "Primary Action", onClick: () => {}, variant: "primary" },
    { children: "Secondary Action", onClick: () => {}, variant: "secondary" },
  ]}
>
  Content
</Sidebar>;
```

## Key Takeaways

The Neuron Sidebar component system provides a comprehensive, responsive, and accessible foundation for side panel interfaces. Key points to remember:

1. **Always use LayoutPortal** for proper sidebar positioning in the layout
2. **Responsive by design** - automatically adapts to desktop/mobile with 1024px breakpoint
3. **State persistence** - automatically saves and restores open/close state
4. **Flexible content modes** - structured content or completely custom content
5. **Controlled/uncontrolled patterns** - choose based on your state management needs
6. **Action integration** - built-in support for buttons and tags
7. **Mobile-first thinking** - design content that works in both sidebar and sidesheet modes
8. **Accessibility compliance** - provide meaningful titles, descriptions, and tooltips
9. **Performance considerations** - use lazy loading for heavy sidebar content
10. **Layout integration** - seamless integration with the Neuron Layout system

By following these guidelines, you'll create consistent, accessible, and user-friendly sidebar interfaces that enhance your Neuron applications across all device types.

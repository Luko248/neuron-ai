---
agent: agent
---

# AI-Assisted Neuron ActionBar Component Integration Guide

## For the AI Assistant

Your task is to integrate and configure the Neuron ActionBar component in a React application. This guide provides comprehensive instructions for implementing ActionBar components, which serve as persistent action containers that must be placed in the Layout bottom content area across all Neuron applications.

---

## ⚠️ CRITICAL MANDATORY RULES - READ FIRST

**🚨 THESE RULES ARE NON-NEGOTIABLE:**

1. **ActionBar MUST be placed in Layout bottom content ONLY** - Never anywhere else
2. **ALL main form submissions MUST be in ActionBar** - Never inline with forms
3. **ALL primary page actions MUST be in ActionBar** - Never scattered in content
4. **NO exceptions to these rules** - This applies to every page, form, and workflow

**If you violate these rules, the implementation is WRONG and must be corrected.**

---

## Sync Metadata

- **Component Version:** v4.0.4
- **Component Source:** `packages/neuron/ui/src/lib/menus/actionBar/ActionBar.tsx`
- **Guideline Command:** `/neuron-ui-actionbar`
- **Related Skill:** `neuron-ui-layout`

## Introduction

The ActionBar component is a specialized layout component designed to display persistent actions that are always visible to users. It serves as a critical interface element for providing quick access to primary and secondary actions within Neuron applications.

### 🚨 CRITICAL: Main Actions Placement Rule

**⚠️ MANDATORY NEURON PATTERN: ALL main form submissions and primary page actions MUST ALWAYS be placed in ActionBar, NEVER inline with content. This is not optional.**

**ALWAYS in ActionBar:**

- **Form submit buttons** (Save, Submit, Create, Update, Delete, etc.)
- **Primary page actions** (Publish, Archive, Export, Import, etc.)
- **Navigation actions** (Back, Cancel, Next, Previous, etc.)
- **Workflow actions** (Approve, Reject, Review, Send, etc.)
- **CRUD operations** (Create, Read, Update, Delete)
- **Any action that affects the main content or form**

**NEVER place these actions:**

- ❌ Inline with form fields
- ❌ At the bottom of forms
- ❌ Scattered throughout page content
- ❌ In other layout sections (top, left, right)
- ❌ Inside modals or sidesheets as primary actions
- ❌ Floating or positioned elsewhere

**This rule applies to ALL pages: forms, lists, detail views, dashboards, workflows, etc.**

### What is the ActionBar Component?

The ActionBar component provides a standardized container for application actions with support for:

- **Dual action zones** - Left and right action areas for organized button placement
- **Responsive behavior** - Automatic icon-only display on mobile/tablet devices
- **Mixed content support** - Both button configurations and custom React nodes
- **Layout integration** - Designed specifically for Layout bottom content placement
- **Maximum action limit** - Displays up to 3 actions per side for optimal UX
- **Automatic responsiveness** - Built-in mobile optimization using responsive media queries

### Key Features

- **Mandatory Layout Placement**: Must be placed in Layout bottom content - no other placement is correct
- **Responsive Design**: Automatically switches to icon-only buttons on tablet and mobile
- **Dual Action Zones**: leftActions and rightActions for organized button placement
- **Mixed Content Support**: Supports both ButtonProps configurations and custom React nodes
- **Built-in Responsiveness**: Uses useResponsiveMediaQuery for automatic mobile optimization
- **Action Limit**: Maximum 3 actions per side to prevent UI overcrowding
- **TypeScript Support**: Full type safety with comprehensive prop definitions

**Storybook Reference:** You can refer to the Storybook documentation for visual examples and interactive demos of the ActionBar component.

### Figma Code Connect Integration

**🎨 Design-to-Code Connection Available**

The ActionBar component has **full Figma Code Connect integration**, enabling direct design-to-code generation from Figma designs.

**Key Features:**

- **Automatic Code Generation**: Figma MCP tools can generate accurate ActionBar component code
- **Action Zone Mapping**: Both left and right action zones are correctly mapped
- **Button Configuration**: Proper handling of button variants, icons, and text
- **Responsive Behavior**: Mobile/tablet responsive configurations are included
- **Layout Integration**: Proper Layout bottom placement is configured

**Code Connect Mappings:**

- **Action Zones**: leftActions, rightActions
- **Button Types**: ButtonProps configurations and custom React nodes
- **Responsive States**: Desktop full buttons, mobile icon-only
- **Layout Placement**: Automatic Layout bottom content integration

**Usage with Figma MCP:**

1. Use `mcp4_get_code` with ActionBar component node-id from Figma
2. Generated code will use proper `@neuron/ui/ActionBar` component structure
3. All props will be correctly mapped from design specifications
4. Layout integration will be properly configured

**Figma Design System Reference:**

- Node ID: Available in VIGo Design System
- All ActionBar variants and configurations are connected
- Direct code generation available through Figma MCP integration

## Step 1: Basic ActionBar Implementation

### 1.1 Import the ActionBar Component

```tsx
import { ActionBar } from "@neuron/ui";
```

### 1.2 CRITICAL: Layout Placement and Action Requirements

**🚨 MANDATORY RULES - NO EXCEPTIONS:**

1. **ActionBar MUST be placed in Layout bottom content ONLY** - Any other placement is WRONG
2. **ALL main form submissions MUST be in ActionBar** - NEVER inline with forms
3. **ALL primary page actions MUST be in ActionBar** - NEVER scattered in content
4. **This applies to EVERY page type** - Forms, lists, detail views, dashboards, workflows

**⚠️ VIOLATION OF THESE RULES = INCORRECT IMPLEMENTATION**

The ActionBar component is specifically designed for the Layout bottom content area and serves as the centralized location for all primary user actions. There are no exceptions to this pattern.

```tsx
import { Layout, LayoutProvider, LayoutPortal, ActionBar } from "@neuron/ui";

const App = () => {
  return (
    <LayoutProvider>
      <Layout>
        {/* Main content */}
        <div className="g-col-12">
          <h1>Application Content</h1>
        </div>
      </Layout>

      {/* CORRECT: ActionBar in Layout bottom content */}
      <LayoutPortal position="bottom">
        <ActionBar
          leftActions={[
            {
              onClick: () => console.log("Back clicked"),
              variant: "secondary",
              iconLeft: baseIcons.arrowLeftSolid,
              children: "Back",
            },
          ]}
          rightActions={[
            {
              onClick: () => console.log("Save clicked"),
              variant: "primary",
              iconLeft: baseIcons.floppyDiskSolid,
              children: "Save",
            },
          ]}
        />
      </LayoutPortal>
    </LayoutProvider>
  );
};
```

### 1.3 Alternative: Direct Layout Props

You can also place ActionBar using direct Layout props:

```tsx
import { Layout, LayoutProvider, ActionBar } from "@neuron/ui";

const App = () => {
  const actionBarComponent = (
    <ActionBar
      leftActions={[
        {
          onClick: () => console.log("Cancel clicked"),
          variant: "secondary",
          children: "Cancel",
        },
      ]}
      rightActions={[
        {
          onClick: () => console.log("Submit clicked"),
          variant: "primary",
          children: "Submit",
        },
      ]}
    />
  );

  return (
    <LayoutProvider>
      <Layout bottomContent={actionBarComponent}>
        {/* Main content */}
        <div className="g-col-12">
          <h1>Application Content</h1>
        </div>
      </Layout>
    </LayoutProvider>
  );
};
```

## ⚠️ CRITICAL: What NOT to Do - Common Violations

### ❌ WRONG: Form with Inline Submit Buttons

```tsx
// ❌ THIS IS COMPLETELY WRONG - NEVER DO THIS
const WrongFormImplementation = () => {
  const { control, handleSubmit } = useForm();

  return (
    <div className="g-col-12">
      <h1>User Form</h1>
      <form onSubmit={handleSubmit(onSubmit)}>
        <Input control={control} name="name" label="Name" />
        <Input control={control} name="email" label="Email" />

        {/* ❌ WRONG: Submit buttons inline with form - NEVER DO THIS */}
        <div className="form-actions">
          <Button variant="secondary">Cancel</Button>
          <Button variant="primary" type="submit">
            Save User
          </Button>
        </div>
      </form>
    </div>
  );
};
```

### ❌ WRONG: Primary Actions Scattered in Content

```tsx
// ❌ THIS IS COMPLETELY WRONG - NEVER DO THIS
const WrongPageActions = () => {
  return (
    <div className="g-col-12">
      <h1>Document Management</h1>
      <div className="document-content">
        <p>Document content...</p>
      </div>

      {/* ❌ WRONG: Primary actions scattered in content - NEVER DO THIS */}
      <div className="page-actions">
        <Button variant="primary">Publish Document</Button>
        <Button variant="danger">Delete Document</Button>
        <Button variant="info">Export PDF</Button>
      </div>
    </div>
  );
};
```

### ✅ CORRECT: All Main Actions in ActionBar

```tsx
// ✅ THIS IS THE ONLY CORRECT WAY
const CorrectImplementation = () => {
  const { control, handleSubmit } = useForm();

  return (
    <>
      {/* Content area - NO ACTION BUTTONS */}
      <div className="g-col-12">
        <h1>User Form</h1>
        <form onSubmit={handleSubmit(onSubmit)}>
          <Input control={control} name="name" label="Name" />
          <Input control={control} name="email" label="Email" />
          {/* NO SUBMIT BUTTONS HERE - THEY GO IN ACTIONBAR */}
        </form>
      </div>

      {/* ALL MAIN ACTIONS IN ACTIONBAR - THIS IS MANDATORY */}
      <LayoutPortal position="bottom">
        <ActionBar
          leftActions={[
            {
              onClick: handleCancel,
              variant: "secondary",
              iconLeft: baseIcons.xmarkSolid,
              children: "Cancel",
            },
          ]}
          rightActions={[
            {
              onClick: handleSubmit(onSubmit),
              variant: "primary",
              iconLeft: baseIcons.floppyDiskSolid,
              children: "Save User",
            },
          ]}
        />
      </LayoutPortal>
    </>
  );
};
```

**🚨 REMEMBER: If you see form submit buttons inline with forms or primary actions scattered in content, the implementation is WRONG and must be fixed.**

## Step 2: Action Configuration and Button Props

### 2.1 Basic Action Configuration

ActionBar actions can be configured using ButtonProps from the Neuron Button component:

```tsx
import { ActionBar, baseIcons } from "@neuron/ui";

const BasicActionBar = () => {
  const handleBack = () => {
    console.log("Back action triggered");
  };

  const handleSave = () => {
    console.log("Save action triggered");
  };

  const handleCancel = () => {
    console.log("Cancel action triggered");
  };

  return (
    <ActionBar
      leftActions={[
        {
          onClick: handleBack,
          variant: "secondary",
          iconLeft: baseIcons.arrowLeftSolid,
          children: "Back to List",
        },
      ]}
      rightActions={[
        {
          onClick: handleCancel,
          variant: "secondary",
          iconLeft: baseIcons.xmarkSolid,
          children: "Cancel",
        },
        {
          onClick: handleSave,
          variant: "primary",
          iconLeft: baseIcons.floppyDiskSolid,
          children: "Save Changes",
        },
      ]}
    />
  );
};
```

### 2.2 Action Variants and Styling

Use appropriate button variants to convey action importance:

```tsx
import { ActionBar, baseIcons } from "@neuron/ui";

const ActionVariants = () => {
  return (
    <ActionBar
      leftActions={[
        // Secondary actions - less prominent
        {
          onClick: () => console.log("Back clicked"),
          variant: "secondary",
          iconLeft: baseIcons.arrowLeftSolid,
          children: "Back",
        },
      ]}
      rightActions={[
        // Destructive action
        {
          onClick: () => console.log("Delete clicked"),
          variant: "danger",
          iconLeft: baseIcons.trashCanSolid,
          children: "Delete",
        },
        // Warning action
        {
          onClick: () => console.log("Review clicked"),
          variant: "warning",
          iconLeft: baseIcons.eyeSolid,
          children: "Review",
        },
        // Primary action - most prominent
        {
          onClick: () => console.log("Submit clicked"),
          variant: "primary",
          iconLeft: baseIcons.checkSolid,
          children: "Submit",
        },
      ]}
    />
  );
};
```

### 2.3 Icons in Actions

Always include meaningful icons in ActionBar buttons for better mobile experience:

```tsx
import { ActionBar, baseIcons } from "@neuron/ui";

const IconActions = () => {
  return (
    <ActionBar
      leftActions={[
        {
          onClick: () => console.log("Edit clicked"),
          variant: "secondary",
          iconLeft: baseIcons.penSolid,
          children: "Edit",
        },
      ]}
      rightActions={[
        {
          onClick: () => console.log("Download clicked"),
          variant: "info",
          iconLeft: baseIcons.downloadSolid,
          children: "Download",
        },
        {
          onClick: () => console.log("Share clicked"),
          variant: "secondary",
          iconLeft: baseIcons.shareSolid,
          children: "Share",
        },
        {
          onClick: () => console.log("Publish clicked"),
          variant: "success",
          iconLeft: baseIcons.rocketSolid,
          children: "Publish",
        },
      ]}
    />
  );
};
```

## Step 3: Responsive Behavior and Mobile Optimization

### 3.1 Automatic Responsive Behavior

**The ActionBar automatically handles responsive behavior:**

- **Desktop/Large screens**: Shows full buttons with text and icons
- **Tablet and Mobile**: Shows icon-only buttons (text is hidden)
- **Uses `useResponsiveMediaQuery("tablet")`** to detect screen size
- **Maximum 3 actions per side** to prevent overcrowding

```tsx
import { ActionBar, baseIcons } from "@neuron/ui";

const ResponsiveActionBar = () => {
  return (
    <ActionBar
      leftActions={[
        {
          onClick: () => console.log("Back clicked"),
          variant: "secondary",
          iconLeft: baseIcons.arrowLeftSolid,
          children: "Back to Dashboard", // Hidden on mobile, icon shows
        },
      ]}
      rightActions={[
        {
          onClick: () => console.log("Save draft clicked"),
          variant: "secondary",
          iconLeft: baseIcons.floppyDiskSolid,
          children: "Save Draft", // Hidden on mobile, icon shows
        },
        {
          onClick: () => console.log("Preview clicked"),
          variant: "info",
          iconLeft: baseIcons.eyeSolid,
          children: "Preview", // Hidden on mobile, icon shows
        },
        {
          onClick: () => console.log("Publish clicked"),
          variant: "primary",
          iconLeft: baseIcons.rocketSolid,
          children: "Publish", // Hidden on mobile, icon shows
        },
      ]}
    />
  );
};
```

### 3.2 Understanding Responsive Behavior

The responsive behavior is handled automatically by the ActionBar component:

```tsx
// Inside ActionBar.helpers.tsx - this is automatic
export const ActionBarItems: React.FC<{ items?: (ReactNode | ActionBarItemProps)[] }> = ({ items }) => {
  const isTablet = useResponsiveMediaQuery("tablet");

  return (
    <>
      {items?.slice(0, maxItemsCount).map((item, index) => {
        if (React.isValidElement(item)) {
          return <React.Fragment key={index}>{item}</React.Fragment>;
        } else if (isActionBarItemProps(item)) {
          return (
            <Button key={index} {...item}>
              {!isTablet && item.children} {/* Text hidden on tablet/mobile */}
            </Button>
          );
        }
        return null;
      })}
    </>
  );
};
```

**Key Points:**

- **Desktop**: Full buttons with text and icons
- **Tablet/Mobile**: Icon-only buttons (text automatically hidden)
- **No manual implementation needed** - handled by the component
- **Always include icons** - essential for mobile usability

## Step 4: Custom React Nodes and Advanced Patterns

### 4.1 Mixed Content: ButtonProps and Custom Nodes

ActionBar supports both ButtonProps configurations and custom React nodes:

```tsx
import { ActionBar, Button, Popover, InnerAction, baseIcons } from "@neuron/ui";
import { useRef } from "react";
import { useResponsiveMediaQuery } from "@neuron/core";

const CustomDropdownButton = () => {
  const popoverRef = useRef(null);
  const isTablet = useResponsiveMediaQuery("tablet");

  return (
    <>
      {!isTablet ? (
        <Button
          onClick={(e) => popoverRef.current?.toggle(e)}
          variant="secondary"
          iconRight={baseIcons.chevronDownSolid}
        >
          Print & Share
        </Button>
      ) : (
        <Button
          onClick={(e) => popoverRef.current?.toggle(e)}
          variant="secondary"
          iconRight={baseIcons.ellipsisVerticalSolid}
        />
      )}

      <Popover ref={popoverRef} showCloseIcon>
        <InnerAction onClick={() => console.log("Print clicked")}>Print Document</InnerAction>
        <InnerAction onClick={() => console.log("Email clicked")}>Email Document</InnerAction>
        <InnerAction onClick={() => console.log("Export clicked")}>Export PDF</InnerAction>
      </Popover>
    </>
  );
};

const MixedContentActionBar = () => {
  return (
    <ActionBar
      leftActions={[
        // Standard ButtonProps configuration
        {
          onClick: () => console.log("Back clicked"),
          variant: "secondary",
          iconLeft: baseIcons.arrowLeftSolid,
          children: "Back",
        },
      ]}
      rightActions={[
        // Custom React node with manual responsive handling
        <CustomDropdownButton key="dropdown" />,
        // Standard ButtonProps configuration
        {
          onClick: () => console.log("Save clicked"),
          variant: "primary",
          iconLeft: baseIcons.floppyDiskSolid,
          children: "Save",
        },
      ]}
    />
  );
};
```

### 4.2 Manual Responsive Implementation for Custom Nodes

When using custom React nodes, you must manually implement responsive behavior:

```tsx
import { ActionBar, Button, baseIcons } from "@neuron/ui";
import { useResponsiveMediaQuery } from "@neuron/core";

const ResponsiveCustomButton = () => {
  const isTablet = useResponsiveMediaQuery("tablet");

  return (
    <Button
      onClick={() => console.log("Custom action clicked")}
      variant="info"
      iconLeft={baseIcons.cogSolid}
      iconRight={!isTablet ? baseIcons.chevronDownSolid : undefined}
    >
      {!isTablet && "Advanced Settings"} {/* Hide text on mobile */}
    </Button>
  );
};

const ManualResponsiveActionBar = () => {
  return (
    <ActionBar
      leftActions={[
        {
          onClick: () => console.log("Back clicked"),
          variant: "secondary",
          iconLeft: baseIcons.arrowLeftSolid,
          children: "Back",
        },
      ]}
      rightActions={[
        // Custom node with manual responsive handling
        <ResponsiveCustomButton key="custom" />,
        // Standard ButtonProps - automatic responsive handling
        {
          onClick: () => console.log("Save clicked"),
          variant: "primary",
          iconLeft: baseIcons.floppyDiskSolid,
          children: "Save",
        },
      ]}
    />
  );
};
```

### 4.3 Complex Dropdown Actions

For complex dropdown menus, implement responsive behavior manually:

```tsx
import { ActionBar, Button, Popover, InnerAction, baseIcons } from "@neuron/ui";
import { useRef } from "react";
import { useResponsiveMediaQuery } from "@neuron/core";

const ComplexDropdownAction = () => {
  const popoverRef = useRef(null);
  const isTablet = useResponsiveMediaQuery("tablet");

  const handleExport = (format) => {
    console.log(`Exporting as ${format}`);
    popoverRef.current?.hide();
  };

  return (
    <>
      {!isTablet ? (
        <Button
          onClick={(e) => popoverRef.current?.toggle(e)}
          variant="secondary"
          iconLeft={baseIcons.downloadSolid}
          iconRight={baseIcons.chevronDownSolid}
        >
          Export Options
        </Button>
      ) : (
        <Button onClick={(e) => popoverRef.current?.toggle(e)} variant="secondary" iconLeft={baseIcons.downloadSolid} />
      )}

      <Popover ref={popoverRef} showCloseIcon>
        <InnerAction iconLeft={baseIcons.filePdfSolid} onClick={() => handleExport("PDF")}>
          Export as PDF
        </InnerAction>
        <InnerAction iconLeft={baseIcons.fileExcelSolid} onClick={() => handleExport("Excel")}>
          Export as Excel
        </InnerAction>
        <InnerAction iconLeft={baseIcons.fileWordSolid} onClick={() => handleExport("Word")}>
          Export as Word
        </InnerAction>
      </Popover>
    </>
  );
};

const ComplexActionBar = () => {
  return (
    <ActionBar
      leftActions={[
        {
          onClick: () => console.log("Back clicked"),
          variant: "secondary",
          iconLeft: baseIcons.arrowLeftSolid,
          children: "Back to List",
        },
      ]}
      rightActions={[
        <ComplexDropdownAction key="export" />,
        {
          onClick: () => console.log("Save clicked"),
          variant: "primary",
          iconLeft: baseIcons.floppyDiskSolid,
          children: "Save Document",
        },
      ]}
    />
  );
};
```

## Step 5: ActionBar Props Reference

### 5.1 Core ActionBar Props

| Prop         | Type              | Default     | Description                                       |
| ------------ | ----------------- | ----------- | ------------------------------------------------- |
| leftActions  | `ActionBarItem[]` | `undefined` | **Optional**. Actions displayed on the left side  |
| rightActions | `ActionBarItem[]` | `undefined` | **Optional**. Actions displayed on the right side |
| className    | `string`          | -           | Additional CSS classes                            |
| testId       | `string`          | -           | Custom test ID for the component                  |

### 5.2 ActionBarItem Type Definition

```tsx
type ActionBarItem = ReactNode | ActionBarItemProps;
type ActionBarItemProps = ButtonProps; // From @neuron/ui Button component
```

**ActionBarItem can be:**

1. **ButtonProps configuration** - Automatically handles responsive behavior
2. **Custom React node** - Requires manual responsive implementation

### 5.3 ButtonProps Properties (for ActionBarItemProps)

| Property  | Type            | Description                                            |
| --------- | --------------- | ------------------------------------------------------ |
| onClick   | `() => void`    | **Required**. Click handler function                   |
| children  | `ReactNode`     | Button text content (hidden on mobile automatically)   |
| variant   | `ButtonVariant` | Button visual style (primary, secondary, danger, etc.) |
| iconLeft  | `TBaseIcons`    | Left icon (essential for mobile display)               |
| iconRight | `TBaseIcons`    | Right icon (optional)                                  |
| disabled  | `boolean`       | Whether button is disabled                             |
| loading   | `boolean`       | Whether button shows loading state                     |

### 5.4 Responsive Behavior Rules

| Screen Size  | Behavior                            | Implementation                          |
| ------------ | ----------------------------------- | --------------------------------------- |
| Desktop      | Full buttons with text and icons    | Automatic                               |
| Tablet       | Icon-only buttons (text hidden)     | Automatic for ButtonProps               |
| Mobile       | Icon-only buttons (text hidden)     | Automatic for ButtonProps               |
| Custom Nodes | Manual responsive handling required | Use `useResponsiveMediaQuery("tablet")` |

### 5.5 Action Limits and Constraints

| Constraint               | Value                | Reason                         |
| ------------------------ | -------------------- | ------------------------------ |
| Maximum actions per side | 3                    | Prevents UI overcrowding       |
| Minimum icon requirement | Always include icons | Essential for mobile usability |
| Layout placement         | Bottom content only  | Component design requirement   |

## Step 6: Form Integration and Main Action Placement

### 6.1 CRITICAL: Form Submit Actions in ActionBar

**ALL form submit actions MUST be placed in ActionBar, never inline with the form.**

```tsx
import { Layout, LayoutProvider, LayoutPortal, ActionBar, Input, baseIcons } from "@neuron/ui";
import { useForm } from "react-hook-form";

const UserFormPage = () => {
  const {
    control,
    handleSubmit,
    formState: { errors },
  } = useForm();

  const onSubmit = (data) => {
    console.log("Form submitted:", data);
    // Handle form submission
  };

  const handleCancel = () => {
    console.log("Form cancelled");
    // Handle form cancellation
  };

  return (
    <LayoutProvider>
      <Layout>
        {/* CORRECT: Form content without submit buttons */}
        <div className="g-col-12">
          <h1>Create New User</h1>
          <form onSubmit={handleSubmit(onSubmit)}>
            <Input
              control={control}
              name="firstName"
              label="First Name"
              rules={{ required: "First name is required" }}
            />

            <Input control={control} name="lastName" label="Last Name" rules={{ required: "Last name is required" }} />

            <Input
              control={control}
              name="email"
              label="Email Address"
              rules={{
                required: "Email is required",
                pattern: {
                  value: /^\S+@\S+$/i,
                  message: "Invalid email format",
                },
              }}
            />

            {/* NO SUBMIT BUTTONS HERE - they go in ActionBar */}
          </form>
        </div>
      </Layout>

      {/* CORRECT: Form submit actions in ActionBar */}
      <LayoutPortal position="bottom">
        <ActionBar
          leftActions={[
            {
              onClick: handleCancel,
              variant: "secondary",
              iconLeft: baseIcons.xmarkSolid,
              children: "Cancel",
            },
          ]}
          rightActions={[
            {
              onClick: handleSubmit(onSubmit), // Form submission through ActionBar
              variant: "primary",
              iconLeft: baseIcons.userPlusSolid,
              children: "Create User",
            },
          ]}
        />
      </LayoutPortal>
    </LayoutProvider>
  );
};
```

### 6.2 WRONG vs RIGHT: Form Action Placement

```tsx
// ❌ WRONG: Submit buttons inline with form
const WrongFormImplementation = () => {
  const { control, handleSubmit } = useForm();

  return (
    <div className="g-col-12">
      <h1>User Form</h1>
      <form onSubmit={handleSubmit(onSubmit)}>
        <Input control={control} name="name" label="Name" />
        <Input control={control} name="email" label="Email" />

        {/* WRONG: Submit buttons inline with form */}
        <div className="form-actions">
          <Button variant="secondary">Cancel</Button>
          <Button variant="primary" type="submit">
            Save User
          </Button>
        </div>
      </form>
    </div>
  );
};

// ✅ RIGHT: Submit actions in ActionBar
const CorrectFormImplementation = () => {
  const { control, handleSubmit } = useForm();

  return (
    <>
      {/* Form content without submit buttons */}
      <div className="g-col-12">
        <h1>User Form</h1>
        <form onSubmit={handleSubmit(onSubmit)}>
          <Input control={control} name="name" label="Name" />
          <Input control={control} name="email" label="Email" />
          {/* NO submit buttons here */}
        </form>
      </div>

      {/* Submit actions in ActionBar */}
      <LayoutPortal position="bottom">
        <ActionBar
          leftActions={[
            {
              onClick: handleCancel,
              variant: "secondary",
              iconLeft: baseIcons.xmarkSolid,
              children: "Cancel",
            },
          ]}
          rightActions={[
            {
              onClick: handleSubmit(onSubmit),
              variant: "primary",
              iconLeft: baseIcons.floppyDiskSolid,
              children: "Save User",
            },
          ]}
        />
      </LayoutPortal>
    </>
  );
};
```

### 6.3 Main Page Actions in ActionBar

**ALL primary page actions must be in ActionBar, regardless of page type:**

```tsx
// Document/Article page
const DocumentPage = () => {
  return (
    <>
      {/* Content without action buttons */}
      <div className="g-col-12">
        <h1>Document Title</h1>
        <div className="document-content">
          <p>Document content goes here...</p>
        </div>
      </div>

      {/* ALL main actions in ActionBar */}
      <LayoutPortal position="bottom">
        <ActionBar
          leftActions={[
            {
              onClick: () => console.log("Back to list"),
              variant: "secondary",
              iconLeft: baseIcons.arrowLeftSolid,
              children: "Back to Documents",
            },
          ]}
          rightActions={[
            {
              onClick: () => console.log("Export PDF"),
              variant: "info",
              iconLeft: baseIcons.filePdfSolid,
              children: "Export PDF",
            },
            {
              onClick: () => console.log("Edit document"),
              variant: "primary",
              iconLeft: baseIcons.penSolid,
              children: "Edit Document",
            },
          ]}
        />
      </LayoutPortal>
    </>
  );
};

// Data list/table page
const DataListPage = () => {
  return (
    <>
      {/* Content without action buttons */}
      <div className="g-col-12">
        <h1>User Management</h1>
        <div className="data-table">{/* Table or list content */}</div>
      </div>

      {/* ALL main actions in ActionBar */}
      <LayoutPortal position="bottom">
        <ActionBar
          leftActions={[
            {
              onClick: () => console.log("Export data"),
              variant: "secondary",
              iconLeft: baseIcons.downloadSolid,
              children: "Export Data",
            },
          ]}
          rightActions={[
            {
              onClick: () => console.log("Add new user"),
              variant: "primary",
              iconLeft: baseIcons.userPlusSolid,
              children: "Add New User",
            },
          ]}
        />
      </LayoutPortal>
    </>
  );
};
```

### 6.4 Multi-Step Form Actions

**Even in multi-step forms, navigation and submission actions go in ActionBar:**

```tsx
const MultiStepForm = () => {
  const [currentStep, setCurrentStep] = useState(1);
  const totalSteps = 3;

  const handleNext = () => {
    if (currentStep < totalSteps) {
      setCurrentStep(currentStep + 1);
    } else {
      console.log("Form completed");
    }
  };

  const handlePrevious = () => {
    if (currentStep > 1) {
      setCurrentStep(currentStep - 1);
    }
  };

  const handleCancel = () => {
    console.log("Form cancelled");
  };

  return (
    <>
      {/* Form content without navigation buttons */}
      <div className="g-col-12">
        <h1>
          Multi-Step Form - Step {currentStep} of {totalSteps}
        </h1>
        <div className="form-step">
          {/* Step content without navigation buttons */}
          {currentStep === 1 && <StepOneContent />}
          {currentStep === 2 && <StepTwoContent />}
          {currentStep === 3 && <StepThreeContent />}
        </div>
      </div>

      {/* ALL navigation and submission actions in ActionBar */}
      <LayoutPortal position="bottom">
        <ActionBar
          leftActions={[
            {
              onClick: handleCancel,
              variant: "secondary",
              iconLeft: baseIcons.xmarkSolid,
              children: "Cancel",
            },
            ...(currentStep > 1
              ? [
                  {
                    onClick: handlePrevious,
                    variant: "secondary",
                    iconLeft: baseIcons.arrowLeftSolid,
                    children: "Previous",
                  },
                ]
              : []),
          ]}
          rightActions={[
            {
              onClick: handleNext,
              variant: "primary",
              iconLeft: currentStep < totalSteps ? baseIcons.arrowRightSolid : baseIcons.checkSolid,
              children: currentStep < totalSteps ? "Next" : "Complete",
            },
          ]}
        />
      </LayoutPortal>
    </>
  );
};
```

### 6.5 Workflow and Approval Actions

**Workflow actions like approve, reject, review must be in ActionBar:**

```tsx
const ApprovalPage = () => {
  const handleApprove = () => {
    console.log("Document approved");
  };

  const handleReject = () => {
    console.log("Document rejected");
  };

  const handleRequestChanges = () => {
    console.log("Changes requested");
  };

  return (
    <>
      {/* Content without workflow buttons */}
      <div className="g-col-12">
        <h1>Document Review</h1>
        <div className="document-content">
          <p>Document content for review...</p>
        </div>
      </div>

      {/* ALL workflow actions in ActionBar */}
      <LayoutPortal position="bottom">
        <ActionBar
          leftActions={[
            {
              onClick: handleRequestChanges,
              variant: "warning",
              iconLeft: baseIcons.commentSolid,
              children: "Request Changes",
            },
          ]}
          rightActions={[
            {
              onClick: handleReject,
              variant: "danger",
              iconLeft: baseIcons.xmarkSolid,
              children: "Reject",
            },
            {
              onClick: handleApprove,
              variant: "success",
              iconLeft: baseIcons.checkSolid,
              children: "Approve",
            },
          ]}
        />
      </LayoutPortal>
    </>
  );
};
```

## Step 7: Integration with Layout System

### 6.1 Layout Portal Integration

The recommended approach for ActionBar integration:

```tsx
import { Layout, LayoutProvider, LayoutPortal, ActionBar } from "@neuron/ui";

const ApplicationLayout = () => {
  return (
    <LayoutProvider>
      <Layout>
        {/* Main application content */}
        <div className="g-col-12">
          <h1>Application Content</h1>
          <p>Your main content goes here...</p>
        </div>
      </Layout>

      {/* ActionBar in bottom content - MANDATORY placement */}
      <LayoutPortal position="bottom">
        <ActionBar
          leftActions={[
            {
              onClick: () => console.log("Back clicked"),
              variant: "secondary",
              iconLeft: baseIcons.arrowLeftSolid,
              children: "Back",
            },
          ]}
          rightActions={[
            {
              onClick: () => console.log("Save clicked"),
              variant: "primary",
              iconLeft: baseIcons.floppyDiskSolid,
              children: "Save",
            },
          ]}
        />
      </LayoutPortal>
    </LayoutProvider>
  );
};
```

### 6.2 Page-Specific ActionBars

Different pages can have different ActionBar configurations:

```tsx
import { LayoutPortal, ActionBar, baseIcons } from "@neuron/ui";

// Edit page ActionBar
const EditPageActionBar = () => {
  return (
    <LayoutPortal position="bottom">
      <ActionBar
        leftActions={[
          {
            onClick: () => console.log("Cancel clicked"),
            variant: "secondary",
            iconLeft: baseIcons.xmarkSolid,
            children: "Cancel",
          },
        ]}
        rightActions={[
          {
            onClick: () => console.log("Save draft clicked"),
            variant: "secondary",
            iconLeft: baseIcons.floppyDiskSolid,
            children: "Save Draft",
          },
          {
            onClick: () => console.log("Publish clicked"),
            variant: "primary",
            iconLeft: baseIcons.rocketSolid,
            children: "Publish",
          },
        ]}
      />
    </LayoutPortal>
  );
};

// View page ActionBar
const ViewPageActionBar = () => {
  return (
    <LayoutPortal position="bottom">
      <ActionBar
        leftActions={[
          {
            onClick: () => console.log("Back clicked"),
            variant: "secondary",
            iconLeft: baseIcons.arrowLeftSolid,
            children: "Back to List",
          },
        ]}
        rightActions={[
          {
            onClick: () => console.log("Edit clicked"),
            variant: "primary",
            iconLeft: baseIcons.penSolid,
            children: "Edit",
          },
        ]}
      />
    </LayoutPortal>
  );
};
```

### 6.3 Conditional ActionBar Display

Show ActionBar only when needed:

```tsx
import { LayoutPortal, ActionBar, baseIcons } from "@neuron/ui";
import { useState } from "react";

const ConditionalActionBar = () => {
  const [isEditing, setIsEditing] = useState(false);
  const [hasUnsavedChanges, setHasUnsavedChanges] = useState(false);

  // Only show ActionBar when editing or has unsaved changes
  if (!isEditing && !hasUnsavedChanges) {
    return null;
  }

  return (
    <LayoutPortal position="bottom">
      <ActionBar
        leftActions={[
          {
            onClick: () => {
              setIsEditing(false);
              setHasUnsavedChanges(false);
            },
            variant: "secondary",
            iconLeft: baseIcons.xmarkSolid,
            children: "Cancel",
          },
        ]}
        rightActions={[
          {
            onClick: () => {
              console.log("Saving changes...");
              setHasUnsavedChanges(false);
            },
            variant: "primary",
            iconLeft: baseIcons.floppyDiskSolid,
            children: "Save Changes",
          },
        ]}
      />
    </LayoutPortal>
  );
};
```

## Step 7: Best Practices

### 7.1 CRITICAL: Main Action Placement Rule

**🚨 ALWAYS place main actions in ActionBar:**

- **Form submissions** (Save, Submit, Create, Update, Delete)
- **Page navigation** (Back, Cancel, Next, Previous)
- **Workflow actions** (Approve, Reject, Publish, Archive)
- **Primary operations** (Export, Import, Generate, Process)

**❌ NEVER place main actions:**

- Inline with form fields
- At the bottom of forms
- Scattered in page content
- In modals or sidesheets (unless they're secondary actions)

```tsx
// ❌ WRONG: Main actions scattered in content
const WrongActionPlacement = () => (
  <div className="g-col-12">
    <h1>User Profile</h1>
    <form>
      <Input name="name" label="Name" />
      <Input name="email" label="Email" />

      {/* WRONG: Form actions inline */}
      <div className="form-buttons">
        <Button variant="secondary">Cancel</Button>
        <Button variant="primary">Save Profile</Button>
      </div>
    </form>

    <div className="profile-actions">
      {/* WRONG: Primary actions scattered in content */}
      <Button variant="danger">Delete Account</Button>
      <Button variant="info">Export Data</Button>
    </div>
  </div>
);

// ✅ RIGHT: All main actions in ActionBar
const CorrectActionPlacement = () => (
  <>
    <div className="g-col-12">
      <h1>User Profile</h1>
      <form>
        <Input name="name" label="Name" />
        <Input name="email" label="Email" />
        {/* NO action buttons here */}
      </form>

      <div className="profile-info">{/* Content only, no action buttons */}</div>
    </div>

    {/* ALL main actions in ActionBar */}
    <LayoutPortal position="bottom">
      <ActionBar
        leftActions={[
          {
            onClick: handleCancel,
            variant: "secondary",
            iconLeft: baseIcons.xmarkSolid,
            children: "Cancel",
          },
        ]}
        rightActions={[
          {
            onClick: handleExport,
            variant: "info",
            iconLeft: baseIcons.downloadSolid,
            children: "Export Data",
          },
          {
            onClick: handleDelete,
            variant: "danger",
            iconLeft: baseIcons.trashCanSolid,
            children: "Delete Account",
          },
          {
            onClick: handleSave,
            variant: "primary",
            iconLeft: baseIcons.floppyDiskSolid,
            children: "Save Profile",
          },
        ]}
      />
    </LayoutPortal>
  </>
);
```

### 7.2 Action Placement Conventions

**Left Actions (Secondary):**

- Back/Cancel actions
- Navigation actions
- Less important operations

**Right Actions (Primary):**

- Save/Submit actions
- Primary operations
- Positive actions

```tsx
// Good: Proper action placement
<ActionBar
  leftActions={[
    {
      onClick: handleCancel,
      variant: "secondary",
      iconLeft: baseIcons.xmarkSolid,
      children: "Cancel"
    }
  ]}
  rightActions={[
    {
      onClick: handleSave,
      variant: "primary",
      iconLeft: baseIcons.floppyDiskSolid,
      children: "Save"
    }
  ]}
/>

// Wrong: Primary action on left
<ActionBar
  leftActions={[
    {
      onClick: handleSave,
      variant: "primary", // Wrong: primary on left
      iconLeft: baseIcons.floppyDiskSolid,
      children: "Save"
    }
  ]}
  rightActions={[
    {
      onClick: handleCancel,
      variant: "secondary",
      iconLeft: baseIcons.xmarkSolid,
      children: "Cancel"
    }
  ]}
/>
```

### 7.2 Icon Usage Guidelines

**Always include meaningful icons:**

- Essential for mobile/tablet experience
- Provide visual context
- Improve accessibility

```tsx
// Good: Meaningful icons
<ActionBar
  rightActions={[
    {
      onClick: handleDownload,
      variant: "info",
      iconLeft: baseIcons.downloadSolid, // Clear download icon
      children: "Download"
    },
    {
      onClick: handleSave,
      variant: "primary",
      iconLeft: baseIcons.floppyDiskSolid, // Clear save icon
      children: "Save"
    }
  ]}
/>

// Wrong: Missing or unclear icons
<ActionBar
  rightActions={[
    {
      onClick: handleDownload,
      variant: "info",
      // Missing icon - poor mobile experience
      children: "Download"
    },
    {
      onClick: handleSave,
      variant: "primary",
      iconLeft: baseIcons.questionSolid, // Unclear icon
      children: "Save"
    }
  ]}
/>
```

### 7.3 Responsive Design Considerations

**For ButtonProps (Automatic):**

- Text automatically hidden on mobile
- Icons remain visible
- No manual implementation needed

**For Custom Nodes (Manual):**

- Use `useResponsiveMediaQuery("tablet")`
- Implement responsive behavior manually
- Test on different screen sizes

```tsx
// Good: Manual responsive implementation for custom nodes
const ResponsiveCustomAction = () => {
  const isTablet = useResponsiveMediaQuery("tablet");

  return (
    <Button onClick={handleAction} variant="secondary" iconLeft={baseIcons.cogSolid}>
      {!isTablet && "Settings"} {/* Hide text on mobile */}
    </Button>
  );
};

// Wrong: No responsive handling for custom nodes
const NonResponsiveCustomAction = () => {
  return (
    <Button onClick={handleAction} variant="secondary" iconLeft={baseIcons.cogSolid}>
      Settings {/* Always shows text - poor mobile experience */}
    </Button>
  );
};
```

### 7.4 Action Limit Management

**Respect the 3-action limit per side:**

- Maximum 3 actions per side
- Use dropdowns for additional actions
- Prioritize most important actions

```tsx
// Good: Within action limits
<ActionBar
  rightActions={[
    { onClick: handleAction1, variant: "secondary", iconLeft: baseIcons.icon1, children: "Action 1" },
    { onClick: handleAction2, variant: "secondary", iconLeft: baseIcons.icon2, children: "Action 2" },
    { onClick: handleAction3, variant: "primary", iconLeft: baseIcons.icon3, children: "Action 3" }
  ]}
/>

// Wrong: Too many actions
<ActionBar
  rightActions={[
    { onClick: handleAction1, variant: "secondary", iconLeft: baseIcons.icon1, children: "Action 1" },
    { onClick: handleAction2, variant: "secondary", iconLeft: baseIcons.icon2, children: "Action 2" },
    { onClick: handleAction3, variant: "secondary", iconLeft: baseIcons.icon3, children: "Action 3" },
    { onClick: handleAction4, variant: "secondary", iconLeft: baseIcons.icon4, children: "Action 4" }, // Exceeds limit
    { onClick: handleAction5, variant: "primary", iconLeft: baseIcons.icon5, children: "Action 5" } // Exceeds limit
  ]}
/>

// Good: Use dropdown for additional actions
<ActionBar
  rightActions={[
    <DropdownWithMultipleActions key="dropdown" />, // Custom dropdown component
    { onClick: handleSave, variant: "primary", iconLeft: baseIcons.floppyDiskSolid, children: "Save" }
  ]}
/>
```

## Step 8: Common Mistakes to Avoid

### 8.1 Don't Place Main Actions Outside ActionBar

```tsx
// ❌ WRONG: Form submit buttons inline with form
const WrongFormActions = () => (
  <form onSubmit={handleSubmit}>
    <Input name="name" label="Name" />
    <Input name="email" label="Email" />

    {/* WRONG: Submit actions inline with form */}
    <div className="form-actions">
      <Button variant="secondary">Cancel</Button>
      <Button variant="primary" type="submit">
        Save
      </Button>
    </div>
  </form>
);

// ❌ WRONG: Primary actions scattered in content
const WrongPageActions = () => (
  <div className="page-content">
    <h1>Document</h1>
    <div className="document-body">Content...</div>

    {/* WRONG: Primary actions in content */}
    <div className="document-actions">
      <Button variant="primary">Publish</Button>
      <Button variant="danger">Delete</Button>
    </div>
  </div>
);

// ✅ RIGHT: All main actions in ActionBar
const CorrectActionPlacement = () => (
  <>
    <form onSubmit={handleSubmit}>
      <Input name="name" label="Name" />
      <Input name="email" label="Email" />
      {/* NO submit buttons here */}
    </form>

    <LayoutPortal position="bottom">
      <ActionBar
        leftActions={[{ onClick: handleCancel, variant: "secondary", children: "Cancel" }]}
        rightActions={[{ onClick: handleSubmit, variant: "primary", children: "Save" }]}
      />
    </LayoutPortal>
  </>
);
```

### 8.2 Don't Place ActionBar Outside Layout Bottom

```tsx
// Wrong: ActionBar in main content
<div className="g-col-12">
  <h1>Page Content</h1>
  <ActionBar {...props} /> {/* Wrong placement */}
</div>

// Wrong: ActionBar in top content
<LayoutPortal position="top">
  <ActionBar {...props} /> {/* Wrong placement */}
</LayoutPortal>

// Right: ActionBar in bottom content
<LayoutPortal position="bottom">
  <ActionBar {...props} /> {/* Correct placement */}
</LayoutPortal>
```

### 8.2 Don't Forget Icons for Mobile

```tsx
// Wrong: No icons
<ActionBar
  rightActions={[
    {
      onClick: handleSave,
      variant: "primary",
      children: "Save" // No icon - poor mobile experience
    }
  ]}
/>

// Right: Always include icons
<ActionBar
  rightActions={[
    {
      onClick: handleSave,
      variant: "primary",
      iconLeft: baseIcons.floppyDiskSolid, // Icon for mobile
      children: "Save"
    }
  ]}
/>
```

### 8.3 Don't Ignore Responsive Behavior for Custom Nodes

```tsx
// Wrong: Custom node without responsive handling
const BadCustomAction = () => (
  <Button onClick={handleAction} variant="secondary">
    Always Shows Full Text {/* Poor mobile experience */}
  </Button>
);

// Right: Custom node with responsive handling
const GoodCustomAction = () => {
  const isTablet = useResponsiveMediaQuery("tablet");

  return (
    <Button onClick={handleAction} variant="secondary" iconLeft={baseIcons.cogSolid}>
      {!isTablet && "Settings"} {/* Hide text on mobile */}
    </Button>
  );
};
```

### 8.4 Don't Exceed Action Limits

```tsx
// Wrong: Too many actions
<ActionBar
  rightActions={[
    action1, action2, action3, action4, action5 // Exceeds 3-action limit
  ]}
/>

// Right: Use dropdown for additional actions
<ActionBar
  rightActions={[
    <DropdownAction key="more" />, // Dropdown with multiple actions
    primaryAction,
    secondaryAction
  ]}
/>
```

### 8.5 Don't Mix Action Placement Conventions

```tsx
// Wrong: Primary action on left, secondary on right
<ActionBar
  leftActions={[
    {
      onClick: handleSave,
      variant: "primary", // Wrong: primary on left
      children: "Save"
    }
  ]}
  rightActions={[
    {
      onClick: handleCancel,
      variant: "secondary", // Wrong: secondary on right
      children: "Cancel"
    }
  ]}
/>

// Right: Follow placement conventions
<ActionBar
  leftActions={[
    {
      onClick: handleCancel,
      variant: "secondary", // Correct: secondary on left
      children: "Cancel"
    }
  ]}
  rightActions={[
    {
      onClick: handleSave,
      variant: "primary", // Correct: primary on right
      children: "Save"
    }
  ]}
/>
```

## Summary

The Neuron ActionBar component provides a comprehensive, responsive, and consistent system for persistent application actions. Key points to remember:

1. **MANDATORY Layout placement** - Must be in Layout bottom content, no exceptions
2. **MANDATORY Main action placement** - ALL form submissions and primary actions MUST be in ActionBar, never inline
3. **Automatic responsive behavior** - ButtonProps configurations handle mobile automatically
4. **Manual responsive implementation** - Required for custom React nodes
5. **Action limits** - Maximum 3 actions per side for optimal UX
6. **Icon requirements** - Always include meaningful icons for mobile usability
7. **Placement conventions** - Secondary actions left, primary actions right
8. **Mixed content support** - Both ButtonProps and custom nodes supported
9. **Integration with Layout system** - Use LayoutPortal or direct Layout props

### Key Takeaways

- **Layout bottom placement only** - Component designed specifically for this location
- **Main actions centralization** - ALL form submissions and primary actions MUST be in ActionBar
- **No inline form buttons** - Never place submit/cancel buttons inline with forms
- **Automatic mobile optimization** - Built-in responsive behavior for ButtonProps
- **Manual responsive handling** - Required for custom React nodes using useResponsiveMediaQuery
- **Action organization** - Left for secondary, right for primary actions
- **Icon-first design** - Essential for mobile/tablet experience
- **Action limits** - Maximum 3 per side, use dropdowns for more
- **Consistent behavior** - Standardized across all Neuron applications

By following these guidelines, you'll create effective, responsive, and user-friendly action interfaces that work seamlessly across all device sizes and provide consistent user experience in your Neuron applications.

---

## 🚨 FINAL REMINDER: Critical Rules Checklist

Before implementing any ActionBar, verify these mandatory requirements:

### ✅ ActionBar Placement Checklist

- [ ] ActionBar is placed in Layout bottom content using `LayoutPortal position="bottom"` or `Layout bottomContent` prop
- [ ] ActionBar is NOT placed in main content, top, left, right, or any other location
- [ ] ActionBar is NOT placed inline with forms or scattered in page content

### ✅ Main Actions Checklist

- [ ] ALL form submit buttons (Save, Submit, Create, Update, Delete) are in ActionBar
- [ ] ALL primary page actions (Publish, Export, Archive, etc.) are in ActionBar
- [ ] ALL navigation actions (Back, Cancel, Next, Previous) are in ActionBar
- [ ] ALL workflow actions (Approve, Reject, Review) are in ActionBar
- [ ] NO main actions are inline with forms or scattered in content

### ✅ Implementation Checklist

- [ ] Forms contain NO submit buttons - they are in ActionBar
- [ ] Page content contains NO primary action buttons - they are in ActionBar
- [ ] All main actions use proper icons for mobile responsiveness
- [ ] Action placement follows conventions (secondary left, primary right)

**If any checklist item is not met, the implementation violates Neuron patterns and must be corrected.**

---

## ⚠️ Zero Tolerance Policy

**There are NO exceptions to these rules. Every violation must be fixed:**

- **Form with inline submit buttons** = WRONG implementation
- **Primary actions scattered in content** = WRONG implementation
- **ActionBar placed outside Layout bottom** = WRONG implementation

**These are not suggestions - they are mandatory Neuron UI patterns that ensure consistency, accessibility, and proper mobile behavior across all applications.**

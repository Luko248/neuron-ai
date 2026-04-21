---
agent: agent
---

# AI-Assisted Neuron SideSheet Component Integration Guide

## For the AI Assistant

Your task is to integrate and configure the Neuron SideSheet component in a React application. This guide provides comprehensive instructions for implementing SideSheet overlay dialogs, which serve as side-positioned panels for detailed content and workflows across all Neuron applications.

## Sync Metadata

- **Component Version:** v4.3.1
- **Component Source:** `packages/neuron/ui/src/lib/overlays/sideSheet/SideSheet.tsx`
- **Guideline Command:** `/neuron-ui-sidesheet`
- **Related Skill:** `neuron-ui-overlays`

## Introduction

The SideSheet component provides a flexible and customizable side-positioned overlay dialog used to display detailed content without completely interrupting the user's workflow. It serves as a critical interface element for presenting secondary information, forms, and detailed views within Neuron applications.

### What is the SideSheet Component?

The SideSheet component creates side-positioned overlay dialogs that slide in from the right side of the screen to present detailed information or capture input. It provides standardized side panel rendering with support for:

- **Two width variants** - narrow and wide for different content requirements
- **Action zones** - Dedicated areas for button placement and user interactions
- **Nesting capability** - Support for nested SideSheets with mandatory width hierarchy
- **Accessibility compliance** - Full WCAG 2.1 AA standards support
- **Responsive design** - Optimized for multi-device workflows
- **Icon integration** - Support for baseIcons with proper sizing
- **Backdrop interaction** - Configurable backdrop click behavior
- **Focus management** - Automatic focus trapping and restoration

### Key Features

- **Two Width Variants**: narrow (default) and wide for different content complexity
- **Dual Action Zones**: leftActionsZone and rightActionsZone for organized button placement
- **Nesting Support**: Hierarchical SideSheet nesting with mandatory width ordering
- **baseIcons Integration**: Proper icon support using the Neuron icon system
- **Accessibility Features**: Focus management, keyboard navigation, screen reader support
- **Backdrop Control**: Configurable backdrop click behavior for dismissal
- **Close Button Control**: Optional close button with design-driven visibility
- **Fixed Footer Option**: Sticky footer positioning for action buttons
- **TypeScript Support**: Full type safety with comprehensive prop definitions

**Storybook Reference:** You can refer to the Storybook documentation for visual examples and interactive demos of all available SideSheet configurations.

### Figma Code Connect Integration

**🎨 Design-to-Code Connection Available**

The SideSheet component has **full Figma Code Connect integration**, enabling direct design-to-code generation from Figma designs.

**Key Features:**

- **Automatic Code Generation**: Figma MCP tools can generate accurate SideSheet component code
- **Width Mapping**: Both narrow and wide variants are correctly mapped
- **Content Structure**: Proper handling of title, description, and content areas
- **Action Zone Integration**: Button configurations are automatically included
- **Icon Integration**: Header icons are properly configured

**Code Connect Mappings:**

- **Widths**: narrow, wide
- **Content Types**: title, description, body content
- **Features**: action zones, icons, fixed footer
- **Interactions**: closable, backdrop click, escape key

**Usage with Figma MCP:**

1. Use `mcp4_get_code` with SideSheet component node-id from Figma
2. Generated code will use proper `@neuron/ui/SideSheet` component structure
3. All props will be correctly mapped from design specifications
4. Width and action configurations will be properly set

**Figma Design System Reference:**

- Node ID: Available in VIGo Design System
- All SideSheet variants and configurations are connected
- Direct code generation available through Figma MCP integration

## Step 1: Basic SideSheet Implementation

### 1.1 Import the SideSheet Component

```tsx
import { SideSheet } from "@neuron/ui";
```

### 1.2 Basic Usage

Here's a simple implementation of the SideSheet component:

```tsx
import { SideSheet, Button } from "@neuron/ui";
import { useState } from "react";

const BasicSideSheet = () => {
  const [showSideSheet, setShowSideSheet] = useState(false);

  const handleShow = () => setShowSideSheet(true);
  const handleHide = () => setShowSideSheet(false);

  return (
    <div>
      <Button onClick={handleShow}>Open SideSheet</Button>

      <SideSheet
        show={showSideSheet}
        onHide={handleHide}
        title="Basic SideSheet"
        description="This is a basic side sheet example."
        width="narrow"
      >
        <p>SideSheet content goes here.</p>
      </SideSheet>
    </div>
  );
};
```

### 1.3 SideSheet Width Variants

The SideSheet component supports two width variants for different content requirements:

```tsx
import { SideSheet, Button } from "@neuron/ui";
import { useState } from "react";

const SideSheetWidths = () => {
  const [showNarrow, setShowNarrow] = useState(false);
  const [showWide, setShowWide] = useState(false);

  return (
    <div className="sidesheet-widths">
      {/* Narrow SideSheet - default width for simple content */}
      <Button onClick={() => setShowNarrow(true)}>Open Narrow SideSheet</Button>

      <SideSheet
        show={showNarrow}
        onHide={() => setShowNarrow(false)}
        title="Narrow SideSheet"
        description="Ideal for simple forms and basic information"
        width="narrow"
      >
        <p>This is a narrow side sheet suitable for simple content, forms, and basic information display.</p>
      </SideSheet>

      {/* Wide SideSheet - larger width for complex content */}
      <Button onClick={() => setShowWide(true)}>Open Wide SideSheet</Button>

      <SideSheet
        show={showWide}
        onHide={() => setShowWide(false)}
        title="Wide SideSheet"
        description="Perfect for complex forms and detailed content"
        width="wide"
      >
        <div>
          <p>This is a wide side sheet suitable for:</p>
          <ul>
            <li>Complex forms with multiple fields</li>
            <li>Detailed information displays</li>
            <li>Rich content with multiple sections</li>
            <li>Data tables and lists</li>
          </ul>
        </div>
      </SideSheet>
    </div>
  );
};
```

## Step 2: SideSheet Nesting - MANDATORY HIERARCHY

### 2.1 Nesting Rules and Requirements

**🚨 CRITICAL: SideSheet nesting has mandatory width hierarchy rules:**

- **First SideSheet MUST be "wide"**
- **Nested SideSheet MUST be "narrow"**
- **No reverse option available**
- **Maximum nesting depth: 2 levels**

```tsx
import { SideSheet, Button, baseIcons } from "@neuron/ui";
import { useState } from "react";

const NestedSideSheets = () => {
  const [showFirstSheet, setShowFirstSheet] = useState(false);
  const [showSecondSheet, setShowSecondSheet] = useState(false);

  const handleOpenFirst = () => setShowFirstSheet(true);
  const handleCloseFirst = () => setShowFirstSheet(false);

  const handleOpenSecond = () => setShowSecondSheet(true);
  const handleCloseSecond = () => setShowSecondSheet(false);

  return (
    <div>
      <Button onClick={handleOpenFirst}>Open First SideSheet (Wide)</Button>

      {/* FIRST SIDESHEET - MUST BE WIDE */}
      <SideSheet
        show={showFirstSheet}
        onHide={handleCloseFirst}
        title="Main Settings"
        description="Primary configuration panel"
        width="wide" // MANDATORY: First SideSheet must be wide
        icon={{
          iconDef: baseIcons.gearSolid,
        }}
        rightActionsZone={
          <Button variant="primary" onClick={handleCloseFirst}>
            Apply Settings
          </Button>
        }
      >
        <div className="main-settings">
          <p>This is the main settings panel with comprehensive options.</p>

          <Button variant="secondary" onClick={handleOpenSecond} iconLeft={baseIcons.slidersSolid}>
            Advanced Options
          </Button>

          <div className="settings-content">
            <p>Main configuration options would be displayed here...</p>
          </div>
        </div>
      </SideSheet>

      {/* SECOND SIDESHEET - MUST BE NARROW */}
      <SideSheet
        show={showSecondSheet}
        onHide={handleCloseSecond}
        title="Advanced Options"
        description="Detailed configuration settings"
        width="narrow" // MANDATORY: Nested SideSheet must be narrow
        icon={{
          iconDef: baseIcons.slidersSolid,
        }}
        leftActionsZone={
          <Button variant="secondary" onClick={handleCloseSecond}>
            Cancel
          </Button>
        }
        rightActionsZone={
          <Button variant="primary" onClick={handleCloseSecond}>
            Save Advanced Settings
          </Button>
        }
      >
        <div className="advanced-settings">
          <p>Advanced configuration options for power users.</p>
          <ul>
            <li>Performance tuning</li>
            <li>Debug settings</li>
            <li>Advanced security options</li>
          </ul>
        </div>
      </SideSheet>
    </div>
  );
};
```

## Step 3: Header Configuration and Icons

### 3.1 SideSheet Header Structure

Each SideSheet header consists of:

- **Icon**: Optional icon for visual context (automatically sized)
- **Title**: Main heading for the SideSheet
- **Description**: Optional subtitle providing additional context

```tsx
import { SideSheet, Button, baseIcons, IconSize } from "@neuron/ui";
import { useState } from "react";

const SideSheetHeaders = () => {
  const [showSideSheet, setShowSideSheet] = useState(false);

  return (
    <div className="sidesheet-headers">
      <Button onClick={() => setShowSideSheet(true)}>Open SideSheet with Header</Button>

      <SideSheet
        show={showSideSheet}
        onHide={() => setShowSideSheet(false)}
        title="User Profile Settings"
        description="Manage your account preferences and personal information"
        icon={{
          iconDef: baseIcons.userSolid,
          size: IconSize.large,
        }}
        width="narrow"
      >
        <div className="profile-content">
          <p>Configure your profile settings here.</p>
        </div>
      </SideSheet>
    </div>
  );
};
```

## Step 4: Action Zones and Button Placement

### 4.1 Default Behavior - No Buttons

**By default, SideSheet has no action buttons and no footer.** The footer only appears when you explicitly provide content to the action zones:

```tsx
import { SideSheet, Button } from "@neuron/ui";
import { useState } from "react";

const SideSheetWithoutButtons = () => {
  const [showSideSheet, setShowSideSheet] = useState(false);

  return (
    <div>
      <Button onClick={() => setShowSideSheet(true)}>Open SideSheet (No Buttons)</Button>

      {/* SideSheet without any action buttons - no footer rendered */}
      <SideSheet
        show={showSideSheet}
        onHide={() => setShowSideSheet(false)}
        title="SideSheet Without Buttons"
        description="This SideSheet has no footer because no action zones are provided"
        width="narrow"
        // No leftActionsZone or rightActionsZone = no footer
      >
        <div className="content">
          <p>This SideSheet has no action buttons.</p>
          <p>Users can only close it using the X button (if closable=true) or backdrop click.</p>
        </div>
      </SideSheet>
    </div>
  );
};
```

### 4.2 Basic Action Zone Configuration

SideSheet provides two optional action zones for organized button placement:

```tsx
import { SideSheet, Button, baseIcons } from "@neuron/ui";
import { useState } from "react";

const ActionZoneExample = () => {
  const [showSideSheet, setShowSideSheet] = useState(false);

  const handleSave = () => {
    console.log("Saving changes...");
    setShowSideSheet(false);
  };

  const handleCancel = () => {
    console.log("Cancelling changes...");
    setShowSideSheet(false);
  };

  return (
    <div>
      <Button onClick={() => setShowSideSheet(true)}>Open SideSheet with Actions</Button>

      <SideSheet
        show={showSideSheet}
        onHide={() => setShowSideSheet(false)}
        title="Edit Profile"
        description="Update your profile information"
        width="narrow"
        leftActionsZone={
          <Button variant="secondary" onClick={handleCancel}>
            Cancel
          </Button>
        }
        rightActionsZone={
          <Button variant="primary" onClick={handleSave}>
            Save Changes
          </Button>
        }
      >
        <div className="form-content">
          <p>Form fields would go here...</p>
        </div>
      </SideSheet>
    </div>
  );
};
```

### 4.3 Single Action Zone Usage

You can use just one action zone if needed. The footer will still render properly:

```tsx
import { SideSheet, Button } from "@neuron/ui";
import { useState } from "react";

const SingleActionZone = () => {
  const [showLeftOnly, setShowLeftOnly] = useState(false);
  const [showRightOnly, setShowRightOnly] = useState(false);

  return (
    <div className="single-action-examples">
      {/* Only left action zone */}
      <Button onClick={() => setShowLeftOnly(true)}>Left Action Only</Button>

      <SideSheet
        show={showLeftOnly}
        onHide={() => setShowLeftOnly(false)}
        title="Left Action Only"
        description="Only has a left action button"
        width="narrow"
        leftActionsZone={
          <Button variant="secondary" onClick={() => setShowLeftOnly(false)}>
            Close
          </Button>
        }
        // No rightActionsZone
      >
        <p>This SideSheet only has a left action button.</p>
      </SideSheet>

      {/* Only right action zone */}
      <Button onClick={() => setShowRightOnly(true)}>Right Action Only</Button>

      <SideSheet
        show={showRightOnly}
        onHide={() => setShowRightOnly(false)}
        title="Right Action Only"
        description="Only has a right action button"
        width="narrow"
        // No leftActionsZone
        rightActionsZone={
          <Button variant="primary" onClick={() => setShowRightOnly(false)}>
            Done
          </Button>
        }
      >
        <p>This SideSheet only has a right action button.</p>
      </SideSheet>
    </div>
  );
};
```

## Step 5: SideSheet Props Reference

### 5.1 Core SideSheet Props

| Prop        | Type                 | Default    | Description                                     |
| ----------- | -------------------- | ---------- | ----------------------------------------------- |
| title       | `string`             | -          | The title of the SideSheet                      |
| description | `string`             | -          | Description displayed under the title           |
| children    | `ReactNode`          | -          | **Required**. Content of the SideSheet          |
| show        | `boolean`            | -          | **Required**. Controls SideSheet visibility     |
| onHide      | `() => void`         | -          | **Required**. Callback when SideSheet is hidden |
| width       | `"narrow" \| "wide"` | `"narrow"` | Width variant of the SideSheet                  |
| className   | `string`             | -          | Additional CSS classes                          |
| testId      | `string`             | -          | Custom test ID for the component                |

### 5.2 Action Zone Props

| Prop             | Type        | Default | Description                                  |
| ---------------- | ----------- | ------- | -------------------------------------------- |
| leftActionsZone  | `ReactNode` | -       | Action zone for buttons on the left side     |
| rightActionsZone | `ReactNode` | -       | Action zone for buttons on the right side    |
| fixedFooter      | `boolean`   | `false` | Whether the footer should be fixed at bottom |

### 5.3 Interaction and Dismissal Props

| Prop                 | Type                                             | Default    | Description                             |
| -------------------- | ------------------------------------------------ | ---------- | --------------------------------------- |
| closable             | `boolean`                                        | `true`     | Whether to show close button in header  |
| closeOnEscape        | `boolean`                                        | `false`    | Whether Escape key closes the SideSheet |
| closeOnBackdropClick | `boolean`                                        | `closable` | Whether backdrop click closes SideSheet |
| onBackdropClick      | `(event: React.MouseEvent<HTMLElement>) => void` | -          | Custom backdrop click handler           |

### 5.4 Nesting Hierarchy Rules

| Level        | Width    | Required        | Description                     |
| ------------ | -------- | --------------- | ------------------------------- |
| 1st (Main)   | `wide`   | **MANDATORY**   | Primary SideSheet must be wide  |
| 2nd (Nested) | `narrow` | **MANDATORY**   | Nested SideSheet must be narrow |
| 3rd+         | -        | **NOT ALLOWED** | Maximum nesting depth is 2      |

## Step 6: Best Practices

### 6.1 Nesting Best Practices

**Always Follow Nesting Hierarchy:**

```tsx
// Good: Correct nesting hierarchy
<SideSheet width="wide" title="Main Panel">
  <Button onClick={() => setShowNested(true)}>Open Details</Button>
</SideSheet>

<SideSheet width="narrow" title="Details Panel">
  Nested content
</SideSheet>

// Wrong: Incorrect hierarchy
<SideSheet width="narrow" title="Main Panel">  // Wrong: should be wide
  <Button onClick={() => setShowNested(true)}>Open Details</Button>
</SideSheet>

<SideSheet width="wide" title="Details Panel">  // Wrong: should be narrow
  Nested content
</SideSheet>
```

### 6.2 Width Selection Guidelines

**Use Narrow Width For:**

- Simple forms with few fields
- Quick actions and confirmations
- Basic information display
- Nested/secondary panels

**Use Wide Width For:**

- Complex forms with multiple sections
- Data tables and lists
- Rich content with images/media
- Primary workflow panels

## Step 7: Common Mistakes to Avoid

### 7.1 Don't Violate Nesting Hierarchy

```tsx
// Wrong: Incorrect nesting order
<SideSheet width="narrow" title="Main Panel">  // Should be wide
  <Button onClick={openNested}>Open Details</Button>
</SideSheet>

<SideSheet width="wide" title="Details">  // Should be narrow
  Nested content
</SideSheet>

// Right: Correct nesting hierarchy
<SideSheet width="wide" title="Main Panel">  // Correct: wide first
  <Button onClick={openNested}>Open Details</Button>
</SideSheet>

<SideSheet width="narrow" title="Details">  // Correct: narrow nested
  Nested content
</SideSheet>
```

### 7.2 Don't Ignore Action Zone Conventions

```tsx
// Wrong: Primary action on left
<SideSheet
  leftActionsZone={<Button variant="primary">Save</Button>}  // Wrong side
  rightActionsZone={<Button variant="secondary">Cancel</Button>}
>
  Content
</SideSheet>

// Right: Primary action on right
<SideSheet
  leftActionsZone={<Button variant="secondary">Cancel</Button>}
  rightActionsZone={<Button variant="primary">Save</Button>}  // Correct side
>
  Content
</SideSheet>
```

## Summary

The Neuron SideSheet component provides a comprehensive, accessible, and consistent system for side-positioned overlay dialogs. Key points to remember:

1. **Follow nesting hierarchy** - Wide first, narrow nested, no exceptions
2. **Choose appropriate widths** - Narrow for simple content, wide for complex
3. **Use proper action placement** - Secondary left, primary right
4. **Configure dismissal consistently** - Match closable, backdrop, and escape behaviors
5. **Provide meaningful headers** - Clear titles, descriptions, and contextual icons
6. **Manage state properly** - Control show/hide with proper state management
7. **Follow accessibility guidelines** - Meaningful content and keyboard navigation

### Key Takeaways

- **Two width variants** with specific use cases and mandatory nesting hierarchy
- **Dual action zones** with conventional button placement patterns
- **Flexible dismissal options** with consistent configuration
- **Rich header support** with icons, titles, and descriptions
- **Nesting capability** with strict width ordering requirements
- **Built-in accessibility** features for inclusive design
- **Consistent behavior** across all application contexts

By following these guidelines, you'll create effective, user-friendly side panel interfaces that enhance workflow efficiency and provide clear, accessible information across your Neuron applications.

## Step 4:

Fixed Footer and Scrollable Content

### 4.1 Understanding fixedFooter Behavior

The `fixedFooter` prop controls how action buttons behave with content of different sizes:

**🔧 fixedFooter={true} (Sticky Footer):**

- Action buttons are **always visible at the bottom** of the SideSheet
- Buttons remain **fixed/sticky** regardless of content size
- Content area becomes **scrollable** when it exceeds available space
- **Use for**: Long forms, scrollable lists, content that may vary in length

**🔧 fixedFooter={false} (Content-Based Footer):**

- Action buttons are positioned **directly under the content**
- Buttons **move with content** - if content is short, buttons are higher up
- When content is larger than visible area, **buttons remain visible** (not scrolled away)
- Content shows **scrollbar** but buttons stay accessible
- **Use for**: Short content, predictable content size, better visual flow

```tsx
import { SideSheet, Button } from "@neuron/ui";
import { useState } from "react";

const FixedFooterComparison = () => {
  const [showFixed, setShowFixed] = useState(false);
  const [showContentBased, setShowContentBased] = useState(false);

  // Short content example
  const shortContent = (
    <div>
      <p>This is short content that doesn't require scrolling.</p>
      <p>Notice how the button placement differs between fixed and content-based footer.</p>
    </div>
  );

  // Long content that requires scrolling
  const longContent = Array.from({ length: 50 }, (_, i) => (
    <p key={i}>
      This is paragraph {i + 1} of long content. Scroll down to see how the footer behavior differs between fixed and
      content-based modes.
    </p>
  ));

  return (
    <div className="footer-comparison">
      {/* Fixed Footer - Buttons always at bottom */}
      <Button onClick={() => setShowFixed(true)}>Fixed Footer (Sticky at bottom)</Button>

      <SideSheet
        show={showFixed}
        onHide={() => setShowFixed(false)}
        title="Fixed Footer Example"
        description="Buttons are always sticky at the bottom"
        width="narrow"
        fixedFooter={true} // Buttons stick to bottom
        leftActionsZone={
          <Button variant="secondary" onClick={() => setShowFixed(false)}>
            Cancel
          </Button>
        }
        rightActionsZone={
          <Button variant="primary" onClick={() => console.log("Saving...")}>
            Save
          </Button>
        }
      >
        <div className="content">
          {longContent}
          <p>
            <strong>Notice:</strong> Buttons remain fixed at the bottom while you scroll through this content.
          </p>
        </div>
      </SideSheet>

      {/* Content-Based Footer - Buttons follow content */}
      <Button onClick={() => setShowContentBased(true)}>Content-Based Footer (Follows content)</Button>

      <SideSheet
        show={showContentBased}
        onHide={() => setShowContentBased(false)}
        title="Content-Based Footer Example"
        description="Buttons positioned under content"
        width="narrow"
        fixedFooter={false} // Buttons follow content
        leftActionsZone={
          <Button variant="secondary" onClick={() => setShowContentBased(false)}>
            Cancel
          </Button>
        }
        rightActionsZone={
          <Button variant="primary" onClick={() => console.log("Saving...")}>
            Save
          </Button>
        }
      >
        <div className="content">
          {longContent}
          <p>
            <strong>Notice:</strong> Buttons are positioned right after this content, but remain visible even when
            scrolling.
          </p>
        </div>
      </SideSheet>
    </div>
  );
};
```

### 4.2 When to Use Each Footer Type

**Use fixedFooter={true} when:**

- Content length is **unpredictable** or **highly variable**
- Working with **long forms** that require scrolling
- Users need **constant access** to action buttons
- Content includes **dynamic lists** or **expandable sections**

```tsx
// Good: Long form with fixed footer
<SideSheet
  title="User Registration Form"
  description="Complete all required fields"
  width="wide"
  fixedFooter={true} // Always accessible Save/Cancel
  rightActionsZone={<Button variant="primary">Register User</Button>}
>
  <form className="long-registration-form">{/* Many form fields that require scrolling */}</form>
</SideSheet>
```

**Use fixedFooter={false} when:**

- Content is **short and predictable**
- You want **better visual flow** between content and actions
- Content size is **relatively consistent**
- **Aesthetic preference** for buttons following content naturally

```tsx
// Good: Short content with content-based footer
<SideSheet
  title="Confirm Deletion"
  description="This action cannot be undone"
  width="narrow"
  fixedFooter={false} // Buttons naturally follow content
  leftActionsZone={<Button variant="secondary">Cancel</Button>}
  rightActionsZone={<Button variant="danger">Delete</Button>}
>
  <div className="confirmation-content">
    <p>Are you sure you want to delete this item?</p>
    <p>This action is permanent and cannot be reversed.</p>
  </div>
</SideSheet>
```

### 4.3 Scrollable Content Behavior

Both footer types handle scrollable content properly, but with different visual approaches:

```tsx
import { SideSheet, Button } from "@neuron/ui";
import { useState } from "react";

const ScrollableBehavior = () => {
  const [showExample, setShowExample] = useState(false);
  const [useFixedFooter, setUseFixedFooter] = useState(true);

  const scrollableContent = (
    <div className="scrollable-demo">
      <h4>Scrollable Content Demo</h4>
      {Array.from({ length: 40 }, (_, i) => (
        <div key={i} className="content-item">
          <h5>Section {i + 1}</h5>
          <p>
            This is content section {i + 1}. Notice how the scrolling behavior works with different footer
            configurations.
          </p>
        </div>
      ))}
      <div className="content-end">
        <p>
          <strong>End of content.</strong> The footer behavior depends on the fixedFooter setting.
        </p>
      </div>
    </div>
  );

  return (
    <div>
      <div className="controls">
        <label>
          <input type="checkbox" checked={useFixedFooter} onChange={(e) => setUseFixedFooter(e.target.checked)} />
          Use Fixed Footer
        </label>
        <Button onClick={() => setShowExample(true)}>Open Scrollable Content Example</Button>
      </div>

      <SideSheet
        show={showExample}
        onHide={() => setShowExample(false)}
        title={`Scrollable Content - ${useFixedFooter ? "Fixed" : "Content-Based"} Footer`}
        description="Scroll through the content to see footer behavior"
        width="wide"
        fixedFooter={useFixedFooter}
        leftActionsZone={
          <Button variant="secondary" onClick={() => setShowExample(false)}>
            Cancel
          </Button>
        }
        rightActionsZone={
          <Button variant="primary" onClick={() => console.log("Action performed")}>
            Perform Action
          </Button>
        }
      >
        {scrollableContent}
      </SideSheet>
    </div>
  );
};
```

## Step 5: Interaction and Dismissal Options

### 5.1 Close Button and Escape Key

Configure how users can dismiss the SideSheet:

```tsx
import { SideSheet, Button } from "@neuron/ui";
import { useState } from "react";

const DismissalOptions = () => {
  const [showClosable, setShowClosable] = useState(false);
  const [showNonClosable, setShowNonClosable] = useState(false);
  const [showEscapeEnabled, setShowEscapeEnabled] = useState(false);

  return (
    <div className="dismissal-options">
      {/* SideSheet with close button */}
      <Button onClick={() => setShowClosable(true)}>Closable SideSheet (X button)</Button>

      <SideSheet
        show={showClosable}
        onHide={() => setShowClosable(false)}
        title="Closable SideSheet"
        description="Has close button in header"
        width="narrow"
        closable={true} // Shows X button in header
      >
        <p>This SideSheet can be closed using the X button in the header.</p>
      </SideSheet>

      {/* SideSheet without close button */}
      <Button onClick={() => setShowNonClosable(true)}>Non-Closable SideSheet (Actions only)</Button>

      <SideSheet
        show={showNonClosable}
        onHide={() => setShowNonClosable(false)}
        title="Non-Closable SideSheet"
        description="Must use action buttons to close"
        width="narrow"
        closable={false} // No X button - must use actions
        rightActionsZone={
          <Button variant="primary" onClick={() => setShowNonClosable(false)}>
            Done
          </Button>
        }
      >
        <p>This SideSheet can only be closed using the action buttons.</p>
      </SideSheet>

      {/* SideSheet with escape key enabled */}
      <Button onClick={() => setShowEscapeEnabled(true)}>Escape Key Enabled</Button>

      <SideSheet
        show={showEscapeEnabled}
        onHide={() => setShowEscapeEnabled(false)}
        title="Escape Key SideSheet"
        description="Press Escape to close"
        width="narrow"
        closeOnEscape={true} // Allows Escape key to close
        closable={true}
      >
        <p>Press the Escape key or click the X button to close this SideSheet.</p>
      </SideSheet>
    </div>
  );
};
```

### 5.2 Backdrop Click Behavior

Configure backdrop interaction for dismissal:

```tsx
import { SideSheet, Button } from "@neuron/ui";
import { useState } from "react";

const BackdropBehavior = () => {
  const [showBackdropClose, setShowBackdropClose] = useState(false);
  const [showBackdropDisabled, setShowBackdropDisabled] = useState(false);
  const [showCustomBackdrop, setShowCustomBackdrop] = useState(false);

  const handleCustomBackdropClick = (event) => {
    console.log("Custom backdrop click handler", event);
    // Custom logic here - could show confirmation dialog
    setShowCustomBackdrop(false);
  };

  return (
    <div className="backdrop-behavior">
      {/* Backdrop click closes SideSheet */}
      <Button onClick={() => setShowBackdropClose(true)}>Backdrop Click Closes</Button>

      <SideSheet
        show={showBackdropClose}
        onHide={() => setShowBackdropClose(false)}
        title="Backdrop Closable"
        description="Click outside to close"
        width="narrow"
        closeOnBackdropClick={true}
      >
        <p>Click outside this SideSheet to close it.</p>
      </SideSheet>

      {/* Backdrop click disabled */}
      <Button onClick={() => setShowBackdropDisabled(true)}>Backdrop Click Disabled</Button>

      <SideSheet
        show={showBackdropDisabled}
        onHide={() => setShowBackdropDisabled(false)}
        title="Backdrop Disabled"
        description="Must use buttons to close"
        width="narrow"
        closeOnBackdropClick={false}
        rightActionsZone={
          <Button variant="primary" onClick={() => setShowBackdropDisabled(false)}>
            Close
          </Button>
        }
      >
        <p>Clicking outside won't close this SideSheet. Use the Close button.</p>
      </SideSheet>

      {/* Custom backdrop handler */}
      <Button onClick={() => setShowCustomBackdrop(true)}>Custom Backdrop Handler</Button>

      <SideSheet
        show={showCustomBackdrop}
        onHide={() => setShowCustomBackdrop(false)}
        title="Custom Backdrop"
        description="Custom backdrop click behavior"
        width="narrow"
        closeOnBackdropClick={false}
        onBackdropClick={handleCustomBackdropClick}
      >
        <p>This SideSheet has custom backdrop click handling.</p>
      </SideSheet>
    </div>
  );
};
```

## Step 6: Integration with Forms and Workflows

### 6.1 Form Integration

SideSheet is ideal for form-based workflows:

```tsx
import { SideSheet, Button, Input, baseIcons } from "@neuron/ui";
import { useState } from "react";
import { useForm } from "react-hook-form";

const FormIntegration = () => {
  const [showForm, setShowForm] = useState(false);
  const {
    control,
    handleSubmit,
    reset,
    formState: { errors },
  } = useForm();

  const onSubmit = async (data) => {
    console.log("Form submitted:", data);
    // Simulate API call
    await new Promise((resolve) => setTimeout(resolve, 1000));
    setShowForm(false);
    reset();
  };

  const handleCancel = () => {
    reset();
    setShowForm(false);
  };

  return (
    <div>
      <Button onClick={() => setShowForm(true)}>Create New User</Button>

      <SideSheet
        show={showForm}
        onHide={handleCancel}
        title="Create New User"
        description="Enter user information to create a new account"
        width="narrow"
        closable={false} // Prevent accidental closure during form entry
        fixedFooter={true} // Keep buttons accessible during form scrolling
        leftActionsZone={
          <Button variant="secondary" onClick={handleCancel}>
            Cancel
          </Button>
        }
        rightActionsZone={
          <Button variant="primary" onClick={handleSubmit(onSubmit)} iconLeft={baseIcons.userPlusSolid}>
            Create User
          </Button>
        }
      >
        <form className="user-form">
          <Input control={control} name="firstName" label="First Name" rules={{ required: "First name is required" }} />

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

          <Input control={control} name="role" label="User Role" rules={{ required: "Role is required" }} />
        </form>
      </SideSheet>
    </div>
  );
};
```

## Step 7: Updated SideSheet Props Reference

### 7.1 Core SideSheet Props

| Prop        | Type                 | Default    | Description                                     |
| ----------- | -------------------- | ---------- | ----------------------------------------------- |
| title       | `string`             | -          | The title of the SideSheet                      |
| description | `string`             | -          | Description displayed under the title           |
| children    | `ReactNode`          | -          | **Required**. Content of the SideSheet          |
| show        | `boolean`            | -          | **Required**. Controls SideSheet visibility     |
| onHide      | `() => void`         | -          | **Required**. Callback when SideSheet is hidden |
| width       | `"narrow" \| "wide"` | `"narrow"` | Width variant of the SideSheet                  |
| className   | `string`             | -          | Additional CSS classes                          |
| testId      | `string`             | -          | Custom test ID for the component                |

### 7.2 Header and Icon Props

| Prop | Type        | Default | Description                            |
| ---- | ----------- | ------- | -------------------------------------- |
| icon | `IconProps` | -       | Optional icon for the SideSheet header |

#### IconProps Properties

| Property | Type                 | Description                                  |
| -------- | -------------------- | -------------------------------------------- |
| iconDef  | `IconDefinition`     | **Required**. Icon definition from baseIcons |
| size     | `IconSize \| string` | Icon size (defaults to SideSheet icon size)  |

### 7.3 Action Zone Props

| Prop             | Type        | Default     | Description                                                             |
| ---------------- | ----------- | ----------- | ----------------------------------------------------------------------- |
| leftActionsZone  | `ReactNode` | `undefined` | **Optional**. Action zone for buttons on the left side                  |
| rightActionsZone | `ReactNode` | `undefined` | **Optional**. Action zone for buttons on the right side                 |
| fixedFooter      | `boolean`   | `false`     | **CRITICAL**: Controls footer behavior - see detailed explanation above |

**Important**: By default, SideSheet has **no action buttons**. The footer only appears when you provide content to `leftActionsZone` and/or `rightActionsZone`. If neither prop is provided, no footer is rendered.

#### fixedFooter Behavior Details

| Value   | Button Position                     | Scrolling Behavior                                     | Use Case                                                  |
| ------- | ----------------------------------- | ------------------------------------------------------ | --------------------------------------------------------- |
| `true`  | **Always at bottom** (sticky)       | Content scrolls, buttons stay fixed                    | Long forms, variable content, need constant button access |
| `false` | **Under content** (follows content) | Content scrolls, buttons remain visible but not sticky | Short content, predictable size, better visual flow       |

### 7.4 Interaction and Dismissal Props

| Prop                 | Type                                             | Default    | Description                                       |
| -------------------- | ------------------------------------------------ | ---------- | ------------------------------------------------- |
| closable             | `boolean`                                        | `true`     | Whether to show close button in header            |
| closeOnEscape        | `boolean`                                        | `false`    | Whether Escape key closes the SideSheet           |
| closeOnBackdropClick | `boolean`                                        | `closable` | Whether backdrop click closes SideSheet           |
| onBackdropClick      | `(event: React.MouseEvent<HTMLElement>) => void` | -          | Custom backdrop click handler                     |
| onShow               | `() => void`                                     | -          | Callback when SideSheet is shown                  |
| position             | `string`                                         | `"right"`  | Position of the SideSheet (inherited from Dialog) |

### 7.5 Width Variants and Use Cases

| Width    | Use Case                                     | Content Type                   | Nesting Level         |
| -------- | -------------------------------------------- | ------------------------------ | --------------------- |
| `narrow` | Simple forms, basic info, quick actions      | Single column, minimal content | Second level (nested) |
| `wide`   | Complex forms, detailed content, data tables | Multi-column, rich content     | First level (main)    |

### 7.6 Nesting Hierarchy Rules

| Level        | Width    | Required        | Description                     |
| ------------ | -------- | --------------- | ------------------------------- |
| 1st (Main)   | `wide`   | **MANDATORY**   | Primary SideSheet must be wide  |
| 2nd (Nested) | `narrow` | **MANDATORY**   | Nested SideSheet must be narrow |
| 3rd+         | -        | **NOT ALLOWED** | Maximum nesting depth is 2      |

## Step 8: Best Practices

### 8.1 fixedFooter Selection Guidelines

**Use fixedFooter={true} when:**

- Content length is unpredictable or highly variable
- Working with long forms that require scrolling
- Users need constant access to action buttons
- Content includes dynamic lists or expandable sections

**Use fixedFooter={false} when:**

- Content is short and predictable
- You want better visual flow between content and actions
- Content size is relatively consistent
- Aesthetic preference for buttons following content naturally

### 8.2 Nesting Best Practices

**Always Follow Nesting Hierarchy:**

```tsx
// Good: Correct nesting hierarchy
<SideSheet width="wide" title="Main Panel">
  <Button onClick={() => setShowNested(true)}>Open Details</Button>
</SideSheet>

<SideSheet width="narrow" title="Details Panel">
  Nested content
</SideSheet>

// Wrong: Incorrect hierarchy
<SideSheet width="narrow" title="Main Panel">  // Wrong: should be wide
  <Button onClick={() => setShowNested(true)}>Open Details</Button>
</SideSheet>

<SideSheet width="wide" title="Details Panel">  // Wrong: should be narrow
  Nested content
</SideSheet>
```

### 8.3 Width Selection Guidelines

**Use Narrow Width For:**

- Simple forms with few fields
- Quick actions and confirmations
- Basic information display
- Nested/secondary panels

**Use Wide Width For:**

- Complex forms with multiple sections
- Data tables and lists
- Rich content with images/media
- Primary workflow panels

### 8.4 Action Zone Best Practices

**Follow Button Placement Conventions:**

- **Left Zone**: Secondary actions (Cancel, Back, Reset)
- **Right Zone**: Primary actions (Save, Continue, Submit)

```tsx
// Good: Proper action placement
<SideSheet
  leftActionsZone={
    <Button variant="secondary">Cancel</Button>
  }
  rightActionsZone={
    <Button variant="primary">Save Changes</Button>
  }
>
  Content
</SideSheet>

// Wrong: Primary action on left
<SideSheet
  leftActionsZone={
    <Button variant="primary">Save</Button>  // Wrong placement
  }
  rightActionsZone={
    <Button variant="secondary">Cancel</Button>
  }
>
  Content
</SideSheet>
```

## Step 9: Common Mistakes to Avoid

### 9.1 Don't Violate Nesting Hierarchy

```tsx
// Wrong: Incorrect nesting order
<SideSheet width="narrow" title="Main Panel">  // Should be wide
  <Button onClick={openNested}>Open Details</Button>
</SideSheet>

<SideSheet width="wide" title="Details">  // Should be narrow
  Nested content
</SideSheet>

// Right: Correct nesting hierarchy
<SideSheet width="wide" title="Main Panel">  // Correct: wide first
  <Button onClick={openNested}>Open Details</Button>
</SideSheet>

<SideSheet width="narrow" title="Details">  // Correct: narrow nested
  Nested content
</SideSheet>
```

### 9.2 Don't Misuse fixedFooter

```tsx
// Wrong: Fixed footer for short, predictable content
<SideSheet
  title="Simple Confirmation"
  fixedFooter={true}  // Unnecessary for short content
>
  <p>Are you sure?</p>
</SideSheet>

// Right: Content-based footer for short content
<SideSheet
  title="Simple Confirmation"
  fixedFooter={false}  // Better visual flow
>
  <p>Are you sure?</p>
</SideSheet>

// Wrong: Content-based footer for long forms
<SideSheet
  title="Long Registration Form"
  fixedFooter={false}  // Buttons may be hard to reach
>
  <form>{/* Many fields requiring scrolling */}</form>
</SideSheet>

// Right: Fixed footer for long forms
<SideSheet
  title="Long Registration Form"
  fixedFooter={true}  // Always accessible buttons
>
  <form>{/* Many fields requiring scrolling */}</form>
</SideSheet>
```

### 9.3 Don't Ignore Action Zone Conventions

```tsx
// Wrong: Primary action on left
<SideSheet
  leftActionsZone={<Button variant="primary">Save</Button>}  // Wrong side
  rightActionsZone={<Button variant="secondary">Cancel</Button>}
>
  Content
</SideSheet>

// Right: Primary action on right
<SideSheet
  leftActionsZone={<Button variant="secondary">Cancel</Button>}
  rightActionsZone={<Button variant="primary">Save</Button>}  // Correct side
>
  Content
</SideSheet>
```

### 9.4 Don't Forget State Management

```tsx
// Wrong: No proper state control
<SideSheet show={true} title="Always Visible">
  {" "}
  // Always showing Content
</SideSheet>;

// Right: Proper state management
const [showSideSheet, setShowSideSheet] = useState(false);

<SideSheet show={showSideSheet} onHide={() => setShowSideSheet(false)} title="Controlled SideSheet">
  Content
</SideSheet>;
```

## Summary

The Neuron SideSheet component provides a comprehensive, accessible, and consistent system for side-positioned overlay dialogs. Key points to remember:

1. **Follow nesting hierarchy** - Wide first, narrow nested, no exceptions
2. **Choose appropriate widths** - Narrow for simple content, wide for complex
3. **Use proper action placement** - Secondary left, primary right
4. **Configure fixedFooter correctly** - True for long/variable content, false for short/predictable content
5. **Configure dismissal consistently** - Match closable, backdrop, and escape behaviors
6. **Provide meaningful headers** - Clear titles, descriptions, and contextual icons
7. **Manage state properly** - Control show/hide with proper state management
8. **Follow accessibility guidelines** - Meaningful content and keyboard navigation

### Key Takeaways

- **Two width variants** with specific use cases and mandatory nesting hierarchy
- **Dual action zones** with conventional button placement patterns
- **Critical fixedFooter behavior** - sticky vs content-based positioning
- **Flexible dismissal options** with consistent configuration
- **Rich header support** with icons, titles, and descriptions
- **Nesting capability** with strict width ordering requirements
- **Built-in accessibility** features for inclusive design
- **Consistent behavior** across all application contexts

By following these guidelines, you'll create effective, user-friendly side panel interfaces that enhance workflow efficiency and provide clear, accessible information across your Neuron applications.

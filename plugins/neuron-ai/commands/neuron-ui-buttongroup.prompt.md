---
agent: agent
---

# Neuron ButtonGroup Component Guidelines

## For the AI Assistant

This document provides comprehensive guidelines for implementing and configuring the Neuron ButtonGroup component. It explains proper usage, variant selection, segment control implementation, and best practices for creating organized button collections.

## Sync Metadata

- **Component Version:** v3.1.2
- **Component Source:** `packages/neuron/ui/src/lib/buttons/buttonGroup/ButtonGroup.tsx`
- **Guideline Command:** `/neuron-ui-buttongroup`
- **Related Skill:** `neuron-ui-buttons`

## Introduction

The ButtonGroup component provides a structured way to group related buttons together, creating visual cohesion and logical organization for user actions. It serves as a container that manages button relationships and provides specialized functionality like segment controls.

### What is the ButtonGroup Component?

The ButtonGroup component creates organized collections of buttons with consistent spacing, alignment, and interaction patterns. It provides standardized button grouping with support for:

- **Multiple layout options** - Horizontal and vertical orientations with configurable spacing
- **Segment control mode** - Toggle-style button groups with active state management
- **Flexible content** - Support for both children and buttons array patterns
- **Gap control** - Configurable spacing between buttons for visual hierarchy
- **Label integration** - Built-in label and tooltip support for accessibility
- **State management** - Automatic active button tracking for segment controls
- **Accessibility features** - Proper focus management and keyboard navigation

### Key Features

- **Two Implementation Patterns**: Children-based and buttons array-based approaches
- **Segment Control Mode**: Toggle-style button groups with automatic state management
- **Layout Variants**: Horizontal (default) and vertical orientations
- **Gap Control**: Configurable spacing between buttons (enabled by default)
- **Label Support**: Integrated labeling with tooltip functionality
- **Active State Management**: Automatic tracking of selected button in segment mode
- **Accessibility Compliance**: Full keyboard navigation and screen reader support
- **TypeScript Support**: Complete type safety with comprehensive prop definitions

**Storybook Reference:** You can refer to the Storybook documentation for visual examples and interactive demos of all available ButtonGroup configurations.

## Step 1: Basic ButtonGroup Implementation

### 1.1 Import the ButtonGroup Component

```tsx
import { ButtonGroup, Button } from "@neuron/ui";
```

### 1.2 Basic ButtonGroup Usage (Children Pattern)

The most common implementation uses the children pattern:

```tsx
import { ButtonGroup, Button } from "@neuron/ui";

const BasicButtonGroup = () => {
  return (
    <ButtonGroup>
      <Button variant="secondary">Button 1</Button>
      <Button variant="secondary">Button 2</Button>
      <Button variant="secondary">Button 3</Button>
    </ButtonGroup>
  );
};
```

### 1.3 ButtonGroup with Gap Control

Control spacing between buttons using the `gap` prop:

```tsx
import { ButtonGroup, Button } from "@neuron/ui";

const ButtonGroupWithGap = () => {
  return (
    <div className="button-group-examples">
      {/* With gap (default) */}
      <ButtonGroup gap={true}>
        <Button variant="secondary">Save</Button>
        <Button variant="secondary">Cancel</Button>
        <Button variant="primary">Submit</Button>
      </ButtonGroup>

      {/* Without gap - connected buttons */}
      <ButtonGroup gap={false}>
        <Button variant="secondary">Previous</Button>
        <Button variant="secondary">Next</Button>
      </ButtonGroup>
    </div>
  );
};
```

## Step 2: ButtonGroup Variants and Layouts

### 2.1 Vertical ButtonGroup

Create vertical button layouts using the `vertical` prop:

```tsx
import { ButtonGroup, Button } from "@neuron/ui";

const VerticalButtonGroup = () => {
  return (
    <div className="vertical-examples">
      {/* Vertical with gap */}
      <ButtonGroup vertical={true} gap={true}>
        <Button variant="secondary">Option 1</Button>
        <Button variant="secondary">Option 2</Button>
        <Button variant="secondary">Option 3</Button>
      </ButtonGroup>

      {/* Vertical without gap */}
      <ButtonGroup vertical={true} gap={false}>
        <Button variant="secondary">Top</Button>
        <Button variant="secondary">Middle</Button>
        <Button variant="secondary">Bottom</Button>
      </ButtonGroup>
    </div>
  );
};
```

### 2.2 ButtonGroup with Labels and Tooltips

Add descriptive labels and tooltips for better accessibility:

```tsx
import { ButtonGroup, Button } from "@neuron/ui";

const LabeledButtonGroup = () => {
  return (
    <div className="labeled-examples">
      {/* With label */}
      <ButtonGroup labelText="Action Options" tooltip="Choose an action to perform">
        <Button variant="secondary">Edit</Button>
        <Button variant="secondary">Delete</Button>
        <Button variant="primary">Save</Button>
      </ButtonGroup>

      {/* With complex tooltip */}
      <ButtonGroup
        labelText="File Operations"
        tooltip={{
          text: "Available file operations for the selected document",
          placement: "top",
        }}
      >
        <Button variant="secondary">Download</Button>
        <Button variant="secondary">Share</Button>
        <Button variant="secondary">Archive</Button>
      </ButtonGroup>
    </div>
  );
};
```

## Step 3: Segment Control Implementation

### 3.1 Basic Segment Control

Segment controls provide toggle-style button groups with active state management:

```tsx
import { ButtonGroup, ButtonProps } from "@neuron/ui";

const SegmentControlExample = () => {
  const segmentButtons: ButtonProps[] = [
    {
      children: "View 1",
      onClick: () => console.log("View 1 selected"),
    },
    {
      children: "View 2",
      onClick: () => console.log("View 2 selected"),
    },
    {
      children: "View 3",
      onClick: () => console.log("View 3 selected"),
    },
  ];

  return <ButtonGroup segmentControl={true} buttons={segmentButtons} activeButtonIndex={0} gap={false} />;
};
```

### 3.2 Advanced Segment Control with State Management

Implement controlled segment controls with external state:

```tsx
import { ButtonGroup, ButtonProps } from "@neuron/ui";
import { useState } from "react";

const ControlledSegmentControl = () => {
  const [activeIndex, setActiveIndex] = useState(1);

  const viewButtons: ButtonProps[] = [
    {
      children: "List View",
      onClick: () => setActiveIndex(0),
    },
    {
      children: "Grid View",
      onClick: () => setActiveIndex(1),
    },
    {
      children: "Card View",
      onClick: () => setActiveIndex(2),
    },
  ];

  return (
    <div className="controlled-segment">
      <ButtonGroup
        labelText="Display Mode"
        segmentControl={true}
        buttons={viewButtons}
        activeButtonIndex={activeIndex}
        gap={false}
      />

      <div className="content-area">
        {activeIndex === 0 && <div>List View Content</div>}
        {activeIndex === 1 && <div>Grid View Content</div>}
        {activeIndex === 2 && <div>Card View Content</div>}
      </div>
    </div>
  );
};
```

### 3.3 Segment Control with Complex Content

Segment controls can contain complex button content including icons and badges:

```tsx
import { ButtonGroup, ButtonProps, NotificationBadge, baseIcons, IconSize } from "@neuron/ui";

const ComplexSegmentControl = () => {
  const tabButtons: ButtonProps[] = [
    {
      children: (
        <>
          <Icon iconDef={baseIcons.messagesSolid} size={IconSize.small} />
          Messages
          <NotificationBadge variant="brand" count={3} />
        </>
      ),
      onClick: () => console.log("Messages tab selected"),
    },
    {
      children: (
        <>
          <Icon iconDef={baseIcons.bellNotificationSolid} size={IconSize.small} />
          Notifications
          <NotificationBadge variant="brand" count={6} />
        </>
      ),
      onClick: () => console.log("Notifications tab selected"),
    },
    {
      children: (
        <>
          <Icon iconDef={baseIcons.gearSolid} size={IconSize.small} />
          Settings
        </>
      ),
      onClick: () => console.log("Settings tab selected"),
    },
  ];

  return (
    <ButtonGroup
      segmentControl={true}
      buttons={tabButtons}
      activeButtonIndex={1}
      gap={false}
      labelText="Navigation Tabs"
    />
  );
};
```

## Step 4: Implementation Patterns

### 4.1 Children vs Buttons Array Pattern

ButtonGroup supports two implementation patterns:

```tsx
import { ButtonGroup, Button, ButtonProps } from "@neuron/ui";

const ImplementationPatterns = () => {
  // Pattern 1: Children-based (recommended for simple cases)
  const childrenPattern = (
    <ButtonGroup>
      <Button variant="secondary">Action 1</Button>
      <Button variant="secondary">Action 2</Button>
      <Button variant="primary">Action 3</Button>
    </ButtonGroup>
  );

  // Pattern 2: Buttons array (recommended for dynamic content and segment controls)
  const buttonsArray: ButtonProps[] = [
    {
      children: "Action 1",
      variant: "secondary",
      onClick: () => console.log("Action 1"),
    },
    {
      children: "Action 2",
      variant: "secondary",
      onClick: () => console.log("Action 2"),
    },
    {
      children: "Action 3",
      variant: "primary",
      onClick: () => console.log("Action 3"),
    },
  ];

  const arrayPattern = <ButtonGroup buttons={buttonsArray} />;

  return (
    <div className="pattern-examples">
      {childrenPattern}
      {arrayPattern}
    </div>
  );
};
```

### 4.2 Dynamic ButtonGroup Generation

Generate button groups dynamically based on data:

```tsx
import { ButtonGroup, ButtonProps } from "@neuron/ui";

interface ActionItem {
  id: string;
  label: string;
  action: () => void;
  isPrimary?: boolean;
}

const DynamicButtonGroup = ({ actions }: { actions: ActionItem[] }) => {
  const buttonProps: ButtonProps[] = actions.map((action) => ({
    children: action.label,
    variant: action.isPrimary ? "primary" : "secondary",
    onClick: action.action,
    key: action.id,
  }));

  return <ButtonGroup buttons={buttonProps} labelText="Available Actions" gap={true} />;
};

// Usage example
const ExampleUsage = () => {
  const userActions: ActionItem[] = [
    {
      id: "edit",
      label: "Edit Profile",
      action: () => console.log("Edit profile"),
    },
    {
      id: "settings",
      label: "Settings",
      action: () => console.log("Open settings"),
    },
    {
      id: "save",
      label: "Save Changes",
      action: () => console.log("Save changes"),
      isPrimary: true,
    },
  ];

  return <DynamicButtonGroup actions={userActions} />;
};
```

## Step 5: ButtonGroup Props Reference

### 5.1 Core ButtonGroup Props

| Prop              | Type                             | Default | Description                                              |
| ----------------- | -------------------------------- | ------- | -------------------------------------------------------- |
| children          | `ReactNode \| (() => ReactNode)` | -       | Button components to group together                      |
| buttons           | `ButtonProps[]`                  | `[]`    | Array of button configurations (alternative to children) |
| gap               | `boolean`                        | `true`  | Adds spacing between buttons                             |
| vertical          | `boolean`                        | `false` | Renders buttons in vertical layout                       |
| segmentControl    | `boolean`                        | `false` | Enables segment control mode with active state           |
| activeButtonIndex | `number`                         | `0`     | Index of the active button in segment control mode       |

### 5.2 Label and Accessibility Props

| Prop      | Type                     | Default | Description                        |
| --------- | ------------------------ | ------- | ---------------------------------- |
| labelText | `string`                 | -       | Label text for the button group    |
| tooltip   | `string \| TooltipProps` | -       | Tooltip for the button group label |
| className | `string`                 | -       | Additional CSS classes             |
| testId    | `string`                 | -       | Custom test ID for the component   |

### 5.3 Inherited PrimeReact Props

ButtonGroup extends PrimeReact's ButtonGroup component and inherits all its props.

## Step 6: Common Use Cases and Patterns

### 6.1 Form Action Groups

Button groups for form actions with proper hierarchy:

```tsx
import { ButtonGroup, Button } from "@neuron/ui";

const FormActionGroup = ({ onSave, onCancel, onReset }) => {
  return (
    <ButtonGroup labelText="Form Actions" className="form-actions">
      <Button variant="tertiary" onClick={onReset}>
        Reset
      </Button>
      <Button variant="secondary" onClick={onCancel}>
        Cancel
      </Button>
      <Button variant="primary" onClick={onSave}>
        Save Changes
      </Button>
    </ButtonGroup>
  );
};
```

### 6.2 Navigation Button Groups

Button groups for navigation with active states:

```tsx
import { ButtonGroup, ButtonProps, baseIcons, IconSize } from "@neuron/ui";
import { useState } from "react";

const NavigationButtonGroup = () => {
  const [currentPage, setCurrentPage] = useState(1);

  const navigationButtons: ButtonProps[] = [
    {
      children: (
        <>
          <Icon iconDef={baseIcons.chevronLeftSolid} size={IconSize.small} />
          Previous
        </>
      ),
      onClick: () => setCurrentPage((prev) => Math.max(1, prev - 1)),
      disabled: currentPage === 1,
    },
    {
      children: `Page ${currentPage}`,
      variant: "primary",
      disabled: true,
    },
    {
      children: (
        <>
          Next
          <Icon iconDef={baseIcons.chevronRightSolid} size={IconSize.small} />
        </>
      ),
      onClick: () => setCurrentPage((prev) => prev + 1),
    },
  ];

  return <ButtonGroup buttons={navigationButtons} gap={false} labelText="Page Navigation" />;
};
```

### 6.3 Filter Button Groups

Segment controls for filtering and view options:

```tsx
import { ButtonGroup, ButtonProps } from "@neuron/ui";
import { useState } from "react";

const FilterButtonGroup = () => {
  const [activeFilter, setActiveFilter] = useState(0);

  const filterButtons: ButtonProps[] = [
    {
      children: "All Items",
      onClick: () => setActiveFilter(0),
    },
    {
      children: "Active",
      onClick: () => setActiveFilter(1),
    },
    {
      children: "Completed",
      onClick: () => setActiveFilter(2),
    },
    {
      children: "Archived",
      onClick: () => setActiveFilter(3),
    },
  ];

  return (
    <ButtonGroup
      segmentControl={true}
      buttons={filterButtons}
      activeButtonIndex={activeFilter}
      gap={false}
      labelText="Filter Options"
    />
  );
};
```

## Step 7: Best Practices

### 7.1 Implementation Pattern Selection

**Choose the right pattern based on your use case:**

```tsx
// ✅ CORRECT: Use children pattern for simple, static button groups
<ButtonGroup>
  <Button variant="secondary">Cancel</Button>
  <Button variant="primary">Save</Button>
</ButtonGroup>

// ✅ CORRECT: Use buttons array for dynamic content and segment controls
<ButtonGroup
  segmentControl={true}
  buttons={dynamicButtons}
  activeButtonIndex={activeIndex}
/>

// ❌ WRONG: Don't mix patterns
<ButtonGroup buttons={someButtons}>
  <Button>Extra Button</Button>
</ButtonGroup>
```

### 7.2 Segment Control Guidelines

**Use segment controls appropriately:**

```tsx
// ✅ CORRECT: Segment control for view modes
<ButtonGroup
  segmentControl={true}
  buttons={viewModeButtons}
  activeButtonIndex={currentView}
  gap={false} // Always false for segment controls
/>

// ❌ WRONG: Segment control for unrelated actions
<ButtonGroup
  segmentControl={true}
  buttons={[
    { children: "Save" },
    { children: "Delete" }, // These are not related toggle options
    { children: "Cancel" }
  ]}
/>
```

### 7.3 Gap Usage Guidelines

**Use appropriate gap settings:**

```tsx
// ✅ CORRECT: Gap for action groups
<ButtonGroup gap={true}>
  <Button variant="secondary">Cancel</Button>
  <Button variant="primary">Save</Button>
</ButtonGroup>

// ✅ CORRECT: No gap for segment controls
<ButtonGroup segmentControl={true} gap={false} buttons={toggleButtons} />

// ✅ CORRECT: No gap for connected navigation
<ButtonGroup gap={false}>
  <Button>Previous</Button>
  <Button>Next</Button>
</ButtonGroup>
```

### 7.4 Accessibility Guidelines

**Ensure proper accessibility:**

```tsx
// ✅ CORRECT: Proper labeling and structure
<ButtonGroup
  labelText="Document Actions"
  tooltip="Available actions for the selected document"
  testId="document-actions"
>
  <Button>Edit</Button>
  <Button>Share</Button>
  <Button>Delete</Button>
</ButtonGroup>

// ✅ CORRECT: Meaningful button content
<ButtonGroup segmentControl={true} buttons={[
  { children: "List View", "aria-label": "Switch to list view" },
  { children: "Grid View", "aria-label": "Switch to grid view" },
]} />
```

## Step 8: Common Mistakes to Avoid

### 8.1 Don't Mix Implementation Patterns

```tsx
{
  /* ❌ WRONG: Mixing children and buttons array */
}
<ButtonGroup buttons={arrayButtons}>
  <Button>Additional Button</Button>
</ButtonGroup>;

{
  /* ✅ CORRECT: Use one pattern consistently */
}
<ButtonGroup buttons={[...arrayButtons, additionalButton]} />;
```

### 8.2 Don't Use Gap with Segment Controls

```tsx
{
  /* ❌ WRONG: Gap with segment control breaks visual cohesion */
}
<ButtonGroup segmentControl={true} gap={true} buttons={toggleButtons} />;

{
  /* ✅ CORRECT: No gap for segment controls */
}
<ButtonGroup segmentControl={true} gap={false} buttons={toggleButtons} />;
```

### 8.3 Don't Forget State Management for Segment Controls

```tsx
{
  /* ❌ WRONG: No state management for segment control */
}
<ButtonGroup segmentControl={true} buttons={buttons} />;

{
  /* ✅ CORRECT: Proper state management */
}
const [activeIndex, setActiveIndex] = useState(0);
<ButtonGroup segmentControl={true} buttons={buttons} activeButtonIndex={activeIndex} />;
```

### 8.4 Don't Use Inappropriate Button Variants

```tsx
{
  /* ❌ WRONG: All primary buttons in a group */
}
<ButtonGroup>
  <Button variant="primary">Action 1</Button>
  <Button variant="primary">Action 2</Button>
  <Button variant="primary">Action 3</Button>
</ButtonGroup>;

{
  /* ✅ CORRECT: Proper hierarchy with one primary action */
}
<ButtonGroup>
  <Button variant="secondary">Cancel</Button>
  <Button variant="secondary">Reset</Button>
  <Button variant="primary">Save</Button>
</ButtonGroup>;
```

### 8.5 Don't Ignore Responsive Considerations

```tsx
{
  /* ❌ WRONG: No consideration for mobile layouts */
}
<ButtonGroup>
  <Button>Very Long Button Text</Button>
  <Button>Another Very Long Button</Button>
  <Button>Yet Another Long Button</Button>
</ButtonGroup>;

{
  /* ✅ CORRECT: Consider vertical layout for mobile */
}
<ButtonGroup vertical={isMobile}>
  <Button>Action 1</Button>
  <Button>Action 2</Button>
  <Button>Action 3</Button>
</ButtonGroup>;
```

## Key Takeaways

The Neuron ButtonGroup component provides a comprehensive foundation for organizing related buttons. Key points to remember:

1. **Choose appropriate patterns** - Use children for simple cases, buttons array for dynamic content
2. **Use segment controls correctly** - Only for related toggle options, always with gap={false}
3. **Maintain proper hierarchy** - Use appropriate button variants to show action importance
4. **Provide accessibility features** - Include labels, tooltips, and proper ARIA attributes
5. **Consider responsive design** - Use vertical layout when appropriate for mobile devices
6. **Manage state properly** - Implement controlled components for segment controls
7. **Use consistent spacing** - Apply gap settings appropriately based on use case
8. **Test thoroughly** - Verify keyboard navigation and screen reader compatibility

By following these guidelines, you'll create consistent, accessible, and user-friendly button groups across your Neuron applications.

## Additional Resources

For more detailed examples and advanced usage patterns, refer to the Neuron UI Documentation (`README-AI.md`).

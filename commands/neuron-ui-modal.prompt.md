---
agent: agent
---

# Neuron Modal Component Guidelines

## For the AI Assistant

This document provides comprehensive guidelines for implementing and configuring the Neuron Modal component. It explains proper usage, size selection, action zone configuration, and best practices for creating accessible overlay dialogs.

## Sync Metadata

- **Component Version:** v4.2.2
- **Component Source:** `packages/neuron/ui/src/lib/overlays/modal/Modal.tsx`
- **Guideline Command:** `/neuron-ui-modal`
- **Related Skill:** `neuron-ui-overlays`

## Introduction

The Modal component provides a flexible and customizable overlay dialog used to display content in a focused and interruptive manner. It serves as a critical interface element for capturing user attention and facilitating important interactions within Neuron applications.

### What is the Modal Component?

The Modal component creates overlay dialogs that interrupt the user's workflow to present important information or capture input. It provides standardized modal rendering with support for:

- **Multiple sizes** - sm, lg, xl, and window sizes for various use cases
- **Action zones** - Dedicated areas for button placement and user interactions
- **Accessibility compliance** - Full WCAG 2.1 AA standards support
- **Responsive design** - Optimized for multi-device workflows
- **Icon integration** - Support for baseIcons with proper sizing
- **Backdrop interaction** - Configurable backdrop click behavior
- **Focus management** - Automatic focus trapping and restoration

### Key Features

- **Four Size Variants**: sm (small), lg (large), xl (extra large), window (full-screen)
- **Dual Action Zones**: leftActionsZone and rightActionsZone for organized button placement
- **baseIcons Integration**: Proper icon support using the Neuron icon system
- **Accessibility Features**: Focus management, keyboard navigation, screen reader support
- **Backdrop Control**: Configurable backdrop click behavior for dismissal
- **Close Button Control**: Optional close button with design-driven visibility
- **TypeScript Support**: Full type safety with comprehensive prop definitions

**Storybook Reference:** You can refer to the Storybook documentation for visual examples and interactive demos of all available modal configurations.

## Key Features

- **Customizable Size**: Supports `sm`, `lg`, `xl`, and `window` sizes for various use cases.
- **Action Zones**: Provides `leftActionsZone` and `rightActionsZone` for button placement.
- **Accessibility**: Fully compliant with WCAG 2.1 AA standards.
- **Responsive Design**: Optimized for multi-device workflows.
- **Icon Support**: Allows optional icons for enhanced visual context.
- **Backdrop Interaction**: Configurable backdrop click behavior.

## Usage

### Basic Example

```tsx
import { Modal } from "@neuron/ui";

const BasicModal = ({ show, onHide }) => (
  <Modal
    show={show}
    onHide={onHide}
    title="Basic Modal"
    description="This is a basic modal example."
    size="lg"
    closable={false}
  >
    <p>Modal content goes here.</p>
  </Modal>
);
```

### Advanced Example with Action Zones and Icon

```tsx
import { Modal, Button, baseIcons, IconSize } from "@neuron/ui";

const AdvancedModal = ({ show, onHide, onConfirm }) => (
  <Modal
    show={show}
    onHide={onHide}
    title="Advanced Modal"
    description="This modal demonstrates advanced usage with action zones and an icon."
    size="lg"
    icon={{ iconDef: baseIcons.circleInfoSolid, size: IconSize.large }}
    leftActionsZone={
      <Button variant="secondary" onClick={onHide}>
        Cancel
      </Button>
    }
    rightActionsZone={
      <Button variant="primary" onClick={onConfirm}>
        Confirm
      </Button>
    }
    closable={false}
    closeOnBackdropClick
  >
    <p>Advanced modal content goes here.</p>
  </Modal>
);
```

### Props Overview

| Prop                   | Type                                             | Description                                                   |
| ---------------------- | ------------------------------------------------ | ------------------------------------------------------------- | ---- | --------- | ------------------------------------- |
| `title`                | `string`                                         | The title of the modal.                                       |
| `description`          | `string`                                         | Description displayed under the title.                        |
| `size`                 | `"sm"                                            | "lg"                                                          | "xl" | "window"` | Controls the dimensions of the modal. |
| `icon`                 | `IconProps` (optional)                           | Optional icon displayed in the modal header.                  |
| `leftActionsZone`      | `ReactNode`                                      | Action zone for buttons on the left side.                     |
| `rightActionsZone`     | `ReactNode`                                      | Action zone for buttons on the right side.                    |
| `show`                 | `boolean`                                        | Controls the visibility of the modal.                         |
| `closable`             | `boolean`                                        | If `true`, the modal will have a close button.                |
| `closeOnBackdropClick` | `boolean`                                        | If `true`, the modal will close when the backdrop is clicked. |
| `onHide`               | `() => void`                                     | Callback invoked when the modal is hidden.                    |
| `onBackdropClick`      | `(event: React.MouseEvent<HTMLElement>) => void` | Callback invoked when the backdrop is clicked.                |

## Best Practices

1. **Size Selection**: Use the appropriate `size` for the modal based on the content and context.
   - `sm`: Small modals for lightweight interactions.
   - `lg`: Default size for most use cases.
   - `xl`: Large modals for detailed content.
   - `window`: Full-screen modals for immersive workflows.
2. **Action Zones**: Place primary actions (e.g., "Save", "Confirm") in the `rightActionsZone` and secondary actions (e.g., "Cancel") in the `leftActionsZone`.
3. **Accessibility**: Always provide meaningful `title` and `description` for screen readers.
4. **Closable Behavior**: Use `closable` and `closeOnBackdropClick` judiciously to avoid accidental dismissals.
   - If the Figma file does not include a close button (cross) at the top-right corner, set `closable` to `false` to match the design intent.
5. **Icons for Context**: Use the `icon` prop to provide visual cues for the modal's purpose.

## Testing Guidelines

1. **Unit Tests**:

   - Verify that the modal renders correctly with all required props.
   - Test visibility toggling using the `show` prop.
   - Ensure `onHide` and `onBackdropClick` callbacks are triggered appropriately.

2. **Integration Tests**:

   - Test interaction with action buttons in `leftActionsZone` and `rightActionsZone`.
   - Verify that the modal closes when clicking the backdrop (if `closeOnBackdropClick` is enabled).

3. **Accessibility Tests**:

   - Ensure the modal is focus-trapped when open.
   - Verify that the `title` and `description` are announced by screen readers.

4. **Visual Regression Tests**:
   - Test modal appearance across different sizes (`sm`, `lg`, `xl`, `window`).
   - Verify responsiveness on various screen resolutions.

## Accessibility Checklist

- **Keyboard Navigation**:

  - Ensure the modal is focus-trapped and focus returns to the triggering element when closed.
  - Verify that all interactive elements are keyboard accessible.

- **Screen Reader Support**:

  - Provide meaningful `title` and `description` props.
  - Ensure the modal is announced as a dialog when opened.

- **Color Contrast**:

  - Verify that text and interactive elements meet WCAG 2.1 AA contrast requirements.

- **Dismissal Options**:
  - Ensure the modal can be dismissed using the close button, backdrop click (if enabled), or the `Esc` key.

## Performance Considerations

1. **Lazy Loading**:

   - Use lazy loading for modal content to improve initial page load performance.

2. **Minimize Re-renders**:

   - Avoid passing inline functions or objects as props to prevent unnecessary re-renders.

3. **Optimize Large Content**:

   - For modals with large or scrollable content, use virtualization techniques to improve performance.

4. **Avoid Overuse**:
   - Use modals sparingly to avoid interrupting the user experience unnecessarily.

## Integration with Other Components

- **Buttons**: Use Neuron's `Button` component for actions within the modal.
- **Icons**: Leverage the `Icon` component for visual context in the modal header.
- **Test IDs**: Use the `testId` prop for automated testing and QA workflows.

## Related Guidelines

- Button Component Guidelines (`/neuron-ui-button`)
- Icon Component Guidelines (`/neuron-ui-icon`)
- Accessibility Standards

## Technical Notes

- The `Modal` component is built on top of the `Prime.Dialog` component, ensuring robust functionality and enterprise-grade performance.
- For advanced customization, use the `pt` prop to pass additional attributes or styles.

## 📝 Understanding Figma vs Implementation

### Figma Design Representation

In Figma, you might see modal designs that include:

- A header with a title, optional description, and optional icon.
- Action buttons (e.g., "Cancel", "Confirm") placed in specific zones.
- Optional close buttons or backdrop interactions.
- Content areas with scrollable or static content.

**Important**: If the Figma design does not include a close button (cross) at the top-right corner, ensure the `closable` prop is set to `false` in the React implementation to align with the design.

### Actual React Implementation

The React `Modal` component works differently:

- **Single Component**: The modal is implemented as a single component with props for configuration.
- **Props-Based**: All header, footer, and content configurations are done via props.
- **Integrated Rendering**: Header, content, and action zones are rendered automatically based on the provided props.

### Translation Guide: Figma → React

| Figma Element        | React Implementation                           |
| -------------------- | ---------------------------------------------- |
| Modal Header Title   | `title` prop                                   |
| Header Description   | `description` prop                             |
| Header Icon          | `icon` prop                                    |
| Action Buttons       | `leftActionsZone` and `rightActionsZone` props |
| Close Button         | `closable` prop                                |
| Backdrop Interaction | `closeOnBackdropClick` prop                    |
| Content Area         | `children` prop                                |

**Important**: Even if Figma shows separate elements, always implement the modal as a single component with props.

## 🎯 Figma Identification Rules

### Modal Component Indicators:

- **Static header** with a title and optional description.
- **Action buttons** placed at the bottom (aligned left or right).
- **Backdrop interaction** (optional) for dismissing the modal.
- **Scrollable content** for large modals or static content for smaller ones.

### NOT Modal (Use Other Components Instead):

- ❌ Expandable/collapsible content → Use **Accordion Panel**.
- ❌ Persistent side panels → Use **Drawer**.
- ❌ Inline dialogs → Use **Popover** or **Tooltip**.

**Selection Rule**: If the design includes a full-screen or centered overlay with a header, content, and actions, it is likely a modal.

## Figma to React Example

### Figma Design

- **Header**: Title "Delete Item", description "This action cannot be undone."
- **Content**: "Are you sure you want to delete this item?"
- **Actions**: "Cancel" (secondary button) and "Delete" (primary button).
- **Close Button**: Included in the header.
- **Backdrop Interaction**: Enabled.

### React Implementation

```tsx
import { Modal, Button, baseIcons, IconSize } from "@neuron/ui";

const DeleteModal = ({ show, onHide, onDelete }) => (
  <Modal
    show={show}
    onHide={onHide}
    title="Delete Item"
    description="This action cannot be undone."
    size="sm"
    icon={{ iconDef: baseIcons.circleExclamationSolid, size: IconSize.base }}
    closable={false}
    closeOnBackdropClick
    rightActionsZone={
      <>
        <Button variant="secondary" onClick={onHide}>
          Cancel
        </Button>
        <Button variant="primary" onClick={onDelete}>
          Delete
        </Button>
      </>
    }
  >
    <p>Are you sure you want to delete this item?</p>
  </Modal>
);
```

**Key Points**:

- Match the Figma design by using the `title`, `description`, and `rightActionsZone` props.
- Use `closable` and `closeOnBackdropClick` for dismissing the modal.
- Place action buttons in the `rightActionsZone` for alignment consistency.

## Best Practices for Figma Integration

1. **Follow Design Intent**: Ensure the modal matches the Figma design in terms of layout, spacing, and alignment.
2. **Use Props for Configuration**: Avoid manually creating header, content, or footer sections; use the provided props.
3. **Test Responsiveness**: Verify that the modal behaves as expected on different screen sizes, especially for `window` size modals.
4. **Accessibility**: Ensure the modal's title and description are meaningful and accessible to screen readers.

## Step 8: Common Mistakes to Avoid

### 8.1 Don't Manually Create Modal Sections

```tsx
{/* Wrong: Manually creating header and footer */}
<div className="modal-header">
  <h3>Title</h3>
</div>
<div className="modal-footer">
  <button>Cancel</button>
  <button>Confirm</button>
</div>

{/* Right: Use props for header and footer */}
<Modal
  title="Title"
  rightActionsZone={
    <>
      <Button variant="secondary">Cancel</Button>
      <Button variant="primary">Confirm</Button>
    </>
  }
>
  Content goes here.
</Modal>
```

### 8.2 Don't Ignore Accessibility Requirements

```tsx
{
  /* Wrong: Missing title and description */
}
<Modal show={showModal}>
  <p>Some content</p>
</Modal>;

{
  /* Right: Provide meaningful title and description */
}
<Modal show={showModal} title="Important Information" description="Please review the following details">
  <p>Accessible content with proper labeling</p>
</Modal>;
```

### 8.3 Don't Mix Action Zone Placement

```tsx
{
  /* Wrong: Primary action on the left */
}
<Modal
  leftActionsZone={<Button variant="primary">Save</Button>}
  rightActionsZone={<Button variant="secondary">Cancel</Button>}
>
  Content
</Modal>;

{
  /* Right: Primary action on the right */
}
<Modal
  leftActionsZone={<Button variant="secondary">Cancel</Button>}
  rightActionsZone={<Button variant="primary">Save</Button>}
>
  Content
</Modal>;
```

### 8.4 Don't Ignore Size Guidelines

```tsx
{
  /* Wrong: Using xl size for simple confirmation */
}
<Modal size="xl" title="Delete Item">
  Are you sure?
</Modal>;

{
  /* Right: Use appropriate size for content */
}
<Modal size="sm" title="Delete Item">
  Are you sure you want to delete this item?
</Modal>;
```

### 8.5 Don't Forget State Management

```tsx
{
  /* Wrong: No proper state control */
}
<Modal show={true} title="Always Visible">
  This modal is always shown
</Modal>;

{
  /* Right: Proper state management */
}
const [showModal, setShowModal] = useState(false);

<Modal show={showModal} onHide={() => setShowModal(false)} title="Controlled Modal">
  Properly controlled modal
</Modal>;
```

## Key Takeaways

The Neuron Modal component system provides a comprehensive, accessible, and consistent foundation for overlay dialogs. Key points to remember:

1. **Use appropriate sizes** based on content complexity and user needs
2. **Follow action zone conventions** - primary actions on the right, secondary on the left
3. **Provide accessibility features** - meaningful titles, descriptions, and focus management
4. **Control visibility properly** - use state management for show/hide behavior
5. **Choose appropriate interaction patterns** - closable and backdrop click behavior
6. **Use icons meaningfully** - enhance context and visual communication
7. **Follow design intent** - match Figma designs with proper prop configuration
8. **Test thoroughly** - verify accessibility, keyboard navigation, and responsive behavior

By following these guidelines, you'll create consistent, accessible, and user-friendly modal interfaces across your Neuron applications.

## Additional Resources

For more detailed examples and advanced usage patterns, refer to the Neuron UI Documentation (`README-AI.md`).

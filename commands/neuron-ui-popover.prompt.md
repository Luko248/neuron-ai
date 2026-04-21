---
agent: agent
---

# AI-Assisted Neuron Popover Component Integration Guide

## For the AI Assistant

Your task is to integrate and configure the Neuron Popover component in a React application. This guide provides essential instructions for implementing Popover, which provides contextual popup overlays for displaying additional content or actions.

## Sync Metadata

- **Component Version:** v4.0.1
- **Component Source:** `packages/neuron/ui/src/lib/popups/popover/Popover.tsx`
- **Guideline Command:** `/neuron-ui-popover`
- **Related Skill:** `neuron-ui-popups`

## Introduction

The Popover component is a flexible popup overlay built on top of PrimeReact's OverlayPanel. It provides a way to display contextual content, actions, or information in a floating panel that appears relative to a trigger element.

Key features include:

- **Flexible Trigger** - Can be triggered by any element or used with external ref control
- **Built-in Button** - Provides default question mark icon button when no ref is used
- **Positioning Control** - Supports centered positioning and automatic placement
- **Dismissal Options** - Configurable escape key and outside click dismissal
- **Close Icon** - Optional close button for explicit dismissal
- **Accessibility** - Built-in ARIA support and keyboard navigation
- **Custom Content** - Supports any React content including forms, actions, and rich media
- **Responsive** - Adapts to screen size and viewport constraints

### Key Features

- **Dual Usage Modes**: Standalone with built-in trigger button or ref-controlled with custom trigger
- **Smart Positioning**: Automatic positioning with optional centering
- **Flexible Content**: Supports any React content including InnerAction components
- **Dismissal Control**: Configurable escape key and outside click behavior
- **Icon Integration**: Uses baseIcons for consistent visual design
- **Test Support**: Built-in test ID support for automated testing
- **Accessibility**: Full keyboard navigation and screen reader support

**Storybook Reference:** You can refer to the Storybook documentation for visual examples and interactive demos of all available Popover configurations.

## Step 1: Basic Popover Implementation

### 1.1 Import the Popover Component

```tsx
import { Popover, PopoverRefType } from "@neuron/ui";
```

### 1.2 Standalone Popover (Built-in Trigger)

Use without a ref for a simple popover with default question mark trigger:

```tsx
import { Popover } from "@neuron/ui";

const StandalonePopover = () => {
  return (
    <Popover showCloseIcon>
      <div>
        <h3>Help Information</h3>
        <p>This popover provides contextual help for the current section.</p>
      </div>
    </Popover>
  );
};
```

**Note**: When used without a ref, Popover automatically renders a question mark icon button as the trigger.

### 1.3 Ref-Controlled Popover (Custom Trigger)

Use with a ref for custom trigger elements:

```tsx
import { Popover, PopoverRefType, Button } from "@neuron/ui";
import { useRef } from "react";

const CustomTriggerPopover = () => {
  const popoverRef = useRef<PopoverRefType>(null);

  return (
    <>
      <Button onClick={(e) => popoverRef.current?.toggle(e)}>Open Options</Button>

      <Popover ref={popoverRef} showCloseIcon>
        <div>
          <p>Custom popover content triggered by the button above.</p>
        </div>
      </Popover>
    </>
  );
};
```

## Step 2: Popover with Actions

### 2.1 Using InnerAction Components

Combine Popover with InnerAction for action menus. InnerAction is a specialized helper component designed specifically for use within popovers and container contexts:

```tsx
import { Popover, PopoverRefType, Button, InnerAction, baseIcons } from "@neuron/ui";
import { useRef } from "react";

const ActionPopover = () => {
  const popoverRef = useRef<PopoverRefType>(null);

  const handlePrint = () => {
    console.info("Print action triggered");
    popoverRef.current?.hide();
  };

  const handleShare = () => {
    console.info("Share action triggered");
    popoverRef.current?.hide();
  };

  return (
    <>
      <Button onClick={(e) => popoverRef.current?.toggle(e)} variant="secondary" iconRight={baseIcons.chevronDownSolid}>
        Print & Share
      </Button>

      <Popover ref={popoverRef} showCloseIcon>
        <InnerAction onClick={handlePrint}>Print Document</InnerAction>
        <InnerAction onClick={handleShare} iconRight={baseIcons.shareRegular}>
          Share Document
        </InnerAction>
        <InnerAction onClick={() => console.info("Export triggered")} iconRight={baseIcons.downloadRegular}>
          Export as PDF
        </InnerAction>
      </Popover>
    </>
  );
};
```

### 2.2 Form Content in Popover

Use Popover for forms or complex interactive content:

```tsx
import { Popover, PopoverRefType, Button, Form } from "@neuron/ui";
import { useRef } from "react";
import { useForm } from "react-hook-form";

const FormPopover = () => {
  const popoverRef = useRef<PopoverRefType>(null);
  const { control, handleSubmit } = useForm();

  const onSubmit = (data: any) => {
    console.info("Form submitted:", data);
    popoverRef.current?.hide();
  };

  return (
    <>
      <Button onClick={(e) => popoverRef.current?.toggle(e)}>Quick Settings</Button>

      <Popover ref={popoverRef} showCloseIcon>
        <form onSubmit={handleSubmit(onSubmit)}>
          <div className="d-grid content-gap">
            <h4>Quick Settings</h4>

            <Form.Input name="username" labelText="Username" control={control} required />

            <Form.Select
              name="theme"
              labelText="Theme"
              control={control}
              options={[
                { value: "light", label: "Light" },
                { value: "dark", label: "Dark" },
              ]}
            />

            <div className="d-flex content-gap justify-content-end">
              <Button type="button" variant="secondary" onClick={() => popoverRef.current?.hide()}>
                Cancel
              </Button>
              <Button type="submit">Save</Button>
            </div>
          </div>
        </form>
      </Popover>
    </>
  );
};
```

## Step 3: Positioning and Behavior

### 3.1 Centered Popover

Use the `centered` prop for center-aligned positioning:

```tsx
import { Popover, PopoverRefType, Button } from "@neuron/ui";
import { useRef } from "react";

const CenteredPopover = () => {
  const popoverRef = useRef<PopoverRefType>(null);

  return (
    <>
      <Button onClick={(e) => popoverRef.current?.toggle(e)}>Show Centered</Button>

      <Popover ref={popoverRef} centered showCloseIcon>
        <div className="text-center">
          <h3>Centered Content</h3>
          <p>This popover is centered relative to its trigger.</p>
        </div>
      </Popover>
    </>
  );
};
```

### 3.2 Dismissal Configuration

Control how the popover can be dismissed:

```tsx
import { Popover, PopoverRefType, Button } from "@neuron/ui";
import { useRef } from "react";

const ConfiguredDismissal = () => {
  const popoverRef = useRef<PopoverRefType>(null);

  return (
    <>
      <Button onClick={(e) => popoverRef.current?.toggle(e)}>Persistent Popover</Button>

      <Popover
        ref={popoverRef}
        closeOnEscape={false} // Disable escape key dismissal
        dismissable={false} // Disable outside click dismissal
        showCloseIcon // Require explicit close button
      >
        <div>
          <h4>Important Notice</h4>
          <p>This popover requires explicit dismissal using the close button.</p>
          <p>Escape key and outside clicks are disabled.</p>
        </div>
      </Popover>
    </>
  );
};
```

### 3.3 Disabled State

Disable the popover trigger when needed:

```tsx
import { Popover } from "@neuron/ui";

const DisabledPopover = ({ isDisabled }: { isDisabled: boolean }) => {
  return (
    <Popover disabled={isDisabled} showCloseIcon>
      <div>
        <p>This popover is conditionally disabled.</p>
      </div>
    </Popover>
  );
};
```

## Step 4: Advanced Usage Patterns

### 4.1 Programmatic Control

Control popover visibility programmatically:

```tsx
import { Popover, PopoverRefType, Button } from "@neuron/ui";
import { useRef, useEffect } from "react";

const ProgrammaticPopover = () => {
  const popoverRef = useRef<PopoverRefType>(null);
  const buttonRef = useRef<HTMLButtonElement>(null);

  const showPopover = () => {
    if (buttonRef.current) {
      // Create synthetic event for positioning
      const syntheticEvent = {
        currentTarget: buttonRef.current,
      } as React.SyntheticEvent<Element, Event>;

      popoverRef.current?.toggle(syntheticEvent);
    }
  };

  const hidePopover = () => {
    popoverRef.current?.hide();
  };

  const checkIsOpen = () => {
    const isOpen = popoverRef.current?.isOpen;
    console.info("Popover is open:", isOpen);
  };

  return (
    <>
      <div className="d-flex content-gap">
        <Button ref={buttonRef} onClick={showPopover}>
          Show Popover
        </Button>
        <Button onClick={hidePopover} variant="secondary">
          Hide Popover
        </Button>
        <Button onClick={checkIsOpen} variant="outline">
          Check Status
        </Button>
      </div>

      <Popover ref={popoverRef} showCloseIcon>
        <div>
          <h4>Programmatically Controlled</h4>
          <p>This popover can be controlled via external buttons.</p>
        </div>
      </Popover>
    </>
  );
};
```

### 4.2 Rich Content Popover

Display rich content with images, lists, and formatted text:

```tsx
import { Popover, PopoverRefType, Button, baseIcons, Icon } from "@neuron/ui";
import { useRef } from "react";

const RichContentPopover = () => {
  const popoverRef = useRef<PopoverRefType>(null);

  return (
    <>
      <Button onClick={(e) => popoverRef.current?.toggle(e)} iconLeft={baseIcons.infoCircleRegular}>
        View Details
      </Button>

      <Popover ref={popoverRef} showCloseIcon>
        <div className="d-grid content-gap">
          <div className="d-flex align-items-center content-gap">
            <Icon iconDef={baseIcons.userRegular} size="24px" />
            <h4 className="mb-0">User Profile</h4>
          </div>

          <div className="d-grid content-gap-sm">
            <div>
              <strong>Name:</strong> John Doe
            </div>
            <div>
              <strong>Email:</strong> john.doe@example.com
            </div>
            <div>
              <strong>Role:</strong> Administrator
            </div>
          </div>

          <div>
            <strong>Permissions:</strong>
            <ul className="mb-0 mt-1">
              <li>Read access to all modules</li>
              <li>Write access to user management</li>
              <li>System configuration access</li>
            </ul>
          </div>

          <div className="d-flex justify-content-end">
            <Button size="sm">Edit Profile</Button>
          </div>
        </div>
      </Popover>
    </>
  );
};
```

## Step 5: Props Reference

### 5.1 PopoverProps Interface

| Prop            | Type        | Default | Description                                       |
| --------------- | ----------- | ------- | ------------------------------------------------- |
| `children`      | `ReactNode` | -       | Content to display inside the popover             |
| `showCloseIcon` | `boolean`   | `false` | Whether to show the close icon button             |
| `closeOnEscape` | `boolean`   | `true`  | Whether pressing Escape closes the popover        |
| `dismissable`   | `boolean`   | `true`  | Whether clicking outside closes the popover       |
| `centered`      | `boolean`   | `false` | Whether to center the popover relative to trigger |
| `disabled`      | `boolean`   | `false` | Whether the popover trigger is disabled           |
| `onShow`        | `function`  | -       | Callback function called when popover is shown    |
| `onHide`        | `function`  | -       | Callback function called when popover is hidden   |
| `testId`        | `string`    | -       | Custom test ID for the component                  |

**Note**: Popover also accepts all PrimeReact OverlayPanel props for advanced customization.

### 5.2 PopoverRefType Interface

| Method   | Type                          | Description                      |
| -------- | ----------------------------- | -------------------------------- |
| `toggle` | `(e: SyntheticEvent) => void` | Toggles popover visibility       |
| `hide`   | `() => void`                  | Hides the popover                |
| `isOpen` | `boolean`                     | Returns current visibility state |

## Step 6: Common Use Cases

### 6.1 Help and Documentation

Use for contextual help and documentation:

```tsx
import { Popover } from "@neuron/ui";

const HelpPopover = () => {
  return (
    <div className="d-flex align-items-center content-gap">
      <label>Complex Setting</label>
      <Popover showCloseIcon>
        <div>
          <h5>Complex Setting Help</h5>
          <p>
            This setting controls the advanced behavior of the system. When enabled, it will automatically optimize
            performance based on current usage patterns.
          </p>
          <p>
            <strong>Note:</strong> Changes take effect after restart.
          </p>
        </div>
      </Popover>
    </div>
  );
};
```

### 6.2 Quick Actions Menu

Use for dropdown-style action menus:

```tsx
import { Popover, PopoverRefType, Button, InnerAction, baseIcons } from "@neuron/ui";
import { useRef } from "react";

const QuickActionsMenu = ({ item }: { item: any }) => {
  const popoverRef = useRef<PopoverRefType>(null);

  return (
    <>
      <Button
        onClick={(e) => popoverRef.current?.toggle(e)}
        variant="ghost"
        iconOnly
        iconLeft={baseIcons.ellipsisVerticalSolid}
        aria-label="More actions"
      />

      <Popover ref={popoverRef}>
        <InnerAction onClick={() => console.info("Edit", item.id)} iconLeft={baseIcons.editRegular}>
          Edit
        </InnerAction>
        <InnerAction onClick={() => console.info("Duplicate", item.id)} iconLeft={baseIcons.copyRegular}>
          Duplicate
        </InnerAction>
        <InnerAction onClick={() => console.info("Delete", item.id)} iconLeft={baseIcons.trashRegular} variant="danger">
          Delete
        </InnerAction>
      </Popover>
    </>
  );
};
```

### 6.3 Filter and Settings Panel

Use for compact settings or filter interfaces:

```tsx
import { Popover, PopoverRefType, Button, Form, baseIcons } from "@neuron/ui";
import { useRef } from "react";
import { useForm } from "react-hook-form";

const FilterPopover = () => {
  const popoverRef = useRef<PopoverRefType>(null);
  const { control, handleSubmit, reset } = useForm();

  const onApplyFilters = (data: any) => {
    console.info("Applying filters:", data);
    popoverRef.current?.hide();
  };

  const onClearFilters = () => {
    reset();
    console.info("Filters cleared");
  };

  return (
    <>
      <Button onClick={(e) => popoverRef.current?.toggle(e)} variant="outline" iconLeft={baseIcons.filterRegular}>
        Filters
      </Button>

      <Popover ref={popoverRef} showCloseIcon>
        <form onSubmit={handleSubmit(onApplyFilters)}>
          <div className="d-grid content-gap">
            <h5>Filter Options</h5>

            <Form.Select
              name="status"
              labelText="Status"
              control={control}
              options={[
                { value: "active", label: "Active" },
                { value: "inactive", label: "Inactive" },
                { value: "pending", label: "Pending" },
              ]}
            />

            <Form.Input name="search" labelText="Search" control={control} placeholder="Enter search term" />

            <Form.DatePicker name="dateFrom" labelText="Date From" control={control} />

            <div className="d-flex content-gap justify-content-between">
              <Button type="button" variant="ghost" onClick={onClearFilters}>
                Clear
              </Button>
              <div className="d-flex content-gap">
                <Button type="button" variant="secondary" onClick={() => popoverRef.current?.hide()}>
                  Cancel
                </Button>
                <Button type="submit">Apply</Button>
              </div>
            </div>
          </div>
        </form>
      </Popover>
    </>
  );
};
```

## Step 7: Best Practices

### 7.1 Content Organization

**Structure content clearly:**

```tsx
{
  /* ✅ CORRECT: Well-structured content */
}
<Popover ref={popoverRef} showCloseIcon>
  <div className="d-grid content-gap">
    <h4>Section Title</h4>

    <div className="d-grid content-gap-sm">
      <p>Primary content goes here.</p>
      <p>Additional details or instructions.</p>
    </div>

    {/* Actions at the bottom */}
    <div className="d-flex justify-content-end content-gap">
      <Button variant="secondary">Cancel</Button>
      <Button>Confirm</Button>
    </div>
  </div>
</Popover>;

{
  /* ❌ WRONG: Unstructured content */
}
<Popover ref={popoverRef}>
  <div>
    Some text
    <Button>Action</Button>
    More text
    <Form.Input name="example" />
    <Button>Another Action</Button>
  </div>
</Popover>;
```

### 7.2 Appropriate Sizing

**Control popover dimensions:**

```tsx
{
  /* ✅ CORRECT: Appropriate sizing */
}
<Popover ref={popoverRef} showCloseIcon>
  <div>
    <h4>User Information</h4>
    <p>Content that needs specific width constraints.</p>
  </div>
</Popover>;

{
  /* ❌ WRONG: No size constraints */
}
<Popover ref={popoverRef}>
  <div>Very long content that might make the popover too wide or too narrow</div>
</Popover>;
```

### 7.3 Proper Event Handling

**Handle events appropriately:**

```tsx
{
  /* ✅ CORRECT: Proper event handling */
}
const handleAction = () => {
  // Perform action
  console.info("Action completed");
  // Close popover after action
  popoverRef.current?.hide();
};

{
  /* ❌ WRONG: Not closing popover after actions */
}
const handleAction = () => {
  console.info("Action completed");
  // Popover stays open, confusing user
};
```

### 7.4 Accessibility Considerations

**Ensure proper accessibility:**

```tsx
{
  /* ✅ CORRECT: Accessible trigger */
}
<Button
  onClick={(e) => popoverRef.current?.toggle(e)}
  aria-label="Open user options menu"
  aria-expanded={popoverRef.current?.isOpen}
>
  Options
</Button>;

{
  /* ❌ WRONG: No accessibility attributes */
}
<Button onClick={(e) => popoverRef.current?.toggle(e)}>⋮</Button>;
```

## Step 8: Common Mistakes to Avoid

### 8.1 Don't Forget the Ref

**Always use ref for custom triggers:**

```tsx
{/* ❌ WRONG: Custom trigger without ref */}
<Button onClick={() => {/* No way to control popover */}}>
  Trigger
</Button>
<Popover>
  Content
</Popover>

{/* ✅ CORRECT: Proper ref usage */}
const popoverRef = useRef<PopoverRefType>(null);

<Button onClick={e => popoverRef.current?.toggle(e)}>
  Trigger
</Button>
<Popover ref={popoverRef}>
  Content
</Popover>
```

### 8.2 Don't Overload with Content

**Keep content focused and manageable:**

```tsx
{
  /* ❌ WRONG: Too much content */
}
<Popover ref={popoverRef}>
  <div>
    <h3>Very Long Title That Takes Up Too Much Space</h3>
    <p>Paragraph 1 with lots of text...</p>
    <p>Paragraph 2 with more text...</p>
    <p>Paragraph 3 with even more text...</p>
    <form>
      <Form.Input name="field1" />
      <Form.Input name="field2" />
      <Form.Input name="field3" />
      <Form.Input name="field4" />
      <Form.TextArea name="description" rows={10} />
    </form>
    <div>More content...</div>
  </div>
</Popover>;

{
  /* ✅ CORRECT: Focused content */
}
<Popover ref={popoverRef} showCloseIcon>
  <div className="d-grid content-gap">
    <h4>Quick Settings</h4>
    <Form.Select name="theme" options={themeOptions} />
    <Button>Apply</Button>
  </div>
</Popover>;
```

### 8.3 Don't Ignore Mobile Experience

**Consider how popovers work on touch devices:**

```tsx
{
  /* ✅ CORRECT: Touch-friendly triggers */
}
<Button onClick={(e) => popoverRef.current?.toggle(e)}>Options</Button>;

{
  /* ❌ WRONG: Too small for touch */
}
<Button onClick={(e) => popoverRef.current?.toggle(e)} size="xs">
  ?
</Button>;
```

### 8.4 Don't Nest Popovers

**Avoid complex popover hierarchies:**

```tsx
{
  /* ❌ WRONG: Nested popovers */
}
<Popover ref={popoverRef1}>
  <div>
    Content
    <Button onClick={(e) => popoverRef2.current?.toggle(e)}>More Options</Button>
    <Popover ref={popoverRef2}>Nested content</Popover>
  </div>
</Popover>;

{
  /* ✅ CORRECT: Single level with navigation */
}
<Popover ref={popoverRef} showCloseIcon>
  <div className="d-grid content-gap">
    <InnerAction onClick={handleShowMore}>Show More Options</InnerAction>
  </div>
</Popover>;
```

## Key Takeaways

The Neuron Popover component provides flexible popup functionality for contextual content and actions. Key points to remember:

**InnerAction Integration**: Use the InnerAction component for action items within popovers - it's specifically designed for this purpose and provides selection states, notification badges, and proper styling for container contexts.

1. **Dual Usage Modes** - Use standalone for simple help popovers or with ref for custom triggers
2. **Content Structure** - Organize content with proper spacing and clear hierarchy
3. **Event Handling** - Always close popover after completing actions
4. **Accessibility** - Provide proper ARIA labels and keyboard navigation
5. **Positioning Control** - Use `centered` prop for center alignment when needed
6. **Dismissal Options** - Configure escape key and outside click behavior appropriately
7. **Size Management** - Control popover dimensions with CSS constraints
8. **Touch Friendly** - Ensure triggers are large enough for mobile interaction
9. **Icon Consistency** - Use baseIcons for all iconography
10. **Test Support** - Use `testId` prop for automated testing

By following these guidelines, you'll create intuitive, accessible popovers that enhance the user experience across all device types in your Neuron applications.

---
agent: agent
---

# AI-Assisted Neuron Flag Component Integration Guide

## For the AI Assistant

Your task is to integrate and configure the Neuron Flag component in React applications. This guide provides comprehensive instructions for implementing Flag components, which serve as status badges positioned absolutely on content containers, with support for disabled/historical states and tooltip integration.

## Sync Metadata

- **Component Version:** v3.2.1
- **Component Source:** `packages/neuron/ui/src/lib/misc/flag/Flag.tsx`
- **Guideline Command:** `/neuron-ui-flag`
- **Related Skill:** `neuron-ui-misc`

## Introduction

The Flag component is a specialized status badge designed to display important status information positioned absolutely on top of content containers. It serves as a visual indicator for various states, including historical flags that may be disabled but still need to be displayed for informational purposes.

### What is the Flag Component?

The Flag component provides status badge functionality with support for:

- **Text or icon content** - Display text labels or baseIcons
- **Semantic variants** - info, success, warning, danger, new styling
- **Absolute positioning** - Positioned on top of relative containers
- **Tooltip integration** - Optional tooltip for additional context
- **Disabled states** - For historical flags that need to remain visible
- **Line-through styling** - Visual indication of removed/cancelled states
- **Accessibility** - Built-in accessibility features and test ID support

### Key Features

- **Status Badge Display**: Compact visual status indicators
- **Absolute Positioning**: Positioned on top of content containers
- **Historical State Support**: Disabled flags for removed but important information
- **Tooltip Integration**: Optional tooltips for additional context
- **Flexible Content**: Support for both text and icon content
- **Semantic Variants**: Color-coded variants for different status types
- **Line-Through Styling**: Visual indication for cancelled/removed states
- **Accessibility Compliant**: Full screen reader support and proper ARIA attributes

**Storybook Reference:** You can refer to the Storybook documentation for visual examples and interactive demos of the Flag component.

## Step 1: Basic Flag Implementation

### 1.1 Import the Flag Component

```tsx
import { Flag } from "@neuron/ui";
```

### 1.2 Basic Flag Usage

Here's a simple implementation of the Flag component:

```tsx
import { Flag, Container } from "@neuron/ui";

const BasicFlagExample = () => {
  return (
    <Container className="position-relative">
      {/* Content container */}
      <div>
        <h3>Content with Status Flag</h3>
        <p>This content has a status flag positioned on top</p>
      </div>

      {/* Basic flag with text */}
      <Flag text="NEW" variant="success" className="position-absolute top-0 end-0" />
    </Container>
  );
};
```

### 1.3 Flag with Icon

Use icons instead of text for more visual status indicators:

```tsx
import { Flag, Container, baseIcons } from "@neuron/ui";

const IconFlagExample = () => {
  return (
    <Container className="position-relative">
      {/* Content container */}
      <div>
        <h3>Content with Icon Flag</h3>
        <p>This content has an icon-based status flag</p>
      </div>

      {/* Flag with icon */}
      <Flag icon={baseIcons.checkSolid} variant="success" className="position-absolute top-0 end-0" />
    </Container>
  );
};
```

### 1.4 Flag with Tooltip

Add tooltips to provide additional context:

```tsx
import { Flag, Container } from "@neuron/ui";

const TooltipFlagExample = () => {
  return (
    <Container className="position-relative">
      {/* Content container */}
      <div>
        <h3>Content with Tooltip Flag</h3>
        <p>Hover over the flag to see additional information</p>
      </div>

      {/* Flag with tooltip */}
      <Flag
        text="VIP"
        variant="warning"
        tooltipText="This is a VIP customer with special privileges"
        className="position-absolute top-0 end-0"
      />
    </Container>
  );
};
```

## Step 2: Flag Variants and States

### 2.1 Semantic Variants

Use semantic variants to communicate different types of status:

```tsx
import { Flag, Container } from "@neuron/ui";

const VariantsExample = () => {
  return (
    <div className="d-grid content-gap">
      {/* Info variant */}
      <Container className="position-relative mb-3">
        <div>
          <h4>Information Status</h4>
          <p>Content with info flag</p>
        </div>
        <Flag text="INFO" variant="info" className="position-absolute top-0 end-0" />
      </Container>

      {/* Success variant */}
      <Container className="position-relative mb-3">
        <div>
          <h4>Success Status</h4>
          <p>Content with success flag</p>
        </div>
        <Flag text="DONE" variant="success" className="position-absolute top-0 end-0" />
      </Container>

      {/* Warning variant */}
      <Container className="position-relative mb-3">
        <div>
          <h4>Warning Status</h4>
          <p>Content with warning flag</p>
        </div>
        <Flag text="WARN" variant="warning" className="position-absolute top-0 end-0" />
      </Container>

      {/* Danger variant */}
      <Container className="position-relative mb-3">
        <div>
          <h4>Error Status</h4>
          <p>Content with danger flag</p>
        </div>
        <Flag text="ERROR" variant="danger" className="position-absolute top-0 end-0" />
      </Container>

      {/* New variant */}
      <Container className="position-relative mb-3">
        <div>
          <h4>New Status</h4>
          <p>Content with new flag</p>
        </div>
        <Flag text="NEW" variant="new" className="position-absolute top-0 end-0" />
      </Container>
    </div>
  );
};
```

### 2.2 Disabled and Historical Flags

Handle disabled states for historical flags that need to remain visible, including combination with line-through for cancelled historical states:

```tsx
import { Flag, Container, baseIcons } from "@neuron/ui";

const DisabledFlagsExample = () => {
  return (
    <div>
      {/* Active flag */}
      <Container className="position-relative mb-3">
        <div>
          <h4>Active Status</h4>
          <p>Current active flag</p>
        </div>
        <Flag text="ACTIVE" variant="success" className="position-absolute top-0 end-0" />
      </Container>

      {/* Disabled historical flag */}
      <Container className="position-relative mb-3">
        <div>
          <h4>Historical Status</h4>
          <p>This flag was removed but remains visible for historical context</p>
        </div>
        <Flag
          text="EXPIRED"
          variant="warning"
          disabled
          tooltipText="This status was removed but is shown for historical reference"
          className="position-absolute top-0 end-0"
        />
      </Container>

      {/* Line-through flag */}
      <Container className="position-relative mb-3">
        <div>
          <h4>Cancelled Status</h4>
          <p>This flag indicates a cancelled or removed state</p>
        </div>
        <Flag
          text="CANCELLED"
          variant="danger"
          lineThrough
          tooltipText="This item was cancelled"
          className="position-absolute top-0 end-0"
        />
      </Container>

      {/* Disabled with line-through combination */}
      <Container className="position-relative mb-3">
        <div>
          <h4>Historical Cancelled Status</h4>
          <p>This flag was historically cancelled but remains visible for reference</p>
        </div>
        <Flag
          text="REMOVED"
          variant="warning"
          disabled
          lineThrough
          tooltipText="This status was historically cancelled and removed, but shown for reference"
          className="position-absolute top-0 end-0"
        />
      </Container>
    </div>
  );
};
```

## Step 3: Multiple Flags Positioning

### 3.1 Multiple Flags with Proper Wrapper

When using multiple flags, wrap them in a div with proper Bootstrap 5 utility classes:

```tsx
import { Flag, Container, baseIcons } from "@neuron/ui";

const MultipleFlagsExample = () => {
  return (
    <Container className="position-relative">
      {/* Content container */}
      <div>
        <h3>Content with Multiple Flags</h3>
        <p>This content has multiple status flags positioned on top</p>
      </div>

      {/* Multiple flags wrapper - MANDATORY for multiple flags */}
      <div className="d-flex gap-8 position-absolute top-0 end-0">
        <Flag text="NEW" variant="success" />
        <Flag text="VIP" variant="warning" />
        <Flag icon={baseIcons.starSolid} variant="info" />
      </div>
    </Container>
  );
};
```

### 3.2 Positioned Flags (Left/Right)

Position flags on different sides using Bootstrap 5 utility classes:

```tsx
import { Flag, Container, baseIcons } from "@neuron/ui";

const PositionedFlagsExample = () => {
  return (
    <div className="d-grid content-gap">
      {/* Flags positioned on the left */}
      <Container className="position-relative mb-3">
        <div>
          <h4>Left Positioned Flags</h4>
          <p>Flags positioned on the left side</p>
        </div>
        <div className="d-flex gap-8 position-absolute top-0 start-0">
          <Flag text="LEFT" variant="info" />
          <Flag icon={baseIcons.flagSolid} variant="success" />
        </div>
      </Container>

      {/* Flags positioned on the right */}
      <Container className="position-relative mb-3">
        <div>
          <h4>Right Positioned Flags</h4>
          <p>Flags positioned on the right side</p>
        </div>
        <div className="d-flex gap-8 position-absolute top-0 end-0">
          <Flag text="RIGHT" variant="warning" />
          <Flag icon={baseIcons.crownSolid} variant="danger" />
        </div>
      </Container>
    </div>
  );
};
```

## Step 4: Integration with Neuron Containers

### 4.1 Flag with Container Component

Integrate flags with Neuron Container components:

```tsx
import { Flag, Container, baseIcons } from "@neuron/ui";

const ContainerFlagExample = () => {
  return (
    <Container className="position-relative">
      {/* Container content */}
      <div>
        <h3>Container with Status Flags</h3>
        <p>This container has status flags positioned on top</p>
        <div>Additional container content...</div>
      </div>

      {/* Flags positioned on container */}
      <div className="d-flex gap-8 position-absolute top-0 end-0">
        <Flag text="PREMIUM" variant="success" tooltipText="Premium content" />
        <Flag icon={baseIcons.lockSolid} variant="warning" tooltipText="Restricted access" />
      </div>
    </Container>
  );
};
```

### 4.2 Flag with Panel Component

Use flags with Panel components for status indication:

```tsx
import { Flag, Panel, baseIcons } from "@neuron/ui";

const PanelFlagExample = () => {
  return (
    <Panel className="position-relative">
      <Panel.Header>
        <h3>Panel with Status Flags</h3>
      </Panel.Header>
      <Panel.Body>
        <p>Panel content with status indicators</p>
        <div>Additional panel information...</div>
      </Panel.Body>

      {/* Flags on panel */}
      <div className="d-flex gap-8 position-absolute top-0 end-0">
        <Flag text="DRAFT" variant="info" />
        <Flag text="REVIEW" variant="warning" tooltipText="Pending review approval" />
      </div>
    </Panel>
  );
};
```

## Step 5: Common Use Cases and Patterns

### 5.1 Document Status Flags

Use flags to indicate document or item status:

```tsx
import { Flag, baseIcons } from "@neuron/ui";

const DocumentStatusExample = () => {
  const documents = [
    {
      id: 1,
      title: "Project Proposal",
      content: "Initial project proposal document",
      flags: [
        { text: "DRAFT", variant: "info" as const },
        { icon: baseIcons.clockSolid, variant: "warning" as const, tooltipText: "Pending approval" },
      ],
    },
    {
      id: 2,
      title: "Final Report",
      content: "Completed project report",
      flags: [
        { text: "FINAL", variant: "success" as const },
        { icon: baseIcons.checkSolid, variant: "success" as const, tooltipText: "Approved" },
      ],
    },
    {
      id: 3,
      title: "Old Specification",
      content: "Outdated specification document",
      flags: [
        {
          text: "OUTDATED",
          variant: "danger" as const,
          disabled: true,
          tooltipText: "This document is no longer current but kept for reference",
        },
      ],
    },
  ];

  return (
    <div>
      <h3>Document Status Examples</h3>
      {documents.map((doc) => (
        <div
          key={doc.id}
          style={{ position: "relative", padding: "20px", border: "1px solid #ccc", marginBottom: "10px" }}
        >
          <div>
            <h4>{doc.title}</h4>
            <p>{doc.content}</p>
          </div>

          {/* Document flags */}
          <div className="d-flex gap-8 position-absolute top-0 end-0">
            {doc.flags.map((flag, index) => (
              <Flag key={index} {...flag} />
            ))}
          </div>
        </div>
      ))}
    </div>
  );
};
```

### 5.2 User Profile Flags

Display user status and privileges with flags:

```tsx
import { Flag, baseIcons } from "@neuron/ui";

const UserProfileFlagsExample = () => {
  return (
    <div style={{ position: "relative", padding: "20px", border: "1px solid #ccc" }}>
      {/* User profile content */}
      <div>
        <h3>John Doe</h3>
        <p>Senior Developer</p>
        <div>
          <strong>Email:</strong> john.doe@example.com
          <br />
          <strong>Department:</strong> Engineering
          <br />
          <strong>Join Date:</strong> January 2020
        </div>
      </div>

      {/* User status flags */}
      <div className="d-flex gap-8 position-absolute top-0 end-0">
        <Flag text="VIP" variant="warning" tooltipText="VIP customer with premium support" />
        <Flag icon={baseIcons.shieldSolid} variant="success" tooltipText="Verified account" />
        <Flag text="ADMIN" variant="danger" tooltipText="Administrator privileges" />
      </div>
    </div>
  );
};
```

### 5.3 Product Status Flags

Indicate product or item status with flags:

```tsx
import { Flag, baseIcons } from "@neuron/ui";

const ProductStatusExample = () => {
  const products = [
    {
      id: 1,
      name: "Premium Widget",
      price: "$99.99",
      flags: [
        { text: "NEW", variant: "success" as const },
        { text: "HOT", variant: "danger" as const, tooltipText: "Popular item" },
      ],
    },
    {
      id: 2,
      name: "Standard Widget",
      price: "$49.99",
      flags: [{ text: "SALE", variant: "warning" as const, tooltipText: "20% off original price" }],
    },
    {
      id: 3,
      name: "Legacy Widget",
      price: "$29.99",
      flags: [
        {
          text: "DISCONTINUED",
          variant: "danger" as const,
          lineThrough: true,
          tooltipText: "This product is no longer available",
        },
      ],
    },
  ];

  return (
    <div>
      <h3>Product Catalog</h3>
      <div style={{ display: "grid", gridTemplateColumns: "repeat(auto-fit, minmax(250px, 1fr))", gap: "16px" }}>
        {products.map((product) => (
          <div key={product.id} style={{ position: "relative", padding: "20px", border: "1px solid #ccc" }}>
            <div>
              <h4>{product.name}</h4>
              <p>
                <strong>{product.price}</strong>
              </p>
              <div>Product description and details...</div>
            </div>

            {/* Product flags */}
            <div className="d-flex gap-8 position-absolute top-0 end-0">
              {product.flags.map((flag, index) => (
                <Flag key={index} {...flag} />
              ))}
            </div>
          </div>
        ))}
      </div>
    </div>
  );
};
```

### 5.4 Integration with SectionHeader

Flags are commonly used with SectionHeader component (as shown in the provided code):

```tsx
import { SectionHeader, Flag } from "@neuron/ui";

const SectionHeaderFlagsExample = () => {
  const sectionFlags = [
    { text: "BETA", variant: "info" as const, tooltipText: "This feature is in beta" },
    { text: "NEW", variant: "success" as const },
  ];

  return (
    <div>
      <SectionHeader
        title="Section with Flags"
        subtitle="This section demonstrates flag integration"
        flags={sectionFlags}
      >
        <div>
          <p>Section content goes here...</p>
          <div>Additional section information and controls</div>
        </div>
      </SectionHeader>
    </div>
  );
};
```

## Step 6: Custom Content Integration

### 6.1 Custom Container with Flags

Create custom content containers with flag integration:

```tsx
import { Flag, baseIcons } from "@neuron/ui";

const CustomContainerExample = () => {
  return (
    <div
      className="custom-content-container position-relative"
      style={{
        padding: "24px",
        border: "2px solid #e0e0e0",
        borderRadius: "8px",
        backgroundColor: "#f9f9f9",
      }}
    >
      {/* Custom content */}
      <div>
        <h3>Custom Content Container</h3>
        <p>This is a custom container with integrated status flags</p>
        <div>
          <button>Action Button</button>
          <button>Secondary Action</button>
        </div>
      </div>

      {/* Status flags */}
      <div className="d-flex gap-8 position-absolute top-0 end-0" style={{ margin: "8px" }}>
        <Flag text="CUSTOM" variant="info" />
        <Flag icon={baseIcons.starSolid} variant="success" tooltipText="Featured content" />
      </div>
    </div>
  );
};
```

### 6.2 Card Component with Flags

Integrate flags with card-like components:

```tsx
import { Flag, baseIcons } from "@neuron/ui";

const CardWithFlagsExample = () => {
  return (
    <div
      className="card position-relative"
      style={{
        border: "1px solid #ddd",
        borderRadius: "8px",
        overflow: "hidden",
      }}
    >
      {/* Card header */}
      <div style={{ padding: "16px", borderBottom: "1px solid #eee", backgroundColor: "#f8f9fa" }}>
        <h4>Card Title</h4>
      </div>

      {/* Card body */}
      <div style={{ padding: "16px" }}>
        <p>Card content with status indicators</p>
        <div>Additional card information and actions</div>
      </div>

      {/* Card flags */}
      <div className="d-flex gap-8 position-absolute top-0 end-0" style={{ margin: "8px" }}>
        <Flag text="FEATURED" variant="warning" />
        <Flag icon={baseIcons.heartSolid} variant="danger" tooltipText="Favorite item" />
      </div>
    </div>
  );
};
```

## Step 7: Accessibility and Best Practices

### 7.1 Accessibility Features

The Flag component includes built-in accessibility features:

- **ARIA Attributes**: Proper aria-disabled for disabled states
- **Screen Reader Support**: Meaningful content for assistive technologies
- **Tooltip Integration**: Accessible tooltip implementation
- **Test Integration**: Built-in test ID support for automated testing

```tsx
import { Flag, baseIcons } from "@neuron/ui";

const AccessibilityExample = () => {
  return (
    <div style={{ position: "relative", padding: "20px", border: "1px solid #ccc" }}>
      <div>
        <h3>Accessible Content with Flags</h3>
        <p>This content demonstrates proper accessibility practices</p>
      </div>

      {/* Accessible flags with proper test IDs */}
      <div className="d-flex gap-8 position-absolute top-0 end-0">
        <Flag
          text="ACCESSIBLE"
          variant="success"
          testId="accessibility-flag"
          tooltipText="This content meets accessibility standards"
        />
        <Flag
          icon={baseIcons.universalAccessSolid}
          variant="info"
          testId="universal-access-flag"
          tooltipText="Universal access compliant"
        />
      </div>
    </div>
  );
};
```

### 7.2 Best Practices

**Do:**

- Always use Neuron Container with `position-relative` className
- Add `position-absolute` className to Flag components
- Use `d-flex gap-8` wrapper for multiple flags
- Provide meaningful tooltips for context
- Use semantic variants that match the flag purpose
- Include test IDs for automated testing
- Use disabled state for historical flags that need to remain visible

**Don't:**

- Use inline styles instead of proper Container components
- Place flags without a relative positioned parent container
- Use multiple flags without proper wrapper classes
- Rely solely on color to convey meaning
- Use flags for primary navigation or actions
- Overuse flags - they should highlight important status only

### 7.3 Performance Considerations

- Flag components are lightweight with minimal performance impact
- Tooltip rendering is optimized and only rendered when needed
- Icon rendering is efficient through the baseIcons system
- Consider the visual weight of multiple flags in dense layouts

## Step 8: Bootstrap 5 Utility Classes Reference

### 8.1 Required Classes for Flag Positioning

**Parent Container:**

- `position-relative` - MANDATORY for flag positioning

**Multiple Flags Wrapper:**

- `d-flex gap-8` - MANDATORY for multiple flags
- `position-absolute` - MANDATORY for absolute positioning

**Positioning Options:**

- `top-0 start-0` - Top left positioning
- `top-0 end-0` - Top right positioning (most common)
- `bottom-0 start-0` - Bottom left positioning
- `bottom-0 end-0` - Bottom right positioning

### 8.2 Complete Positioning Example

```tsx
import { Flag, Container } from "@neuron/ui";

const PositioningReferenceExample = () => {
  return (
    <div>
      {/* MANDATORY: Use Neuron Container with position-relative */}
      <Container className="position-relative">
        <div>Content goes here</div>

        {/* MANDATORY: Multiple flags need d-flex gap-8 wrapper */}
        <div className="d-flex gap-8 position-absolute top-0 end-0">
          <Flag text="FLAG1" variant="success" />
          <Flag text="FLAG2" variant="info" />
        </div>
      </Container>
    </div>
  );
};
```

## Summary

The Neuron Flag component provides versatile status badge functionality with support for:

- **Status Badge Display**: Compact visual indicators for various states
- **Absolute Positioning**: Positioned on top of relative containers
- **Historical States**: Disabled flags for removed but important information
- **Semantic Variants**: Info, success, warning, danger, and new styling
- **Flexible Content**: Both text and icon content support
- **Tooltip Integration**: Optional tooltips for additional context
- **Multiple Flag Support**: Proper wrapper classes for multiple flags
- **Bootstrap 5 Integration**: Utility classes for flexible positioning
- **Accessibility**: Full screen reader support and proper ARIA attributes

Use flags strategically to highlight important status information on content containers. Remember to always use proper positioning classes and consider the historical context when using disabled states. Flags work excellently with Neuron containers and can be integrated into custom content layouts following the positioning guidelines.

---
agent: agent
---

# AI-Assisted Neuron Literal Component Integration Guide

## For the AI Assistant

Your task is to integrate and configure the Neuron Literal component in a React application. This guide provides essential instructions for implementing Literal components, which display labeled text values with enhanced features like icons, tooltips, quick actions, and notification badges across all Neuron applications.

## Sync Metadata

- **Component Version:** v5.0.3
- **Component Source:** `packages/neuron/ui/src/lib/misc/literal/Literal.tsx`
- **Guideline Command:** `/neuron-ui-literal`
- **Related Skill:** `neuron-ui-misc`

## Introduction

The Literal component provides a standardized way to display labeled read-only text values with rich enhancements. It's commonly used in forms, data displays, and information sections where you need to present data in a structured, labeled format.

Key features include:

- **Labeled Display** - Combines Label and value display in one component
- **Multiple Values** - Support for displaying multiple values under one label
- **Rich Content** - Text, links, and interactive content support
- **Visual Variants** - Multiple color variants for semantic meaning
- **Icon Integration** - baseIcons support for visual enhancement
- **Tooltip Support** - Built-in tooltip functionality for additional context
- **Quick Actions** - Integrated contextual action menus
- **Notification Badges** - Badge support for status indicators
- **Responsive Design** - Inline and block layout options
- **Fallback Handling** - Automatic dash display for empty values

### Key Features

- **Labeled Values**: Combines label and content in structured layout
- **Multiple Content Types**: String text, Link components, or interactive content
- **Visual Variants**: 14 different color variants for semantic meaning
- **Rich Enhancements**: Icons, tooltips, quick actions, and notification badges
- **Layout Options**: Block (default) or inline display modes
- **Accessibility**: Proper semantic structure and ARIA support
- **Fallback Display**: Shows "-" when no values are provided

## 🚨 CRITICAL API USAGE RULES

**MANDATORY PROP STRUCTURE - READ THIS FIRST:**

### ✅ CORRECT Literal Component Usage:

```tsx
<Literal
  label={{ text: "Label Text" }} // ← REQUIRED: label must be object with text property
  values={[
    // ← REQUIRED: values must be array
    { content: "Value Text" }, // ← REQUIRED: each value needs content property
  ]}
/>
```

### ❌ WRONG Usage (TypeScript will error):

```tsx
{
  /* WRONG: label as string */
}
<Literal label="Label Text" values={[{ content: "Value" }]} />;

{
  /* WRONG: value as string */
}
<Literal label={{ text: "Label" }} value="Value" />;

{
  /* WRONG: content without object wrapper */
}
<Literal label={{ text: "Label" }} values={["Value"]} />;
```

### 🔧 Required Props Structure:

1. **`label`**: Always an object with `text` property

   - ✅ `label={{ text: "User Name" }}`
   - ❌ `label="User Name"`

2. **`values`**: Always an array of objects

   - ✅ `values={[{ content: "John Doe" }]}`
   - ❌ `values="John Doe"`
   - ❌ `value={{ content: "John Doe" }}`

3. **`content`**: Always inside value object
   - ✅ `{ content: "Text" }`
   - ❌ `"Text"`

## 🎯 Advanced Features from Figma Analysis

### 🔍 Figma MCP Analysis Guidelines

**CRITICAL: Always read the complete Figma code structure when using Figma MCP tools:**

1. **Full Code Inspection**: Use `get_code` to retrieve the complete component structure
2. **Component Matching**: Identify all Neuron UI components by their names and props
3. **Visual Verification**: Use `get_image` to confirm visual appearance matches code structure
4. **Node Analysis**: Check Figma layer names for component type hints (Link, Button, etc.)
5. **Color Mapping**: Map visual colors to semantic variants in Neuron UI

**Component Identification Priority:**

- **Node Names**: Look for "Link", "Button", "Literal", "NotificationBadge" in layer names
- **Color Analysis**: Different colors indicate different component types or variants
- **Structure Analysis**: Nested components reveal parent-child relationships
- **Props Mapping**: Figma props should map to Neuron UI component props

### Label Tooltips

**When you see a tooltip icon (ℹ️) next to a label in Figma:**

```tsx
<Literal
  label={{
    text: "KIPO",
    tooltipText: "Additional information about KIPO",
  }}
  values={[{ content: "6230974357623865" }]}
/>
```

### Multiple Values with Mixed Content Types

**Visual Identification in Figma:**

- **Plain Text**: Default text color (usually dark gray/black)
- **Link Text**: Different color (usually blue/teal) and/or underlined
- **Notification Badges**: Small circular indicators with numbers
- **Node Names**: Check Figma layer names for "Link" or similar component indicators

```tsx
<Literal
  label={{ text: "Field Label" }}
  values={[
    { content: "Plain text value" },
    {
      content: { href: "#", children: "Link text value" },
      notificationBadge: { count: 2, variant: "info" },
    },
  ]}
/>
```

**Critical Identification Rules:**

- **Color Analysis**: Any text with different color than default = Link component
- **Node Inspection**: Read Figma layer names to identify Link components
- **Mixed Content**: You can combine string content and link content in the same Literal
- **Badge + Link**: Notification badges can be applied to both text and link values
- **Automatic Separation**: Literal automatically handles comma separation between values

### Link Values

**Visual Identification in Figma:**

- **Color Different from Default**: Text in blue, teal, or any non-default color
- **Underlined Text**: Text with underline decoration
- **Node Name Contains "Link"**: Check Figma layer/component names
- **Cursor Changes**: Interactive elements in Figma prototypes

```tsx
<Literal
  label={{ text: "Field Label" }}
  values={[
    {
      content: {
        href: "#",
        children: "Link text content",
      },
    },
  ]}
/>
```

### Empty Values (Comma Behavior)

**When no values are provided, Literal automatically shows "-":**

```tsx
{
  /* Shows "-" automatically */
}
<Literal label={{ text: "Titul za" }} values={[]} />;

{
  /* Or omit values prop entirely */
}
<Literal label={{ text: "Titul za" }} />;
```

## 🎯 Figma Identification Rules

**CRITICAL RULE**: When you see a label positioned above a literal/value in Figma:

- **ALWAYS** use the integrated `label` prop within the `Literal` component
- **NEVER** use separate `Label` + `Literal` components
- The Literal component has built-in label functionality

**Examples:**

- ✅ `<Literal label={{ text: "Jméno" }} values={[{ content: "Veronika" }]} />`
- ❌ `<Label>Jméno</Label><Literal values={[{ content: "Veronika" }]} />`

**Visual Indicators in Figma:**

- Label text positioned above the value/content
- Structured layout with label-value pairs
- Read-only content display (not editable inputs)
- Often used in detail views, information panels, or data displays

**Selection Rule**: If you see label + value pairs in Figma → Use Literal component with integrated label prop

**Storybook Reference:** You can refer to the Storybook documentation for visual examples and interactive demos of all available Literal configurations.

## Step 1: Basic Literal Implementation

### 1.1 Import the Literal Component

```tsx
import { Literal } from "@neuron/ui";
```

### 1.2 Basic Usage

Simple text display with label:

```tsx
import { Literal } from "@neuron/ui";

const BasicLiteral = () => {
  return <Literal label={{ text: "User Name" }} values={[{ content: "John Doe" }]} />;
};
```

### 1.3 Multiple Values

Display multiple values under one label:

```tsx
import { Literal } from "@neuron/ui";

const MultipleValues = () => {
  return (
    <Literal
      label={{ text: "Tags" }}
      values={[
        { content: "React", variant: "brand" },
        { content: "TypeScript", variant: "info" },
        { content: "Frontend", variant: "success" },
      ]}
    />
  );
};
```

### 1.4 Inline Layout

Use inline layout for compact display:

```tsx
import { Literal } from "@neuron/ui";

const InlineLiteral = () => {
  return <Literal label={{ text: "Status" }} values={[{ content: "Active", variant: "success" }]} inline={true} />;
};
```

## Step 2: Content Types

### 2.1 Text Content

Simple string content display:

```tsx
import { Literal } from "@neuron/ui";

const TextContent = () => {
  return (
    <Literal
      label={{ text: "Description" }}
      values={[
        {
          content: "This is a simple text description",
          bold: true,
        },
      ]}
    />
  );
};
```

### 2.2 Link Content

Display clickable links:

```tsx
import { Literal } from "@neuron/ui";
import { baseIcons } from "@neuron/ui";

const LinkContent = () => {
  return (
    <Literal
      label={{ text: "Website" }}
      values={[
        {
          content: {
            href: "https://example.com",
            children: "Visit our website",
            target: "_blank",
            iconRight: baseIcons.arrowUpRightFromSquareRegular,
          },
        },
      ]}
    />
  );
};
```

### 2.3 Interactive Content

Display content with click handlers:

```tsx
import { Literal } from "@neuron/ui";

const InteractiveContent = () => {
  const handleClick = () => {
    console.info("Content clicked");
  };

  return (
    <Literal
      label={{ text: "Action Item" }}
      values={[
        {
          content: {
            children: "Click to perform action",
            onClick: handleClick,
          },
        },
      ]}
    />
  );
};
```

## Step 3: Visual Variants

### 3.1 Semantic Variants

Use semantic variants for meaning:

```tsx
import { Literal } from "@neuron/ui";

const SemanticVariants = () => {
  return (
    <div>
      <Literal label={{ text: "Status" }} values={[{ content: "Success", variant: "success" }]} />

      <Literal label={{ text: "Error" }} values={[{ content: "Failed", variant: "danger" }]} />

      <Literal label={{ text: "Warning" }} values={[{ content: "Attention needed", variant: "warning" }]} />

      <Literal label={{ text: "Information" }} values={[{ content: "Additional info", variant: "info" }]} />
    </div>
  );
};
```

### 3.2 Brand and Custom Variants

Use brand and custom color variants:

```tsx
import { Literal } from "@neuron/ui";

const ColorVariants = () => {
  return (
    <div>
      <Literal label={{ text: "Brand" }} values={[{ content: "Brand color", variant: "brand" }]} />

      <Literal
        label={{ text: "Custom Colors" }}
        values={[
          { content: "Apple Green", variant: "appleGreen" },
          { content: "Celestial Blue", variant: "celestialBlue" },
          { content: "Electric Purple", variant: "electricPurple" },
        ]}
      />
    </div>
  );
};
```

### 3.3 All Available Variants

Complete list of available variants:

- `default` - Standard text color
- `brand` - Brand color theme
- `danger` - Red for errors/warnings
- `info` - Blue for information
- `success` - Green for success states
- `warning` - Yellow/orange for warnings
- `appleGreen` - Custom green variant
- `emeraldGreen` - Custom emerald variant
- `celestialBlue` - Custom blue variant
- `majorelleBlue` - Custom blue variant
- `electricPurple` - Custom purple variant
- `indianRed` - Custom red variant
- `sendyBrown` - Custom brown variant
- `citrine` - Custom yellow variant

## Step 4: Icon Integration

### 4.1 Basic Icon Usage

Add icons to enhance visual meaning:

```tsx
import { Literal, baseIcons } from "@neuron/ui";

const IconUsage = () => {
  return (
    <Literal
      label={{ text: "Contact Information" }}
      values={[
        {
          content: "john@example.com",
          icon: { iconDef: baseIcons.envelopeRegular },
          variant: "info",
        },
        {
          content: "+1 234 567 8900",
          icon: { iconDef: baseIcons.phoneRegular },
          variant: "success",
        },
      ]}
    />
  );
};
```

### 4.2 Icon with Custom Properties

Customize icon appearance:

```tsx
import { Literal, baseIcons } from "@neuron/ui";
import { IconSize } from "@neuron/core";

const CustomIconUsage = () => {
  return (
    <Literal
      label={{ text: "Files" }}
      values={[
        {
          content: "document.pdf",
          icon: {
            iconDef: baseIcons.filePdfRegular,
            size: IconSize.small,
          },
          variant: "danger",
        },
      ]}
    />
  );
};
```

## Step 5: Enhanced Features

### 5.1 Tooltip Integration

Add tooltips for additional context:

```tsx
import { Literal, baseIcons } from "@neuron/ui";

const TooltipUsage = () => {
  return (
    <Literal
      label={{
        text: "Configuration",
        tooltipText: "System configuration settings",
        tooltipPlacement: "top",
      }}
      values={[
        {
          content: "Production Environment",
          tooltip: {
            tooltipContent: "Currently running in production mode with full security",
            placement: "right",
          },
          icon: { iconDef: baseIcons.serverRegular },
          variant: "success",
        },
      ]}
    />
  );
};
```

### 5.2 Quick Actions Integration

Add contextual actions to values:

```tsx
import { Literal, baseIcons, quickActions } from "@neuron/ui";

const QuickActionsUsage = () => {
  const actions = [
    quickActions.copy({ value: "john@example.com" }),
    {
      labelText: "Send Email",
      iconDef: baseIcons.envelopeRegular,
      eventHandler: () => console.info("Send email triggered"),
    },
  ];

  return (
    <Literal
      label={{ text: "Email Address" }}
      values={[
        {
          content: "john@example.com",
          quickActions: {
            actions: actions,
            targetValue: "john@example.com",
          },
          icon: { iconDef: baseIcons.envelopeRegular },
          variant: "info",
        },
      ]}
    />
  );
};
```

### 5.3 Notification Badge Integration

Add status badges to values:

```tsx
import { Literal, baseIcons } from "@neuron/ui";

const BadgeUsage = () => {
  return (
    <Literal
      label={{ text: "Messages" }}
      values={[
        {
          content: "Inbox",
          notificationBadge: {
            count: 5,
            variant: "danger",
            tooltip: "You have 5 unread messages",
          },
          icon: { iconDef: baseIcons.inboxRegular },
          variant: "default",
        },
      ]}
    />
  );
};
```

## Step 6: Combined Features

### 6.1 Full Feature Integration

Combine multiple enhancements:

```tsx
import { Literal, baseIcons, quickActions } from "@neuron/ui";

const FullFeatured = () => {
  const emailActions = [
    quickActions.copy({ value: "admin@company.com" }),
    {
      labelText: "Send Email",
      iconDef: baseIcons.envelopeRegular,
      eventHandler: () => console.info("Send email"),
    },
  ];

  return (
    <Literal
      label={{
        text: "Administrator Contact",
        tooltipText: "Primary system administrator contact information",
      }}
      values={[
        {
          content: "admin@company.com",
          bold: true,
          icon: { iconDef: baseIcons.envelopeRegular },
          tooltip: {
            tooltipContent: "Click the actions menu to copy or send email",
            placement: "top",
          },
          quickActions: {
            actions: emailActions,
            targetValue: "Administrator Email",
          },
          notificationBadge: {
            count: 2,
            variant: "warning",
            tooltip: "2 pending admin requests",
          },
          variant: "brand",
        },
      ]}
    />
  );
};
```

### 6.2 Complex Multi-Value Display

Display related information with different enhancements:

```tsx
import { Literal, baseIcons, quickActions } from "@neuron/ui";

const ComplexDisplay = () => {
  return (
    <Literal
      label={{ text: "User Profile" }}
      values={[
        {
          content: "John Doe",
          icon: { iconDef: baseIcons.userRegular },
          variant: "brand",
          bold: true,
        },
        {
          content: {
            href: "mailto:john@company.com",
            children: "john@company.com",
          },
          icon: { iconDef: baseIcons.envelopeRegular },
          variant: "info",
        },
        {
          content: "Administrator",
          icon: { iconDef: baseIcons.shieldRegular },
          variant: "success",
          notificationBadge: {
            icon: baseIcons.checkRegular,
            variant: "success",
            tooltip: "Verified administrator",
          },
        },
      ]}
    />
  );
};
```

## Step 7: Literal Props Reference

### 7.1 Core Props

| Prop        | Type                  | Default | Description                               |
| ----------- | --------------------- | ------- | ----------------------------------------- |
| `label`     | `LabelProps`          | -       | Label configuration (text, tooltip, etc.) |
| `values`    | `LiteralValueProps[]` | -       | Array of values to display                |
| `className` | `string`              | -       | Additional CSS class for styling          |
| `inline`    | `boolean`             | `false` | Whether to render in inline layout        |

### 7.2 LiteralValueProps Interface

| Property            | Type                                  | Default     | Description                           |
| ------------------- | ------------------------------------- | ----------- | ------------------------------------- |
| `content`           | `string \| LinkProps`                 | -           | Content to display (text or link)     |
| `icon`              | `IconProps`                           | -           | Icon configuration using baseIcons    |
| `tooltip`           | `TooltipProps`                        | -           | Tooltip configuration                 |
| `quickActions`      | `Omit<QuickActionsProps, "children">` | -           | Quick actions menu configuration      |
| `notificationBadge` | `NotificationBadgeProps`              | -           | Notification badge configuration      |
| `variant`           | `TLiteralVariant`                     | `"default"` | Visual variant/color theme            |
| `bold`              | `boolean`                             | `false`     | Whether to display content in bold    |
| `lineClamp`         | `number`                              | `3`         | Number of lines to clamp text content |
| `testId`            | `string`                              | -           | Test identifier for testing           |

### 7.3 Content Types

**String Content:**

```tsx
{
  content: "Simple text";
}
```

**Link Content:**

```tsx
{
  content: {
    href: "https://example.com",
    children: "Link text",
    target: "_blank"
  }
}
```

**Interactive Content:**

```tsx
{
  content: {
    children: "Clickable text",
    onClick: () => console.info("Clicked")
  }
}
```

## Step 8: Common Use Cases

### 8.1 User Information Display

Display user profile information:

```tsx
import { Literal, baseIcons } from "@neuron/ui";

const UserInfo = ({ user }) => {
  return (
    <div>
      <Literal
        label={{ text: "Full Name" }}
        values={[
          {
            content: `${user.firstName} ${user.lastName}`,
            bold: true,
            icon: { iconDef: baseIcons.userRegular },
          },
        ]}
      />

      <Literal
        label={{ text: "Email" }}
        values={[
          {
            content: {
              href: `mailto:${user.email}`,
              children: user.email,
            },
            icon: { iconDef: baseIcons.envelopeRegular },
            variant: "info",
          },
        ]}
      />

      <Literal
        label={{ text: "Role" }}
        values={[
          {
            content: user.role,
            icon: { iconDef: baseIcons.shieldRegular },
            variant: user.role === "admin" ? "danger" : "success",
          },
        ]}
      />
    </div>
  );
};
```

### 8.2 System Status Display

Display system information with status indicators:

```tsx
import { Literal, baseIcons } from "@neuron/ui";

const SystemStatus = ({ status }) => {
  return (
    <div>
      <Literal
        label={{ text: "System Status" }}
        values={[
          {
            content: status.overall,
            icon: {
              iconDef:
                status.overall === "Operational" ? baseIcons.checkCircleRegular : baseIcons.exclamationTriangleRegular,
            },
            variant: status.overall === "Operational" ? "success" : "danger",
            bold: true,
          },
        ]}
      />

      <Literal
        label={{ text: "Services" }}
        values={status.services.map((service) => ({
          content: service.name,
          variant: service.status === "up" ? "success" : "danger",
          notificationBadge:
            service.status !== "up"
              ? {
                  icon: baseIcons.exclamationRegular,
                  variant: "danger",
                  tooltip: "Service is down",
                }
              : undefined,
        }))}
      />
    </div>
  );
};
```

### 8.3 Document Information

Display document metadata with actions:

```tsx
import { Literal, baseIcons, quickActions } from "@neuron/ui";

const DocumentInfo = ({ document }) => {
  const documentActions = [
    quickActions.copy({ value: document.id }),
    quickActions.openInNewTab({ url: document.downloadUrl }),
    {
      labelText: "Share Document",
      iconDef: baseIcons.shareRegular,
      eventHandler: () => shareDocument(document),
    },
  ];

  return (
    <div>
      <Literal
        label={{ text: "Document Name" }}
        values={[
          {
            content: document.name,
            icon: { iconDef: baseIcons.fileRegular },
            quickActions: {
              actions: documentActions,
              targetValue: document.name,
            },
            bold: true,
          },
        ]}
      />

      <Literal
        label={{ text: "File Size" }}
        values={[
          {
            content: formatFileSize(document.size),
            icon: { iconDef: baseIcons.hardDriveRegular },
            variant: "info",
          },
        ]}
      />

      <Literal
        label={{ text: "Last Modified" }}
        values={[
          {
            content: formatDate(document.modified),
            icon: { iconDef: baseIcons.clockRegular },
            tooltip: {
              tooltipContent: `Modified by ${document.modifiedBy}`,
              placement: "top",
            },
          },
        ]}
      />
    </div>
  );
};
```

## Step 9: Best Practices

### 9.1 Content Organization

**Use appropriate variants for semantic meaning:**

```tsx
{
  /* ✅ CORRECT: Semantic variant usage */
}
<Literal
  label={{ text: "Status" }}
  values={[
    { content: "Active", variant: "success" }, // Green for positive
    { content: "Pending", variant: "warning" }, // Yellow for caution
    { content: "Failed", variant: "danger" }, // Red for negative
  ]}
/>;

{
  /* ❌ WRONG: Arbitrary variant usage */
}
<Literal
  label={{ text: "Status" }}
  values={[
    { content: "Active", variant: "electricPurple" }, // Purple doesn't convey status meaning
  ]}
/>;
```

### 9.2 Icon Selection

**Use semantically appropriate icons:**

```tsx
{
  /* ✅ CORRECT: Semantic icon usage */
}
<Literal
  label={{ text: "Contact Info" }}
  values={[
    {
      content: "email@example.com",
      icon: { iconDef: baseIcons.envelopeRegular }, // Email icon for email
    },
    {
      content: "+1234567890",
      icon: { iconDef: baseIcons.phoneRegular }, // Phone icon for phone
    },
  ]}
/>;
```

### 9.3 Quick Actions Usage

**Provide relevant actions for content type:**

```tsx
{
  /* ✅ CORRECT: Relevant actions for email */
}
const emailActions = [
  quickActions.copy({ value: email }),
  {
    labelText: "Send Email",
    iconDef: baseIcons.envelopeRegular,
    eventHandler: () => window.open(`mailto:${email}`),
  },
];

{
  /* ✅ CORRECT: Relevant actions for URL */
}
const urlActions = [quickActions.copy({ value: url }), quickActions.openInNewTab({ url: url })];
```

### 9.4 Tooltip Guidelines

**Provide helpful, non-redundant tooltip content:**

```tsx
{
  /* ✅ CORRECT: Helpful additional context */
}
<Literal
  label={{ text: "API Key" }}
  values={[
    {
      content: "sk-1234...abcd",
      tooltip: {
        tooltipContent: "This API key expires in 30 days",
        placement: "top",
      },
    },
  ]}
/>;

{
  /* ❌ WRONG: Redundant tooltip */
}
<Literal
  label={{ text: "API Key" }}
  values={[
    {
      content: "sk-1234...abcd",
      tooltip: {
        tooltipContent: "API Key", // Just repeats the label
      },
    },
  ]}
/>;
```

## Step 10: Common Mistakes to Avoid

### 10.1 Don't Overuse Enhancements

**Use enhancements purposefully:**

```tsx
{
  /* ❌ WRONG: Too many unnecessary enhancements */
}
<Literal
  label={{ text: "Simple Text" }}
  values={[
    {
      content: "Basic text",
      icon: { iconDef: baseIcons.textRegular }, // Unnecessary icon
      tooltip: { tooltipContent: "This is text" }, // Redundant tooltip
      notificationBadge: { count: 1 }, // Meaningless badge
      variant: "electricPurple", // Arbitrary color
    },
  ]}
/>;

{
  /* ✅ CORRECT: Clean, purposeful display */
}
<Literal label={{ text: "Description" }} values={[{ content: "Basic descriptive text" }]} />;
```

### 10.2 Don't Mix Content Types Inconsistently

**Be consistent with content types within related values:**

```tsx
{
  /* ❌ WRONG: Inconsistent content types */
}
<Literal
  label={{ text: "Contact Methods" }}
  values={[
    { content: "email@example.com" }, // Plain text
    {
      content: {
        href: "tel:+1234567890",
        children: "+1234567890",
      },
    }, // Link
  ]}
/>;

{
  /* ✅ CORRECT: Consistent interactive content */
}
<Literal
  label={{ text: "Contact Methods" }}
  values={[
    {
      content: {
        href: "mailto:email@example.com",
        children: "email@example.com",
      },
    },
    {
      content: {
        href: "tel:+1234567890",
        children: "+1234567890",
      },
    },
  ]}
/>;
```

### 10.3 Don't Forget Empty State Handling

**The component automatically handles empty values:**

```tsx
{/* ✅ CORRECT: Component automatically shows "-" for empty values */}
<Literal
  label={{ text: "Optional Field" }}
  values={[]} // Shows fallback dash automatically
/>

<Literal
  label={{ text: "Optional Field" }}
  values={undefined} // Shows fallback dash automatically
/>
```

### 10.4 Don't Ignore Line Clamping for Long Text

**Control text overflow with lineClamp:**

```tsx
{
  /* ✅ CORRECT: Control long text display */
}
<Literal
  label={{ text: "Long Description" }}
  values={[
    {
      content: "Very long text content that might overflow...",
      lineClamp: 2, // Limit to 2 lines
      tooltip: {
        tooltipContent: "Full text: Very long text content...", // Show full text in tooltip
      },
    },
  ]}
/>;
```

### 10.5 Don't Use Inline Layout for Complex Content

**Use inline layout only for simple content:**

```tsx
{
  /* ❌ WRONG: Inline with complex content */
}
<Literal
  inline={true}
  label={{ text: "Complex Data" }}
  values={[
    {
      content: "Complex content with many features",
      icon: { iconDef: baseIcons.gearRegular },
      quickActions: { actions: manyActions },
      notificationBadge: { count: 5 },
    },
  ]}
/>;

{
  /* ✅ CORRECT: Inline with simple content */
}
<Literal inline={true} label={{ text: "Status" }} values={[{ content: "Active", variant: "success" }]} />;
```

## Key Takeaways

The Neuron Literal component provides rich, labeled data display with extensive enhancement options. Key points to remember:

1. **Labeled Structure** combines label and values in standardized layout
2. **Multiple Content Types** support text, links, and interactive content
3. **Visual Variants** provide 14 color options for semantic meaning
4. **Rich Enhancements** include icons, tooltips, quick actions, and badges
5. **Layout Options** offer block and inline display modes
6. **Automatic Fallbacks** show "-" when no values are provided
7. **Icon Integration** uses baseIcons for consistent visual design
8. **Responsive Features** work seamlessly across device types
9. **Accessibility Support** provides proper semantic structure
10. **Performance Optimized** with line clamping and efficient rendering

By following these guidelines, you'll create rich, informative data displays that enhance user experience while maintaining consistency across your Neuron applications.

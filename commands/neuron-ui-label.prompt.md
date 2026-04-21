---
agent: agent
---

# AI-Assisted Neuron Label Component Integration Guide

## For the AI Assistant

Your task is to integrate and configure the Neuron Label components in a React application. This guide provides essential instructions for implementing Label components, which serve as form field labels and descriptive text across all Neuron applications.

## Sync Metadata

- **Component Version:** v5.0.2
- **Component Source:** `packages/neuron/ui/src/lib/helpers/label/Label.tsx`
- **Guideline Command:** `/neuron-ui-label`
- **Related Skill:** `neuron-ui-label`

## Introduction

The Label component provides standardized form labeling for your application. **Note**: This component is already integrated into all Neuron form fields and literals, so you typically won't need to use it directly for form inputs. Use this component for standalone labeling scenarios.

Key features include:

- **Semantic HTML labels** - Proper `<label>` elements with `htmlFor` attribute
- **Required/Optional indicators** - Visual indicators for field requirements
- **Tooltip integration** - Built-in tooltip support with icon and customizable placement
- **Internationalization** - Built-in i18n support for optional labels
- **Accessibility features** - Proper ARIA support and semantic meaning
- **Auto-hiding logic** - Component not rendered when no text is provided

### Key Features

- **Required Indicators**: Automatic "\*" display for required fields
- **Optional Indicators**: Automatic "(volitelné)" display for optional fields
- **Tooltip Integration**: Built-in tooltip with icon, customizable placement, width, and variant
- **Internationalization**: Built-in translation support for optional labels
- **Accessibility**: Proper label association with form controls
- **Smart Display**: Component not rendered when text is empty
- **Standalone Usage**: For cases where you need labels outside of form fields

**Important**: Label component is already integrated into all Neuron form fields and literals. Use this component only for standalone labeling scenarios.

**Storybook Reference:** You can refer to the Storybook documentation for visual examples and interactive demos of all available label configurations.

## Step 1: Basic Label Implementation

### 1.1 Import the Label Component

```tsx
import { Label } from "@neuron/ui";
```

### 1.2 When to Use Label Component

## 🎯 Figma Identification Rules

**CRITICAL RULE**: When you see a label positioned above a literal/value in Figma:

- **NEVER** use separate `Label` + `Literal` components
- **ALWAYS** use the integrated label functionality within the `Literal` component
- The `Label` component is standalone and should only be used for independent labels

**Examples:**

- ✅ `<Literal label="Jméno" value="Veronika" />`
- ❌ `<Label>Jméno</Label><Literal>Veronika</Literal>`

**Same rule applies to form components:**

- Input, TextArea, Select, etc. have integrated label props
- Use `<Input label="Field Name" />` instead of separate Label component

**Use Label component for:**

- Standalone labels not associated with form inputs
- Custom UI elements that need labeling
- Section headers or descriptive text with tooltip support
- Custom form controls not covered by Neuron form components
- Independent informational labels (NOT above literals/form fields)

**Don't use for:**

- Standard form inputs (Input, Select, Textarea) - they have integrated labels
- Neuron form fields - they already include Label functionality
- Labels positioned above Literal components - use Literal's label prop instead
- Labels positioned above any form component - use the component's label prop instead

### 1.3 Basic Usage Examples

```tsx
import { Label } from "@neuron/ui";

const StandaloneLabels = () => {
  return (
    <div className="label-examples">
      {/* Simple standalone label */}
      <Label text="Section Title" />

      {/* Label with tooltip for additional information */}
      <Label text="Configuration Settings" tooltipText="These settings affect the entire application behavior" />

      {/* Required indicator for custom controls */}
      <Label text="Custom Field" required={true} />

      {/* Optional indicator */}
      <Label text="Additional Info" optional={true} />
    </div>
  );
};
```

## Step 2: Tooltip Integration

### 2.1 Basic Tooltip Usage

The Label component includes built-in tooltip support with an info icon:

```tsx
import { Label } from "@neuron/ui";

const LabelsWithTooltips = () => {
  return (
    <div className="tooltip-labels">
      {/* Basic tooltip - shows info icon next to label */}
      <Label
        text="API Configuration"
        tooltipText="These settings control how the application connects to external APIs"
      />

      {/* Tooltip with custom placement */}
      <Label
        text="Advanced Settings"
        tooltipText="Modify these settings only if you understand their implications"
        tooltipPlacement="right"
      />

      {/* Tooltip with maximum width control */}
      <Label
        text="Description"
        tooltipText="Provide a comprehensive description that explains the purpose, functionality, and any important considerations for this configuration option."
        tooltipMaxWidth="300px"
      />
    </div>
  );
};
```

### 2.2 Tooltip Customization

Customize tooltip appearance and behavior:

```tsx
import { Label } from "@neuron/ui";

const CustomTooltipLabels = () => {
  return (
    <div className="custom-tooltip-labels">
      {/* Tooltip with custom variant */}
      <Label
        text="Warning Configuration"
        tooltipText="This setting may affect system performance"
        tooltipVariant="warning"
      />

      {/* Tooltip with different placements */}
      <Label text="Bottom Tooltip" tooltipText="Tooltip appears below the label" tooltipPlacement="bottom" />

      <Label text="Left Tooltip" tooltipText="Tooltip appears to the left" tooltipPlacement="left" />

      {/* React node as tooltip content */}
      <Label
        text="Rich Content"
        tooltipText={
          <span>
            Tooltip with <strong>formatted</strong> content
          </span>
        }
      />
    </div>
  );
};
```

### 2.3 Tooltip Placement Options

Available tooltip placement options:

| Placement  | Description                               |
| ---------- | ----------------------------------------- |
| `"top"`    | Tooltip appears above the label (default) |
| `"bottom"` | Tooltip appears below the label           |
| `"left"`   | Tooltip appears to the left of the label  |
| `"right"`  | Tooltip appears to the right of the label |

## Step 3: Required and Optional Indicators

### 3.1 Required Indicators

Use the `required` prop to display required field indicators:

```tsx
import { Label } from "@neuron/ui";

const RequiredLabels = () => {
  return (
    <div className="required-labels">
      {/* Required indicator with asterisk */}
      <Label text="Mandatory Field" required={true} />

      {/* Required with tooltip */}
      <Label
        text="Critical Setting"
        required={true}
        tooltipText="This setting is required for proper system operation"
      />
    </div>
  );
};
```

### 3.2 Optional Indicators

Use the `optional` prop to display optional field indicators:

```tsx
import { Label } from "@neuron/ui";

const OptionalLabels = () => {
  return (
    <div className="optional-labels">
      {/* Optional indicator with translated text */}
      <Label text="Additional Info" optional={true} />

      {/* Optional with description */}
      <Label
        text="Extra Configuration"
        optional={true}
        tooltipText="These settings are optional and can be configured later"
      />
    </div>
  );
};
```

**Note**: The optional indicator displays "(volitelné)" and uses i18n translation keys.

## Step 4: Label Props Reference

### 4.1 Core Label Props

| Prop       | Type      | Default | Description                                               |
| ---------- | --------- | ------- | --------------------------------------------------------- |
| `text`     | `string`  | -       | Text to be displayed as the label (not rendered if empty) |
| `htmlFor`  | `string`  | -       | Associates label with form control (for accessibility)    |
| `required` | `boolean` | `false` | Shows "\*" indicator for required fields                  |
| `optional` | `boolean` | `false` | Shows "(volitelné)" indicator for optional fields         |

### 4.2 Tooltip Props

| Prop               | Type                             | Default | Description                        |
| ------------------ | -------------------------------- | ------- | ---------------------------------- |
| `tooltipText`      | `ReactNode \| (() => ReactNode)` | -       | Content for the tooltip            |
| `tooltipPlacement` | `TTooltipPlacement`              | `"top"` | Tooltip position relative to label |
| `tooltipMaxWidth`  | `string`                         | -       | Maximum width of the tooltip       |
| `tooltipVariant`   | `TooltipProps["variant"]`        | -       | Visual variant of the tooltip      |

### 4.3 Tooltip Placement Options

| Placement  | Description                               |
| ---------- | ----------------------------------------- |
| `"top"`    | Tooltip appears above the label (default) |
| `"bottom"` | Tooltip appears below the label           |
| `"left"`   | Tooltip appears to the left of the label  |
| `"right"`  | Tooltip appears to the right of the label |

### 4.4 Tooltip Variants

| Variant     | Description                                 |
| ----------- | ------------------------------------------- |
| `"info"`    | Default informational tooltip               |
| `"warning"` | Warning-style tooltip (yellow/amber colors) |
| `"error"`   | Error-style tooltip (red colors)            |
| `"success"` | Success-style tooltip (green colors)        |

## Step 6: Best Practices

### 6.1 Label Text Guidelines

**Provide clear, concise label text:**

```tsx
{/* ✅ CORRECT: Clear and descriptive */}
<Label text="Database Connection Status" />
<Label text="Performance Configuration" />
<Label text="User Preferences" />

{/* ❌ WRONG: Vague or unclear text */}
<Label text="Info" />
<Label text="Data" />
<Label text="Settings" />
```

### 6.2 Tooltip Best Practices

**Provide helpful, contextual tooltip content:**

```tsx
{
  /* ✅ CORRECT: Helpful context */
}
<Label text="Cache Duration" tooltipText="How long data should be stored in cache before refresh (in minutes)" />;

{
  /* ❌ WRONG: Redundant tooltip */
}
<Label text="Cache Duration" tooltipText="Cache Duration" />;
```

### 6.3 When to Use Tooltips

**Use tooltips for:**

- Explaining complex concepts or technical terms
- Providing additional context that doesn't fit in the label
- Giving examples or format requirements
- Warning about potential impacts of settings

**Don't use tooltips for:**

- Repeating the label text
- Information that should be visible by default
- Critical information users must know before proceeding

### 6.4 Accessibility Considerations

**Remember accessibility best practices:**

```tsx
{/* ✅ CORRECT: Proper form association when needed */}
<Label text="Custom Control" htmlFor="customControl" />
<div id="customControl" role="slider" tabIndex={0}>Custom Control</div>

{/* ✅ CORRECT: Standalone informational label */}
<Label text="System Information" tooltipText="Current system metrics" />
```

## Step 5: Common Use Cases

### 5.1 Standalone Section Labels

Use labels for sections or groups that need tooltips:

```tsx
import { Label } from "@neuron/ui";

const SectionLabels = () => {
  return (
    <div className="section-labels">
      {/* Section header with explanation */}
      <Label text="Database Configuration" tooltipText="Configure your database connection settings here" />

      {/* Configuration group */}
      <Label
        text="API Settings"
        tooltipText="These settings control how the application communicates with external services"
        tooltipPlacement="right"
      />

      {/* Advanced settings section */}
      <Label
        text="Advanced Options"
        optional={true}
        tooltipText="These settings are for advanced users only. Modify with caution."
        tooltipVariant="warning"
      />
    </div>
  );
};
```

### 5.2 Custom Form Controls

When creating custom form controls not covered by Neuron components:

```tsx
import { Label } from "@neuron/ui";

const CustomControls = () => {
  return (
    <div className="custom-controls">
      {/* Custom control with associated label */}
      <Label
        text="Color Selection"
        htmlFor="colorPicker"
        required={true}
        tooltipText="Choose your preferred theme color"
      />
      <div id="colorPicker" className="custom-color-picker">
        {/* Custom color picker implementation */}
      </div>

      {/* Custom slider with label */}
      <Label
        text="Performance Level"
        htmlFor="performanceSlider"
        tooltipText="Higher values may impact battery life"
        tooltipMaxWidth="250px"
      />
      <input type="range" id="performanceSlider" min="1" max="10" className="custom-slider" />
    </div>
  );
};
```

### 5.3 Informational Labels

Use for displaying information that needs context:

```tsx
import { Label } from "@neuron/ui";

const InformationalLabels = () => {
  return (
    <div className="info-labels">
      {/* Status information */}
      <Label text="System Status" tooltipText="Current operational status of all system components" />

      {/* Metric with explanation */}
      <Label
        text="Response Time: 120ms"
        tooltipText="Average response time over the last 24 hours"
        tooltipPlacement="bottom"
      />

      {/* Configuration summary */}
      <Label text="Active Connections: 45" tooltipText="Number of currently active database connections" />
    </div>
  );
};
```

## Step 7: Common Mistakes to Avoid

### 7.1 Don't Use for Standard Form Fields

**Label component is already integrated in Neuron form components:**

```tsx
{/* ❌ WRONG: Don't duplicate labels for Neuron form fields */}
<Label text="Username" htmlFor="username" />
<Input id="username" label="Username" />

{/* ✅ CORRECT: Use Input's built-in label */}
<Input id="username" label="Username" tooltipText="Your unique identifier" />
```

### 7.2 Don't Combine Required and Optional

**Don't use conflicting indicators:**

```tsx
{/* ❌ WRONG: Conflicting indicators */}
<Label text="Field" required={true} optional={true} />

{/* ✅ CORRECT: Use one or neither */}
<Label text="Required Field" required={true} />
<Label text="Optional Field" optional={true} />
<Label text="Standard Field" />
```

### 7.3 Don't Create Redundant Tooltips

**Avoid tooltips that just repeat the label text:**

```tsx
{
  /* ❌ WRONG: Redundant tooltip */
}
<Label text="Performance Settings" tooltipText="Performance Settings" />;

{
  /* ✅ CORRECT: Helpful additional context */
}
<Label text="Performance Settings" tooltipText="Adjust CPU and memory usage limits for optimal performance" />;
```

### 7.4 Don't Use Empty Text

**Component won't render with empty text:**

```tsx
{/* ❌ WRONG: Will not render anything */}
<Label text="" />
<Label />

{/* ✅ CORRECT: Provide meaningful text */}
<Label text="Configuration Status" />
```

### 7.5 Don't Ignore Tooltip Placement

**Consider layout and space when positioning tooltips:**

```tsx
{
  /* ❌ WRONG: Default placement might be cut off */
}
<div className="bottom-section">
  <Label text="Bottom Label" tooltipText="This tooltip might be cut off" />
</div>;

{
  /* ✅ CORRECT: Appropriate placement for layout */
}
<div className="bottom-section">
  <Label text="Bottom Label" tooltipText="This tooltip is positioned correctly" tooltipPlacement="top" />
</div>;
```

## Key Takeaways

The Neuron Label component provides standalone labeling functionality with built-in tooltip support. Key points to remember:

1. **Label is already integrated** in all Neuron form components - use this component only for standalone scenarios
2. **Tooltip integration** includes automatic info icon display when `tooltipText` is provided
3. **Tooltip customization** supports placement, width, variant, and content customization
4. **Required/Optional indicators** use `required` and `optional` props with automatic translation
5. **Component auto-hides** when no text is provided
6. **Use for standalone labeling** like section headers, informational text, or custom controls
7. **Provide meaningful tooltips** that add value beyond just repeating the label text
8. **Consider tooltip placement** based on your layout and available space
9. **Don't duplicate functionality** that's already built into Neuron form components
10. **Follow accessibility practices** when associating with custom controls

By following these guidelines, you'll effectively use the Label component for standalone scenarios while avoiding duplication with existing Neuron form component functionality.

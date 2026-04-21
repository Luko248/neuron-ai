---
agent: agent
---

# AI-Assisted Neuron Chip Component Integration Guide

## For the AI Assistant

Your task is to integrate and configure the Neuron Chip component in React applications. This guide provides comprehensive instructions for implementing Chip components, which serve as compact UI elements for displaying tags, status indicators, and removable selections.

## 🚨 CRITICAL: Product Variant Selection from Figma

**MANDATORY RULE: When you see product color tokens in Figma designs, you MUST use the corresponding product variant. Custom styling is FORBIDDEN.**

### Figma Design → Product Variant Mapping

When analyzing Figma designs, if you see these **product color tokens**, you MUST use the corresponding **product variant**:

| Figma Product Color Token         | Required Product Variant    | Implementation                                        |
| --------------------------------- | --------------------------- | ----------------------------------------------------- |
| `--colorProductsBusiness*` colors | `productVariant="business"` | `<Chip label="Business" productVariant="business" />` |
| `--colorProductsLife*` colors     | `productVariant="life"`     | `<Chip label="Life" productVariant="life" />`         |
| `--colorProductsProperty*` colors | `productVariant="property"` | `<Chip label="Property" productVariant="property" />` |
| `--colorProductsTravel*` colors   | `productVariant="travel"`   | `<Chip label="Travel" productVariant="travel" />`     |
| `--colorProductsVehicles*` colors | `productVariant="vehicles"` | `<Chip label="Vehicles" productVariant="vehicles" />` |

### ❌ FORBIDDEN: Custom Styling

**NEVER do this when you see product colors in Figma:**

```tsx
// ❌ WRONG - Custom styling is forbidden
<Chip
  label="Business"
  style={{ backgroundColor: '#1a73e8', color: 'white' }}
/>

// ❌ WRONG - Custom CSS classes for product colors
<Chip
  label="Life"
  className="custom-life-styling"
/>

// ❌ WRONG - Inline styles for product theming
<Chip
  label="Property"
  style={{ backgroundColor: 'var(--colorProductsProperty600)' }}
/>
```

### ✅ CORRECT: Product Variant Usage

**Always do this when you see product colors in Figma:**

```tsx
// ✅ CORRECT - Use product variants
<Chip label="Business" productVariant="business" />
<Chip label="Life" productVariant="life" />
<Chip label="Property" productVariant="property" />
<Chip label="Travel" productVariant="travel" />
<Chip label="Vehicles" productVariant="vehicles" />
```

### Design Analysis Checklist

When analyzing Figma designs for Chip components:

1. **Look for color tokens**: Check if the chip uses any product-specific color tokens
2. **Identify the product**: Determine which product line the colors represent
3. **Use corresponding variant**: Apply the correct `productVariant` prop
4. **Never use custom styling**: Even if the design looks slightly different, use the variant
5. **Report discrepancies**: If the design doesn't match available variants, report to design team

**Remember**: Product variants ensure consistent theming across the entire Neuron project and maintain design system integrity.

## Sync Metadata

- **Component Version:** v3.2.2
- **Component Source:** `packages/neuron/ui/src/lib/misc/chip/Chip.tsx`
- **Guideline Command:** `/neuron-ui-chip`
- **Related Skill:** `neuron-ui-misc`

## Introduction

The Chip component is a versatile UI element designed to display concise information in a compact, pill-shaped container. It serves as an essential interface element for tags, status indicators, and interactive selections within Neuron applications.

### What is the Chip Component?

The Chip component provides standardized compact information display with support for:

- **Text labels** - Primary content display with truncation support
- **Optional icons** - Visual enhancement from baseIcons
- **Removable functionality** - Interactive removal with onRemove callback
- **Status variants** - info, success, warning, danger styling
- **Product variants** - business, life, property, travel, vehicles theming
- **Size options** - small and large (default) sizes
- **Loading states** - Built-in loading indicator support
- **Accessibility** - Full keyboard navigation and screen reader support

### Key Features

- **Compact Information Display**: Efficient space usage for tags and labels
- **Interactive Removal**: Optional removable functionality with callbacks
- **Status Communication**: Semantic variants for different message types
- **Product Theming**: Specialized styling for different product lines
- **Icon Integration**: Support for baseIcons to enhance visual context
- **Text Truncation**: Automatic text truncation with tooltip support
- **Loading States**: Built-in loading indicators for async operations
- **Accessibility Compliant**: Full keyboard and screen reader support

**Storybook Reference:** You can refer to the Storybook documentation for visual examples and interactive demos of the Chip component.

## Step 1: Basic Chip Implementation

### 1.1 Import the Chip Component

```tsx
import { Chip } from "@neuron/ui";
```

### 1.2 Basic Chip Usage

Here's a simple implementation of the Chip component:

```tsx
import { Chip } from "@neuron/ui";

const BasicChipExample = () => {
  return (
    <div>
      {/* Basic chip with label */}
      <Chip label="Basic Tag" />

      {/* Chip with click handler */}
      <Chip label="Clickable" onClick={(event) => console.log("Chip clicked")} />

      {/* Removable chip */}
      <Chip label="Removable" removable onRemove={(event) => console.log("Chip removed")} />
    </div>
  );
};
```

### 1.3 Chip with Icon

Add icons to enhance visual context:

```tsx
import { Chip, baseIcons } from "@neuron/ui";

const IconChipExample = () => {
  return (
    <div>
      {/* Chip with icon */}
      <Chip label="With Icon" icon={{ iconDef: baseIcons.tagSolid }} />

      {/* Chip without icon display */}
      <Chip label="No Icon" icon={{ iconDef: baseIcons.tagSolid }} showIcon={false} />
    </div>
  );
};
```

## Step 2: Chip Variants and Styling

### 2.1 Status Variants

Use semantic variants to communicate different types of information:

```tsx
import { Chip } from "@neuron/ui";

const StatusVariantsExample = () => {
  return (
    <div>
      {/* Default chip */}
      <Chip label="Default" />

      {/* Info variant */}
      <Chip label="Information" variant="info" />

      {/* Success variant */}
      <Chip label="Success" variant="success" />

      {/* Warning variant */}
      <Chip label="Warning" variant="warning" />

      {/* Danger variant */}
      <Chip label="Error" variant="danger" />
    </div>
  );
};
```

### 2.2 Product Variants

**🚨 MANDATORY: If you see product color tokens in Figma designs (--colorProductsBusiness*, --colorProductsLife*, --colorProductsProperty*, --colorProductsTravel*, --colorProductsVehicles\*), you MUST use the corresponding product variant. Custom styling is forbidden in the Neuron project.**

Use product variants for product-specific theming:

```tsx
import { Chip } from "@neuron/ui";

const ProductVariantsExample = () => {
  return (
    <div>
      {/* Business product variant */}
      <Chip label="Business" productVariant="business" />

      {/* Life insurance variant */}
      <Chip label="Life" productVariant="life" />

      {/* Property insurance variant */}
      <Chip label="Property" productVariant="property" />

      {/* Travel insurance variant */}
      <Chip label="Travel" productVariant="travel" />

      {/* Vehicle insurance variant */}
      <Chip label="Vehicles" productVariant="vehicles" />
    </div>
  );
};
```

### 2.3 Size Options

Configure chip sizes based on layout requirements:

```tsx
import { Chip, baseIcons } from "@neuron/ui";

const SizeOptionsExample = () => {
  return (
    <div>
      {/* Large size (default) */}
      <Chip label="Large Chip" size="large" icon={{ iconDef: baseIcons.tagSolid }} />

      {/* Small size */}
      <Chip label="Small Chip" size="small" icon={{ iconDef: baseIcons.tagSolid }} />
    </div>
  );
};
```

## Step 2.4: Identifying Product Colors in Figma Designs

**🔍 How to Identify Product Colors in Figma:**

When analyzing Figma designs, look for these indicators that require product variants:

### Visual Color Identification

| Product      | Color Indicators                   | Required Variant            |
| ------------ | ---------------------------------- | --------------------------- |
| **Business** | Blue tones, corporate colors       | `productVariant="business"` |
| **Life**     | Green tones, life insurance colors | `productVariant="life"`     |
| **Property** | Orange/red tones, property colors  | `productVariant="property"` |
| **Travel**   | Purple/violet tones, travel colors | `productVariant="travel"`   |
| **Vehicles** | Teal/cyan tones, vehicle colors    | `productVariant="vehicles"` |

### Token Name Patterns

If you can inspect the design tokens in Figma, look for:

- `--colorProductsBusiness200`, `--colorProductsBusiness300`, `--colorProductsBusiness600`
- `--colorProductsLife200`, `--colorProductsLife300`, `--colorProductsLife600`
- `--colorProductsProperty200`, `--colorProductsProperty300`, `--colorProductsProperty600`
- `--colorProductsTravel200`, `--colorProductsTravel300`, `--colorProductsTravel600`
- `--colorProductsVehicles200`, `--colorProductsVehicles300`, `--colorProductsVehicles600`

### ⚠️ Important Rules

1. **Never guess**: If you see product-specific colors, use the corresponding variant
2. **No custom styling**: Don't try to replicate the color with custom CSS
3. **When in doubt**: Use the product variant that matches the color family
4. **Report mismatches**: If design doesn't match available variants, report to design team

```tsx
// ✅ CORRECT - When you see business blue colors in Figma
<Chip label="Business Insurance" productVariant="business" />

// ✅ CORRECT - When you see life green colors in Figma
<Chip label="Life Coverage" productVariant="life" />

// ❌ WRONG - Never use custom colors even if they match design
<Chip label="Business" style={{ backgroundColor: '#1a73e8' }} />
```

## Step 3: Interactive Chip Functionality

### 3.1 Removable Chips

Implement removable chips for dynamic content management:

```tsx
import { Chip } from "@neuron/ui";
import { useState } from "react";

const RemovableChipsExample = () => {
  const [tags, setTags] = useState([
    { id: 1, label: "React" },
    { id: 2, label: "TypeScript" },
    { id: 3, label: "Neuron UI" },
  ]);

  const handleRemoveTag = (tagId: number) => {
    setTags(tags.filter((tag) => tag.id !== tagId));
  };

  return (
    <div>
      <h3>Selected Technologies</h3>
      <div>
        {tags.map((tag) => (
          <Chip key={tag.id} label={tag.label} removable onRemove={() => handleRemoveTag(tag.id)} />
        ))}
      </div>
    </div>
  );
};
```

### 3.2 Clickable Chips

Implement clickable chips for selection and navigation:

```tsx
import { Chip } from "@neuron/ui";
import { useState } from "react";

const ClickableChipsExample = () => {
  const [selectedOption, setSelectedOption] = useState<string | null>(null);

  const options = ["All", "Active", "Completed", "Pending"];

  const handleOptionClick = (option: string) => {
    setSelectedOption(option === selectedOption ? null : option);
  };

  return (
    <div>
      <h3>Selection Options</h3>
      <div>
        {options.map((option) => (
          <Chip
            key={option}
            label={option}
            variant={selectedOption === option ? "info" : undefined}
            onClick={() => handleOptionClick(option)}
          />
        ))}
      </div>
    </div>
  );
};
```

### 3.3 Loading State Chips

Display loading states for async operations:

```tsx
import { Chip } from "@neuron/ui";
import { useState } from "react";

const LoadingChipsExample = () => {
  const [isLoading, setIsLoading] = useState(false);

  const handleAsyncAction = async () => {
    setIsLoading(true);
    // Simulate async operation
    await new Promise((resolve) => setTimeout(resolve, 2000));
    setIsLoading(false);
  };

  return (
    <div>
      <h3>Async Operation</h3>
      <Chip label="Process Data" isLoading={isLoading} onClick={handleAsyncAction} disabled={isLoading} />
    </div>
  );
};
```

## Step 4: Text Guidelines and Best Practices

### 4.1 Optimal Text Content

Follow these guidelines for chip text content:

**Clarity and Directness:**

- Text should immediately communicate the information or value
- Use clear, unambiguous language

**Brevity:**

- Limit text to 1-3 words (approximately 20 characters maximum)
- Chip width depends on label length
- Text wrapping is not allowed

**Visual Description:**

- Optional icons can be used if they add visual context to the text
- Icons should enhance, not replace, clear text

**Consistency:**

- Use consistent terminology and style across the application
- Colors and icons for message and product chips are predefined

### 4.2 Text Examples

```tsx
import { Chip, baseIcons } from "@neuron/ui";

const TextExamplesExample = () => {
  return (
    <div>
      {/* Good examples - concise and clear */}
      <Chip label="Azure" variant="info" />
      <Chip label="Life" productVariant="life" />
      <Chip label="Travel" productVariant="travel" />
      <Chip label="Success" variant="success" />
      <Chip label="Selected" variant="info" />
      <Chip label="Active" variant="success" />

      {/* Examples with meaningful icons */}
      <Chip label="Livestock" icon={{ iconDef: baseIcons.shieldSolid }} productVariant="property" />

      {/* Avoid - too long or redundant */}
      {/* <Chip label="Azure Color" /> - redundant */}
      {/* <Chip label="Life Insurance Policy" /> - too long */}
      {/* <Chip label="Travel Insurance for International Trips" /> - way too long */}
    </div>
  );
};
```

### 4.3 Text Truncation

Handle long text with truncation and tooltips:

```tsx
import { Chip } from "@neuron/ui";

const TextTruncationExample = () => {
  const longText = "Very long text that should be truncated after 160px text width and on mobile after 80px text width";

  return (
    <div>
      <h3>Text Truncation</h3>
      <Chip
        label={longText}
        textTruncate={true}
        // Tooltip will automatically show full text when truncated
      />
    </div>
  );
};
```

## Step 5: Common Use Cases and Patterns

### 5.1 Tag Management System

Implement a complete tag management interface:

```tsx
import { Chip, baseIcons } from "@neuron/ui";
import { useState } from "react";

const TagManagementExample = () => {
  const [availableTags] = useState([
    { id: 1, label: "React", category: "frontend" },
    { id: 2, label: "Node.js", category: "backend" },
    { id: 3, label: "TypeScript", category: "language" },
    { id: 4, label: "Database", category: "storage" },
  ]);

  const [selectedTags, setSelectedTags] = useState<number[]>([]);

  const handleTagSelect = (tagId: number) => {
    if (selectedTags.includes(tagId)) {
      setSelectedTags(selectedTags.filter((id) => id !== tagId));
    } else {
      setSelectedTags([...selectedTags, tagId]);
    }
  };

  const handleTagRemove = (tagId: number) => {
    setSelectedTags(selectedTags.filter((id) => id !== tagId));
  };

  return (
    <div>
      <h3>Available Tags</h3>
      <div>
        {availableTags.map((tag) => (
          <Chip
            key={tag.id}
            label={tag.label}
            variant={selectedTags.includes(tag.id) ? "info" : undefined}
            onClick={() => handleTagSelect(tag.id)}
          />
        ))}
      </div>

      <h3>Selected Tags</h3>
      <div>
        {selectedTags.map((tagId) => {
          const tag = availableTags.find((t) => t.id === tagId);
          return tag ? (
            <Chip key={tag.id} label={tag.label} variant="info" removable onRemove={() => handleTagRemove(tag.id)} />
          ) : null;
        })}
      </div>
    </div>
  );
};
```

### 5.2 Status Indicators

Use chips to display various status information:

```tsx
import { Chip } from "@neuron/ui";

const StatusIndicatorsExample = () => {
  const orders = [
    { id: 1, name: "Order #1001", status: "pending" },
    { id: 2, name: "Order #1002", status: "processing" },
    { id: 3, name: "Order #1003", status: "completed" },
    { id: 4, name: "Order #1004", status: "cancelled" },
  ];

  const getStatusVariant = (status: string) => {
    switch (status) {
      case "completed":
        return "success";
      case "processing":
        return "info";
      case "pending":
        return "warning";
      case "cancelled":
        return "danger";
      default:
        return undefined;
    }
  };

  const getStatusLabel = (status: string) => {
    switch (status) {
      case "pending":
        return "Pending";
      case "processing":
        return "Processing";
      case "completed":
        return "Completed";
      case "cancelled":
        return "Cancelled";
      default:
        return status;
    }
  };

  return (
    <div>
      <h3>Order Status</h3>
      {orders.map((order) => (
        <div key={order.id}>
          <span>{order.name}: </span>
          <Chip label={getStatusLabel(order.status)} variant={getStatusVariant(order.status)} />
        </div>
      ))}
    </div>
  );
};
```

## Step 6: Accessibility and States

### 6.1 Disabled and ReadOnly States

Handle different interaction states properly:

```tsx
import { Chip, baseIcons } from "@neuron/ui";

const StatesExample = () => {
  return (
    <div>
      <h3>Chip States</h3>

      {/* Normal interactive chip */}
      <Chip label="Interactive" removable onRemove={() => console.log("Removed")} />

      {/* Disabled chip */}
      <Chip label="Disabled" disabled removable onRemove={() => console.log("This won't fire")} />

      {/* ReadOnly chip */}
      <Chip label="ReadOnly" readOnly removable onRemove={() => console.log("This won't fire")} />

      {/* Loading chip */}
      <Chip label="Loading" isLoading icon={{ iconDef: baseIcons.spinnerSolid }} />
    </div>
  );
};
```

### 6.2 Accessibility Features

The Chip component includes built-in accessibility features:

- **ARIA Labels**: Proper labeling for screen readers
- **Keyboard Navigation**: Full keyboard support for interactive elements
- **Focus Management**: Proper focus indicators and management
- **State Communication**: Clear communication of disabled/readonly states

```tsx
import { Chip } from "@neuron/ui";

const AccessibilityExample = () => {
  return (
    <div>
      <h3>Accessible Chips</h3>

      {/* Chip with custom test ID for testing */}
      <Chip label="Testable" testId="custom-chip-test-id" removable onRemove={() => console.log("Removed")} />

      {/* Chip with proper ARIA attributes */}
      <Chip
        label="Accessible"
        disabled
        // aria-disabled is automatically set
      />
    </div>
  );
};
```

## Step 7: Integration Patterns

### 7.1 Form Integration

Integrate chips with form controls:

```tsx
import { Chip } from "@neuron/ui";
import { useState } from "react";

const FormIntegrationExample = () => {
  const [selectedSkills, setSelectedSkills] = useState<string[]>([]);
  const availableSkills = ["React", "TypeScript", "Node.js", "Python", "Java"];

  const handleSkillAdd = (skill: string) => {
    if (!selectedSkills.includes(skill)) {
      setSelectedSkills([...selectedSkills, skill]);
    }
  };

  const handleSkillRemove = (skill: string) => {
    setSelectedSkills(selectedSkills.filter((s) => s !== skill));
  };

  return (
    <div>
      <h3>Select Your Skills</h3>

      <div>
        <h4>Available Skills</h4>
        <div>
          {availableSkills
            .filter((skill) => !selectedSkills.includes(skill))
            .map((skill) => (
              <Chip key={skill} label={skill} onClick={() => handleSkillAdd(skill)} />
            ))}
        </div>
      </div>

      <div>
        <h4>Selected Skills</h4>
        <div>
          {selectedSkills.map((skill) => (
            <Chip key={skill} label={skill} variant="info" removable onRemove={() => handleSkillRemove(skill)} />
          ))}
        </div>
      </div>
    </div>
  );
};
```

## Summary

The Neuron Chip component provides versatile compact information display with support for:

- **Text Labels**: Clear, concise content with truncation support
- **Interactive Functionality**: Clickable and removable capabilities
- **Status Variants**: Semantic styling for different message types
- **Product Variants**: Specialized theming for product lines
- **Icon Integration**: baseIcons support for visual enhancement
- **Size Options**: Small and large size configurations
- **Loading States**: Built-in loading indicators
- **Accessibility**: Full keyboard navigation and screen reader support

Use chips strategically for tags, status indicators, and compact information display. Follow text guidelines for clarity and brevity, and leverage the various variants and states to create intuitive user interfaces in your Neuron applications.

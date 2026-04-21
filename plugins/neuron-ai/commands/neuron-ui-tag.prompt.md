---
agent: agent
---

# AI-Assisted Neuron Tag Component Integration Guide

## For the AI Assistant

Your task is to integrate and configure the Neuron Tag components in a React application. This guide provides comprehensive instructions for implementing the Tag component, which serves as a visual indicator for application states, statuses, and metadata across all Neuron applications.

## Sync Metadata

- **Component Version:** v4.0.1
- **Component Source:** `packages/neuron/ui/src/lib/misc/tag/Tag.tsx`
- **Guideline Command:** `/neuron-ui-tag`
- **Related Skill:** `neuron-ui-misc`

## Introduction

The Tag system is a versatile display component of the Neuron UI framework, designed to create consistent, accessible, and informative status indicators that communicate application states and metadata across all Neuron applications.

### What is the Tag System?

The Tag component provides a standardized status indicator interface for your application with support for:

- Multiple semantic variants (default, info, success, warning, danger)
- Extended color variants for specialized use cases
- Icon integration with baseIcons system
- Text truncation with automatic tooltip fallback
- State indicators (disabled, line-through, uppercase)
- Tooltip integration for additional context
- Historical state representation
- Accessibility features and ARIA support

### Key Features

- **Semantic Variants**: Standard variants for common application states (info, success, warning, danger)
- **Extended Color Palette**: Specialized color variants for custom categorization
- **Icon Integration**: Support for baseIcons with proper sizing and alignment
- **Text Truncation**: Automatic text truncation with ellipsis and tooltip fallback
- **State Indicators**: Visual states for disabled, historical, and emphasized content
- **Tooltip Support**: Built-in tooltip functionality with configurable placement
- **Historical States**: Support for disabled and line-through states for historical data
- **Accessibility**: Proper ARIA attributes and semantic meaning
- **TypeScript Support**: Full type safety with comprehensive prop interfaces

**Storybook Reference:** You can refer to the Storybook documentation for visual examples and interactive demos of the Tag component.

### Figma Code Connect Integration

## Step 1: Basic Tag Implementation

### 1.1 Import the Tag Component

```tsx
import { Tag } from "@neuron/ui";
```

### 1.2 Basic Usage

Here's a simple implementation of the Tag component:

```tsx
import { Tag } from "@neuron/ui";

const MyComponent = () => {
  return (
    <div className="tag-examples">
      {/* Basic tag */}
      <Tag text="Default Tag" />

      {/* Tag with variant */}
      <Tag text="Success" variant="success" />

      {/* Tag with tooltip */}
      <Tag text="Info Tag" variant="info" tooltipText="Additional information about this tag" />
    </div>
  );
};
```

### 1.3 Semantic Variants

Use semantic variants to communicate application states:

```tsx
import { Tag } from "@neuron/ui";

const SemanticTags = () => {
  return (
    <div className="semantic-tags">
      {/* Default state */}
      <Tag text="Default" variant="default" />

      {/* Informational state */}
      <Tag text="Information" variant="info" />

      {/* Success state */}
      <Tag text="Success" variant="success" />

      {/* Warning state */}
      <Tag text="Warning" variant="warning" />

      {/* Error/danger state */}
      <Tag text="Error" variant="danger" />
    </div>
  );
};
```

## Step 2: Extended Color Variants

### 2.1 Specialized Color Variants

Use extended color variants for custom categorization:

```tsx
import { Tag } from "@neuron/ui";

const ColorVariantTags = () => {
  return (
    <div className="color-variant-tags">
      {/* Green variants */}
      <Tag text="Apple Green" variant="appleGreen" />
      <Tag text="Emerald Green" variant="emeraldGreen" />

      {/* Blue variants */}
      <Tag text="Celestial Blue" variant="celestialBlue" />
      <Tag text="Majorelle Blue" variant="majorelleBlue" />

      {/* Purple variant */}
      <Tag text="Electric Purple" variant="electricPurple" />

      {/* Red variant */}
      <Tag text="Indian Red" variant="indianRed" />

      {/* Brown variant */}
      <Tag text="Sandy Brown" variant="sendyBrown" />

      {/* Yellow variant */}
      <Tag text="Citrine" variant="citrine" />
    </div>
  );
};
```

### 2.2 Category-Based Usage

Use color variants for consistent categorization:

```tsx
import { Tag } from "@neuron/ui";

const CategoryTags = ({ item }) => {
  const getCategoryVariant = (category) => {
    const categoryMap = {
      environment: "emeraldGreen",
      performance: "celestialBlue",
      security: "indianRed",
      feature: "electricPurple",
      bugfix: "citrine",
      documentation: "majorelleBlue",
    };
    return categoryMap[category] || "default";
  };

  return (
    <div className="category-tags">
      {item.categories.map((category) => (
        <Tag
          key={category}
          text={category}
          variant={getCategoryVariant(category)}
          tooltipText={`Category: ${category}`}
        />
      ))}
    </div>
  );
};
```

## Step 3: Icon Integration

### 3.1 Tags with Icons

Add icons to enhance tag meaning:

```tsx
import { Tag, baseIcons } from "@neuron/ui";

const IconTags = () => {
  return (
    <div className="icon-tags">
      {/* Success with check icon */}
      <Tag text="Completed" variant="success" icon={baseIcons.checkCircleSolid} />

      {/* Warning with alert icon */}
      <Tag text="Attention" variant="warning" icon={baseIcons.exclamationTriangleSolid} />

      {/* Info with info icon */}
      <Tag text="Information" variant="info" icon={baseIcons.circleInfoSolid} />

      {/* Error with error icon */}
      <Tag text="Failed" variant="danger" icon={baseIcons.circleXmarkSolid} />
    </div>
  );
};
```

### 3.2 Status Tags with Icons

Create status indicators with meaningful icons:

```tsx
import { Tag, baseIcons } from "@neuron/ui";

const StatusTags = ({ status }) => {
  const getStatusConfig = (status) => {
    const statusMap = {
      active: {
        text: "Active",
        variant: "success",
        icon: baseIcons.playCircleSolid,
      },
      pending: {
        text: "Pending",
        variant: "warning",
        icon: baseIcons.clockSolid,
      },
      inactive: {
        text: "Inactive",
        variant: "default",
        icon: baseIcons.pauseCircleSolid,
      },
      error: {
        text: "Error",
        variant: "danger",
        icon: baseIcons.circleXmarkSolid,
      },
    };
    return statusMap[status] || statusMap["inactive"];
  };

  const config = getStatusConfig(status);

  return <Tag text={config.text} variant={config.variant} icon={config.icon} tooltipText={`Status: ${config.text}`} />;
};
```

## Step 4: Text Truncation and Tooltips

### 4.1 Text Truncation

**⚠️ CRITICAL: Always provide tooltip when using text truncation**

```tsx
import { Tag } from "@neuron/ui";

const TruncatedTags = () => {
  const longText = "This is a very long tag text that will be truncated";

  return (
    <div className="truncated-tags">
      {/* Truncated tag with tooltip */}
      <Tag
        text={longText}
        textTruncate={true}
        tooltipText={longText} // Always provide full text in tooltip
        variant="info"
      />

      {/* Non-truncated tag for comparison */}
      <Tag text="Short text" variant="info" />
    </div>
  );
};
```

### 4.2 Dynamic Truncation

Implement dynamic truncation based on content length:

```tsx
import { Tag } from "@neuron/ui";

const DynamicTruncationTags = ({ items }) => {
  const shouldTruncate = (text) => text.length > 20;

  return (
    <div className="dynamic-truncation-tags">
      {items.map((item, index) => (
        <Tag
          key={index}
          text={item.name}
          textTruncate={shouldTruncate(item.name)}
          tooltipText={shouldTruncate(item.name) ? item.name : undefined}
          variant={item.variant}
        />
      ))}
    </div>
  );
};
```

### 4.3 Tooltip Placement

Configure tooltip placement based on layout:

```tsx
import { Tag } from "@neuron/ui";

const TooltipPlacementTags = () => {
  return (
    <div className="tooltip-placement-tags">
      {/* Top placement for bottom-positioned tags */}
      <Tag text="Bottom Tag" tooltipText="This tooltip appears above" tooltipPlacement="top" variant="info" />

      {/* Left placement for right-aligned tags */}
      <Tag text="Right Tag" tooltipText="This tooltip appears to the left" tooltipPlacement="left" variant="success" />

      {/* Auto placement (default) */}
      <Tag text="Auto Tag" tooltipText="This tooltip appears automatically" tooltipPlacement="auto" variant="warning" />
    </div>
  );
};
```

## Step 5: State Indicators

### 5.1 Disabled State

Use disabled state for historical or inactive tags:

```tsx
import { Tag } from "@neuron/ui";

const DisabledTags = ({ historicalData }) => {
  return (
    <div className="disabled-tags">
      {/* Current active tag */}
      <Tag text="Current Status" variant="success" />

      {/* Historical/disabled tag */}
      <Tag text="Previous Status" variant="warning" disabled={true} tooltipText="This status is no longer active" />

      {/* Conditional disabled state */}
      {historicalData.map((item) => (
        <Tag
          key={item.id}
          text={item.status}
          variant={item.variant}
          disabled={!item.isActive}
          tooltipText={item.isActive ? undefined : "Historical status"}
        />
      ))}
    </div>
  );
};
```

### 5.2 Line-Through State

Use line-through for deprecated or cancelled states:

```tsx
import { Tag } from "@neuron/ui";

const LineThroughTags = ({ tasks }) => {
  return (
    <div className="line-through-tags">
      {tasks.map((task) => (
        <Tag
          key={task.id}
          text={task.name}
          variant={task.completed ? "success" : "default"}
          lineThrough={task.cancelled}
          disabled={task.cancelled}
          tooltipText={
            task.cancelled ? "This task was cancelled" : task.completed ? "Task completed" : "Task in progress"
          }
        />
      ))}
    </div>
  );
};
```

### 5.3 Uppercase State

Use uppercase for emphasis or formal contexts:

```tsx
import { Tag } from "@neuron/ui";

const UppercaseTags = () => {
  return (
    <div className="uppercase-tags">
      {/* Priority tags with uppercase */}
      <Tag text="high priority" variant="danger" uppercase={true} />
      <Tag text="medium priority" variant="warning" uppercase={true} />
      <Tag text="low priority" variant="info" uppercase={true} />

      {/* Status tags with uppercase */}
      <Tag text="approved" variant="success" uppercase={true} />
      <Tag text="pending" variant="warning" uppercase={true} />
      <Tag text="rejected" variant="danger" uppercase={true} />
    </div>
  );
};
```

## Step 6: Real-World Implementation Patterns

### 6.1 User Role Tags

```tsx
import { Tag, baseIcons } from "@neuron/ui";

const UserRoleTags = ({ user }) => {
  const getRoleConfig = (role) => {
    const roleMap = {
      admin: {
        variant: "danger",
        icon: baseIcons.shieldSolid,
        tooltip: "Administrator - Full system access",
      },
      moderator: {
        variant: "warning",
        icon: baseIcons.userShieldSolid,
        tooltip: "Moderator - Content management access",
      },
      user: {
        variant: "info",
        icon: baseIcons.userSolid,
        tooltip: "Standard user access",
      },
      guest: {
        variant: "default",
        icon: baseIcons.userGroupSolid,
        tooltip: "Guest - Limited access",
      },
    };
    return roleMap[role] || roleMap["guest"];
  };

  const config = getRoleConfig(user.role);

  return (
    <Tag
      text={user.role}
      variant={config.variant}
      icon={config.icon}
      tooltipText={config.tooltip}
      uppercase={true}
      disabled={!user.isActive}
    />
  );
};
```

### 6.2 Project Status Tags

```tsx
import { Tag, baseIcons } from "@neuron/ui";

const ProjectStatusTags = ({ project }) => {
  const getStatusConfig = (status, isArchived) => {
    if (isArchived) {
      return {
        variant: "default",
        icon: baseIcons.archiveBoxSolid,
        disabled: true,
        lineThrough: true,
        tooltip: "This project has been archived",
      };
    }

    const statusMap = {
      planning: {
        variant: "info",
        icon: baseIcons.lightbulbSolid,
        tooltip: "Project in planning phase",
      },
      active: {
        variant: "success",
        icon: baseIcons.playCircleSolid,
        tooltip: "Project is actively being developed",
      },
      "on-hold": {
        variant: "warning",
        icon: baseIcons.pauseCircleSolid,
        tooltip: "Project development is paused",
      },
      completed: {
        variant: "emeraldGreen",
        icon: baseIcons.checkCircleSolid,
        tooltip: "Project has been completed",
      },
      cancelled: {
        variant: "danger",
        icon: baseIcons.circleXmarkSolid,
        lineThrough: true,
        tooltip: "Project has been cancelled",
      },
    };
    return statusMap[status] || statusMap["planning"];
  };

  const config = getStatusConfig(project.status, project.isArchived);

  return (
    <Tag
      text={project.status}
      variant={config.variant}
      icon={config.icon}
      tooltipText={config.tooltip}
      disabled={config.disabled}
      lineThrough={config.lineThrough}
      uppercase={true}
    />
  );
};
```

### 6.3 Feature Flag Tags

```tsx
import { Tag, baseIcons } from "@neuron/ui";

const FeatureFlagTags = ({ feature }) => {
  const getFeatureConfig = (isEnabled, environment) => {
    if (!isEnabled) {
      return {
        text: "Disabled",
        variant: "default",
        icon: baseIcons.toggleOffSolid,
        disabled: true,
        tooltip: "Feature is currently disabled",
      };
    }

    const envMap = {
      production: {
        text: "Live",
        variant: "success",
        icon: baseIcons.toggleOnSolid,
        tooltip: "Feature is live in production",
      },
      staging: {
        text: "Staging",
        variant: "warning",
        icon: baseIcons.toggleOnSolid,
        tooltip: "Feature is enabled in staging",
      },
      development: {
        text: "Dev",
        variant: "info",
        icon: baseIcons.toggleOnSolid,
        tooltip: "Feature is enabled in development",
      },
    };
    return envMap[environment] || envMap["development"];
  };

  const config = getFeatureConfig(feature.isEnabled, feature.environment);

  return (
    <Tag
      text={config.text}
      variant={config.variant}
      icon={config.icon}
      tooltipText={config.tooltip}
      disabled={config.disabled}
      uppercase={true}
    />
  );
};
```

### 6.4 Content Tags with Truncation

```tsx
import { Tag } from "@neuron/ui";

const ContentTags = ({ article }) => {
  return (
    <div className="content-tags">
      {/* Category tags */}
      {article.categories.map((category) => (
        <Tag
          key={category}
          text={category}
          variant="celestialBlue"
          textTruncate={category.length > 15}
          tooltipText={category.length > 15 ? category : `Category: ${category}`}
        />
      ))}

      {/* Author tag */}
      <Tag
        text={`By ${article.author.name}`}
        variant="majorelleBlue"
        textTruncate={article.author.name.length > 12}
        tooltipText={`Author: ${article.author.name}`}
      />

      {/* Publication status */}
      <Tag
        text={article.status}
        variant={article.status === "published" ? "success" : "warning"}
        uppercase={true}
        disabled={article.status === "archived"}
        lineThrough={article.status === "deleted"}
      />
    </div>
  );
};
```

## Step 7: Tag Props Reference

### 7.1 Core Tag Props

| Prop      | Type         | Default     | Description                 |
| --------- | ------------ | ----------- | --------------------------- |
| text      | `string`     | -           | Tag text content (required) |
| variant   | `TagVariant` | `"default"` | Visual variant style        |
| className | `string`     | -           | Additional CSS classes      |
| testId    | `string`     | -           | Test identifier             |

### 7.2 Visual Props

| Prop         | Type             | Default | Description                          |
| ------------ | ---------------- | ------- | ------------------------------------ |
| icon         | `IconDefinition` | -       | Icon from baseIcons system           |
| textTruncate | `boolean`        | `false` | Enable text truncation with ellipsis |
| uppercase    | `boolean`        | `false` | Transform text to uppercase          |
| lineThrough  | `boolean`        | `false` | Add line-through text decoration     |
| disabled     | `boolean`        | `false` | Disabled state for historical tags   |

### 7.3 Tooltip Props

| Prop             | Type                | Default   | Description                |
| ---------------- | ------------------- | --------- | -------------------------- |
| tooltipText      | `string`            | -         | Tooltip content text       |
| tooltipPlacement | `TTooltipPlacement` | `"right"` | Tooltip placement position |

### 7.4 Variant Options

**Semantic Variants:**

- `"default"` - Default neutral state
- `"info"` - Informational state
- `"success"` - Success/positive state
- `"warning"` - Warning/caution state
- `"danger"` - Error/negative state

**Color Variants:**

- `"appleGreen"` - Apple green color
- `"emeraldGreen"` - Emerald green color
- `"celestialBlue"` - Celestial blue color
- `"majorelleBlue"` - Majorelle blue color
- `"electricPurple"` - Electric purple color
- `"indianRed"` - Indian red color
- `"sendyBrown"` - Sandy brown color
- `"citrine"` - Citrine yellow color

## Step 8: Best Practices

### 8.1 When to Use Each Variant

**Semantic Variants:**

- **Default**: Neutral information, general labels
- **Info**: Informational content, help text
- **Success**: Completed actions, positive states
- **Warning**: Caution, pending actions
- **Danger**: Errors, critical issues

```tsx
{/* Good: Semantic meaning matches variant */}
<Tag text="Completed" variant="success" />
<Tag text="Error" variant="danger" />
<Tag text="Pending Review" variant="warning" />
```

**Color Variants:**

- Use for categorization and visual grouping
- Maintain consistency across similar content types
- Consider accessibility and color contrast

```tsx
{/* Good: Consistent categorization */}
<Tag text="Frontend" variant="celestialBlue" />
<Tag text="Backend" variant="emeraldGreen" />
<Tag text="Database" variant="indianRed" />
```

### 8.2 Text Truncation Best Practices

**⚠️ CRITICAL: Always provide tooltip for truncated text**

```tsx
{
  /* ❌ INCORRECT: Truncated without tooltip */
}
<Tag text="Very long tag text" textTruncate={true} />;

{
  /* ✅ CORRECT: Truncated with tooltip */
}
<Tag text="Very long tag text" textTruncate={true} tooltipText="Very long tag text" />;

{
  /* ✅ CORRECT: Conditional truncation */
}
<Tag text={tagText} textTruncate={tagText.length > 20} tooltipText={tagText.length > 20 ? tagText : undefined} />;
```

### 8.3 State Management Best Practices

- Use `disabled` for historical or inactive states
- Use `lineThrough` for cancelled or deprecated items
- Use `uppercase` for emphasis or formal contexts
- Combine states appropriately for complex scenarios

```tsx
{
  /* Good: Historical state representation */
}
<Tag text="Old Status" variant="warning" disabled={true} tooltipText="This status is no longer active" />;

{
  /* Good: Cancelled item */
}
<Tag text="Cancelled Task" variant="danger" lineThrough={true} disabled={true} tooltipText="This task was cancelled" />;
```

### 8.4 Icon Usage Guidelines

- Use icons to enhance meaning, not replace text
- Choose icons that clearly represent the tag's purpose
- Maintain consistency in icon usage across similar tags

```tsx
{/* Good: Icon enhances meaning */}
<Tag text="Approved" variant="success" icon={baseIcons.checkCircleSolid} />
<Tag text="Pending" variant="warning" icon={baseIcons.clockSolid} />

{/* Avoid: Icon doesn't match meaning */}
<Tag text="Error" variant="danger" icon={baseIcons.heartSolid} />
```

### 8.5 Accessibility Considerations

- Provide meaningful text content
- Use tooltips to provide additional context
- Ensure color variants have sufficient contrast
- Use proper ARIA attributes through disabled state

```tsx
{
  /* Good: Accessible tag implementation */
}
<Tag
  text="High Priority"
  variant="danger"
  tooltipText="This item requires immediate attention"
  icon={baseIcons.exclamationTriangleSolid}
/>;
```

## Step 9: Common Mistakes to Avoid

### 9.1 Don't Forget Tooltips for Truncated Text

```tsx
{
  /* ❌ INCORRECT: No tooltip for truncated text */
}
<Tag text="Very long text that gets truncated" textTruncate={true} />;

{
  /* ✅ CORRECT: Tooltip provides full text */
}
<Tag text="Very long text that gets truncated" textTruncate={true} tooltipText="Very long text that gets truncated" />;
```

### 9.2 Don't Misuse Semantic Variants

```tsx
{/* ❌ INCORRECT: Semantic mismatch */}
<Tag text="Error occurred" variant="success" />
<Tag text="Task completed" variant="danger" />

{/* ✅ CORRECT: Semantic alignment */}
<Tag text="Error occurred" variant="danger" />
<Tag text="Task completed" variant="success" />
```

### 9.3 Don't Overuse State Combinations

```tsx
{
  /* ❌ INCORRECT: Conflicting states */
}
<Tag text="Active" variant="success" disabled={true} lineThrough={true} />;

{
  /* ✅ CORRECT: Logical state combination */
}
<Tag text="Cancelled" variant="danger" disabled={true} lineThrough={true} tooltipText="This item was cancelled" />;
```

### 9.4 Don't Ignore Empty Text

```tsx
{/* ❌ INCORRECT: Empty or undefined text */}
<Tag text="" variant="info" />
<Tag text={undefined} variant="success" />

{/* ✅ CORRECT: Conditional rendering */}
{tagText && <Tag text={tagText} variant="info" />}
{item.status && <Tag text={item.status} variant="success" />}
```

## Key Takeaways

The Neuron Tag component system provides a comprehensive, accessible, and consistent foundation for status indicators and metadata display. Key points to remember:

1. **Semantic variants** for application states (info, success, warning, danger)
2. **Color variants** for categorization and visual grouping
3. **Text truncation** must always include tooltip with full text
4. **State indicators** (disabled, lineThrough, uppercase) for complex scenarios
5. **Icon integration** to enhance meaning and visual clarity
6. **Tooltip support** for additional context and accessibility
7. **Historical states** using disabled and lineThrough for inactive content
8. **Accessibility compliance** through proper ARIA attributes and meaningful content
9. **Consistent usage** across similar content types and categories
10. **Performance considerations** with conditional rendering and state management

By following these guidelines, you'll create consistent, accessible, and meaningful tag interfaces that enhance your Neuron applications' information architecture and user experience.

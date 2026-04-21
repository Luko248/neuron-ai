---
agent: agent
---

# Neuron Panel Component Guidelines

## For the AI Assistant

This document provides comprehensive guidelines for implementing and configuring the Neuron Panel component. It explains proper usage, variant selection based on nesting levels, and best practices.

## Sync Metadata

- **Component Version:** v5.1.4
- **Component Source:** `packages/neuron/ui/src/lib/panels/panel/Panel.tsx`
- **Guideline Command:** `/neuron-ui-panel`
- **Related Skill:** `neuron-ui-panels`

## Introduction

The Panel component provides a structured container with a header and content area. It's used to group related content and actions while providing clear visual hierarchy and organization within the interface.

## ⚠️ CRITICAL: Component Architecture

**The Panel component has an INTEGRATED structure - header and content are built-in:**

- ✅ **CORRECT**: Use Panel as a single component with props for header configuration
- ❌ **INCORRECT**: Do NOT use Panel.Header or Panel.Content nested syntax
- ✅ **CORRECT**: Pass header properties (title, icon, description) as direct props
- ✅ **CORRECT**: Pass content via the `children` prop
- ✅ **CORRECT**: Nest complete Panel components inside each other for hierarchical layouts

### Real Implementation Patterns

```tsx
// ✅ CORRECT - Single Panel with props
<Panel
  title="Customer Information"
  headerDescription="Customer details"
  icon={{ iconDef: baseIcons.user }}
  actionZone={<Button>Add</Button>}
>
  <p>Content goes here</p>
</Panel>

// ✅ CORRECT - Nesting complete Panel components with proper variants
<Panel title="First Level" variant={StyleVariant.Default}>
  <Panel title="Second Level" variant={StyleVariant.Outline}>
    <Panel title="Third Level" variant={StyleVariant.Contrast}>
      <p>Nested content</p>
    </Panel>
  </Panel>
</Panel>

// ❌ INCORRECT - Don't use Panel.Header/Panel.Content syntax (doesn't exist)
<Panel>
  <Panel.Header title="Title" />
  <Panel.Content>Content</Panel.Content>
</Panel>
```

**Key Points**:

- Panel has integrated header/content structure
- Supports nesting complete Panel components as children
- Follow variant progression for nested levels

## 📝 Understanding Figma vs Implementation

### Figma Design Representation

In Figma, you might see Panel designs that appear to have:

- Separate header and content sections
- Individual header elements (title, icon, actions)
- Distinct content areas

### Actual React Implementation

The React component works differently:

- **Single Component**: Panel is one component, not multiple nested ones
- **Props-Based**: All header configuration is done via props
- **Integrated Rendering**: Header and content are rendered automatically
- **No Compound Pattern**: Unlike some components, Panel doesn't use Panel.Header syntax

### Translation Guide: Figma → React

| Figma Element           | React Implementation     |
| ----------------------- | ------------------------ |
| Panel Header with Title | `title` prop             |
| Header Description      | `headerDescription` prop |
| Header Icon             | `icon` prop              |
| Header Actions          | `actionZone` prop        |
| Header Tags             | `tags` prop              |
| Header Tooltip          | `headerTooltip` prop     |
| Content Area            | `children` prop          |
| Close Button            | `closable` prop          |

**Remember**: Even if Figma shows separate elements, always implement as a single Panel component with props.

## 🎯 Figma Identification Rules

**How to identify Panel vs Accordion Panel in Figma:**

### Panel Component Indicators

- **NO chevron/arrow icon** in the header
- **Static content** - always visible, not collapsible
- **Informational header** - not interactive for expanding/collapsing
- Simple container with optional header section
- Used for permanent content display

### NOT Panel (Use Accordion instead)

- ❌ Has chevron/expand arrow in header → Use **Accordion Panel**
- ❌ Content can be expanded/collapsed → Use **Accordion Panel**
- ❌ Header is clickable for expand/collapse → Use **Accordion Panel**

**Selection Rule**: If you see ANY chevron or expand/collapse functionality → Use Accordion Panel, NOT Panel

## Component Structure

The Panel component has an integrated structure with built-in header and content areas:

```
Panel (Single Component)
├── Header Area (automatically rendered when header props provided)
│   ├── Title (via title prop)
│   ├── Description (via headerDescription prop)
│   ├── Icon (via icon prop)
│   ├── Tags (via tags prop)
│   ├── Notification Badge (via notification prop)
│   ├── Action Zone (via actionZone prop)
│   └── Close Button (via closable prop)
└── Content Area (automatically rendered)
    └── Children (via children prop)
```

**Implementation Note**: PanelHeader and PanelContent are internal components used by Panel - they are NOT exposed for direct use.

## Panel Variants

The Panel component supports multiple visual variants:

1. **Default/Base** (`variant="default"` or `StyleVariant.Default`): Standard panel with subtle background
2. **Outline** (`variant="outline"` or `StyleVariant.Outline`): Panel with border outline
3. **Contrast** (`variant="contrast"` or `StyleVariant.Contrast`): High contrast panel

## Variant System

The Panel component provides variant options for different visual styles, with automatic nesting support through CSS Container Queries.

### Available Variants

1. **Default/Base** (`variant="default"` or `StyleVariant.Default`): Standard panel with subtle background
2. **Outline** (`variant="outline"` or `StyleVariant.Outline`): Panel with border outline
3. **Contrast** (`variant="contrast"` or `StyleVariant.Contrast`): High contrast panel

### Universal Nesting Pattern

**IMPORTANT**: Panel follows the universal variant hierarchy pattern defined for all container components:

- **Level 1**: `variant: default`
- **Level 2**: `variant: outline`
- **Level 3**: `variant: contrast`
- **Level 4+**: Pattern repeats (default → outline → contrast → default...)

This pattern applies regardless of how components are nested. For example:

- `Panel > Panel > Panel` follows this hierarchy
- `Container > AccordionPanel > Panel` follows this hierarchy
- `FeatureContainer > Panel > Container > Panel` follows this hierarchy

See the Layout System Guidelines (`neuron-content-layout-system`) for complete details on the universal nesting pattern.

### Explicit Variant Assignment Required

**IMPORTANT**: You MUST explicitly set the variant property for each nesting level:

```tsx
// ✅ CORRECT: Explicit variant assignment
<Panel variant="default" title="First Level">
  <Panel variant="outline" title="Second Level">
    <Panel variant="contrast" title="Third Level">
      <p>Nested content with proper styling</p>
    </Panel>
  </Panel>
</Panel>

// ✅ CORRECT: Mixed component nesting with explicit variants
<Panel variant="default" title="Customer Management">
  <AccordionPanel variant="outline" title="Personal Information">
    <Container variant="contrast">
      <Input name="firstName" />
      <Input name="lastName" />
    </Container>
  </AccordionPanel>
  <AccordionPanel variant="outline" title="Contact Details">
    <Panel variant="contrast" title="Primary Contact">
      <Input name="email" />
      <Input name="phone" />
    </Panel>
  </AccordionPanel>
</Panel>

// ❌ WRONG: Missing variant properties
<Panel title="First Level">
  <Panel title="Second Level">
    <p>Content</p>
  </Panel>
</Panel>
```

### Manual Variant Override

You can still manually specify variants when needed for specific design requirements:

```tsx
// Manual variant specification when needed
<Panel title="Special Section" variant={StyleVariant.Contrast}>
  <Panel title="Highlighted Content" variant={StyleVariant.Outline}>
    <p>Custom styled content</p>
  </Panel>
</Panel>
```

## Basic Usage

**Always use Panel as a single component with props - never use nested Panel.Header/Panel.Content syntax.**

```tsx
import { Panel, StyleVariant, baseIcons } from "@neuron/ui";

// Basic panel with title and content
<Panel
  title="Customer Information"
  variant={StyleVariant.Default}
>
  <p>Customer details go here...</p>
</Panel>

// Panel with description and icon
<Panel
  title="Settings"
  headerDescription="System configuration"
  icon={{ iconDef: baseIcons.settingsRegular }}
>
  <p>Settings content...</p>
</Panel>

// Panel with notification badge
<Panel
  title="Messages"
  notification={{ count: 5 }}
>
  <MessageList />
</Panel>

// Panel with header tooltip
<Panel
  title="Advanced Settings"
  headerTooltip="These settings require admin privileges"
>
  <AdvancedForm />
</Panel>
```

## Panel with Action Zone

The action zone allows you to add interactive elements like buttons to the panel header:

```tsx
import { Panel, Button, baseIcons } from "@neuron/ui";

<Panel
  title="Users"
  actionZone={
    <Button variant="primary" size="small" iconLeft={baseIcons.plusRegular}>
      Add User
    </Button>
  }
>
  <UserList />
</Panel>;
```

## Closable Panel

Panels can be made closable by setting the `closable` prop:

```tsx
<Panel title="Notifications" closable={true}>
  <NotificationList />
</Panel>
```

## Panel States

The Panel component supports various states:

```tsx
// Disabled panel
<Panel
  title="Form"
  disabled={true}
>
  <FormContent />
</Panel>

// Locked/Read-only panel
<Panel
  title="Contract Details"
  locked={true}
>
  <ContractInfo />
</Panel>

// Active panel
<Panel
  title="Selected Item"
  active={true}
>
  <ItemDetails />
</Panel>
```

## Panel Props Reference

The Panel component accepts the following props (all header configuration is done via props):

### Content Props

| Prop     | Type      | Required | Description                          |
| -------- | --------- | -------- | ------------------------------------ |
| children | ReactNode | Yes      | Content to display in the panel body |
| title    | string    | No       | Title text for the panel header      |

### Header Configuration Props

| Prop                   | Type                           | Required | Description                               |
| ---------------------- | ------------------------------ | -------- | ----------------------------------------- |
| headerDescription      | string                         | No       | Optional description text below the title |
| headerTooltip          | ReactNode \| (() => ReactNode) | No       | Tooltip content for the header title      |
| headerTooltipPlacement | TTooltipPlacement              | No       | Tooltip placement (default: "right")      |
| headerTooltipVariant   | TooltipProps["variant"]        | No       | Tooltip visual variant                    |
| headerTooltipMaxWidth  | TooltipProps["maxWidth"]       | No       | Maximum width for tooltip                 |
| icon                   | IconProps                      | No       | Icon to display in the header             |
| tags                   | TagProps[]                     | No       | Array of tags to display in header        |
| notification           | NotificationBadgeProps         | No       | Notification badge for the header         |

### Action & Interaction Props

| Prop       | Type      | Required | Description                                      |
| ---------- | --------- | -------- | ------------------------------------------------ |
| actionZone | ReactNode | No       | Content for the action area (buttons, dropdowns) |
| closable   | boolean   | No       | Whether the panel can be closed (shows X button) |

### State & Appearance Props

| Prop     | Type              | Required | Description                                       |
| -------- | ----------------- | -------- | ------------------------------------------------- |
| variant  | StyleVariant      | No       | Visual style variant (default, outline, contrast) |
| size     | "base" \| "small" | No       | Size variant of the panel (default: "base")       |
| active   | boolean           | No       | Whether the panel is in active/selected state     |
| disabled | boolean           | No       | Whether the panel is disabled                     |
| locked   | boolean           | No       | Whether the panel is locked/read-only             |
| readOnly | boolean           | No       | Read-only state of the panel                      |

### Access Control Props (from @neuron/auth)

| Prop           | Type   | Required | Description                         |
| -------------- | ------ | -------- | ----------------------------------- |
| readonlyAccess | string | No       | Role-based read-only access control |
| fullAccess     | string | No       | Role-based full access control      |

### Development Props

| Prop      | Type   | Required | Description                      |
| --------- | ------ | -------- | -------------------------------- |
| className | string | No       | Additional CSS classes           |
| testId    | string | No       | Custom test ID for the component |

## Accessibility

- Use appropriate header hierarchy for panel titles
- Ensure any interactive elements in the action zone are keyboard accessible
- Consider locked/disabled states when implementing focus management

## Best Practices

1. **Single Component Usage**: Always use Panel as one component with props - never use Panel.Header or Panel.Content
2. **Descriptive Titles**: Always use clear, concise titles for panels
3. **Action Placement**: Place most important actions in the action zone
4. **Size Considerations**: Use the appropriate size based on content density
5. **Locked State**: Use the locked state for read-only information that shouldn't be editable
6. **Header Props**: Use specific header props (headerDescription, headerTooltip) instead of trying to nest components
7. **Automatic Nesting**: Let CSS Container Queries handle variant progression automatically - only specify variants when you need custom styling

## Common Mistakes to Avoid

### ❌ WRONG: Panel.Header/Panel.Content Syntax (Doesn't Exist)

```tsx
// WRONG - This compound component pattern doesn't exist
<Panel>
  <Panel.Header title="Title" />
  <Panel.Content>Content</Panel.Content>
</Panel>
```

### ✅ CORRECT: Integrated Props Approach

```tsx
// CORRECT - Single component with props
<Panel title="Title">Content</Panel>
```

### ✅ CORRECT: Nesting Complete Panel Components

```tsx
// CORRECT - Nesting entire Panel components
<Panel title="Outer Panel" variant={StyleVariant.Default}>
  <Panel title="Inner Panel" variant={StyleVariant.Outline}>
    Inner content
  </Panel>
</Panel>
```

### ❌ WRONG: Manual PanelHeader/PanelContent Usage

```tsx
// WRONG - These are internal components, not for direct use
<PanelHeader title="Title" />
<PanelContent>Content</PanelContent>
```

**Key Distinction**:

- ❌ Don't use Panel.Header/Panel.Content (doesn't exist)
- ✅ Do nest complete Panel components inside each other
- ✅ Do use props for header configuration

## Component Combinations

Panels work well with:

- AccordionPanels for expandable content
- Containers for additional content grouping
- Tabs for organizing multiple panels

Always remember to follow the nesting pattern for variants when combining different container components.

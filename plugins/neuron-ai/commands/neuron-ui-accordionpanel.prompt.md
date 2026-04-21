---
agent: agent
---

# Neuron AccordionPanel Component Guidelines

## For the AI Assistant

This document provides comprehensive guidelines for implementing and configuring the Neuron AccordionPanel component. It explains proper usage, variant selection based on nesting levels, and best practices.

## Sync Metadata

- **Component Version:** v5.5.6
- **Component Source:** `packages/neuron/ui/src/lib/panels/panel/AccordionPanel.tsx`
- **Guideline Command:** `/neuron-ui-accordionpanel`
- **Related Skill:** `neuron-ui-panels`

## Introduction

The AccordionPanel component extends the Panel component by adding collapsible/expandable functionality. It allows users to show or hide content sections, making interfaces more compact and organized while providing clear visual hierarchy.

## 🎯 Figma Identification Rules

**How to identify Accordion Panel vs Panel in Figma:**

### Accordion Panel Component Indicators

- **HAS chevron/arrow icon** in the header (right side)
- **Expandable/collapsible content** - can be shown/hidden
- **Interactive header** - clickable for expanding/collapsing
- Often named `AccordionPanelHeader` or similar in Figma
- Content visibility can be toggled

### NOT Accordion Panel (Use Panel instead)

- ❌ No chevron/expand arrow in header → Use **Panel**
- ❌ Static content always visible → Use **Panel**
- ❌ Header is purely informational → Use **Panel**

**Selection Rule**: If you see ANY chevron or expand/collapse functionality → Use Accordion Panel. If NO chevron → Use Panel

## Component Structure

The AccordionPanel component consists of:

```
AccordionPanel
├── Header (always visible)
│   ├── Expand/Collapse Control
│   ├── Optional Checkbox
│   ├── Title
│   ├── Description (optional)
│   ├── Icon (optional)
│   ├── Tags (optional)
│   ├── Notification Badge (optional)
│   └── Action Zone (optional)
└── Content (expandable)
    └── Children Content
```

## AccordionPanel Variants

The AccordionPanel component supports multiple visual variants:

1. **Default/Base** (`variant="default"` or `StyleVariant.Default`): Standard panel with subtle background
2. **Outline** (`variant="outline"` or `StyleVariant.Outline`): Panel with border outline
3. **Contrast** (`variant="contrast"` or `StyleVariant.Contrast`): High contrast panel

### Universal Nesting Pattern

**IMPORTANT**: AccordionPanel follows the universal variant hierarchy pattern defined for all container components:

- **Level 1**: `variant: default`
- **Level 2**: `variant: outline`
- **Level 3**: `variant: contrast`
- **Level 4+**: Pattern repeats (default → outline → contrast → default...)

This pattern applies regardless of how components are nested. For example:

- `AccordionPanel > AccordionPanel > AccordionPanel` follows this hierarchy
- `Container > AccordionPanel > Panel` follows this hierarchy
- `FeatureContainer > Panel > AccordionPanel > Container` follows this hierarchy

See the Layout System Guidelines (`neuron-content-layout-system`) for complete details on the universal nesting pattern.

**IMPORTANT**: You MUST explicitly set the variant property for each nesting level. The `variant` prop is required when nesting components.

## Basic Usage

```tsx
import { AccordionPanel, StyleVariant } from "@neuron/ui";

// Basic accordion panel
<AccordionPanel
  title="Frequently Asked Questions"
  defaultOpen={true}
>
  <p>FAQ content goes here...</p>
</AccordionPanel>

// Accordion with description and icon
<AccordionPanel
  title="System Settings"
  headerDescription="Configure system parameters"
  icon={{ iconDef: baseIcons.settingsRegular }}
>
  <SettingsForm />
</AccordionPanel>
```

## Controlled vs Uncontrolled State

AccordionPanel can be used in either controlled or uncontrolled mode:

```tsx
// Uncontrolled - with initial state
<AccordionPanel title="Details" defaultOpen={true}>
  <p>Content is open by default</p>
</AccordionPanel>;

// Controlled - state is managed externally
const [isOpen, setIsOpen] = useState(false);

<AccordionPanel title="Advanced Options" open={isOpen} onOpenChange={setIsOpen}>
  <AdvancedOptionsForm />
</AccordionPanel>;
```

## AccordionPanel with Checkbox

AccordionPanel can include an optional checkbox:

```tsx
const [isChecked, setIsChecked] = useState(false);

<AccordionPanel title="Terms and Conditions" showCheckbox={true} checked={isChecked} onCheckboxChange={setIsChecked}>
  <TermsContent />
</AccordionPanel>;
```

## Mixed Component Nesting Example

Here's an example of mixing AccordionPanel with other components with explicit variant assignment:

```tsx
// ✅ CORRECT: Explicit variant assignment for each level
<AccordionPanel variant="default" title="Customer Information">
  <CustomerBasicInfo />

  {/* Panel with explicit variant */}
  <Panel variant="outline" title="Contact Details">
    <ContactInfo />
  </Panel>

  {/* AccordionPanel with explicit variant */}
  <AccordionPanel variant="outline" title="Order History">
    <OrderList />

    {/* Container with explicit variant */}
    <Container variant="contrast">
      <OrderStats />
    </Container>
  </AccordionPanel>
</AccordionPanel>

// ❌ WRONG: Missing variant properties
<AccordionPanel title="Customer Information">
  <Panel title="Contact Details">
    <Container>
      <OrderStats />
    </Container>
  </Panel>
</AccordionPanel>
```

## AccordionPanel Props

The AccordionPanel component accepts the following key props:

| Prop              | Type                   | Description                                                                                                           |
| ----------------- | ---------------------- | --------------------------------------------------------------------------------------------------------------------- |
| title             | string                 | Title for the panel header                                                                                            |
| headerDescription | string                 | Optional description for the panel header                                                                             |
| variant           | StyleVariant           | Visual style variant (default, outline, contrast)                                                                     |
| icon              | IconProps              | Optional icon to display in the header                                                                                |
| actionZone        | ReactNode              | Content for the action area in the header                                                                             |
| open              | boolean                | Controlled open state                                                                                                 |
| defaultOpen       | boolean                | Initial open state (uncontrolled)                                                                                     |
| onOpenChange      | function               | Callback fired only on user interactions (header click/keyboard). Not called when the `open` prop changes externally. |
| showCheckbox      | boolean                | Whether to display a checkbox                                                                                         |
| checked           | boolean                | Controlled checkbox state                                                                                             |
| onCheckboxChange  | function               | Callback when checkbox state changes                                                                                  |
| disabledCheckbox  | boolean                | Whether the checkbox is disabled                                                                                      |
| locked            | boolean                | Whether the panel is locked/read-only                                                                                 |
| disabled          | boolean                | Whether the panel is disabled                                                                                         |
| active            | boolean                | Whether the panel is in active state                                                                                  |
| size              | "base" \| "small"      | Size variant of the panel                                                                                             |
| tags              | TagProps[]             | Optional tags to display in the header                                                                                |
| notification      | NotificationBadgeProps | Optional notification badge                                                                                           |

## Accessibility

- The component uses proper ARIA attributes for expandable content
- Ensure keyboard navigation works properly for expanding/collapsing
- Make sure checkbox functionality is fully accessible if used
- Maintain proper focus management when panels open/close

## Best Practices

1. **Descriptive Titles**: Use clear, concise titles for accordion panels
2. **Performance**: For complex content, consider lazy loading when panels are expanded
3. **Controlled State**: Use controlled state when accordion behavior needs to be coordinated with other components
4. **Meaningful Grouping**: Group related content within the same accordion panel
5. **Progressive Disclosure**: Use accordions to progressively disclose complex information
6. **Initial State**: Consider which panels should be open by default based on user needs

## Performance Considerations

- Use memoization for complex content within accordion panels
- Consider code-splitting or lazy loading for heavy content that's initially hidden
- Avoid deep nesting of many accordion panels when possible
- Use callbacks appropriately to prevent unnecessary re-renders

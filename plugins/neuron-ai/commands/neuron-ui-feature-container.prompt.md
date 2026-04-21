---
agent: agent
---

# Neuron FeatureContainer Pattern Guidelines

## For the AI Assistant

This document provides comprehensive guidelines for implementing the Neuron FeatureContainer component. It covers usage patterns for entity showcases, search results, and complex content containers with metadata, actions, and collapsible sections.

## Sync Metadata

- **Component Version:** v1.4.0
- **Component Source:** `packages/neuron/ui/src/lib/patterns/featureContainer/FeatureContainer.tsx`
- **Guideline Command:** `/neuron-ui-feature-container`
- **Related Skill:** `neuron-ui-composites`

## Introduction

The FeatureContainer is a rich, composite container component designed to display feature-centric content. It primarily serves as the **Section Header** of a page, situated at the top, providing high-level context for the content below. It supports a wide range of metadata including flags, tags, chips, actions, and collapsible content sections.

### What is the FeatureContainer?

The FeatureContainer component creates structured content areas for displaying complex entities like Partners, Contracts, or Search Results. It provides a standardized layout for:

- **Entity Details** - showcasing detailed information with status indicators
- **Search Results** - rich result items with metadata and quick actions
- **Content Grouping** - wrapping calculators, forms, or feature highlights
- **Collapsible Sections** - managing complex content with expand/collapse functionality

### Key Features

- **Rich Metadata Support**: Integrated flags, tags, chips, and notification badges
- **Flexible Actions**: Support for button groups, dropdowns, and primary actions
- **Collapsible Content**: Built-in expand/collapse logic for managing detail density
- **Visual Variants**: Default, contrast, and base styles for different contexts
- **Product Branding**: Customizable stripe colors for product differentiation
- **Responsive Design**: Adaptive layout for mobile and desktop viewports
- **Sticky Header Integration**: Seamless transition to StickyHeader on scroll

**Storybook Reference:** You can refer to the Storybook documentation for visual examples and interactive demos of FeatureContainer configurations.

## Step 1: Basic FeatureContainer Implementation

Used for displaying person or partner details with status flags and tags.

```tsx
<FeatureContainer
  title={{ text: "Ing. Veronika Nováková-Millerová", icon: { iconDef: baseIcons.userRegular } }}
  subtitle="Nová kalkulace"
  flags={[
    { variant: "success", text: "kifoa" },
    { variant: "success", text: "active" },
  ]}
  tags={[
    { text: "FO", variant: "default" },
    { text: "CZE", variant: "default" },
  ]}
  actions={[
    {
      children: "Upravit partnera",
      variant: "secondary",
      iconLeft: baseIcons.penRegular,
    },
  ]}
>
  <div>Content goes here</div>
</FeatureContainer>
```

### 2. Contract Showcase (Collapsible)

Used for contracts with collapsible detail sections and product-specific branding (stripe color).

```tsx
<FeatureContainer
  title={{
    text: "Karel Hromek pojistník",
    href: "/contract/123",
    target: "_blank",
  }}
  label="PS 3952234727"
  icon={{ type: "custom", name: "property" }}
  stripeColor="var(--colorFcontainerStripeBase)"
  collapsible={true}
  open={true}
  flags={[
    { variant: "danger", text: "INS" },
    { variant: "warning", text: "exp", tooltipText: "Expiration info" },
  ]}
  tags={[{ text: "OP6 - Pojištění rod. domu", variant: "default" }]}
  actions={[{ variant: "secondary", iconLeft: baseIcons.clockRotateLeftRegular }]}
  hiddenChildren={<div>Hidden details content</div>}
>
  <div>Main summary content</div>
</FeatureContainer>
```

### 3. Search Result Showcase

Used in search lists, supporting notification badges on the title and "Show" button.

```tsx
<FeatureContainer
  title={{
    text: "Radek Zelenka",
    icon: { iconDef: baseIcons.userRegular },
    notification: { count: 2, variant: "danger" },
  }}
  variant="default"
  tags={[{ text: "ČN: 6254965432", variant: "default" }]}
  actions={[{ children: "Upravit", variant: "secondary" }]}
  isShowButtonVisible={true}
>
  <div>Search result details</div>
</FeatureContainer>
```

### 4. Complex Usage (Contrast Variant)

High-density information with tooltips, chips, and active state.

```tsx
<FeatureContainer
  variant="contrast"
  active={true}
  title={{
    text: "Complex Title",
    tooltip: { tooltipContent: "Info about title" },
    notification: { count: 5, variant: "danger" },
  }}
  subtitle={{
    text: "Subtitle with Link",
    to: "/route",
  }}
  stripeColor="var(--colorFcontainerStripeBase)"
  collapsible={true}
  chips={[
    { label: "Chip 1", variant: "default" },
    { label: "Chip 2", variant: "info" },
  ]}
  actions={[{ children: "Action 1", onClick: handleAction1 }]}
>
  <ContentPlaceholder />
</FeatureContainer>
```

## ⚙️ API Reference

### Key Props

**Icons overview**

- **Title icon**: Use `title.icon` inside the `title` object.
- **Stripe icon**: Use `icon` for the product/feature icon rendered in the left stripe.

| Prop                       | Type                                            | Description                                                                         |
| -------------------------- | ----------------------------------------------- | ----------------------------------------------------------------------------------- |
| `title`                    | `string` \| `TitleObject`                       | Main title. Object supports `text`, `icon`, `href`/`to`, `tooltip`, `notification`. |
| `subtitle`                 | `string` \| `SubtitleObject`                    | Secondary title.                                                                    |
| `description`              | `string`                                        | Description text displayed under the subtitle.                                      |
| `label`                    | `string`                                        | Label text displayed on top of the title.                                           |
| `labelAddon`               | `string`                                        | Label text displayed next to the label.                                             |
| `titleQuickActions`        | `QuickActionsProps`                             | Quick actions rendered next to the title area.                                      |
| `subtitleQuickActions`     | `QuickActionsProps`                             | Quick actions rendered next to the subtitle area.                                   |
| `labelQuickActions`        | `QuickActionsProps`                             | Quick actions rendered next to the label.                                           |
| `labelAddonQuickActions`   | `QuickActionsProps`                             | Quick actions rendered next to the label addon.                                     |
| `icon`                     | `IconProps`                                     | Main product/feature icon displayed in the stripe.                                  |
| `stripeColor`              | `string`                                        | CSS color variable for the left stripe (e.g., product color).                       |
| `flags`                    | `FlagProps[]`                                   | Array of status flags (top right).                                                  |
| `tags`                     | `TagProps[]`                                    | Array of metadata tags (below title).                                               |
| `chips`                    | `ChipProps[]`                                   | Array of chips (in content area).                                                   |
| `actions`                  | `(ButtonProps \| DropdownProps \| ReactNode)[]` | Array of action buttons, dropdowns, or custom ReactNode elements (header right).    |
| `collapsible`              | `boolean`                                       | Enables expand/collapse functionality.                                              |
| `open`                     | `boolean`                                       | Default open state if collapsible (defaults to true).                               |
| `onClose`                  | `() => void`                                    | Callback when close button is clicked.                                              |
| `closeButtonLabel`         | `string`                                        | Custom label for the close button.                                                  |
| `children`                 | `ReactNode` \| `() => ReactNode`                | Main visible content.                                                               |
| `hiddenChildren`           | `ReactNode` \| `() => ReactNode`                | Content visible only when expanded.                                                 |
| `isShowButtonVisible`      | `boolean`                                       | Shows a primary "Show" button if title has a link.                                  |
| `isChildrenInActionsPlace` | `boolean`                                       | If true, moves children content into the actions zone.                              |
| `variant`                  | `"default"` \| `"contrast"` \| `"base"`         | Visual style variant.                                                               |
| `active`                   | `boolean`                                       | Active state of the container.                                                      |
| `className`                | `string`                                        | Additional CSS class name.                                                          |
| `testId`                   | `string`                                        | Custom test ID.                                                                     |

### Universal Nesting Pattern

**IMPORTANT**: FeatureContainer follows the universal variant hierarchy pattern defined for all container components:

- **Level 1**: `variant: default`
- **Level 2**: `variant: outline`
- **Level 3**: `variant: contrast`
- **Level 4+**: Pattern repeats (default → outline → contrast → default...)

This pattern applies regardless of how components are nested. For example:

- `FeatureContainer > Container > Panel > Container` follows this hierarchy
- `FeatureContainer > Panel > AccordionPanel` follows this hierarchy
- `FeatureContainer > AccordionPanel > Container > Panel` follows this hierarchy

See the Layout System Guidelines (`neuron-content-layout-system`) for complete details on the universal nesting pattern.

**IMPORTANT**: You MUST explicitly set the variant property for each nesting level when nesting components inside FeatureContainer.

### Title Object Interface

```typescript
interface TitleObject {
  text: string;
  icon?: IconProps;
  href?: string; // External link
  to?: string; // Router link
  onClick?: () => void;
  target?: string;
  tooltip?: TooltipProps;
  notification?: NotificationBadgeProps;
}
```

## 🧩 Associated Patterns

- **StickyHeader**: Often used in conjunction with FeatureContainer to provide a sticky navigation header that triggers when the FeatureContainer scrolls out of view. See StickyHeader Guidelines (`/neuron-ui-sticky-header`).

## 📱 Responsive Behavior

- **Mobile**:
  - Icon moves to top content area.
  - Actions may wrap or stack.
  - Stripe is hidden or adjusted.
- **Desktop**:
  - Icon displays in the left stripe.
  - Full horizontal layout.

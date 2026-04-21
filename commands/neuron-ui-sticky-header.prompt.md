---
agent: agent
---

# Neuron StickyHeader Pattern Guidelines

## For the AI Assistant

This document provides comprehensive guidelines for implementing the Neuron StickyHeader component. It explains how to implement persistent navigation headers that anchor to page sections and maintain context while scrolling.

## Sync Metadata

- **Component Version:** v3.2.1
- **Component Source:** `packages/neuron/ui/src/lib/patterns/stickyHeader/StickyHeader.tsx`
- **Guideline Command:** `/neuron-ui-sticky-header`
- **Related Skill:** `neuron-ui-composites`

## Introduction

The StickyHeader is a navigation pattern that provides persistent context and navigation anchors at the top of the viewport. It acts as a minimal version of the FeatureContainer, ensuring critical information remains accessible when the main content scrolls out of view.

### What is the StickyHeader?

The StickyHeader component creates a fixed positioning header that stays visible at the top of the screen. It is primarily used for:

- **Persistent Context** - keeping entity names or status visible while scrolling
- **Navigation Anchors** - providing quick jumps to specific page sections
- **Secondary Actions** - offering access to key actions from anywhere on the page
- **Scroll Interactions** - automatically appearing when trigger elements scroll out of view

### Key Features

- **Scroll-based Animation**: Automatic fade-in when trigger elements (like FeatureContainer) leave the viewport
- **LayoutPortal Integration**: Correct positioning within the application layout stacking context
- **Rich Metadata**: Support for title, icon, flags, and tags to maintain context
- **Anchor Navigation**: Integrated navigation links for page sections
- **Responsive Design**: Adapts to different viewport sizes
- **Theme Integration**: Consistent styling with the application theme

**Storybook Reference:** You can refer to the Storybook documentation for visual examples and interactive demos of StickyHeader configurations.

## Step 1: Basic StickyHeader Implementation

### 1.1 Import the StickyHeader Component

```tsx
import { StickyHeader, LayoutPortal, baseIcons } from "@neuron/ui";
```

### 1.2 Basic Usage with LayoutPortal

The `StickyHeader` should typically be wrapped in a `LayoutPortal` with `position="top"` to ensure it sits correctly in the layout stacking context.

```tsx
<LayoutPortal position="top">
  <StickyHeader
    title="Ing. Veronika Nováková-Millerová"
    icon={{ iconDef: baseIcons.personSolid }}
    iconColor="var(--colorProductsTravel600)"
    anchors={[
      { label: "Basics", href: "#basics", iconLeft: baseIcons.houseSolid },
      { label: "Usage", href: "#usage" },
      { label: "Settings", href: "#settings" },
    ]}
    flags={[{ text: "Active", variant: "success" }]}
  />
</LayoutPortal>
```

### 2. Animation Trigger (Scroll-based Appearance)

Use `triggerElementId` to automatically show/hide the sticky header based on the visibility of a trigger element (usually the main page header or a FeatureContainer).

**Setup:**

1. Define an ID on the trigger element.
2. Pass `animation={true}` and `triggerElementId="..."` to `StickyHeader`.

```tsx
// In your page component
const MyPage = () => {
  return (
    <>
      {/* The Sticky Header (initially hidden if animation=true) */}
      <LayoutPortal position="top">
        <StickyHeader
          title="Page Title"
          animation={true}
          triggerElementId="page-header-trigger"
          anchors={[...]}
        />
      </LayoutPortal>

      {/* The Trigger Element */}
      <section id="page-header-trigger">
        <FeatureContainer title="Page Title" ... />
      </section>

      {/* Page Content */}
      <div id="basics">...</div>
    </>
  );
};
```

### 3. With Metadata (Flags and Tags)

StickyHeader can display flags and tags similar to FeatureContainer to maintain context.

```tsx
<StickyHeader
  title="Contract #12345"
  flags={[
    { text: "INS", variant: "danger" },
    { text: "EKA", variant: "warning" }
  ]}
  tags={[
    { text: "Outdated", variant: "warning", uppercase: true }
  ]}
  anchors={[...]}
/>
```

## ⚙️ API Reference

### Key Props

| Prop               | Type                   | Description                                                                          |
| ------------------ | ---------------------- | ------------------------------------------------------------------------------------ |
| `title`            | `string`               | Main title text.                                                                     |
| `subtitle`         | `string`               | Subtitle text.                                                                       |
| `description`      | `string`               | Description text.                                                                    |
| `anchors`          | `StickyHeaderAnchor[]` | Array of navigation links/anchors.                                                   |
| `anchorsIcon`      | `IconProps`            | Icon displayed in the anchors section.                                               |
| `icon`             | `IconProps`            | Icon displayed next to the title.                                                    |
| `iconColor`        | `string`               | Color of the icon.                                                                   |
| `animation`        | `boolean`              | Enables scroll-based fade-in animation.                                              |
| `triggerElementId` | `string`               | DOM ID of the element that triggers the header appearance when scrolled out of view. |
| `flags`            | `FlagProps[]`          | Status flags to display.                                                             |
| `tags`             | `TagProps[]`           | Metadata tags to display.                                                            |
| `chips`            | `ChipProps[]`          | Chips to display.                                                                    |
| `className`        | `string`               | Custom CSS class.                                                                    |
| `testId`           | `string`               | Custom test ID.                                                                      |

### Anchor Interface

```typescript
interface StickyHeaderAnchor {
  label: string;
  href: string;
  iconLeft?: IconDefinition;
  onClick?: () => void;
}
```

## 🧩 Associated Patterns

- **FeatureContainer**: Commonly serves as the `triggerElementId` target. See FeatureContainer Guidelines (`/neuron-ui-feature-container`).

---
agent: agent
---

# AI-Assisted Neuron Divider Component Integration Guide

## For the AI Assistant

Your task is to integrate the Neuron Divider component for visual content separation. This is a primitive component for creating visual dividers between content sections.

## Sync Metadata

- **Component Version:** v3.0.1
- **Component Source:** `packages/neuron/ui/src/lib/misc/divider/Divider.tsx`
- **Guideline Command:** `/neuron-ui-divider`
- **Related Skill:** `neuron-ui-misc`

## Introduction

The Divider component creates visual separation between content sections with optional labels and icons.

### Key Features

- **Horizontal dividers** (default) - separate vertical content
- **Vertical dividers** - separate horizontal content
- **Optional labels** - add descriptive text
- **Optional icons** - enhance visual context
- **Custom styling** - via className prop

## Basic Usage

### Import and Basic Implementation

```tsx
import { Divider } from "@neuron/ui";

const BasicExample = () => {
  return (
    <div>
      <div>
        <h2>First Section</h2>
        <p>Content above divider</p>
      </div>

      <Divider />

      <div>
        <h2>Second Section</h2>
        <p>Content below divider</p>
      </div>
    </div>
  );
};
```

### Divider with Label

```tsx
import { Divider } from "@neuron/ui";

const LabeledExample = () => {
  return (
    <div>
      <div>
        <h3>Personal Information</h3>
        <p>Name and basic details</p>
      </div>

      <Divider label="Contact Details" />

      <div>
        <h3>Contact Information</h3>
        <p>Email and phone</p>
      </div>
    </div>
  );
};
```

### Vertical Divider

```tsx
import { Divider } from "@neuron/ui";

const VerticalExample = () => {
  return (
    <div style={{ display: "flex" }}>
      <div>
        <h3>Left Content</h3>
        <p>Content on left</p>
      </div>

      <Divider vertical />

      <div>
        <h3>Right Content</h3>
        <p>Content on right</p>
      </div>
    </div>
  );
};
```

### Divider with Icon

```tsx
import { Divider, baseIcons } from "@neuron/ui";

const IconExample = () => {
  return (
    <div>
      <div>
        <h3>User Profile</h3>
        <p>Basic user information</p>
      </div>

      <Divider label="Settings" icon={{ iconDef: baseIcons.gearSolid }} />

      <div>
        <h3>Account Settings</h3>
        <p>Configuration options</p>
      </div>
    </div>
  );
};
```

## Common Patterns

### Form Section Separation

```tsx
import { Divider } from "@neuron/ui";

const FormExample = () => {
  return (
    <div>
      <div>
        <h3>Personal Information</h3>
        {/* Form fields */}
      </div>

      <Divider label="Contact Information" />

      <div>
        <h3>Contact Details</h3>
        {/* Form fields */}
      </div>

      <Divider />

      <div>{/* Form actions */}</div>
    </div>
  );
};
```

### List Item Separation

```tsx
import { Divider } from "@neuron/ui";

const ListExample = () => {
  return (
    <div>
      <div>
        <h4>First Item</h4>
        <p>Item content</p>
      </div>

      <Divider />

      <div>
        <h4>Second Item</h4>
        <p>Item content</p>
      </div>

      <Divider />

      <div>
        <h4>Third Item</h4>
        <p>Item content</p>
      </div>
    </div>
  );
};
```

## Props Reference

```tsx
interface DividerProps {
  label?: string; // Optional text label
  vertical?: boolean; // Vertical orientation (default: false)
  icon?: IconProps; // Optional icon from baseIcons
  className?: string; // Custom CSS classes
}
```

## Best Practices

**Do:**

- Use for logical content separation
- Choose meaningful labels
- Use vertical dividers in horizontal layouts
- Keep icon choices contextual

**Don't:**

- Overuse dividers
- Use as the only organization method
- Mix orientations inconsistently

## Summary

The Divider component provides simple visual separation with optional labels and icons. Use it to organize content sections and improve visual hierarchy in your Neuron applications.

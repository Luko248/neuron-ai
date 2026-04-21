---
agent: agent
---

# Neuron Container Component Guidelines

## For the AI Assistant

This document provides comprehensive guidelines for implementing and configuring the Neuron Container component. It explains proper usage, variant selection based on nesting levels, and best practices.

## Sync Metadata

- **Component Version:** v4.0.3
- **Component Source:** `packages/neuron/ui/src/lib/panels/container/Container.tsx`
- **Guideline Command:** `/neuron-ui-container`
- **Related Skill:** `neuron-ui-panels`

## Introduction

The Container component is a lightweight wrapper that provides consistent styling for content sections. It's simpler than Panel, without a header, and is used to visually group related content while maintaining the design system's visual hierarchy.

## Component Structure

The Container is a simple wrapper component:

```
Container
└── Children Content
```

## Variant System

The Container component provides variant options for different visual styles, with automatic nesting support through CSS Container Queries.

### Available Variants

1. **Default/Base** (`variant="default"` or `StyleVariant.Default`): Standard container with subtle background
2. **Outline** (`variant="outline"` or `StyleVariant.Outline`): Container with border outline
3. **Contrast** (`variant="contrast"` or `StyleVariant.Contrast`): High contrast container

### Universal Nesting Pattern

**IMPORTANT**: Container follows the universal variant hierarchy pattern defined for all container components:

- **Level 1**: `variant: default`
- **Level 2**: `variant: outline`
- **Level 3**: `variant: contrast`
- **Level 4+**: Pattern repeats (default → outline → contrast → default...)

This pattern applies regardless of how components are nested. For example:

- `Container > Container > Container` follows this hierarchy
- `FeatureContainer > Container > Panel > Container` follows this hierarchy
- `Panel > Container > AccordionPanel` follows this hierarchy

See the Layout System Guidelines (`neuron-content-layout-system`) for complete details on the universal nesting pattern.

### Explicit Variant Assignment Required

**IMPORTANT**: You MUST explicitly set the variant property for each nesting level:

```tsx
// ✅ CORRECT: Explicit variant assignment
<Container variant="default">
  <Container variant="outline">
    <Container variant="contrast">
      <p>Deep nested content with proper styling</p>
    </Container>
  </Container>
</Container>

// ✅ CORRECT: Mixed component nesting with explicit variants
<Panel variant="default" title="Main Section">
  <Container variant="outline">
    <AccordionPanel variant="contrast" title="Details">
      <p>Content here</p>
    </AccordionPanel>
  </Container>
</Panel>

// ❌ WRONG: Missing variant properties
<Container>
  <Container>
    <p>Content</p>
  </Container>
</Container>
```

### Manual Variant Override

You can still manually specify variants when needed for specific design requirements:

```tsx
// Manual variant specification when needed
<Container variant={StyleVariant.Contrast}>
  <Container variant={StyleVariant.Outline}>
    <p>Custom styled content</p>
  </Container>
</Container>
```

## Basic Usage

```tsx
import { Container, StyleVariant } from "@neuron/ui";

// Basic container
<Container>
  <p>Content goes here...</p>
</Container>

// Container with specific variant
<Container variant={StyleVariant.Outline}>
  <p>Content with outline...</p>
</Container>
```

## Container in Active State

Containers can be displayed in an active state:

```tsx
import { Container } from "@neuron/ui";

<Container active={true}>
  <p>This container is currently active</p>
</Container>;
```

## Combining with Other Components

Containers work well to wrap form elements, lists, or other content that needs visual grouping:

```tsx
import { Container, FormField, Input, Button } from "@neuron/ui";

<Container>
  <FormField label="Name">
    <Input placeholder="Enter your name" />
  </FormField>
  <FormField label="Email">
    <Input type="email" placeholder="Enter your email" />
  </FormField>
  <Button variant="primary">Submit</Button>
</Container>;
```

## Container Props

The Container component accepts the following key props:

| Prop      | Type                  | Description                                       |
| --------- | --------------------- | ------------------------------------------------- |
| variant   | StyleVariant          | Visual style variant (default, outline, contrast) |
| children  | ReactNode or function | The content to display inside the container       |
| className | string                | Additional CSS class names                        |
| active    | boolean               | Whether the container is in active state          |

## Mixed Component Nesting Example

Here's an example of mixing Container with other panel components with automatic variant handling:

```tsx
// Automatic variant nesting - no manual specification needed
<Container>
  <h2>User Management</h2>

  <Panel title="User Details">
    <UserInfo />

    <Container>
      <h4>Additional Settings</h4>
      <SettingsForm />
    </Container>
  </Panel>
</Container>
```

## Best Practices

1. **Keep it Simple**: Containers are meant to be lightweight grouping elements; use Panels for more complex structures
2. **Purpose-Oriented**: Use Containers to visually group related items
3. **Avoid Over-nesting**: Try to limit nesting to 3-4 levels for clarity
4. **Maintain Spacing**: Ensure proper spacing inside and between containers
5. **Automatic Nesting**: Let CSS Container Queries handle variant progression automatically - only specify variants when you need custom styling

## Accessibility

- Ensure content within containers maintains proper heading hierarchy
- Use semantic HTML elements for content inside containers
- Consider how containers affect the overall page structure

## Performance Considerations

The Container component is lightweight, but be mindful of:

- Nesting many containers can impact readability and maintenance
- For dynamic containers, memoize content when appropriate
- Use functions for conditional content rendering when needed

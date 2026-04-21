---
name: neuron-content-layout-system
description: Apply the Neuron layout system for page composition, grid structure, spacing, and nested containers. Use when deciding between CSS Grid, simple d-grid stacking, AutoLayout, and Neuron spacing utilities.
---

# Layout System

Use this skill when the task is about page structure, grid composition, spacing, or nested layout rules across multiple components.

## Layout Decision Guide

### Use simple `d-grid` with gap utilities for

- vertical stacking
- single-column content sections
- simple page sections without explicit column structure

```tsx
<div className="d-grid content-gap">
  <Panel>Section 1</Panel>
  <Panel>Section 2</Panel>
</div>
```

### Use Neuron CSS Grid for

- multi-column layouts
- structured Figma layouts
- forms or data blocks with explicit column spans
- responsive column control

```tsx
<div className="grid content-gap">
  <Literal className="g-col-6" label={{ text: "Name" }} values={[{ content: "John" }]} />
  <Literal className="g-col-6" label={{ text: "Email" }} values={[{ content: "john@email.com" }]} />
</div>
```

### Use `AutoLayout` for

- dynamic flowing button groups
- chip or tag collections
- content that should wrap naturally

```tsx
<AutoLayout itemMinSize="200px" className="align-items-end">
  <Button>Action 1</Button>
  <Button>Action 2</Button>
</AutoLayout>
```

## Container Nesting Pattern

For all container-like components, enforce the same variant cycle:

1. `default`
2. `outline`
3. `contrast`
4. repeat

Applies to:

- `Container`
- `Panel`
- `AccordionPanel`
- `FeatureContainer`

```tsx
<Container variant="default">
  <AccordionPanel variant="outline" title="Section">
    <Panel variant="contrast" title="Details">
      <Container variant="default">Content</Container>
    </Panel>
  </AccordionPanel>
</Container>
```

## Core Rules

- Use Neuron grid utilities, not custom layout CSS.
- Apply `g-col-*` classes directly to Neuron components where possible.
- Use wrappers only when the component API does not expose the needed class hook.
- Prefer Bootstrap 5.3 utilities and Neuron utility classes only.
- Do not add inline styles or custom layout classes for standard layout cases.

## Allowed Utility Sources

### Bootstrap 5.3 utilities

- `d-flex`, `d-grid`, `justify-content-*`, `align-items-*`
- `p-*`, `m-*`, `gap-*`
- `w-*`, `h-*`

### Neuron utilities

- `grid`
- `g-col-*`
- `content-gap`, `form-gap`, `canvas-gap`
- `content-p`, `content-px`, `content-py`

## Forbidden Patterns

- custom CSS classes for standard layout composition
- inline styles for spacing or width
- CSS-in-JS for normal page layout
- unnecessary wrapper divs around Neuron components

## Validation Checklist

- The chosen layout primitive matches the design intent.
- Grid spans are applied directly to the relevant components.
- Container variants follow the nesting cycle.
- Spacing comes from Bootstrap or Neuron utilities only.
- No custom layout CSS or inline style shortcuts were introduced.

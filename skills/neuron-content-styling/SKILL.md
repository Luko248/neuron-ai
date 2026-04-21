---
name: neuron-content-styling
description: Apply styling in Neuron applications using Bootstrap 5.3 utilities and Neuron tokens. Use when styling components, layouts, or spacing. MANDATORY: No custom CSS, no inline styles, no CSS-in-JS. Only Bootstrap utilities (d-flex, p-*, m-*, gap-*) and Neuron grid system (grid, g-col-*, content-gap). Apply classes directly to components via className props.
---

# Styling with Bootstrap & Neuron Utilities

Apply styling using Bootstrap 5.3 utilities and Neuron tokens only.

## Core Rules

**MANDATORY: No custom CSS, inline styles, or CSS-in-JS**

❌ **Forbidden:**

- Custom CSS classes (`.my-class`)
- Inline styles (`style={{ ... }}`)
- CSS-in-JS (styled-components)
- Custom CSS/SCSS files
- Unnecessary wrapper divs

✅ **Allowed:**

- Bootstrap 5.3 utilities
- Neuron UI utilities
- Direct component className props

## Available Utilities

**Bootstrap 5.3:**

- Layout: `d-flex`, `flex-column`, `justify-content-*`, `align-items-*`
- Spacing: `p-*`, `m-*`, `px-*`, `py-*`, `gap-*`
- Sizing: `w-*`, `h-*`
- Text: `text-*`, `fs-*`, `fw-*`
- Colors: `text-primary`, `bg-light`, `border-*`
- Display: `d-none`, `d-block`
- Borders: `rounded`, `border-*`

**Neuron UI:**

- Grid: `grid`, `g-col-*`, `g-col-md-*`, `g-col-lg-*`
- Spacing: `content-gap`, `form-gap`, `canvas-gap`
- Padding: `content-p`, `content-px`, `content-py`
- Justify: `justify-content-stretch`
- Container: `cont`, `g-col-cont-*`

## Correct Patterns

**Grid layout:**

```tsx
<div className="grid content-gap">
  <Panel className="g-col-6">Panel 1</Panel>
  <Panel className="g-col-6">Panel 2</Panel>
</div>
```

**Flexbox layout:**

```tsx
<div className="d-flex justify-content-between align-items-center gap-3">
  <Panel className="flex-grow-1">Content</Panel>
  <Button>Action</Button>
</div>
```

**Spacing:**

```tsx
<Container className="p-4 mb-3 border rounded">
  <h1 className="mb-2 text-primary">Title</h1>
  <p className="mb-0">Content</p>
</Container>
```

**Form layout:**

```tsx
<form className="grid form-gap">
  <Input className="g-col-6" name="firstName" control={control} />
  <Input className="g-col-6" name="lastName" control={control} />
</form>
```

## Forbidden Patterns

```tsx
// ❌ Custom CSS classes
<div className="custom-layout-wrapper">
  <Panel className="my-panel-style">Content</Panel>
</div>

// ❌ Inline styles
<Panel style={{ width: '50%', marginBottom: '20px' }}>Content</Panel>

// ❌ CSS-in-JS
const StyledPanel = styled(Panel)`width: 50%;`;

// ❌ Unnecessary wrappers
<div className="g-col-6">
  <Panel>Content</Panel>  {/* Apply g-col-6 directly to Panel */}
</div>
```

## Examples

**Example 1: Grid layout with panels**

```tsx
<div className="grid content-gap">
  <Panel className="g-col-8 p-4">Main content</Panel>
  <Panel className="g-col-4 p-3">Sidebar</Panel>
</div>
```

**Example 2: Flexbox layout**

```tsx
<div className="d-flex justify-content-between align-items-center p-3">
  <h1 className="mb-0">Title</h1>
  <Button>Action</Button>
</div>
```

**Example 3: Responsive grid**

```tsx
<div className="grid content-gap">
  <Panel className="g-col-12 g-col-md-6 g-col-lg-4">Responsive</Panel>
  <Panel className="g-col-12 g-col-md-6 g-col-lg-8">Responsive</Panel>
</div>
```

**Example 4: Form layout**

```tsx
<form className="grid form-gap">
  <Input className="g-col-6" name="firstName" control={control} />
  <Input className="g-col-6" name="lastName" control={control} />
  <Button className="g-col-12">Submit</Button>
</form>
```

**Example 5: Component-specific className props**

```tsx
<AccordionPanel panelClassName="g-col-8 border rounded" title="Section">
  Content
</AccordionPanel>

<Tile className="g-col-4 p-3" onClick={handleClick}>Tile</Tile>
```

## Best Practices

- Apply `g-col-*` directly to components, not wrapper divs
- Use `content-gap` for content layouts, `form-gap` for forms
- Use Bootstrap spacing utilities (`p-*`, `m-*`, `gap-*`)
- Use responsive classes (`g-col-12 g-col-md-6 g-col-lg-4`)
- Use flexbox for simple layouts (`d-flex`, `justify-content-*`)
- Use grid for complex layouts (`grid`, `g-col-*`)
- Never create custom CSS classes or inline styles
- Never wrap components unnecessarily

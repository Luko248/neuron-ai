---
agent: agent
---

# Spacing System AI Guidelines

## Sync Metadata

- **Component Version:** v1.0.0
- **Component Source:** `packages/neuron/ui/src/scss/vendor/_bootstrap-override.scss`
- **Guideline Command:** `/neuron-ui-spacing`
- **Related Skill:** `neuron-ui-layout`

## Overview

The Neuron spacing system combines Bootstrap utility classes overridden with design token values and custom utility classes for consistent spacing across applications. The system provides comprehensive spacing control for different content types and layout contexts.

## Spacing Categories

The spacing system is organized into four main categories:

- **Bootstrap Override Utilities**: Standard Bootstrap spacing with design token values
- **Canvas Spacing**: Viewport-level spacing for main layout components
- **Content Spacing**: Component-level spacing for containers, panels, modals
- **Form Spacing**: Specialized spacing for form layouts and interactive elements

## Bootstrap Override Utilities

### Spacer Values

All Bootstrap spacing utilities use design token values instead of default Bootstrap spacing:

```scss
$spacers: (
  0: var(--spacing0),
  // 0px
  2: var(--spacing2),
  // 2px
  4: var(--spacing4),
  // 4px
  6: var(--spacing6),
  // 6px
  8: var(--spacing8),
  // 8px
  10: var(--spacing10),
  // 10px
  12: var(--spacing12),
  // 12px
  16: var(--spacing16),
  // 16px
  20: var(--spacing20),
  // 20px
  24: var(--spacing24),
  // 24px
  28: var(--spacing28),
  // 28px
  32: var(--spacing32),
  // 32px
  36: var(--spacing36),
  // 36px
  40: var(--spacing40),
  // 40px
  44: var(--spacing44),
  // 44px
  48: var(--spacing48),
  // 48px
  52: var(--spacing52),
  // 52px
  56: var(--spacing56),
  // 56px
  60: var(--spacing60),
  // 60px
  64: var(--spacing64),
  // 64px
  80: var(--spacing80),
  // 80px
  96: var(--spacing96),
  // 96px
  112: var(--spacing112),
  // 112px
  160: var(--spacing160),
  // 160px
  200: var(--spacing200),
  // 200px
  300: var(--spacing300), // 300px
);
```

### Padding Utilities

```tsx
// All directions
<div className="p-16">16px padding all sides</div>
<div className="p-24">24px padding all sides</div>

// Horizontal and vertical
<div className="px-20">20px horizontal padding</div>
<div className="py-12">12px vertical padding</div>

// Individual sides
<div className="pt-16">16px top padding</div>
<div className="pb-24">24px bottom padding</div>
<div className="ps-20">20px start (left) padding</div>
<div className="pe-20">20px end (right) padding</div>

// Form elements
<Form.Input
  className="p-16"
  name="field"
  labelText="Field with padding"
  control={control}
/>
```

### Margin Utilities

```tsx
// All directions
<div className="m-16">16px margin all sides</div>
<div className="m-24">24px margin all sides</div>

// Horizontal and vertical
<div className="mx-auto">Centered with auto margins</div>
<div className="my-20">20px vertical margin</div>

// Individual sides
<div className="mt-24">24px top margin</div>
<div className="mb-16">16px bottom margin</div>
<div className="ms-12">12px start (left) margin</div>
<div className="me-12">12px end (right) margin</div>

// Button spacing
<div className="d-flex gap-16">
  <Button className="mb-12">Action 1</Button>
  <Button className="mb-12">Action 2</Button>
</div>
```

### Gap Utilities

```tsx
// Flexbox and grid gaps
<div className="d-flex gap-16">
  <Button>Item 1</Button>
  <Button>Item 2</Button>
  <Button>Item 3</Button>
</div>

<div className="grid gap-24">
  <div className="g-col-6">Column 1</div>
  <div className="g-col-6">Column 2</div>
</div>

// Form layouts
<form className="d-grid gap-20">
  <Form.Input name="field1" control={control} />
  <Form.Input name="field2" control={control} />
  <Form.Input name="field3" control={control} />
</form>
```

## Custom Design Token Utilities

### Custom Gap Utilities

Custom gap utilities based on design tokens for semantic spacing:

```tsx
// Gap utilities with semantic naming
<div className="d-flex spacing-gap-xs">
  <Button>Small gap</Button>
  <Button>Between items</Button>
</div>

<div className="grid spacing-gap-m">
  <div className="g-col-6">Medium gap</div>
  <div className="g-col-6">Between columns</div>
</div>

<AutoLayout className="spacing-gap-l" mode="fit">
  <Card>Large gap</Card>
  <Card>Between cards</Card>
</AutoLayout>

// Available gap utilities:
// spacing-gap-none, spacing-gap-2xs, spacing-gap-xs
// spacing-gap-s, spacing-gap-m, spacing-gap-l
// spacing-gap-Xl, spacing-gap-2Xl
```

### Padding Inline Utilities

```tsx
// Horizontal padding with semantic naming
<Panel className="spacing-pi-m">
  Medium horizontal padding
</Panel>

<Container className="spacing-pi-l">
  Large horizontal padding for wide content
</Container>

<Button className="spacing-pi-xs">
  Compact horizontal padding
</Button>

// Available padding-inline utilities:
// spacing-pi-none, spacing-pi-2xs, spacing-pi-xs
// spacing-pi-s, spacing-pi-m, spacing-pi-l
// spacing-pi-Xl, spacing-pi-2Xl
```

### Padding Block Utilities

```tsx
// Vertical padding with semantic naming
<Panel className="spacing-pb-m">
  Medium vertical padding
</Panel>

<Modal className="spacing-pb-l">
  Large vertical padding for content
</Modal>

<Card className="spacing-pb-s">
  Small vertical padding for compact cards
</Card>

// Available padding-block utilities:
// spacing-pb-none, spacing-pb-2xs, spacing-pb-xs
// spacing-pb-s, spacing-pb-m, spacing-pb-l
// spacing-pb-Xl, spacing-pb-2Xl
```

## Context-Specific Spacing

### Form Spacing

Specialized utilities for custom form wrappers - Fieldset and AutoLayout have built-in spacing:

```tsx
// ✅ CORRECT: Fieldset has built-in form spacing
<Fieldset legend="User Information" columnCount={12}>
  <Form.Input className="g-col-6" name="field1" control={control} />
  <Form.Input className="g-col-6" name="field2" control={control} />
</Fieldset>

// ✅ CORRECT: AutoLayout has built-in spacing
<AutoLayout mode="fit" itemMinSize="200px">
  <Form.Input name="search" control={control} />
  <Button>Search</Button>
</AutoLayout>

// ✅ CORRECT: Custom form wrapper with unified spacing
<div className="grid form-gap align-items-start">
  <Form.Input className="g-col-6" name="firstName" control={control} />
  <Form.Input className="g-col-6" name="lastName" control={control} />
</div>

// ❌ AVOID: Separate utilities when unified works
<div className="grid form-row-gap form-col-gap">
  {/* Use form-gap instead */}
</div>

// Available form utilities (for custom wrappers only):
// form-gap - unified row and column spacing (preferred)
// form-row-gap, form-col-gap - only when different spacing needed
```

### Content Spacing

Utilities for custom content wrappers only - Neuron components (Panel, Container, Accordion, AutoLayout) have built-in spacing:

```tsx
// ✅ CORRECT: Content utilities for custom wrappers
<div className="grid content-gap">
  <Panel className="g-col-6">Panel with built-in padding</Panel>
  <Panel className="g-col-6">Another panel</Panel>
</div>

<div className="content-p">
  <h2>Custom content wrapper</h2>
  <p>Content that needs padding</p>
</div>

// ❌ INCORRECT: Don't add spacing to components with built-in padding
<Panel className="content-p">
  {/* Panel already has internal padding */}
</Panel>

<Container className="content-gap">
  {/* Container already handles spacing */}
</Container>

// ✅ CORRECT: Use unified spacing utilities
<div className="grid content-gap">
  <div className="g-col-6">Column 1</div>
  <div className="g-col-6">Column 2</div>
</div>

// ❌ AVOID: Separate utilities when unified works
<div className="grid content-col-gap content-row-gap">
  {/* Use content-gap instead */}
</div>

// Available content utilities (for custom wrappers only):
// content-gap - unified row and column spacing (preferred)
// content-p - all-around padding for custom content
// content-px, content-py - only when different spacing needed
// content-col-gap, content-row-gap - only when mixed spacing needed
```

### Canvas Spacing

Viewport-level spacing for main layout components (usually handled by Layout components):

```tsx
// Canvas utilities (typically used internally by Layout components)
<Layout className="canvas-px canvas-py">
  Main application layout
</Layout>

// Individual canvas spacing (rarely used directly)
<div className="canvas-pl">Left padding from viewport edge</div>
<div className="canvas-pr">Right padding from viewport edge</div>
<div className="canvas-pt">Top padding from viewport edge</div>
<div className="canvas-pb">Bottom padding from viewport edge</div>

// Available canvas utilities:
// canvas-pl, canvas-pr, canvas-px - horizontal spacing
// canvas-pt, canvas-pb, canvas-py - vertical spacing
// canvas-p - all-around spacing
// canvas-gap - grid gap for main layout
```

## Additional Layout Utilities

### Grid Span Utilities

Direct grid span utilities for custom grid layouts:

```tsx
// Column span utilities (alternative to g-col-*)
<div className="grid">
  <div className="col-span-3">Spans 3 columns</div>
  <div className="col-span-6">Spans 6 columns</div>
  <div className="col-span-3">Spans 3 columns</div>
</div>

// Row span utilities for multi-row layouts
<div className="grid">
  <div className="row-span-2 col-span-4">Spans 2 rows, 4 columns</div>
  <div className="col-span-8">Single row content</div>
</div>

// Available utilities:
// col-span-1 through col-span-12
// row-span-1 through row-span-12
```

### Text Utilities

Typography and text formatting utilities:

```tsx
// Text trimming for better typography
<h1 className="trim-alphabetic">
  Improved text alignment with cap height trimming
</h1>

// Text truncation with ellipsis
<p className="truncate">
  Long text that will be truncated with ellipsis when it overflows
</p>

// Available text utilities:
// trim-alphabetic - trims text box to cap and alphabetic baseline
// truncate - truncates text with ellipsis at 100% width
```

## Spacing Patterns

### Form Layout Spacing

```tsx
// ✅ PREFERRED: Use Fieldset (has built-in form spacing)
<Fieldset legend="Personal Information" columnCount={12}>
  <Form.Input
    className="g-col-12 g-col-md-6"
    name="firstName"
    control={control}
  />
  <Form.Input
    className="g-col-12 g-col-md-6"
    name="lastName"
    control={control}
  />
</Fieldset>

// ✅ ALTERNATIVE: Custom wrapper with unified spacing
<div className="grid form-gap align-items-start">
  <div className="g-col-12">
    <h2 className="mb-24">Form Title</h2>
  </div>

  <Form.Input
    className="g-col-12 g-col-md-6"
    name="firstName"
    control={control}
  />
  <Form.Input
    className="g-col-12 g-col-md-6"
    name="lastName"
    control={control}
  />

  <div className="g-col-12 d-flex gap-16 justify-content-end">
    <SubmitButton control={control}>Submit</SubmitButton>
    <Button variant="secondary">Cancel</Button>
  </div>
</div>

// ❌ AVOID: Separate spacing utilities when unified works
<div className="grid form-row-gap form-col-gap">
  {/* Use form-gap instead */}
</div>
```

### Card Grid with Consistent Spacing

```tsx
// ✅ CORRECT: Cards have built-in padding, use wrapper for grid spacing
<div className="grid content-gap">
  {items.map(item => (
    <Card
      key={item.id}
      className="g-col-12 g-col-sm-6 g-col-md-4"
    >
      <div className="d-flex flex-column gap-12">
        <h3>{item.title}</h3>
        <p>{item.description}</p>
        <div className="d-flex gap-8 mt-auto">
          <Button size="small">Action 1</Button>
          <Button size="small" variant="outline">Action 2</Button>
        </div>
      </div>
    </Card>
  ))}
</div>

// ❌ INCORRECT: Don't add content-p to Card (has built-in padding)
<Card className="content-p">
  {/* Card already has internal padding */}
</Card>
```

### Panel Content Layout

```tsx
// ✅ CORRECT: Panel has built-in padding, use wrapper for internal grid
<Panel>
  <div className="grid content-gap">
    <header className="g-col-12">
      <h2 className="mb-8">Panel Title</h2>
      <p className="mb-0">Panel description</p>
    </header>

    <main className="g-col-12">
      <div className="grid form-gap align-items-start">
        <Form.Input className="g-col-6" name="field1" control={control} />
        <Form.Input className="g-col-6" name="field2" control={control} />
      </div>
    </main>

    <footer className="g-col-12">
      <div className="d-flex justify-content-end gap-16">
        <Button variant="outline">Cancel</Button>
        <Button variant="primary">Save</Button>
      </div>
    </footer>
  </div>
</Panel>

// ❌ INCORRECT: Don't add content-p to Panel (has built-in padding)
<Panel className="content-p">
  {/* Panel already has internal padding */}
</Panel>
```

### Modal Dialog Spacing

```tsx
// ✅ CORRECT: Modal has built-in padding, use wrapper for internal layout
<Modal>
  <div className="grid content-gap">
    <ModalHeader className="g-col-12">
      <h2>Modal Title</h2>
    </ModalHeader>

    <ModalBody className="g-col-12">
      <div className="grid form-gap align-items-start">
        <Form.Input className="g-col-12" name="field" control={control} />
        <Form.TextArea className="g-col-12" name="description" control={control} />
      </div>
    </ModalBody>

    <ModalFooter className="g-col-12">
      <div className="d-flex justify-content-end gap-16">
        <Button variant="outline">Cancel</Button>
        <Button variant="primary">Confirm</Button>
      </div>
    </ModalFooter>
  </div>
</Modal>

// ❌ INCORRECT: Don't add content-p to Modal (has built-in padding)
<Modal className="content-p">
  {/* Modal already has internal padding */}
</Modal>
```

## Best Practices

### Utility Selection

1. **Use Bootstrap utilities for basic spacing** (p-_, m-_, gap-\*) with standard increments
2. **Use custom utilities for semantic spacing** (spacing-gap-_, spacing-pi-_, spacing-pb-\*)
3. **Use unified spacing utilities** (content-gap, form-gap) instead of separate row/column utilities
4. **Apply spacing only to custom wrappers** - Neuron components (Panel, Container, Modal, Accordion, AutoLayout, Fieldset) have built-in padding/spacing
5. **Avoid canvas utilities in components** - these are handled by Layout components
6. **Use grid span utilities** (col-span-_, row-span-_) for direct grid control when g-col-\* utilities don't fit
7. **Apply text utilities** (trim-alphabetic, truncate) for typography enhancement

### Component Spacing Rules

1. **Neuron components have built-in spacing** - Panel, Container, Modal, Accordion, AutoLayout, Fieldset
2. **Use spacing utilities only for custom content wrappers** - div elements with grid/flex layouts
3. **Prefer unified utilities** - content-gap over content-col-gap + content-row-gap
4. **Use form-gap for custom form wrappers** - when not using Fieldset or AutoLayout
5. **Never add spacing classes to components with built-in spacing**

```tsx
// ✅ CORRECT: Components with built-in spacing
<Panel>Content with built-in padding</Panel>
<Container>Content with built-in spacing</Container>
<Fieldset legend="Form">Built-in form spacing</Fieldset>
<AutoLayout>Built-in item spacing</AutoLayout>

// ✅ CORRECT: Custom wrappers need spacing utilities
<div className="grid content-gap">
  <Panel className="g-col-6">Panel 1</Panel>
  <Panel className="g-col-6">Panel 2</Panel>
</div>

// ❌ INCORRECT: Don't add spacing to components with built-in spacing
<Panel className="content-p">Double padding</Panel>
<Container className="content-gap">Already has spacing</Container>
```

### Consistency Guidelines

1. **Maintain consistent spacing ratios** across similar components
2. **Use unified spacing utilities** (content-gap, form-gap) over separate row/column utilities
3. **Apply spacing only to custom wrappers** - not to Neuron components with built-in spacing
4. **Use semantic spacing names** when available over numeric values
5. **Test spacing at different breakpoints** for responsive layouts

### Form-Specific Recommendations

1. **Use Fieldset component** for form layouts (has built-in form spacing)
2. **Use AutoLayout** for dynamic form layouts (has built-in spacing)
3. **Use form-gap for custom form wrappers** when Fieldset/AutoLayout don't apply
4. **Apply align-items-start** to prevent validation message layout breaks
5. **Maintain button spacing** with gap utilities in action areas

### Component Integration

1. **Respect built-in component spacing** - Panel, Container, Modal, Accordion, AutoLayout, Fieldset
2. **Use spacing utilities only for custom div wrappers** that need layout control
3. **Prefer unified spacing utilities** for consistency and simplicity
4. **Test spacing combinations** to ensure visual hierarchy and readability
5. **Apply grid span utilities for complex layouts** - when standard g-col-\* pattern doesn't fit
6. **Use text utilities for typography enhancement** - trim-alphabetic for headings, truncate for overflow text

## Alternative Utility Usage Patterns

### When to Use Grid Span Utilities

```tsx
// ✅ Use col-span-* for direct grid control
<div className="grid content-gap" style={{ gridTemplateColumns: "repeat(16, 1fr)" }}>
  <div className="col-span-4">Custom 16-column grid</div>
  <div className="col-span-12">Remaining space</div>
</div>

// ✅ Use row-span-* for multi-row layouts
<div className="grid content-gap">
  <div className="col-span-6 row-span-2">Tall sidebar</div>
  <div className="col-span-6">Content top</div>
  <div className="col-span-6">Content bottom</div>
</div>
```

### When to Use Text Utilities

```tsx
// ✅ Typography enhancement
<h1 className="trim-alphabetic">
  Better vertical alignment with trimmed text box
</h1>

// ✅ Overflow handling
<div className="g-col-8">
  <p className="truncate">
    Long description that needs to be truncated gracefully
  </p>
</div>

// ✅ Card content with consistent text treatment
<Card>
  <h3 className="trim-alphabetic mb-8">Card Title</h3>
  <p className="truncate">Card description that may overflow</p>
</Card>
```

## Common Spacing Combinations

### Unified Spacing (Preferred)

```tsx
// ✅ PREFERRED: Unified form spacing
className="grid form-gap align-items-start"

// ✅ PREFERRED: Unified content spacing
className="grid content-gap"

// ✅ PREFERRED: Built-in component spacing
<Fieldset legend="Form">Built-in form spacing</Fieldset>
<AutoLayout mode="fit">Built-in content spacing</AutoLayout>
```

### Legacy Spacing (Use Only When Different Row/Column Spacing Needed)

```tsx
// ⚠️ ONLY when different row and column spacing is required
className = "grid content-col-gap form-row-gap";
```

### Individual Element Spacing

```tsx
// ✅ CORRECT: Bootstrap utilities for individual elements
className = "d-flex gap-16"; // Button groups
className = "mb-32 pb-24"; // Section spacing
className = "p-16"; // Custom content padding

// ✅ CORRECT: Text utilities for typography
className = "trim-alphabetic"; // Headings with better alignment
className = "truncate"; // Text overflow handling

// ✅ CORRECT: Grid span utilities for custom layouts
className = "col-span-8"; // Direct column span (not using g-col-*)
className = "row-span-2"; // Multi-row spanning
```

### Built-in Component Spacing Reference

```tsx
// ✅ CORRECT: Components with built-in spacing (no spacing classes needed)
<Panel>Built-in padding</Panel>
<Container>Built-in spacing</Container>
<Modal>Built-in padding</Modal>
<Accordion>Built-in spacing</Accordion>
<Fieldset>Built-in form spacing</Fieldset>
<AutoLayout>Built-in item spacing</AutoLayout>

// ✅ CORRECT: Custom wrappers need spacing classes
<div className="grid content-gap">
  <Panel className="g-col-6">Panel 1</Panel>
  <Panel className="g-col-6">Panel 2</Panel>
</div>
```

This spacing system provides comprehensive and consistent spacing control across all levels of the application, from viewport-level canvas spacing down to individual component padding and margins.

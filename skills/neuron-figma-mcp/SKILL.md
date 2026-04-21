---
name: neuron-figma-mcp
description: Implement Figma designs using Figma MCP and Code Connect for 1:1 accuracy with Neuron UI. Use when converting Figma designs to code. Extracts component mappings, identifies exact Neuron UI components, and ensures design system compliance. CRITICAL: Use exact Figma component names (DatePicker not Input, Switch not CheckBox), verify all components in Neuron UI, use baseIcons only.
---

# Figma to Code with MCP & Code Connect

Implement Figma designs using Figma MCP and Code Connect for 1:1 accuracy.

## Process

1. **Extract Figma code** - Get component names and interfaces
2. **Analyze visual design** - Count all elements and layout
3. **Get Code Connect mapping** - Extract component mappings
4. **Identify components** - Match to exact Neuron UI components
5. **Implement layout** - Use Neuron UI grid system
6. **Verify completeness** - Check all elements implemented

## Figma Exploration

**1. Get Figma code (`get_code`):**

- Extract interface definitions and component function names
- Look for component names in data-name attributes
- Note CodeConnect snippets

**2. Get visual design (`get_image`):**

- Count exact number of fields/components
- Identify layout structure (rows, columns)
- Note component types (dropdowns, toggles, inputs, buttons, icons)
- Document all icons and layout elements

**3. Get Code Connect mapping (`get_code_connect_map`):**

```tsx
const codeConnectMap = await mcp4_get_code_connect_map({
  nodeId: "123:456",
  clientName: "windsurf",
  clientLanguages: "typescript,javascript",
  clientFrameworks: "react",
});
```

## Component Identification

**Priority order:**

1. Figma code component names (highest priority)
2. CodeConnect mapped components
3. Interface/function names in generated code
4. Visual analysis (lowest priority)

**CRITICAL: Use exact Figma component names, not visual appearance**

**Component mapping:**

- Text Input → `Input`
- Number Input → `NumberInput`
- Date Input → `DatePicker` (NOT Input)
- Dropdown → `Select`, `AutoComplete`, or `MultiSelect`
- Phone/Masked → `MaskedInput`
- Toggle → `Switch` (NOT CheckBox)
- Radio Buttons → `RadioSet`
- Checkbox → `CheckBox`
- Buttons → `Button` with baseIcons
- Icons → `baseIcons` ONLY
- Containers → `Panel`, `AccordionPanel`, `Container`
- Layout → Neuron UI grid (`grid`, `content-gap`, `g-col-*`)

## Layout Implementation

**Container types:**

- `Panel` - Static with optional actionZone
- `AccordionPanel` - Collapsible with defaultOpen/open
- `Container` - Basic wrapper
- `Modal` - Overlay with leftActionsZone/rightActionsZone

**Grid layout:**

```tsx
<div className="grid content-gap">
  <div className="g-col-6">Column 1</div>
  <div className="g-col-6">Column 2</div>
</div>
```

**Form layout:**

```tsx
<div className="grid content-col-gap form-row-gap">
  <Input className="g-col-4" name="field1" control={control} />
  <Input className="g-col-4" name="field2" control={control} />
  <Input className="g-col-4" name="field3" control={control} />
</div>
```

**Layout rules:**

- Follow Figma layout structure exactly
- Count all elements per row
- Calculate grid classes (e.g., 6 fields = g-col-2 each)
- Use `grid + content-gap` pattern only
- Never use custom CSS Grid or Flexbox

## Code Connect Usage

```tsx
// 1. Extract mapping
const codeConnectMap = await mcp4_get_code_connect_map({...});

// 2. Use exact component
import { Button } from "@neuron/ui";

// 3. Apply Figma properties
<Button
  variant="primary"
  size="medium"
  iconLeft={<SaveIcon />}
  tooltip="Save data"
>
  {t("actions.save")}
</Button>
```

**Benefits:**

- Automatic prop mapping (design variants → component props)
- 1:1 accuracy (direct Figma-to-React)
- No guessing component APIs

## Critical Rules

**❌ Forbidden:**

- Guessing component types from visual appearance
- Substituting similar components
- Using CheckBox for toggles (use Switch)
- Using Input for dates (use DatePicker)
- Using Input for masked fields (use MaskedInput)
- Custom icons (use baseIcons only)
- Custom CSS (use Neuron UI classes only)
- Custom components (use Neuron UI only)

**✅ Required:**

- Follow Figma component names exactly
- Verify all components exist in Neuron UI
- Check component APIs in .types.ts files
- Count all elements systematically
- Use Neuron UI grid system
- Use baseIcons from `@neuron/ui`
- Import from `@neuron/ui` only

## Examples

**Example 1: Form with DatePicker**

```tsx
<div className="grid form-gap">
  <Input className="g-col-6" name="name" control={control} />
  <DatePicker className="g-col-6" name="date" control={control} />
</div>
```

**Example 2: Panel with Switch**

```tsx
<Panel title="Settings" className="g-col-12">
  <Switch name="enabled" control={control} labelText="Enable feature" />
</Panel>
```

**Example 3: Button with baseIcon**

```tsx
import { Button, baseIcons } from "@neuron/ui";

<Button iconLeft={<baseIcons.Save />} variant="primary">
  {t("actions.save")}
</Button>;
```

## Best Practices

- Check Code Connect mapping first
- Use exact Figma component names
- Verify all components in Neuron UI
- Count and map every element
- Use Neuron UI grid system
- Use baseIcons only
- Add localization
- Never guess component types from appearance
- ❌ Don't use custom icons or CSS
- ❌ Don't create custom implementations

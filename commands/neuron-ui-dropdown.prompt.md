---
agent: agent
---

# Neuron Dropdown Component Guidelines

## For the AI Assistant

This document provides comprehensive guidelines for implementing and configuring the Neuron Dropdown component. It explains proper usage, action configuration, custom templates, and best practices for creating interactive dropdown menus.

## Sync Metadata

- **Component Version:** v4.3.5
- **Component Source:** `packages/neuron/ui/src/lib/buttons/dropdown/Dropdown.tsx`
- **Guideline Command:** `/neuron-ui-dropdown`
- **Related Skill:** `neuron-ui-buttons`

## Introduction

The Dropdown component is a core part of the Neuron UI framework, designed to provide consistent, accessible, and interactive dropdown menus across all Neuron applications.

### What is the Dropdown Component?

The Dropdown component offers standardized menu functionality for your application with support for:

- Multiple button variants (primary, secondary, plain, info, success, warning, danger)
- Different sizes (small, medium, large)
- Icon support (custom icons and icon-only dropdowns)
- Disabled state handling
- Customizable actions with onClick handlers
- Custom templates for complex menu items
- Built-in accessibility features
- TypeScript support with comprehensive prop interfaces

### Key Features

- **Action-Based Architecture**: Uses `actions` array to define menu items with onClick handlers
- **Custom Templates**: Support for complex menu items using custom template functions
- **Icon Integration**: Support for baseIcons from the Neuron icon system
- **Button Integration**: Built on top of the Neuron Button component
- **Accessibility**: Built-in ARIA support and keyboard navigation
- **Focus Styling**: Focus outline položek menu je vyhrazen pouze pro navigaci klávesnicí a při hoveru nad popoverem se skryje
- **TypeScript Support**: Full type safety with comprehensive prop interfaces

**Storybook Reference:** You can refer to the Storybook documentation for visual examples and interactive demos of the Dropdown component.

## Step 1: Basic Dropdown Implementation

### 1.1 Import the Dropdown Component

```tsx
import { Dropdown } from "@neuron/ui";
```

### 1.2 Basic Usage

Here's a simple implementation of the Dropdown component:

```tsx
import { Dropdown, baseIcons } from "@neuron/ui";

const basicActions = [
  {
    labelText: "Edit",
    onClick: () => console.log("Edit clicked"),
    icon: { iconDef: baseIcons.editSolid },
  },
  {
    labelText: "Delete",
    onClick: () => console.log("Delete clicked"),
    icon: { iconDef: baseIcons.trashSolid },
  },
  {
    labelText: "Archive",
    onClick: () => console.log("Archive clicked"),
    disabled: true,
  },
];

const MyComponent = () => <Dropdown buttonText="Actions" actions={basicActions} />;
```

## Step 2: Dropdown Action Configuration

### 2.1 Action Properties

Each action in the `actions` array supports the following properties:

```tsx
interface IDropdownAction {
  labelText: string; // Display text for the menu item
  onClick?: () => void; // Click handler function
  icon?: { iconDef: any }; // Icon configuration
  disabled?: boolean; // Disable the menu item
  template?: (item: MenuItem, options: MenuItemOptions) => JSX.Element; // Custom template
}
```

### 2.2 Standard Actions Example

```tsx
import { Dropdown, baseIcons } from "@neuron/ui";

const standardActions = [
  {
    labelText: "Profile",
    onClick: () => navigateToProfile(),
    icon: { iconDef: baseIcons.userRegular },
  },
  {
    labelText: "Settings",
    onClick: () => openSettings(),
    icon: { iconDef: baseIcons.cogSolid },
  },
  {
    labelText: "Help",
    onClick: () => openHelp(),
    icon: { iconDef: baseIcons.questionCircleRegular },
  },
  {
    labelText: "Logout",
    onClick: () => handleLogout(),
    icon: { iconDef: baseIcons.signOutAltSolid },
  },
];

const UserMenu = () => <Dropdown buttonText="User Menu" actions={standardActions} variant="secondary" />;
```

## Step 3: Button Variants and Sizes

### 3.1 Button Variants

The Dropdown component supports all Button component variants:

```tsx
// Primary dropdown (default)
<Dropdown buttonText="Primary" actions={actions} variant="primary" />

// Secondary dropdown
<Dropdown buttonText="Secondary" actions={actions} variant="secondary" />

// Plain dropdown
<Dropdown buttonText="Plain" actions={actions} variant="plain" />

// Other variants
<Dropdown buttonText="Info" actions={actions} variant="info" />
<Dropdown buttonText="Success" actions={actions} variant="success" />
<Dropdown buttonText="Warning" actions={actions} variant="warning" />
<Dropdown buttonText="Danger" actions={actions} variant="danger" />
```

### 3.2 Button Sizes

```tsx
// Small dropdown
<Dropdown buttonText="Small" actions={actions} size="small" />

// Medium dropdown (default)
<Dropdown buttonText="Medium" actions={actions} size="medium" />

// Large dropdown
<Dropdown buttonText="Large" actions={actions} size="large" />
```

### 3.3 Icon-Only Dropdown

```tsx
<Dropdown
  iconDef={baseIcons.elipsisVerticalSolid}
  buttonText="" // Empty string for icon-only
  actions={actions}
  size="medium"
/>
```

## Step 4: Custom Templates for Complex Menu Items

### 4.1 Understanding Custom Templates

Custom templates allow you to create complex menu items beyond simple text and icons. The template function receives two parameters:

- `item`: The MenuItem object from PrimeReact
- `options`: MenuItemOptions containing className and onClick handlers

### 4.2 Checkbox Template Example

```tsx
import { Dropdown, CheckBox } from "@neuron/ui";
import { useState } from "react";

const CheckboxDropdown = () => {
  const [filters, setFilters] = useState({
    active: false,
    inactive: false,
    archived: false,
  });

  const filterActions = [
    {
      labelText: "Active",
      template: (item, options) => (
        <div
          className={`${options.className} p-8`}
          onClick={(e) => e.stopPropagation()} // Prevent dropdown from closing
          role="menuitem"
        >
          <CheckBox
            name="active"
            labelText="Active"
            checked={filters.active}
            onChange={(e) => setFilters((prev) => ({ ...prev, active: e.target.checked }))}
          />
        </div>
      ),
    },
    {
      labelText: "Inactive",
      template: (item, options) => (
        <div className={`${options.className} p-8`} onClick={(e) => e.stopPropagation()} role="menuitem">
          <CheckBox
            name="inactive"
            labelText="Inactive"
            checked={filters.inactive}
            onChange={(e) => setFilters((prev) => ({ ...prev, inactive: e.target.checked }))}
          />
        </div>
      ),
    },
    {
      labelText: "Archived",
      template: (item, options) => (
        <div className={`${options.className} p-8`} onClick={(e) => e.stopPropagation()} role="menuitem">
          <CheckBox
            name="archived"
            labelText="Archived"
            checked={filters.archived}
            onChange={(e) => setFilters((prev) => ({ ...prev, archived: e.target.checked }))}
          />
        </div>
      ),
    },
  ];

  return <Dropdown buttonText="Filters" actions={filterActions} />;
};
```

### 4.3 Custom Content Template

```tsx
const customActions = [
  {
    labelText: "User Info",
    template: (item, options) => (
      <div className={`${options.className} p-12`} role="menuitem">
        <div className="flex items-center gap-8">
          <div className="w-32 h-32 bg-primary-500 rounded-full flex items-center justify-center">
            <span className="text-white font-bold">JD</span>
          </div>
          <div>
            <div className="font-semibold">John Doe</div>
            <div className="text-sm text-gray-600">john.doe@example.com</div>
          </div>
        </div>
      </div>
    ),
  },
  {
    labelText: "Separator",
    template: (item, options) => <div className="border-t border-gray-200 my-4" role="separator" />,
  },
  {
    labelText: "Logout",
    onClick: () => handleLogout(),
    icon: { iconDef: baseIcons.signOutAltSolid },
  },
];
```

## Step 5: Advanced Configuration

### 5.1 Disabled State

```tsx
// Disable entire dropdown
<Dropdown buttonText="Disabled Dropdown" actions={actions} disabled={true} />;

// Disable specific actions
const actionsWithDisabled = [
  {
    labelText: "Available Action",
    onClick: () => console.log("Available"),
  },
  {
    labelText: "Disabled Action",
    onClick: () => console.log("This won't execute"),
    disabled: true,
  },
];
```

### 5.2 Custom Click Handler

```tsx
<Dropdown
  buttonText="Custom Handler"
  actions={actions}
  onClick={(e) => {
    console.log("Dropdown button clicked");
    // Custom logic before dropdown opens
  }}
/>
```

### 5.3 Test ID Configuration

```tsx
<Dropdown buttonText="Test Dropdown" actions={actions} testId="user-menu-dropdown" />
```

## Step 6: Best Practices

### 6.1 Action Organization

- Group related actions together
- Use separators (via custom templates) for logical grouping
- Place destructive actions (like Delete) at the bottom
- Use icons consistently across similar action types

### 6.2 Template Guidelines

- Always include `role="menuitem"` for accessibility
- Use `e.stopPropagation()` when you don't want the dropdown to close
- Apply consistent padding using design system classes
- Maintain visual hierarchy with proper typography

### 6.3 Performance Considerations

- Memoize action arrays when they don't change frequently
- Use callback functions for onClick handlers to prevent unnecessary re-renders
- Consider lazy loading for complex template content

## Step 7: Complete Example

```tsx
import { Dropdown, baseIcons, CheckBox } from "@neuron/ui";
import { useState } from "react";

const ComprehensiveDropdown = () => {
  const [showArchived, setShowArchived] = useState(false);

  const actions = [
    {
      labelText: "New Item",
      onClick: () => createNewItem(),
      icon: { iconDef: baseIcons.plusSolid },
    },
    {
      labelText: "Import",
      onClick: () => importItems(),
      icon: { iconDef: baseIcons.uploadSolid },
    },
    {
      labelText: "Export",
      onClick: () => exportItems(),
      icon: { iconDef: baseIcons.downloadSolid },
    },
    {
      labelText: "Separator",
      template: (item, options) => <div className="border-t border-gray-200 my-4" role="separator" />,
    },
    {
      labelText: "Show Archived",
      template: (item, options) => (
        <div className={`${options.className} p-8`} onClick={(e) => e.stopPropagation()} role="menuitem">
          <CheckBox
            name="showArchived"
            labelText="Show Archived Items"
            checked={showArchived}
            onChange={(e) => setShowArchived(e.target.checked)}
          />
        </div>
      ),
    },
    {
      labelText: "Settings",
      onClick: () => openSettings(),
      icon: { iconDef: baseIcons.cogSolid },
    },
  ];

  return <Dropdown buttonText="Options" actions={actions} variant="primary" size="medium" testId="options-dropdown" />;
};
```

**For the AI:**

- Always use the `actions` array to define menu items
- Include `labelText` for each action
- Use `onClick` for standard menu item actions
- Use `template` for complex menu items like checkboxes or custom content
- Apply `disabled: true` to disable specific menu items
- Use `icon` property for visual enhancement
- Remember to use `e.stopPropagation()` in templates when you don't want the dropdown to close
- Always include proper accessibility attributes in custom templates

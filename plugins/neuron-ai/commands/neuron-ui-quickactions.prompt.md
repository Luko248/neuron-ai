---
agent: agent
---

# AI-Assisted Neuron QuickActions Component Integration Guide

## For the AI Assistant

Your task is to integrate and configure the Neuron QuickActions component in a React application. This guide provides essential instructions for implementing QuickActions, which provides contextual action menus with responsive behavior across all Neuron applications.

## Sync Metadata

- **Component Version:** v4.3.1
- **Component Source:** `packages/neuron/ui/src/lib/popups/quickActions/QuickActions.tsx`
- **Guideline Command:** `/neuron-ui-quickactions`
- **Related Skill:** `neuron-ui-popups-quick-actions`

## Introduction

The QuickActions component provides a contextual action menu system with responsive adaptation. It displays a set of actions in different formats based on screen size and provides built-in common actions through the `quickActions` helper.

Key features include:

- **Responsive Design** - Popover on desktop, dialog on mobile/tablet screens
- **Pre-built Actions** - Built-in copy and open-in-new-tab functionality
- **Custom Actions** - Support for application-specific actions
- **Access Control** - Integrated with Neuron auth system
- **Icon Integration** - Uses baseIcons for consistent iconography
- **Internationalization** - Built-in i18n support for action labels
- **Touch Optimization** - Touch-friendly interface on mobile devices

### Key Features

- **Responsive Behavior**: Automatically switches between popover (desktop) and dialog (mobile/tablet)
- **Built-in Actions**: Pre-configured copy and open-in-new-tab actions
- **Custom Actions**: Support for application-specific action implementations
- **Access Control**: Integration with Neuron auth for permission-based display
- **Icon Support**: baseIcons integration for consistent visual design
- **Smart Layout**: Displays up to 3 actions in popover, rest in dropdown menu
- **Touch Friendly**: Optimized for mobile and tablet interactions

**Storybook Reference:** You can refer to the Storybook documentation for visual examples and interactive demos of all available QuickActions configurations.

## Step 1: Basic QuickActions Implementation

### 1.1 Import the QuickActions Component

```tsx
import { QuickActions, quickActions } from "@neuron/ui";
```

### 1.2 Basic Usage with Pre-built Actions

Use the built-in `quickActions` helper for common functionality:

```tsx
import { QuickActions, quickActions } from "@neuron/ui";

const BasicQuickActions = () => {
  const actions = [
    quickActions.copy({ value: "Text to copy" }),
    quickActions.openInNewTab({ url: "https://example.com" }),
  ];

  return (
    <QuickActions actions={actions} targetValue="Data Item">
      <div>Content with actions</div>
    </QuickActions>
  );
};
```

**Note**: `targetValue` provides context in the mobile dialog header, describing what the actions will operate on (e.g., "User Profile", "Invoice #12345", or "Configuration Settings").

### 1.3 Responsive Behavior

The component automatically adapts based on screen size:

**Desktop/Large screens:**

- Displays as popover next to trigger
- Shows up to 3 actions as icon buttons
- Additional actions appear in dropdown menu

**Tablet/Mobile screens:**

- Displays as modal dialog
- Shows all actions as full-width buttons with text labels
- Dialog includes target value as descriptive header

```tsx
import { QuickActions, quickActions } from "@neuron/ui";

const ResponsiveQuickActions = () => {
  const actions = [
    quickActions.copy({ value: "Copy this text" }),
    quickActions.openInNewTab({ url: "https://example.com" }),
    {
      labelText: "Custom Action",
      iconDef: baseIcons.editRegular,
      eventHandler: () => console.info("Custom action triggered"),
    },
  ];

  return (
    <QuickActions actions={actions} targetValue="Responsive Item">
      <span>Item with responsive actions</span>
    </QuickActions>
  );
};
```

## Step 2: Built-in Actions

### 2.1 Copy Action

Use the `quickActions.copy()` helper for clipboard functionality:

```tsx
import { QuickActions, quickActions } from "@neuron/ui";

const CopyActions = () => {
  const actions = [
    // Copy string value
    quickActions.copy({ value: "Text to copy" }),

    // Copy number value
    quickActions.copy({ value: 12345 }),

    // Copy complex data (converted to string)
    quickActions.copy({ value: JSON.stringify({ id: 1, name: "Item" }) }),
  ];

  return (
    <QuickActions actions={actions} targetValue="Copyable Data">
      <div>Content with copy actions</div>
    </QuickActions>
  );
};
```

### 2.2 Open in New Tab Action

Use the `quickActions.openInNewTab()` helper for navigation:

```tsx
import { QuickActions, quickActions } from "@neuron/ui";

const NavigationActions = () => {
  const actions = [
    // Open external URL
    quickActions.openInNewTab({ url: "https://example.com" }),

    // Open internal application URL
    quickActions.openInNewTab({ url: "/internal/path" }),

    // Open with dynamic URL
    quickActions.openInNewTab({ url: `https://api.example.com/item/${itemId}` }),
  ];

  return (
    <QuickActions actions={actions} targetValue="Navigation Item">
      <div>Content with navigation actions</div>
    </QuickActions>
  );
};
```

## Step 3: Custom Actions

### 3.1 Creating Custom Actions

Define custom actions with icons and event handlers:

```tsx
import { QuickActions, baseIcons } from "@neuron/ui";

const CustomActions = () => {
  const actions = [
    {
      labelText: "Edit Item",
      iconDef: baseIcons.editRegular,
      eventHandler: () => {
        console.info("Edit action triggered");
        // Your edit logic here
      },
    },
    {
      labelText: "Delete Item",
      iconDef: baseIcons.trashRegular,
      eventHandler: () => {
        console.info("Delete action triggered");
        // Your delete logic here
      },
    },
    {
      labelText: "View Details",
      iconDef: baseIcons.eyeRegular,
      eventHandler: () => {
        console.info("View action triggered");
        // Your view logic here
      },
    },
  ];

  return (
    <QuickActions actions={actions} targetValue="Custom Item">
      <div>Content with custom actions</div>
    </QuickActions>
  );
};
```

### 3.2 Actions with Translation Keys

Use i18n translation keys for internationalized labels:

```tsx
import { QuickActions, baseIcons } from "@neuron/ui";

const InternationalizedActions = () => {
  const actions = [
    {
      labelTranslationKey: "app.actions.edit",
      iconDef: baseIcons.editRegular,
      eventHandler: () => console.info("Edit triggered"),
    },
    {
      labelTranslationKey: "app.actions.delete",
      iconDef: baseIcons.trashRegular,
      eventHandler: () => console.info("Delete triggered"),
    },
  ];

  return (
    <QuickActions actions={actions} targetValue="Internationalized Item">
      <div>Content with i18n actions</div>
    </QuickActions>
  );
};
```

### 3.3 Disabled Actions

Control action availability with the `disabled` prop:

```tsx
import { QuickActions, baseIcons } from "@neuron/ui";

const ConditionalActions = ({ canEdit, canDelete }) => {
  const actions = [
    {
      labelText: "Edit Item",
      iconDef: baseIcons.editRegular,
      disabled: !canEdit,
      eventHandler: () => console.info("Edit triggered"),
    },
    {
      labelText: "Delete Item",
      iconDef: baseIcons.trashRegular,
      disabled: !canDelete,
      eventHandler: () => console.info("Delete triggered"),
    },
  ];

  return (
    <QuickActions actions={actions} targetValue="Conditional Item">
      <div>Content with conditional actions</div>
    </QuickActions>
  );
};
```

## Step 4: Access Control Integration

### 4.1 Permission-based Actions

Use Neuron auth integration for permission-based action display:

```tsx
import { QuickActions, quickActions, baseIcons } from "@neuron/ui";

const PermissionBasedActions = () => {
  const actions = [
    quickActions.copy({ value: "Public data" }),
    {
      labelText: "Edit Item",
      iconDef: baseIcons.editRegular,
      eventHandler: () => console.info("Edit triggered"),
    },
    {
      labelText: "Delete Item",
      iconDef: baseIcons.trashRegular,
      eventHandler: () => console.info("Delete triggered"),
    },
  ];

  return (
    <QuickActions actions={actions} targetValue="Protected Item" readonlyAccess="view.item" fullAccess="edit.item">
      <div>Content with permission-based actions</div>
    </QuickActions>
  );
};
```

### 4.2 Role-based Action Filtering

Filter actions based on user permissions:

```tsx
import { QuickActions, quickActions, baseIcons } from "@neuron/ui";

const RoleBasedActions = ({ userRole }) => {
  const baseActions = [quickActions.copy({ value: "Item data" }), quickActions.openInNewTab({ url: "/item/details" })];

  const adminActions = [
    {
      labelText: "Admin Panel",
      iconDef: baseIcons.gearRegular,
      eventHandler: () => console.info("Admin panel opened"),
    },
  ];

  const actions = userRole === "admin" ? [...baseActions, ...adminActions] : baseActions;

  return (
    <QuickActions actions={actions} targetValue="Role-based Item">
      <div>Content with role-based actions</div>
    </QuickActions>
  );
};
```

## Step 5: QuickActions Props Reference

### 5.1 Core Props

| Prop          | Type              | Default | Description                                                                                                      |
| ------------- | ----------------- | ------- | ---------------------------------------------------------------------------------------------------------------- |
| `actions`     | `QuickAction[]`   | -       | Array of actions to display                                                                                      |
| `children`    | `ReactNode`       | -       | Content that triggers the actions menu                                                                           |
| `targetValue` | `string`          | -       | Descriptive label displayed in mobile dialog header (e.g., form field label, item title, or content description) |
| `position`    | `PopoverPosition` | -       | Positioning configuration for anchor positioning                                                                 |
| `className`   | `string`          | -       | Additional CSS class for styling                                                                                 |

### 5.2 Access Control Props

| Prop             | Type     | Default | Description                        |
| ---------------- | -------- | ------- | ---------------------------------- |
| `readonlyAccess` | `string` | -       | Permission key for readonly access |
| `fullAccess`     | `string` | -       | Permission key for full access     |

### 5.3 QuickAction Interface

| Property              | Type                           | Default | Description                                         |
| --------------------- | ------------------------------ | ------- | --------------------------------------------------- |
| `labelText`           | `string`                       | -       | Display text for the action                         |
| `labelTranslationKey` | `TFuncKey`                     | -       | i18n translation key for label                      |
| `iconDef`             | `IconDefinition \| TBaseIcons` | -       | Icon to display (baseIcons recommended)             |
| `disabled`            | `boolean`                      | `false` | Whether the action is disabled                      |
| `collapsed`           | `boolean`                      | `false` | Whether the action appears in dropdown (deprecated) |
| `eventHandler`        | `function`                     | -       | Function called when action is triggered            |

### 5.4 Built-in Actions

| Action                        | Parameters                    | Description                  |
| ----------------------------- | ----------------------------- | ---------------------------- |
| `quickActions.copy()`         | `{ value: string \| number }` | Copies value to clipboard    |
| `quickActions.openInNewTab()` | `{ url: string }`             | Opens URL in new browser tab |

## Step 6: Responsive Behavior Details

### 6.1 Desktop/Large Screen Behavior

**Popover Display:**

- Appears as floating popover next to content
- Shows up to 3 actions as icon-only buttons with tooltips
- Additional actions appear in dropdown menu with ellipsis trigger
- Compact, space-efficient design

```tsx
// Desktop: Shows as icon buttons in popover
<QuickActions actions={manyActions} targetValue="Desktop Item">
  <span>Hover for popover</span>
</QuickActions>
```

### 6.2 Tablet/Mobile Screen Behavior

**Dialog Display:**

- Opens as modal dialog overlay
- Shows all actions as full-width buttons with text labels
- Includes target value as descriptive dialog header
- Touch-optimized button sizes

```tsx
// Mobile: Shows as modal dialog with text buttons
<QuickActions actions={manyActions} targetValue="Mobile Item">
  <span>Tap for dialog</span>
</QuickActions>
```

### 6.3 Responsive Breakpoint

The component uses the "tablet" breakpoint from Neuron's responsive system:

- **Desktop**: Screen width > tablet breakpoint → Popover
- **Mobile/Tablet**: Screen width ≤ tablet breakpoint → Dialog

## Step 7: Common Use Cases

### 7.1 Data Grid Actions

Actions for data grid rows or items:

```tsx
import { QuickActions, quickActions, baseIcons } from "@neuron/ui";

const DataGridActions = ({ item }) => {
  const actions = [
    quickActions.copy({ value: item.id }),
    quickActions.openInNewTab({ url: `/items/${item.id}` }),
    {
      labelText: "Edit Item",
      iconDef: baseIcons.editRegular,
      eventHandler: () => openEditModal(item),
    },
    {
      labelText: "Delete Item",
      iconDef: baseIcons.trashRegular,
      eventHandler: () => confirmDelete(item),
    },
  ];

  return (
    <QuickActions actions={actions} targetValue={item.name}>
      <span>{item.name}</span>
    </QuickActions>
  );
};
```

### 7.2 Form Field Actions

Actions for form inputs in literal view:

```tsx
import { QuickActions, quickActions } from "@neuron/ui";
import { Form } from "@neuron/ui";

const FormFieldActions = () => {
  const actions = [
    quickActions.copy({ value: "Field value" }),
    {
      labelText: "View History",
      iconDef: baseIcons.clockRotateLeftRegular,
      eventHandler: () => showFieldHistory(),
    },
  ];

  return (
    <QuickActions actions={actions} targetValue="Field Value">
      <Form.Input labelText="Input Field" name="field" control={control} literalView />
    </QuickActions>
  );
};
```

### 7.3 Content Card Actions

Actions for card-based layouts or content sections:

```tsx
import { QuickActions, quickActions, baseIcons } from "@neuron/ui";

const ContentCardActions = ({ content }) => {
  const actions = [
    quickActions.copy({ value: content.url }),
    quickActions.openInNewTab({ url: content.url }),
    {
      labelText: "Share Content",
      iconDef: baseIcons.shareRegular,
      eventHandler: () => shareContent(content),
    },
    {
      labelText: "Download",
      iconDef: baseIcons.downloadRegular,
      eventHandler: () => downloadContent(content),
    },
  ];

  return (
    <div className="content-card">
      <QuickActions actions={actions} targetValue={content.title}>
        <div className="content-body">
          <h3>{content.title}</h3>
          <p>{content.description}</p>
        </div>
      </QuickActions>
    </div>
  );
};
```

## Step 8: Best Practices

### 8.1 Action Organization

**Order actions by frequency of use:**

```tsx
{
  /* ✅ CORRECT: Most common actions first */
}
const actions = [
  quickActions.copy({ value: data }), // Most common
  quickActions.openInNewTab({ url: detailUrl }), // Common
  editAction, // Less common
  deleteAction, // Least common/destructive
];
```

### 8.2 Icon Selection

**Use appropriate baseIcons for actions:**

```tsx
{
  /* ✅ CORRECT: Semantic icon usage */
}
const actions = [
  {
    labelText: "Edit",
    iconDef: baseIcons.editRegular, // Edit icon for edit action
    eventHandler: handleEdit,
  },
  {
    labelText: "Delete",
    iconDef: baseIcons.trashRegular, // Trash icon for delete action
    eventHandler: handleDelete,
  },
  {
    labelText: "View Details",
    iconDef: baseIcons.eyeRegular, // Eye icon for view action
    eventHandler: handleView,
  },
];
```

### 8.3 Target Value Guidelines

**Provide meaningful target values for mobile dialog headers:**

```tsx
{/* ✅ CORRECT: Descriptive target values for different contexts */}

{/* Form field context */}
<QuickActions
  actions={actions}
  targetValue="Email Address"
>
  <Form.Input name="email" labelText="Email Address" />
</QuickActions>

{/* User/Item context */}
<QuickActions
  actions={actions}
  targetValue="User: John Doe (ID: 12345)"
>
  <UserCard user={user} />
</QuickActions>

{/* Document/File context */}
<QuickActions
  actions={actions}
  targetValue="Invoice INV-2024-001"
>
  <DocumentCard document={invoice} />
</QuickActions>

{/* Configuration context */}
<QuickActions
  actions={actions}
  targetValue="Database Configuration"
>
  <ConfigSection config={dbConfig} />
</QuickActions>

{/* ❌ WRONG: Generic or unclear target values */}
<QuickActions
  actions={actions}
  targetValue="Item" // Too generic
>
  <UserCard user={user} />
</QuickActions>

<QuickActions
  actions={actions}
  targetValue="" // Empty value
>
  <DocumentCard document={invoice} />
</QuickActions>
```

### 8.4 Action Count Considerations

**Consider responsive behavior when designing actions:**

```tsx
{
  /* ✅ CORRECT: Thoughtful action count */
}
const actions = [
  quickActions.copy({ value: data }),
  quickActions.openInNewTab({ url: detailUrl }),
  primaryAction, // Up to 3 show in popover
  secondaryAction, // 4+ show in dropdown menu
  tertiaryAction,
];
```

## Step 9: Common Mistakes to Avoid

### 9.1 Don't Use Too Many Actions

**Limit actions to essential functionality:**

```tsx
{
  /* ❌ WRONG: Too many actions overwhelming the user */
}
const tooManyActions = [action1, action2, action3, action4, action5, action6, action7, action8, action9, action10];

{
  /* ✅ CORRECT: Essential actions only */
}
const essentialActions = [quickActions.copy({ value: data }), editAction, deleteAction];
```

### 9.2 Don't Forget Mobile Experience

**Always consider how actions appear in mobile dialog:**

```tsx
{
  /* ❌ WRONG: Ignoring mobile dialog context */
}
<QuickActions
  actions={actions}
  targetValue="" // Empty target value provides no context
>
  <span>Content</span>
</QuickActions>;

{
  /* ✅ CORRECT: Mobile-friendly target value that describes the content */
}
<QuickActions
  actions={actions}
  targetValue="Customer Record #12345" // Clear identification of what's being acted upon
>
  <span>Customer Data</span>
</QuickActions>;

{
  /* ✅ CORRECT: Form field target value */
}
<QuickActions
  actions={actions}
  targetValue="Phone Number" // Field label for form context
>
  <Form.Input name="phone" labelText="Phone Number" literalView />
</QuickActions>;
```

### 9.3 Don't Use Inconsistent Icons

**Use baseIcons consistently throughout your application:**

```tsx
{
  /* ❌ WRONG: Mixing icon systems */
}
const actions = [
  {
    labelText: "Edit",
    iconDef: baseIcons.editRegular, // baseIcons
    eventHandler: handleEdit,
  },
  {
    labelText: "Delete",
    iconDef: faTrash, // FontAwesome direct import
    eventHandler: handleDelete,
  },
];

{
  /* ✅ CORRECT: Consistent baseIcons usage */
}
const actions = [
  {
    labelText: "Edit",
    iconDef: baseIcons.editRegular,
    eventHandler: handleEdit,
  },
  {
    labelText: "Delete",
    iconDef: baseIcons.trashRegular,
    eventHandler: handleDelete,
  },
];
```

### 9.4 Don't Ignore Accessibility

**Provide proper labels and consider keyboard navigation:**

```tsx
{
  /* ❌ WRONG: Missing or poor labels */
}
const actions = [
  {
    labelText: "Action", // Vague label
    iconDef: baseIcons.gearRegular,
    eventHandler: handleAction,
  },
];

{
  /* ✅ CORRECT: Clear, descriptive labels */
}
const actions = [
  {
    labelText: "Open Settings Panel", // Clear action description
    iconDef: baseIcons.gearRegular,
    eventHandler: handleAction,
  },
];
```

### 9.5 Don't Forget Error Handling

**Handle action failures gracefully:**

```tsx
{
  /* ✅ CORRECT: Proper error handling */
}
const actions = [
  {
    labelText: "Delete Item",
    iconDef: baseIcons.trashRegular,
    eventHandler: async () => {
      try {
        await deleteItem(item.id);
        showSuccessMessage("Item deleted successfully");
      } catch (error) {
        showErrorMessage("Failed to delete item");
      }
    },
  },
];
```

## Key Takeaways

The Neuron QuickActions component provides contextual action menus with responsive behavior. Key points to remember:

1. **Responsive Design** automatically adapts between popover (desktop) and dialog (mobile/tablet)
2. **Built-in Actions** provide common functionality like copy and open-in-new-tab
3. **Custom Actions** support application-specific functionality with baseIcons
4. **Access Control** integrates with Neuron auth for permission-based display
5. **Smart Layout** shows up to 3 actions in popover, additional actions in dropdown
6. **Target Values** provide descriptive context in mobile dialog headers (form labels, item titles, content descriptions)
7. **Icon Consistency** use baseIcons for all action icons
8. **Action Organization** order by frequency of use and importance
9. **Mobile Optimization** considers touch-friendly interactions and clear labeling
10. **Error Handling** implement proper error handling for action failures

By following these guidelines, you'll create intuitive, responsive action menus that work seamlessly across all device types in your Neuron applications.

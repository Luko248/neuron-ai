---
agent: agent
---

# Neuron ConfirmDialog Component Guidelines

## For the AI Assistant

This document provides comprehensive guidelines for implementing and configuring the Neuron ConfirmDialog component. It explains proper usage, variant selection, action button configuration, and best practices for creating accessible confirmation dialogs.

## Sync Metadata

- **Component Version:** v2.0.0
- **Component Source:** `packages/neuron/ui/src/lib/overlays/confirmDialog/ConfirmDialog.tsx`
- **Guideline Command:** `/neuron-ui-confirmdialog`
- **Related Skill:** `neuron-ui-overlays`

## Introduction

The ConfirmDialog component provides a specialized confirmation dialog built on top of the Modal component. It serves as a critical interface element for capturing user confirmation before executing important or destructive actions within Neuron applications.

### What is the ConfirmDialog Component?

The ConfirmDialog component creates overlay dialogs specifically designed for confirmation workflows. It provides standardized dialog rendering with support for:

- **Pre-configured size** - Fixed small size optimized for confirmation messages
- **Two-button pattern** - Confirm and Cancel actions with proper styling
- **Special variants** - Built-in logout and logout_observed variants for authentication
- **Provider pattern** - Centralized dialog management using ConfirmDialogProvider
- **Accessibility compliance** - Full WCAG 2.1 AA standards support
- **Simplified API** - Focused confirmation-specific props without modal complexity
- **Icon support** - Optional icons for visual context and severity indication

### Key Features

- **Three Variant Types**: base (general), logout, logout_observed (authentication)
- **Two Action Buttons**: Confirm (primary) and Cancel (secondary) with configurable props
- **Provider Pattern**: ConfirmDialogProvider and useConfirmDialog hook for centralized management
- **Modal Integration**: Built on top of Neuron Modal with fixed configuration
- **TypeScript Support**: Full type safety with comprehensive prop definitions
- **Localization Ready**: Built-in translations for logout variants

**Storybook Reference:** You can refer to the Storybook documentation for visual examples and interactive demos of all available ConfirmDialog configurations.

## Key Features

- **Simplified Confirmation**: Focused API designed specifically for confirm/cancel workflows.
- **Variant Support**: Supports `base`, `logout`, and `logout_observed` variants.
- **Action Buttons**: Provides `confirmButtonProps` and `cancelButtonProps` for button configuration.
- **Provider Pattern**: Centralized dialog management with `ConfirmDialogProvider` and `useConfirmDialog` hook.
- **Accessibility**: Fully compliant with WCAG 2.1 AA standards.
- **Modal Foundation**: Built on top of Modal component with fixed small size and non-draggable behavior.

## Usage

### Basic Example

```tsx
import { ConfirmDialog, Button } from "@neuron/ui";
import { useState } from "react";

const BasicConfirmDialog = () => {
  const [showConfirm, setShowConfirm] = useState(false);

  return (
    <>
      <Button onClick={() => setShowConfirm(true)}>Delete Item</Button>

      <ConfirmDialog
        show={showConfirm}
        onHide={() => setShowConfirm(false)}
        title="Delete Item"
        message="Are you sure you want to delete this item? This action cannot be undone."
        confirmButtonProps={{
          children: "Delete",
          onClick: () => {
            // Handle delete action
            setShowConfirm(false);
          },
        }}
        cancelButtonProps={{
          children: "Cancel",
          onClick: () => setShowConfirm(false),
        }}
      />
    </>
  );
};
```

### Confirm Only (No Cancel Button)

```tsx
import { ConfirmDialog } from "@neuron/ui";

const ConfirmOnlyDialog = ({ show, onHide, onConfirm }) => (
  <ConfirmDialog
    show={show}
    onHide={onHide}
    title="Action Completed"
    message="The operation was successful."
    confirmButtonProps={{
      children: "OK",
      onClick: onConfirm,
    }}
    cancelButtonProps={{
      onClick: undefined,
      children: undefined,
    }}
  />
);
```

### Provider Pattern Example

```tsx
import { ConfirmDialogProvider, useConfirmDialog, Button } from "@neuron/ui";

const DeleteButton = () => {
  const { openConfirmDialog } = useConfirmDialog();

  const handleDelete = () => {
    openConfirmDialog({
      show: true,
      title: "Delete Record",
      message: "Are you sure you want to delete this record?",
      confirmButtonProps: {
        children: "Delete",
        onClick: () => {
          // Handle delete
          console.log("Item deleted");
        },
      },
      cancelButtonProps: {
        children: "Cancel",
      },
    });
  };

  return <Button onClick={handleDelete}>Delete</Button>;
};

const App = () => (
  <ConfirmDialogProvider>
    <DeleteButton />
  </ConfirmDialogProvider>
);
```

### Logout Variants

```tsx
import { ConfirmDialog } from "@neuron/ui";

// Standard logout variant
const LogoutDialog = ({ show, onHide, onConfirm }) => (
  <ConfirmDialog
    show={show}
    variant="logout"
    confirmButtonProps={{
      onClick: onConfirm,
    }}
    cancelButtonProps={{
      onClick: onHide,
    }}
  />
);

// Logout observed variant (for tabs being monitored)
const LogoutObservedDialog = ({ show, onHide, onConfirm }) => (
  <ConfirmDialog
    show={show}
    variant="logout_observed"
    confirmButtonProps={{
      onClick: onConfirm,
    }}
    cancelButtonProps={{
      onClick: onHide,
    }}
  />
);
```

### Props Overview

| Prop                 | Type                                      | Description                                                                   |
| -------------------- | ----------------------------------------- | ----------------------------------------------------------------------------- |
| `show`               | `boolean`                                 | Controls the visibility of the confirm dialog.                                |
| `variant`            | `"base" \| "logout" \| "logout_observed"` | Dialog variant. Logout variants use pre-configured translations.              |
| `title`              | `string`                                  | The title of the dialog (not used in logout variants).                        |
| `message`            | `string \| ReactNode`                     | The message content displayed in the dialog body.                             |
| `confirmButtonProps` | `ButtonProps`                             | Props for the confirm button (children and onClick required).                 |
| `cancelButtonProps`  | `ButtonProps` (optional)                  | Props for the cancel button. Omit children and onClick to hide cancel button. |
| `icon`               | `IconProps` (optional)                    | Optional icon displayed in the dialog header.                                 |
| `onHide`             | `() => void`                              | Callback invoked when the dialog is hidden.                                   |
| `testId`             | `string` (optional)                       | Custom test ID for the component.                                             |

## Best Practices

1. **Use for Destructive Actions**: ConfirmDialog is ideal for confirming destructive or irreversible actions (delete, logout, data loss).
2. **Clear Message Text**: Provide clear, concise messages that explain the action and consequences.
3. **Button Labels**: Use action-oriented button labels ("Delete", "Save", "Logout") instead of generic "Yes/No".
4. **Provider Pattern**: Use ConfirmDialogProvider for centralized dialog management across your application.
5. **Cancel Button**: Always provide a cancel option unless the dialog is purely informational.
6. **Localization**: Use `t()` function for all user-visible text in base variant.
7. **Logout Variants**: Use built-in logout variants for authentication workflows instead of creating custom logout dialogs.

## Testing Guidelines

1. **Unit Tests**:

   - Verify that the dialog renders correctly with required props.
   - Test visibility toggling using the `show` prop.
   - Ensure `confirmButtonProps.onClick` and `cancelButtonProps.onClick` callbacks are triggered.

2. **Integration Tests**:

   - Test interaction with confirm and cancel buttons.
   - Verify that the dialog closes after button clicks.
   - Test provider pattern with `useConfirmDialog` hook.

3. **Accessibility Tests**:

   - Ensure the dialog is focus-trapped when open.
   - Verify that the `title` and `message` are announced by screen readers.
   - Test keyboard navigation (Tab between buttons).

4. **Visual Regression Tests**:
   - Test dialog appearance across different variants (`base`, `logout`, `logout_observed`).
   - Verify button styling and placement.

## Accessibility Checklist

- **Keyboard Navigation**:

  - Ensure the dialog is focus-trapped and focus returns to the triggering element when closed.
  - Verify that all buttons are keyboard accessible.

- **Screen Reader Support**:

  - Provide meaningful `title` and `message` props.
  - Ensure the dialog is announced as a dialog when opened.

- **Color Contrast**:

  - Verify that text and buttons meet WCAG 2.1 AA contrast requirements.

- **Dismissal Options**:
  - The dialog can only be dismissed via the Confirm or Cancel buttons. There is no close (X) button, no Escape key dismissal, and no backdrop click dismissal.

## Performance Considerations

1. **Lazy Loading**:

   - Consider lazy loading ConfirmDialog for modals that are rarely used.

2. **Minimize Re-renders**:

   - Avoid passing inline functions as props to prevent unnecessary re-renders.
   - Use `useCallback` for button onClick handlers.

3. **Provider Pattern**:
   - Use ConfirmDialogProvider at the root level to avoid multiple dialog instances.

## Integration with Other Components

- **Buttons**: Use Neuron's `Button` component for triggering confirm dialogs.
- **Icons**: Leverage the `Icon` component for visual context in the dialog header.
- **Modal**: ConfirmDialog is built on top of the Modal component with fixed configuration.

## Related Guidelines

- Modal Component Guidelines (`/neuron-ui-modal`)
- Button Component Guidelines (`/neuron-ui-button`)
- Icon Component Guidelines (`/neuron-ui-icon`)
- Accessibility Standards

## Technical Notes

- The `ConfirmDialog` component is built on top of the `Modal` component with fixed `size="sm"`, `draggable={false}`, `closable={false}`, `closeOnEscape={false}`, and `closeOnBackdropClick={false}`. These props are not configurable — the dialog can only be dismissed via action buttons.
- Logout variants automatically use localized text from `neuron.ui.confirm.*` translation keys.
- When `cancelButtonProps.children` is undefined, the cancel button is not rendered.

## 📝 Understanding Variants

### Base Variant (Default)

The base variant is used for general confirmation dialogs. You must provide:

- `title`: Dialog title
- `message`: Confirmation message
- `confirmButtonProps.children`: Confirm button text
- `cancelButtonProps.children`: Cancel button text (optional)

```tsx
<ConfirmDialog
  show={show}
  variant="base" // or omit for default
  title="Delete Record"
  message="Are you sure you want to delete this record?"
  confirmButtonProps={{
    children: "Delete",
    onClick: handleConfirm,
  }}
  cancelButtonProps={{
    children: "Cancel",
    onClick: handleCancel,
  }}
/>
```

### Logout Variant

Used for standard logout confirmation. All text is automatically localized:

- Title: "Log Out" (from translations)
- Message: "Are you sure you want to log out?" (from translations)
- Cancel button: "Cancel" (from translations)
- Confirm button: "Log Out" (from translations)

```tsx
<ConfirmDialog
  show={show}
  variant="logout"
  confirmButtonProps={{
    onClick: handleLogout,
  }}
  cancelButtonProps={{
    onClick: handleCancel,
  }}
/>
```

### Logout Observed Variant

Used when logging out of a monitored/observed tab. All text is automatically localized with different message:

- Title: "Log Out" (from translations)
- Message: "You are about to log out from an observed session." (from translations)
- Cancel button: "Cancel" (from translations)
- Confirm button: "Log Out" (from translations)

```tsx
<ConfirmDialog
  show={show}
  variant="logout_observed"
  confirmButtonProps={{
    onClick: handleLogout,
  }}
  cancelButtonProps={{
    onClick: handleCancel,
  }}
/>
```

## 🎯 Common Use Cases

### Delete Confirmation

```tsx
const DeleteConfirmation = ({ itemName, onConfirm, onCancel }) => {
  const [show, setShow] = useState(false);
  const { t } = useTranslation("translation");

  return (
    <>
      <Button onClick={() => setShow(true)}>{t("common.delete")}</Button>

      <ConfirmDialog
        show={show}
        onHide={() => setShow(false)}
        title={t("confirm.deleteTitle")}
        message={t("confirm.deleteMessage", { item: itemName })}
        icon={{ iconDef: baseIcons.circleExclamationSolid, size: IconSize.base }}
        confirmButtonProps={{
          children: t("common.delete"),
          onClick: () => {
            onConfirm();
            setShow(false);
          },
        }}
        cancelButtonProps={{
          children: t("common.cancel"),
          onClick: () => {
            onCancel();
            setShow(false);
          },
        }}
      />
    </>
  );
};
```

### Unsaved Changes Warning

```tsx
const UnsavedChangesDialog = ({ hasChanges, onSave, onDiscard, onCancel }) => {
  const { t } = useTranslation("translation");

  if (!hasChanges) return null;

  return (
    <ConfirmDialog
      show={hasChanges}
      onHide={onCancel}
      title={t("confirm.unsavedChanges")}
      message={t("confirm.unsavedChangesMessage")}
      confirmButtonProps={{
        children: t("common.save"),
        onClick: onSave,
      }}
      cancelButtonProps={{
        children: t("common.discardChanges"),
        onClick: onDiscard,
      }}
    />
  );
};
```

### Success Notification (Confirm Only)

```tsx
const SuccessNotification = ({ show, onClose }) => {
  const { t } = useTranslation("translation");

  return (
    <ConfirmDialog
      show={show}
      onHide={onClose}
      title={t("success.operationComplete")}
      message={t("success.operationCompleteMessage")}
      icon={{ iconDef: baseIcons.circleCheckSolid, size: IconSize.base }}
      confirmButtonProps={{
        children: t("common.ok"),
        onClick: onClose,
      }}
      cancelButtonProps={{
        onClick: undefined,
        children: undefined,
      }}
    />
  );
};
```

## Common Mistakes to Avoid

### ❌ Don't Use Without Localization

```tsx
{
  /* Wrong: Hardcoded text */
}
<ConfirmDialog
  title="Delete Item"
  message="Are you sure?"
  confirmButtonProps={{ children: "Delete" }}
  cancelButtonProps={{ children: "Cancel" }}
/>;

{
  /* Right: Localized text */
}
<ConfirmDialog
  title={t("confirm.deleteTitle")}
  message={t("confirm.deleteMessage")}
  confirmButtonProps={{ children: t("common.delete") }}
  cancelButtonProps={{ children: t("common.cancel") }}
/>;
```

### ❌ Don't Forget State Management

```tsx
{/* Wrong: No state control */}
<ConfirmDialog
  show={true}
  title={t("confirm.delete")}
  message={t("confirm.deleteMessage")}
/>

{/* Right: Proper state management */}
const [showConfirm, setShowConfirm] = useState(false);

<Button onClick={() => setShowConfirm(true)}>Delete</Button>
<ConfirmDialog
  show={showConfirm}
  onHide={() => setShowConfirm(false)}
  title={t("confirm.delete")}
  message={t("confirm.deleteMessage")}
/>
```

### ❌ Don't Use Generic Button Labels

```tsx
{
  /* Wrong: Generic labels */
}
<ConfirmDialog confirmButtonProps={{ children: "Yes" }} cancelButtonProps={{ children: "No" }} />;

{
  /* Right: Action-oriented labels */
}
<ConfirmDialog
  confirmButtonProps={{ children: t("common.delete") }}
  cancelButtonProps={{ children: t("common.cancel") }}
/>;
```

### ❌ Don't Provide title/message for Logout Variants

```tsx
{
  /* Wrong: Unnecessary props */
}
<ConfirmDialog
  variant="logout"
  title="Logout" // This will be overridden
  message="Are you sure?" // This will be overridden
/>;

{
  /* Right: Let variant handle text */
}
<ConfirmDialog
  variant="logout"
  confirmButtonProps={{ onClick: handleLogout }}
  cancelButtonProps={{ onClick: handleCancel }}
/>;
```

### ❌ Don't Create Multiple Dialog Instances

```tsx
{
  /* Wrong: Multiple instances */
}
const Component1 = () => <ConfirmDialog {...props1} />;
const Component2 = () => <ConfirmDialog {...props2} />;

{
  /* Right: Use Provider pattern */
}
const App = () => (
  <ConfirmDialogProvider>
    <Component1 />
    <Component2 />
  </ConfirmDialogProvider>
);
```

## Key Takeaways

The Neuron ConfirmDialog component system provides a streamlined, accessible, and consistent foundation for confirmation dialogs. Key points to remember:

1. **Use for confirmations** - Designed specifically for confirm/cancel workflows
2. **Built on Modal** - Inherits Modal's accessibility and functionality with fixed configuration
3. **Provider pattern available** - Use ConfirmDialogProvider for centralized management
4. **Three variants** - base (general), logout, logout_observed (authentication)
5. **Localize all text** - Use `t()` function for all user-visible text
6. **Action-oriented labels** - Use specific button labels instead of generic Yes/No
7. **Cancel button optional** - Hide by setting children and onClick to undefined
8. **Logout variants auto-localized** - No need to provide title/message for logout variants

By following these guidelines, you'll create consistent, accessible, and user-friendly confirmation dialogs across your Neuron applications.

## Additional Resources

For more detailed examples and advanced usage patterns, refer to the Neuron UI Documentation (`README-AI.md`).

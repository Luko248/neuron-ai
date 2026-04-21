---
agent: agent
---

# AI-Assisted Neuron Toast Component Integration Guide

## For the AI Assistant

Your task is to integrate and configure the Neuron Toast component in a React application. This guide provides comprehensive instructions for implementing Toast notifications, which serve as non-intrusive feedback messages for user actions across all Neuron applications.

## Sync Metadata

- **Component Version:** v3.1.3
- **Component Source:** `packages/neuron/ui/src/lib/messages/toast/Toast.tsx`
- **Guideline Command:** `/neuron-ui-toast`
- **Related Skill:** `neuron-ui-messages`

## Introduction

The Toast system is a core messaging component of the Neuron UI framework, designed to provide consistent, accessible, and non-intrusive user feedback across all Neuron applications.

### What is the Toast System?

The Toast component provides standardized notification messages for your application with support for:

- Multiple severity levels (info, success, warn, error)
- Automatic dismissal with customizable timing
- Sticky messages that require manual dismissal
- Multiple simultaneous messages
- Icon integration based on severity
- Built-in accessibility features
- Customizable positioning and styling

### Key Features

- **Non-Intrusive Messaging**: Temporary notifications that don't block user workflow
- **Severity-Based Styling**: Visual distinction between info, success, warning, and error messages
- **Automatic Icons**: Icons are automatically assigned based on message severity
- **Flexible Timing**: Customizable display duration or sticky behavior
- **Multiple Messages**: Support for displaying multiple toasts simultaneously
- **Accessibility**: Built-in ARIA support and keyboard navigation
- **Consistent Positioning**: Standardized placement across applications
- **TypeScript Support**: Full type safety with comprehensive interfaces

**Storybook Reference:** You can refer to the Storybook documentation for visual examples and interactive demos of the Toast component.

### Figma Code Connect Integration

**🎨 Design-to-Code Connection Available**

The Toast component has **full Figma Code Connect integration**, enabling direct design-to-code generation from Figma designs.

**Key Features:**

- **Automatic Code Generation**: Figma MCP tools can generate accurate Toast component code
- **Severity Mapping**: All severity levels (info, success, warn, error) are correctly mapped
- **Message Structure**: Proper handling of summary and detail text content
- **Timing Configuration**: Sticky and timed message configurations are supported
- **Icon Integration**: Automatic icon assignment based on severity

**Code Connect Mappings:**

- **Severities**: info, success, warn, error
- **Message Types**: standard, sticky, multiple
- **Content**: summary (title) and detail (message) text mapping
- **Timing**: life duration and sticky behavior
- **Icons**: automatic severity-based icon assignment

**Usage with Figma MCP:**

1. Use `mcp4_get_code` with Toast component node-id from Figma
2. Generated code will use proper `@neuron/ui/Toast` component structure
3. All props will be correctly mapped from design specifications
4. Message content and severity will be properly configured

**Figma Design System Reference:**

- Node ID: Available in VIGo Design System
- All toast variants and configurations are connected
- Direct code generation available through Figma MCP integration

## Step 1: Basic Toast Implementation

### 1.1 Import the Toast Component

```tsx
import { Toast } from "@neuron/ui";
```

### 1.2 Basic Usage

Here's a simple implementation of the Toast component:

```tsx
import { Toast, Button } from "@neuron/ui";
import { useState } from "react";

const MyComponent = () => {
  const [showToast, setShowToast] = useState(false);

  const handleShowToast = () => {
    setShowToast(true);
  };

  const handleHideToast = () => {
    setShowToast(false);
  };

  const toastMessages = [
    {
      severity: "success" as const,
      summary: "Success",
      detail: "Operation completed successfully",
    },
  ];

  return (
    <div>
      <Button onClick={handleShowToast}>Show Toast</Button>

      <Toast show={showToast} toastMessages={toastMessages} onHide={handleHideToast} />
    </div>
  );
};
```

### 1.3 Toast Severity Levels

The Toast component supports four severity levels, each with distinct visual styling and icons:

```tsx
import { Toast, Button } from "@neuron/ui";
import { useState } from "react";

const ToastSeverities = () => {
  const [showToast, setShowToast] = useState(false);
  const [currentMessages, setCurrentMessages] = useState([]);

  const showInfoToast = () => {
    setCurrentMessages([
      {
        severity: "info" as const,
        summary: "Information",
        detail: "Here's some helpful information for you",
      },
    ]);
    setShowToast(true);
  };

  const showSuccessToast = () => {
    setCurrentMessages([
      {
        severity: "success" as const,
        summary: "Success",
        detail: "Your changes have been saved successfully",
      },
    ]);
    setShowToast(true);
  };

  const showWarningToast = () => {
    setCurrentMessages([
      {
        severity: "warn" as const,
        summary: "Warning",
        detail: "Please review your input before proceeding",
      },
    ]);
    setShowToast(true);
  };

  const showErrorToast = () => {
    setCurrentMessages([
      {
        severity: "error" as const,
        summary: "Error",
        detail: "Unable to complete the operation. Please try again",
      },
    ]);
    setShowToast(true);
  };

  return (
    <div className="toast-examples">
      <Button onClick={showInfoToast}>Info Toast</Button>
      <Button onClick={showSuccessToast}>Success Toast</Button>
      <Button onClick={showWarningToast}>Warning Toast</Button>
      <Button onClick={showErrorToast}>Error Toast</Button>

      <Toast show={showToast} toastMessages={currentMessages} onHide={() => setShowToast(false)} />
    </div>
  );
};
```

## Step 2: Message Structure and Content

### 2.1 Message Anatomy

Each toast message consists of:

- **Summary**: The main title/heading of the message
- **Detail**: Additional descriptive text providing context
- **Severity**: Determines the visual style and icon
- **Life**: Duration in milliseconds (optional)
- **Sticky**: Whether the message requires manual dismissal (optional)

```tsx
import { Toast, ToastMessages } from "@neuron/ui";

const MessageStructure = () => {
  const wellStructuredMessages: ToastMessages = [
    {
      severity: "success",
      summary: "Profile Updated", // Clear, concise title
      detail: "Your profile has been updated", // Brief, informative detail
      life: 4000, // 4 seconds display time
    },
    {
      severity: "error",
      summary: "Save Failed", // Direct problem statement
      detail: "Please try again or contact support", // Actionable guidance
      sticky: true, // Requires manual dismissal
    },
  ];

  return <Toast show={true} toastMessages={wellStructuredMessages} onHide={() => {}} />;
};
```

### 2.2 Content Writing Best Practices

Follow these guidelines for effective toast messages:

**Info Messages** - Neutral, informational content:

```tsx
const infoMessages: ToastMessages = [
  {
    severity: "info",
    summary: "Tip",
    detail: "Use Ctrl+F for faster searching",
  },
  {
    severity: "info",
    summary: "Data Updating",
    detail: "Information is being refreshed in the background",
  },
];
```

**Success Messages** - Confirmation of completed actions:

```tsx
const successMessages: ToastMessages = [
  {
    severity: "success",
    summary: "Changes Saved",
    detail: "Your changes have been saved",
  },
  {
    severity: "success",
    summary: "Contract Created",
    detail: "Contract #12345697 has been successfully created",
  },
];
```

**Warning Messages** - Potential issues that allow continuation:

```tsx
const warningMessages: ToastMessages = [
  {
    severity: "warn",
    summary: "Connection Lost",
    detail: "Retrying data transmission automatically",
  },
  {
    severity: "warn",
    summary: "Unsaved Changes",
    detail: "Changes will be lost if you leave this page",
  },
];
```

**Error Messages** - Critical problems requiring attention:

```tsx
const errorMessages: ToastMessages = [
  {
    severity: "error",
    summary: "Insufficient Permissions",
    detail: "You don't have permission to perform this action",
  },
  {
    severity: "error",
    summary: "Save Failed",
    detail: "Please try again or contact support",
  },
];
```

## Step 3: Timing and Persistence

### 3.1 Automatic Dismissal

Control how long messages are displayed using the `life` property:

```tsx
import { Toast, ToastMessages } from "@neuron/ui";

const TimedMessages = () => {
  const timedMessages: ToastMessages = [
    {
      severity: "info",
      summary: "Quick Info",
      detail: "This disappears in 2 seconds",
      life: 2000, // 2 seconds
    },
    {
      severity: "success",
      summary: "Standard Success",
      detail: "This uses default timing (5 seconds)",
      // No life specified = 5000ms default
    },
    {
      severity: "warn",
      summary: "Important Warning",
      detail: "This stays longer for important info",
      life: 8000, // 8 seconds
    },
  ];

  return <Toast show={true} toastMessages={timedMessages} onHide={() => {}} />;
};
```

### 3.2 Sticky Messages

Use sticky messages for critical information that requires user acknowledgment:

```tsx
import { Toast, ToastMessages } from "@neuron/ui";

const StickyMessages = () => {
  const stickyMessages: ToastMessages = [
    {
      severity: "error",
      summary: "Critical Error",
      detail: "System backup failed. Immediate action required",
      sticky: true, // Stays until manually dismissed
    },
    {
      severity: "warn",
      summary: "Maintenance Notice",
      detail: "System maintenance scheduled for tonight at 2 AM",
      sticky: true,
    },
  ];

  return <Toast show={true} toastMessages={stickyMessages} onHide={() => {}} />;
};
```

### 3.3 Mixed Timing Strategies

Combine different timing approaches based on message importance:

```tsx
import { Toast, ToastMessages } from "@neuron/ui";

const MixedTimingMessages = () => {
  const mixedMessages: ToastMessages = [
    {
      severity: "success",
      summary: "Auto-save",
      detail: "Draft saved",
      life: 2000, // Quick confirmation
    },
    {
      severity: "warn",
      summary: "Session Expiring",
      detail: "Your session will expire in 5 minutes",
      life: 10000, // Longer warning
    },
    {
      severity: "error",
      summary: "Connection Error",
      detail: "Unable to reach server. Check your connection",
      sticky: true, // Critical issue
    },
  ];

  return <Toast show={true} toastMessages={mixedMessages} onHide={() => {}} />;
};
```

## Step 4: Multiple Messages and Queuing

### 4.1 Displaying Multiple Messages

Show multiple toast messages simultaneously:

```tsx
import { Toast, ToastMessages, Button } from "@neuron/ui";
import { useState } from "react";

const MultipleMessages = () => {
  const [showToasts, setShowToasts] = useState(false);

  const multipleMessages: ToastMessages = [
    {
      severity: "success",
      summary: "File 1 Uploaded",
      detail: "document.pdf uploaded successfully",
      life: 3000,
    },
    {
      severity: "success",
      summary: "File 2 Uploaded",
      detail: "image.jpg uploaded successfully",
      life: 4000,
    },
    {
      severity: "info",
      summary: "Processing",
      detail: "Files are being processed",
      life: 5000,
    },
  ];

  const showMultipleToasts = () => {
    setShowToasts(true);
  };

  return (
    <div>
      <Button onClick={showMultipleToasts}>Upload Multiple Files</Button>

      <Toast show={showToasts} toastMessages={multipleMessages} onHide={() => setShowToasts(false)} />
    </div>
  );
};
```

### 4.2 Progressive Message Display

Add messages progressively based on user actions:

```tsx
import { Toast, ToastMessages, Button } from "@neuron/ui";
import { useState } from "react";

const ProgressiveMessages = () => {
  const [messages, setMessages] = useState<ToastMessages>([]);
  const [showToasts, setShowToasts] = useState(false);

  const addSuccessMessage = () => {
    const newMessage = {
      severity: "success" as const,
      summary: "Step Completed",
      detail: `Step ${messages.length + 1} completed successfully`,
      life: 4000,
    };

    setMessages((prev) => [...prev, newMessage]);
    setShowToasts(true);
  };

  const addErrorMessage = () => {
    const newMessage = {
      severity: "error" as const,
      summary: "Step Failed",
      detail: "Please review and try again",
      sticky: true,
    };

    setMessages((prev) => [...prev, newMessage]);
    setShowToasts(true);
  };

  const clearMessages = () => {
    setMessages([]);
    setShowToasts(false);
  };

  return (
    <div>
      <Button onClick={addSuccessMessage}>Add Success</Button>
      <Button onClick={addErrorMessage}>Add Error</Button>
      <Button onClick={clearMessages}>Clear All</Button>

      <Toast show={showToasts} toastMessages={messages} onHide={() => setShowToasts(false)} />
    </div>
  );
};
```

## Step 5: Integration with Forms and API Calls

### 5.1 Form Validation Feedback

Integrate Toast with form validation to provide user feedback:

```tsx
import { Toast, ToastMessages, Button, Input } from "@neuron/ui";
import { useState } from "react";
import { useForm } from "react-hook-form";

const FormWithToast = () => {
  const [showToast, setShowToast] = useState(false);
  const [toastMessages, setToastMessages] = useState<ToastMessages>([]);
  const {
    control,
    handleSubmit,
    formState: { errors },
  } = useForm();

  const onSubmit = async (data: any) => {
    try {
      // Simulate API call
      await new Promise((resolve) => setTimeout(resolve, 1000));

      setToastMessages([
        {
          severity: "success",
          summary: "Form Submitted",
          detail: "Your information has been saved successfully",
        },
      ]);
      setShowToast(true);
    } catch (error) {
      setToastMessages([
        {
          severity: "error",
          summary: "Submission Failed",
          detail: "Please check your information and try again",
        },
      ]);
      setShowToast(true);
    }
  };

  const onError = () => {
    setToastMessages([
      {
        severity: "warn",
        summary: "Validation Error",
        detail: "Please correct the highlighted fields",
      },
    ]);
    setShowToast(true);
  };

  return (
    <form onSubmit={handleSubmit(onSubmit, onError)}>
      <Input control={control} name="email" label="Email" rules={{ required: "Email is required" }} />

      <Button type="submit">Submit</Button>

      <Toast show={showToast} toastMessages={toastMessages} onHide={() => setShowToast(false)} />
    </form>
  );
};
```

### 5.2 API Integration Patterns

Show toast messages based on API call results:

```tsx
import { Toast, ToastMessages, Button } from "@neuron/ui";
import { useState } from "react";
import { useApiCall } from "@neuron/core";

const ApiWithToast = () => {
  const [showToast, setShowToast] = useState(false);
  const [toastMessages, setToastMessages] = useState<ToastMessages>([]);

  const [saveData, apiState] = useApiCall(api.saveUserData);

  const handleSave = async () => {
    try {
      await saveData({ name: "John Doe", email: "john@example.com" });

      setToastMessages([
        {
          severity: "success",
          summary: "Data Saved",
          detail: "User information updated successfully",
        },
      ]);
      setShowToast(true);
    } catch (error) {
      setToastMessages([
        {
          severity: "error",
          summary: "Save Failed",
          detail: "Unable to save data. Please try again",
        },
      ]);
      setShowToast(true);
    }
  };

  const handleDelete = async () => {
    try {
      await api.deleteUser();

      setToastMessages([
        {
          severity: "success",
          summary: "User Deleted",
          detail: "User account has been permanently removed",
        },
      ]);
      setShowToast(true);
    } catch (error) {
      setToastMessages([
        {
          severity: "error",
          summary: "Delete Failed",
          detail: "Unable to delete user. Please contact support",
        },
      ]);
      setShowToast(true);
    }
  };

  return (
    <div>
      <Button onClick={handleSave} loading={apiState.fetchingState === "fetching"}>
        Save Data
      </Button>

      <Button onClick={handleDelete} variant="danger">
        Delete User
      </Button>

      <Toast show={showToast} toastMessages={toastMessages} onHide={() => setShowToast(false)} />
    </div>
  );
};
```

## Step 6: Advanced Toast Features

### 6.1 Custom Styling and Positioning

While the Toast component uses standardized positioning, you can customize certain aspects:

```tsx
import { Toast, ToastMessages } from "@neuron/ui";

const CustomStyledToast = () => {
  const customMessages: ToastMessages = [
    {
      severity: "info",
      summary: "Custom Message",
      detail: "This message has custom styling",
      style: {
        backgroundColor: "var(--surface-100)",
        border: "2px solid var(--primary-500)",
      },
    },
  ];

  return <Toast show={true} toastMessages={customMessages} onHide={() => {}} />;
};
```

### 6.2 Toast with Manual Control

Implement manual show/hide control for complex scenarios:

```tsx
import { Toast, ToastMessages, Button } from "@neuron/ui";
import { useState, useRef } from "react";

const ManualToastControl = () => {
  const [showToast, setShowToast] = useState(false);
  const [hideToast, setHideToast] = useState(false);

  const messages: ToastMessages = [
    {
      severity: "info",
      summary: "Manual Control",
      detail: "This toast is manually controlled",
      sticky: true,
    },
  ];

  const showManualToast = () => {
    setHideToast(false);
    setShowToast(true);
  };

  const hideManualToast = () => {
    setHideToast(true);
  };

  const handleToastHide = () => {
    setShowToast(false);
    setHideToast(false);
  };

  return (
    <div>
      <Button onClick={showManualToast}>Show Toast</Button>
      <Button onClick={hideManualToast}>Hide Toast</Button>

      <Toast show={showToast} hide={hideToast} toastMessages={messages} onHide={handleToastHide} />
    </div>
  );
};
```

### 6.3 Toast with Test ID

Add test IDs for automated testing:

```tsx
import { Toast, ToastMessages } from "@neuron/ui";

const TestableToast = () => {
  const messages: ToastMessages = [
    {
      severity: "success",
      summary: "Test Message",
      detail: "This toast can be tested automatically",
    },
  ];

  return <Toast show={true} toastMessages={messages} onHide={() => {}} testId="user-feedback-toast" />;
};
```

## Step 7: Toast Props Reference

### 7.1 Core Toast Props

| Prop          | Type            | Default | Description                                |
| ------------- | --------------- | ------- | ------------------------------------------ |
| show          | `boolean`       | -       | **Required**. Controls toast visibility    |
| toastMessages | `ToastMessages` | -       | **Required**. Array of messages to display |
| hide          | `boolean`       | `false` | Instantly hide/clear all toasts            |
| onHide        | `() => void`    | -       | Callback when message becomes hidden       |
| testId        | `string`        | -       | Custom test ID for the component           |

### 7.2 ToastMessage Properties

| Property | Type                                       | Default | Description                          |
| -------- | ------------------------------------------ | ------- | ------------------------------------ |
| severity | `"info" \| "success" \| "warn" \| "error"` | -       | **Required**. Message severity level |
| summary  | `string`                                   | -       | **Required**. Message title/heading  |
| detail   | `string`                                   | -       | Message description/body text        |
| life     | `number`                                   | `5000`  | Display duration in milliseconds     |
| sticky   | `boolean`                                  | `false` | Requires manual dismissal            |
| style    | `React.CSSProperties`                      | -       | Custom inline styles                 |

### 7.3 Severity Levels and Icons

| Severity  | Icon               | Use Case                  | Visual Style |
| --------- | ------------------ | ------------------------- | ------------ |
| `info`    | Circle Info        | Neutral information, tips | Blue theme   |
| `success` | Check Circle       | Successful operations     | Green theme  |
| `warn`    | Exclamation Circle | Warnings, cautions        | Orange theme |
| `error`   | Hand/Stop          | Errors, failures          | Red theme    |

## Step 8: Best Practices

### 8.1 When to Use Each Severity

**Info Messages:**

- Helpful tips and guidance
- Status updates that don't require action
- Background process notifications

```tsx
// Good: Informational content
{ severity: "info", summary: "Tip", detail: "Use keyboard shortcuts for faster navigation" }

// Good: Status update
{ severity: "info", summary: "Syncing", detail: "Data is being synchronized" }
```

**Success Messages:**

- Confirmation of completed actions
- Successful form submissions
- Completed processes

```tsx
// Good: Action confirmation
{ severity: "success", summary: "Saved", detail: "Your changes have been saved" }

// Good: Process completion
{ severity: "success", summary: "Upload Complete", detail: "All files uploaded successfully" }
```

**Warning Messages:**

- Potential issues that allow continuation
- Important notices
- Recoverable errors

```tsx
// Good: Recoverable issue
{ severity: "warn", summary: "Connection Slow", detail: "Upload may take longer than usual" }

// Good: Important notice
{ severity: "warn", summary: "Session Expiring", detail: "Your session will expire in 5 minutes" }
```

**Error Messages:**

- Critical failures
- Blocking issues
- Actions that cannot be completed

```tsx
// Good: Critical error
{ severity: "error", summary: "Save Failed", detail: "Unable to save changes. Please try again" }

// Good: Permission issue
{ severity: "error", summary: "Access Denied", detail: "You don't have permission for this action" }
```

### 8.2 Content Writing Guidelines

**Keep It Concise:**

```tsx
// Good: Brief and clear
{ summary: "Saved", detail: "Changes saved successfully" }

// Avoid: Too verbose
{ summary: "Save Operation Completed", detail: "Your changes have been successfully saved to the database and are now available" }
```

**Be Specific:**

```tsx
// Good: Specific information
{ summary: "File Uploaded", detail: "document.pdf uploaded successfully" }

// Avoid: Vague messaging
{ summary: "Success", detail: "Operation completed" }
```

**Provide Actionable Information:**

```tsx
// Good: Actionable guidance
{ summary: "Save Failed", detail: "Check your connection and try again" }

// Avoid: Unhelpful messaging
{ summary: "Error", detail: "Something went wrong" }
```

### 8.3 Timing Best Practices

**Use Appropriate Durations:**

- Quick confirmations: 2-3 seconds
- Standard messages: 4-5 seconds (default)
- Important warnings: 6-8 seconds
- Critical errors: Use sticky messages

```tsx
// Good: Appropriate timing
const messages = [
  { severity: "success", summary: "Saved", detail: "Auto-saved", life: 2000 },
  { severity: "warn", summary: "Warning", detail: "Review required", life: 6000 },
  { severity: "error", summary: "Error", detail: "Action required", sticky: true },
];
```

### 8.4 Accessibility Considerations

- Messages are automatically announced by screen readers
- Use clear, descriptive language
- Ensure sufficient contrast in custom styling
- Provide alternative ways to access critical information

```tsx
// Good: Clear, accessible content
{
  severity: "error",
  summary: "Form Validation Error",
  detail: "Please correct the highlighted fields and resubmit"
}
```

## Step 9: Common Patterns and Examples

### 9.1 Real-time Notifications

```tsx
import { Toast, ToastMessages } from "@neuron/ui";
import { useState, useEffect } from "react";

const RealTimeNotifications = () => {
  const [showToast, setShowToast] = useState(false);
  const [messages, setMessages] = useState<ToastMessages>([]);

  useEffect(() => {
    // Simulate real-time notifications
    const interval = setInterval(() => {
      const notifications = [
        {
          severity: "info" as const,
          summary: "New Message",
          detail: "You have a new message from John",
          life: 4000,
        },
        {
          severity: "warn" as const,
          summary: "System Maintenance",
          detail: "Scheduled maintenance in 30 minutes",
          life: 8000,
        },
      ];

      const randomNotification = notifications[Math.floor(Math.random() * notifications.length)];
      setMessages([randomNotification]);
      setShowToast(true);
    }, 10000);

    return () => clearInterval(interval);
  }, []);

  return <Toast show={showToast} toastMessages={messages} onHide={() => setShowToast(false)} />;
};
```

### 9.2 Form Validation Integration

```tsx
import { Toast, ToastMessages, Button, Input } from "@neuron/ui";
import { useState } from "react";
import { useForm } from "react-hook-form";

const FormValidationToast = () => {
  const [showToast, setShowToast] = useState(false);
  const [toastMessages, setToastMessages] = useState<ToastMessages>([]);

  const {
    control,
    handleSubmit,
    formState: { errors },
  } = useForm();

  const onSubmit = (data: any) => {
    setToastMessages([
      {
        severity: "success",
        summary: "Form Submitted",
        detail: "Your information has been saved",
      },
    ]);
    setShowToast(true);
  };

  const onError = () => {
    const errorCount = Object.keys(errors).length;
    setToastMessages([
      {
        severity: "warn",
        summary: "Validation Error",
        detail: `Please correct ${errorCount} field${errorCount > 1 ? "s" : ""}`,
      },
    ]);
    setShowToast(true);
  };

  return (
    <form onSubmit={handleSubmit(onSubmit, onError)}>
      <Input control={control} name="name" label="Name" rules={{ required: "Name is required" }} />

      <Input
        control={control}
        name="email"
        label="Email"
        rules={{
          required: "Email is required",
          pattern: {
            value: /^\S+@\S+$/i,
            message: "Invalid email format",
          },
        }}
      />

      <Button type="submit">Submit</Button>

      <Toast show={showToast} toastMessages={toastMessages} onHide={() => setShowToast(false)} />
    </form>
  );
};
```

## Step 10: Common Mistakes to Avoid

### 10.1 Don't Overuse Toast Messages

```tsx
// Wrong: Too many simultaneous messages
const tooManyMessages = [
  { severity: "info", summary: "Info 1", detail: "Message 1" },
  { severity: "info", summary: "Info 2", detail: "Message 2" },
  { severity: "info", summary: "Info 3", detail: "Message 3" },
  { severity: "info", summary: "Info 4", detail: "Message 4" },
  { severity: "info", summary: "Info 5", detail: "Message 5" },
];

// Right: Limit simultaneous messages
const appropriateMessages = [
  { severity: "success", summary: "Upload Complete", detail: "3 files uploaded successfully" },
];
```

### 10.2 Don't Use Wrong Severity Levels

```tsx
// Wrong: Success for error scenario
{ severity: "success", summary: "Delete Failed", detail: "Unable to delete item" }

// Right: Match severity to scenario
{ severity: "error", summary: "Delete Failed", detail: "Unable to delete item" }

// Wrong: Error for informational content
{ severity: "error", summary: "Tip", detail: "Use shortcuts for faster work" }

// Right: Info for tips
{ severity: "info", summary: "Tip", detail: "Use shortcuts for faster work" }
```

### 10.3 Don't Write Verbose Messages

```tsx
// Wrong: Too verbose
{
  severity: "success",
  summary: "Successful Operation Completion",
  detail: "Your requested operation has been successfully completed and all changes have been saved to the database"
}

// Right: Concise and clear
{
  severity: "success",
  summary: "Changes Saved",
  detail: "Your changes have been saved"
}
```

### 10.4 Don't Forget State Management

```tsx
// Wrong: No proper state management
const BadToastUsage = () => {
  return (
    <Toast
      show={true} // Always showing
      toastMessages={[{ severity: "info", summary: "Test", detail: "Message" }]}
      // No onHide handler
    />
  );
};

// Right: Proper state management
const GoodToastUsage = () => {
  const [showToast, setShowToast] = useState(false);

  return <Toast show={showToast} toastMessages={messages} onHide={() => setShowToast(false)} />;
};
```

## Summary

The Neuron Toast component provides a comprehensive, accessible, and consistent system for user notifications. Key points to remember:

1. **Use appropriate severity levels** based on message type and importance
2. **Keep messages concise** and actionable
3. **Control timing appropriately** - use sticky for critical messages
4. **Manage state properly** with show/hide controls
5. **Integrate with forms and APIs** for comprehensive user feedback
6. **Follow accessibility guidelines** for inclusive design
7. **Limit simultaneous messages** to avoid overwhelming users
8. **Match severity to content** for consistent user experience

### Key Takeaways

- **Non-intrusive feedback** that doesn't block user workflow
- **Automatic icons and styling** based on severity levels
- **Flexible timing control** with automatic and sticky options
- **Multiple message support** for complex scenarios
- **Seamless integration** with forms, APIs, and user actions
- **Built-in accessibility** features for inclusive design
- **Consistent positioning** and behavior across applications

By following these guidelines, you'll create effective, user-friendly notification systems that enhance the user experience across your Neuron applications.

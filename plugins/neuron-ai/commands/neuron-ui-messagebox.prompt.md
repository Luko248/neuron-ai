---
agent: agent
---

# AI-Assisted Neuron MessageBox Component Integration Guide

## For the AI Assistant

Your task is to integrate and configure the Neuron MessageBox component in a React application. This guide provides comprehensive instructions for implementing MessageBox feedback messages, which serve as prominent, contextual information displays across all Neuron applications.

## Sync Metadata

- **Component Version:** v3.2.2
- **Component Source:** `packages/neuron/ui/src/lib/messages/messageBox/MessageBox.tsx`
- **Guideline Command:** `/neuron-ui-messagebox`
- **Related Skill:** `neuron-ui-messages`

## Introduction

The MessageBox system is a core messaging component of the Neuron UI framework, designed to provide consistent, accessible, and contextual user feedback across all Neuron applications.

### What is the MessageBox System?

The MessageBox component provides standardized feedback messages for your application with support for:

- Multiple variants (default, info, success, warning, danger)
- Expandable content with "Show more/less" functionality
- Action buttons with contextual styling
- Auto-hide functionality with customizable timing
- Dismissible messages with close button
- Sticky positioning for persistent messages
- HTML content support for rich formatting
- Built-in accessibility features

### Key Features

- **Contextual Messaging**: Prominent feedback that provides important context to users
- **Variant-Based Styling**: Visual distinction between default, info, success, warning, and danger messages
- **Expandable Content**: Automatic content expansion for longer messages with "Show more/less" controls
- **Action Integration**: Built-in button support with variant-appropriate styling
- **Flexible Timing**: Auto-hide functionality or persistent sticky behavior
- **Rich Content**: Support for HTML content including lists and formatting
- **Accessibility**: Built-in ARIA support and keyboard navigation
- **Consistent Positioning**: Standardized placement and behavior
- **TypeScript Support**: Full type safety with comprehensive interfaces

**Storybook Reference:** You can refer to the Storybook documentation for visual examples and interactive demos of the MessageBox component.

### Figma Code Connect Integration

**🎨 Design-to-Code Connection Available**

The MessageBox component has **full Figma Code Connect integration**, enabling direct design-to-code generation from Figma designs.

**Key Features:**

- **Automatic Code Generation**: Figma MCP tools can generate accurate MessageBox component code
- **Variant Mapping**: All variants (default, info, success, warning, danger) are correctly mapped
- **Content Structure**: Proper handling of title and content text
- **Button Integration**: Action button configurations are automatically included
- **State Management**: Auto-hide, sticky, and dismissible configurations are supported

**Code Connect Mappings:**

- **Variants**: default, info, success, warning, danger
- **Content Types**: title, content, expandable text
- **Features**: buttons, auto-hide, sticky, dismissible
- **Icons**: automatic variant-based icon assignment

**Usage with Figma MCP:**

1. Use `mcp4_get_code` with MessageBox component node-id from Figma
2. Generated code will use proper `@neuron/ui/MessageBox` component structure
3. All props will be correctly mapped from design specifications
4. Content and actions will be properly configured

**Figma Design System Reference:**

- Node ID: Available in VIGo Design System
- All MessageBox variants and configurations are connected
- Direct code generation available through Figma MCP integration

## Step 1: Basic MessageBox Implementation

### 1.1 Import the MessageBox Component

```tsx
import { MessageBox } from "@neuron/ui";
```

### 1.2 Basic Usage

Here's a simple implementation of the MessageBox component:

```tsx
import { MessageBox } from "@neuron/ui";

const MyComponent = () => {
  return (
    <MessageBox
      title="Information"
      content="This is a basic message box with important information for the user."
      variant="info"
    />
  );
};
```

### 1.3 MessageBox Variants

The MessageBox component supports multiple variants for different use cases:

```tsx
import { MessageBox } from "@neuron/ui";

const MessageBoxVariants = () => {
  return (
    <div className="messagebox-variants">
      {/* Default variant - neutral information */}
      <MessageBox
        title="Default Message"
        content="This is a default message box for general information."
        variant="default"
      />

      {/* Info variant - informational content */}
      <MessageBox title="Information" content="This provides helpful information to guide the user." variant="info" />

      {/* Success variant - positive feedback */}
      <MessageBox title="Success" content="Your action was completed successfully." variant="success" />

      {/* Warning variant - caution messages */}
      <MessageBox title="Warning" content="Please review this information before proceeding." variant="warning" />

      {/* Danger variant - critical issues */}
      <MessageBox title="Error" content="An error occurred that requires your attention." variant="danger" />
    </div>
  );
};
```

## Step 2: Content Structure and Formatting

### 2.1 Message Anatomy

Each MessageBox consists of:

- **Title**: Optional heading that summarizes the message
- **Content**: Main message body (supports HTML and React nodes)
- **Icon**: Automatically assigned based on variant
- **Action Button**: Optional button for user actions
- **Expand/Collapse**: Automatic for longer content

```tsx
import { MessageBox } from "@neuron/ui";

const MessageStructure = () => {
  return (
    <div className="message-structure">
      {/* Message with title and content */}
      <MessageBox
        title="Profile Updated"
        content="Your profile information has been successfully updated and saved."
        variant="success"
      />

      {/* Message without title */}
      <MessageBox content="This message doesn't have a title, just content." variant="info" />

      {/* Message with HTML content */}
      <MessageBox
        title="Validation Errors"
        content={
          <>
            Please correct the following issues:
            <ul>
              <li>Email address is required</li>
              <li>Password must be at least 8 characters</li>
              <li>Phone number format is invalid</li>
            </ul>
          </>
        }
        variant="warning"
      />
    </div>
  );
};
```

### 2.2 Content Writing Best Practices

Follow these guidelines for effective MessageBox content based on the provided Czech guidelines:

**Info Messages** - Neutral, informational content:

```tsx
const infoMessages = [
  {
    title: "Updated",
    content: "Your profile has been updated.",
    variant: "info",
  },
  {
    title: "Search Tip",
    content: "Use Ctrl+F for faster searching.",
    variant: "info",
  },
  {
    title: "Connection Stable",
    content: "Internet connection is stable.",
    variant: "info",
  },
];
```

**Success Messages** - Positive confirmation:

```tsx
const successMessages = [
  {
    title: "Changes Saved",
    content: "Your changes have been saved.",
    variant: "success",
  },
  {
    title: "Contract Created",
    content: "Contract #12345697 has been successfully created.",
    variant: "success",
  },
  {
    title: "File Uploaded",
    content: "Your document has been successfully uploaded.",
    variant: "success",
  },
];
```

**Warning Messages** - Caution without blocking:

```tsx
const warningMessages = [
  {
    title: "Connection Lost",
    content: "Connection was interrupted during data transmission. Retrying automatically.",
    variant: "warning",
  },
  {
    title: "Low Coverage Limit",
    content: "In case of accident, may not cover all damages.",
    variant: "warning",
  },
  {
    title: "Unsaved Changes",
    content: "Do you really want to leave? Changes will be lost.",
    variant: "warning",
  },
];
```

**Danger/Error Messages** - Critical issues:

```tsx
const dangerMessages = [
  {
    title: "Insufficient Permissions",
    content: "You don't have permission to perform this action.",
    variant: "danger",
  },
  {
    title: "Save Failed",
    content: "Please try again or contact support.",
    variant: "danger",
  },
  {
    title: "System Error",
    content: "An internal server error occurred. We're working to resolve it.",
    variant: "danger",
  },
];
```

## Step 3: Action Buttons and Interactions

### 3.1 Adding Action Buttons

MessageBox supports action buttons that are automatically styled to match the variant:

```tsx
import { MessageBox } from "@neuron/ui";

const MessageBoxWithActions = () => {
  const handleContactSupport = () => {
    // Handle contact support action
    console.log("Contacting support...");
  };

  const handleRetry = () => {
    // Handle retry action
    console.log("Retrying operation...");
  };

  const handleDownload = () => {
    // Handle download action
    console.log("Downloading contract...");
  };

  return (
    <div className="messagebox-actions">
      {/* Error message with support button */}
      <MessageBox
        title="Insufficient Permissions"
        content="You don't have permission to perform this action."
        variant="danger"
        button={{
          buttonText: "Contact Support",
          onClick: handleContactSupport
        }}
      />

      {/* Warning with retry button */}
      <MessageBox
        title="Save Failed"
        content="Please try again or contact support."
        variant="danger"
        button={{
          buttonText: "Retry Save",
          onClick: handleRetry
        }}
      />

      {/* Success with download action */}
      <MessageBox
        title: "Contract Created",
        content: "Contract #12345697 has been successfully created.",
        variant: "success",
        button={{
          buttonText: "Download Contract",
          onClick: handleDownload
        }}
      />
    </div>
  );
};
```

### 3.2 Buttons with Icons

Add icons to action buttons for better visual communication:

```tsx
import { MessageBox, baseIcons } from "@neuron/ui";

const MessageBoxWithIconButtons = () => {
  return (
    <div className="messagebox-icon-buttons">
      {/* Button with left icon */}
      <MessageBox
        title="System Error"
        content="An internal server error occurred. We're working to resolve it."
        variant="danger"
        button={{
          buttonText: "Refresh Page",
          iconLeft: baseIcons.arrowRotateRightSolid,
          onClick: () => window.location.reload(),
        }}
      />

      {/* Button with right icon */}
      <MessageBox
        title="File Ready"
        content="Your report has been generated and is ready for download."
        variant="success"
        button={{
          buttonText: "Download",
          iconRight: baseIcons.downloadSolid,
          onClick: () => console.log("Downloading..."),
        }}
      />
    </div>
  );
};
```

### 3.3 Button Variants and Styling

Buttons automatically inherit appropriate variants based on the MessageBox variant:

```tsx
import { MessageBox } from "@neuron/ui";

const ButtonVariantMapping = () => {
  return (
    <div className="button-variant-mapping">
      {/* Default MessageBox = Secondary button */}
      <MessageBox
        title="Default Message"
        content="This uses a secondary button variant."
        variant="default"
        button={{
          buttonText: "Action",
          onClick: () => {},
        }}
      />

      {/* Info MessageBox = Info button */}
      <MessageBox
        title="Information"
        content="This uses an info button variant."
        variant="info"
        button={{
          buttonText: "Learn More",
          onClick: () => {},
        }}
      />

      {/* Success MessageBox = Success button */}
      <MessageBox
        title="Success"
        content="This uses a success button variant."
        variant="success"
        button={{
          buttonText: "Continue",
          onClick: () => {},
        }}
      />

      {/* Warning MessageBox = Warning button */}
      <MessageBox
        title="Warning"
        content="This uses a warning button variant."
        variant="warning"
        button={{
          buttonText: "Review",
          onClick: () => {},
        }}
      />

      {/* Danger MessageBox = Danger button */}
      <MessageBox
        title="Error"
        content="This uses a danger button variant."
        variant="danger"
        button={{
          buttonText: "Fix Issue",
          onClick: () => {},
        }}
      />
    </div>
  );
};
```

## Step 4: Content Expansion and Display Control

### 4.1 Automatic Content Expansion

MessageBox automatically provides "Show more/less" functionality for longer content:

```tsx
import { MessageBox } from "@neuron/ui";

const ExpandableContent = () => {
  const longContent = `
    This is a very long message that will automatically show expansion controls 
    when the content exceeds the available space. Users can click "Show more" 
    to see the full content and "Show less" to collapse it back. This provides 
    a clean interface while still allowing access to detailed information when needed.
    The expansion behavior is automatic and doesn't require any additional configuration.
  `;

  return <MessageBox title="Expandable Message" content={longContent} variant="info" />;
};
```

### 4.2 Fixed Height Display

Use `fixedHeight` to always show the full content without expansion controls:

```tsx
import { MessageBox } from "@neuron/ui";

const FixedHeightContent = () => {
  const longContent = `
    This message uses fixed height, so all content is always visible 
    without any expansion controls. This is useful when you want to 
    ensure users see the complete message immediately without any 
    interaction required.
  `;

  return <MessageBox title="Fixed Height Message" content={longContent} variant="warning" fixedHeight={true} />;
};
```

### 4.3 Content with HTML and Lists

MessageBox supports rich HTML content for better formatting:

```tsx
import { MessageBox } from "@neuron/ui";

const RichContent = () => {
  const htmlContent = (
    <>
      <p>Please review the following validation errors:</p>
      <ul>
        <li>Email address is required and must be valid</li>
        <li>Password must be at least 8 characters long</li>
        <li>Phone number must follow the format: +1 (555) 123-4567</li>
        <li>Date of birth is required for age verification</li>
      </ul>
      <p>Correct these issues and try submitting the form again.</p>
    </>
  );

  return (
    <MessageBox
      title="Form Validation Errors"
      content={htmlContent}
      variant="warning"
      button={{
        buttonText: "Review Form",
        onClick: () => console.log("Reviewing form..."),
      }}
    />
  );
};
```

## Step 5: Timing and Persistence Control

### 5.1 Auto-Hide Functionality

Configure messages to automatically disappear after a specified time:

```tsx
import { MessageBox } from "@neuron/ui";
import { useState } from "react";

const AutoHideMessages = () => {
  const [showMessage, setShowMessage] = useState(true);

  return (
    <div className="auto-hide-messages">
      {showMessage && (
        <MessageBox
          title="Auto-Hide Message"
          content="This message will automatically disappear in 5 seconds."
          variant="info"
          autoHide={true}
          autoHideDelay={5000}
          onRemove={() => setShowMessage(false)}
        />
      )}

      <button onClick={() => setShowMessage(true)}>Show Auto-Hide Message</button>
    </div>
  );
};
```

### 5.2 Sticky Messages

Use sticky messages for persistent information that should remain visible:

```tsx
import { MessageBox } from "@neuron/ui";

const StickyMessages = () => {
  return (
    <div className="sticky-messages">
      {/* Sticky message that stays at the top */}
      <MessageBox
        title="System Maintenance"
        content="Scheduled maintenance will begin at 2:00 AM tonight. Some features may be unavailable."
        variant="warning"
        sticky={true}
      />

      {/* Regular content below */}
      <div style={{ height: "200vh", padding: "20px" }}>
        <p>Scroll down to see how the sticky message stays at the top...</p>
        <p>More content here...</p>
      </div>
    </div>
  );
};
```

### 5.3 Dismissible Messages

Allow users to manually close messages with a close button:

```tsx
import { MessageBox } from "@neuron/ui";
import { useState } from "react";

const DismissibleMessages = () => {
  const [showMessage, setShowMessage] = useState(true);

  return (
    <div className="dismissible-messages">
      {showMessage && (
        <MessageBox
          title="Dismissible Message"
          content="You can close this message by clicking the X button."
          variant="info"
          dismissible={true}
          onRemove={() => setShowMessage(false)}
        />
      )}

      {!showMessage && <button onClick={() => setShowMessage(true)}>Show Dismissible Message</button>}
    </div>
  );
};
```

## Step 6: Integration with Forms and Workflows

### 6.1 Form Validation Feedback

Integrate MessageBox with form validation to provide comprehensive feedback:

```tsx
import { MessageBox, Input, Button } from "@neuron/ui";
import { useState } from "react";
import { useForm } from "react-hook-form";

const FormWithMessageBox = () => {
  const [validationMessage, setValidationMessage] = useState(null);
  const [successMessage, setSuccessMessage] = useState(null);

  const {
    control,
    handleSubmit,
    formState: { errors },
  } = useForm();

  const onSubmit = async (data) => {
    try {
      // Simulate API call
      await new Promise((resolve) => setTimeout(resolve, 1000));

      setValidationMessage(null);
      setSuccessMessage({
        title: "Form Submitted",
        content: "Your information has been successfully saved.",
        variant: "success",
      });
    } catch (error) {
      setSuccessMessage(null);
      setValidationMessage({
        title: "Submission Failed",
        content: "Please check your information and try again.",
        variant: "danger",
      });
    }
  };

  const onError = () => {
    const errorCount = Object.keys(errors).length;
    const errorList = Object.entries(errors).map(([field, error]) => <li key={field}>{error.message}</li>);

    setSuccessMessage(null);
    setValidationMessage({
      title: "Form Validation Errors",
      content: (
        <>
          <p>
            Please correct the following {errorCount} error{errorCount > 1 ? "s" : ""}:
          </p>
          <ul>{errorList}</ul>
        </>
      ),
      variant: "warning",
    });
  };

  return (
    <form onSubmit={handleSubmit(onSubmit, onError)}>
      {/* Success message */}
      {successMessage && (
        <MessageBox
          title={successMessage.title}
          content={successMessage.content}
          variant={successMessage.variant}
          autoHide={true}
          autoHideDelay={5000}
          onRemove={() => setSuccessMessage(null)}
        />
      )}

      {/* Validation message */}
      {validationMessage && (
        <MessageBox
          title={validationMessage.title}
          content={validationMessage.content}
          variant={validationMessage.variant}
          dismissible={true}
          onRemove={() => setValidationMessage(null)}
        />
      )}

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
    </form>
  );
};
```

### 6.2 API Integration Patterns

Show MessageBox based on API call results:

```tsx
import { MessageBox, Button } from "@neuron/ui";
import { useState } from "react";
import { useApiCall } from "@neuron/core";

const ApiWithMessageBox = () => {
  const [message, setMessage] = useState(null);
  const [saveData, apiState] = useApiCall(api.saveUserData);

  const handleSave = async () => {
    try {
      await saveData({ name: "John Doe", email: "john@example.com" });

      setMessage({
        title: "Data Saved",
        content: "User information has been successfully updated.",
        variant: "success",
        button: {
          buttonText: "View Profile",
          onClick: () => console.log("Viewing profile..."),
        },
      });
    } catch (error) {
      setMessage({
        title: "Save Failed",
        content: "Unable to save data. Please check your connection and try again.",
        variant: "danger",
        button: {
          buttonText: "Retry",
          onClick: handleSave,
        },
      });
    }
  };

  const handleDelete = async () => {
    try {
      await api.deleteUser();

      setMessage({
        title: "User Deleted",
        content: "User account has been permanently removed from the system.",
        variant: "success",
      });
    } catch (error) {
      setMessage({
        title: "Delete Failed",
        content: "Unable to delete user account. Please contact support for assistance.",
        variant: "danger",
        button: {
          buttonText: "Contact Support",
          onClick: () => console.log("Contacting support..."),
        },
      });
    }
  };

  return (
    <div>
      {message && (
        <MessageBox
          title={message.title}
          content={message.content}
          variant={message.variant}
          button={message.button}
          dismissible={true}
          onRemove={() => setMessage(null)}
        />
      )}

      <Button onClick={handleSave} loading={apiState.fetchingState === "fetching"}>
        Save Data
      </Button>

      <Button onClick={handleDelete} variant="danger">
        Delete User
      </Button>
    </div>
  );
};
```

## Step 7: Advanced MessageBox Features

### 7.1 Custom Styling and Icons

While MessageBox uses automatic icon assignment, you can customize certain aspects:

```tsx
import { MessageBox } from "@neuron/ui";

const CustomStyledMessageBox = () => {
  return (
    <div className="custom-messagebox">
      {/* MessageBox without icon */}
      <MessageBox
        title="Custom Message"
        content="This message box doesn't show an icon."
        variant="info"
        showIcon={false}
      />

      {/* MessageBox with custom className */}
      <MessageBox
        title="Styled Message"
        content="This message box has custom styling applied."
        variant="success"
        className="my-custom-messagebox"
      />
    </div>
  );
};
```

### 7.2 MessageBox with Test ID

Add test IDs for automated testing:

```tsx
import { MessageBox } from "@neuron/ui";

const TestableMessageBox = () => {
  return (
    <MessageBox
      title="Test Message"
      content="This message box can be tested automatically."
      variant="info"
      testId="user-feedback-messagebox"
      button={{
        buttonText: "Test Action",
        onClick: () => console.log("Test action clicked"),
      }}
    />
  );
};
```

### 7.3 Conditional MessageBox Display

Show MessageBox based on application state:

```tsx
import { MessageBox } from "@neuron/ui";
import { useState, useEffect } from "react";

const ConditionalMessageBox = () => {
  const [userRole, setUserRole] = useState("user");
  const [connectionStatus, setConnectionStatus] = useState("connected");
  const [lastSaved, setLastSaved] = useState(null);

  useEffect(() => {
    // Simulate connection monitoring
    const interval = setInterval(() => {
      setConnectionStatus(Math.random() > 0.8 ? "disconnected" : "connected");
    }, 5000);

    return () => clearInterval(interval);
  }, []);

  return (
    <div className="conditional-messagebox">
      {/* Show warning for admin users */}
      {userRole === "admin" && (
        <MessageBox
          title="Admin Notice"
          content="You have administrative privileges. Use them responsibly."
          variant="warning"
          sticky={true}
        />
      )}

      {/* Show connection status */}
      {connectionStatus === "disconnected" && (
        <MessageBox
          title="Connection Lost"
          content="Your internet connection was interrupted. Attempting to reconnect..."
          variant="warning"
          autoHide={false}
        />
      )}

      {/* Show last saved status */}
      {lastSaved && (
        <MessageBox
          title="Auto-saved"
          content={`Last saved at ${lastSaved.toLocaleTimeString()}`}
          variant="success"
          autoHide={true}
          autoHideDelay={3000}
          onRemove={() => setLastSaved(null)}
        />
      )}

      {/* Controls for testing */}
      <div className="controls">
        <button onClick={() => setUserRole(userRole === "admin" ? "user" : "admin")}>Toggle Admin Role</button>
        <button onClick={() => setLastSaved(new Date())}>Trigger Auto-save</button>
      </div>
    </div>
  );
};
```

## Step 8: MessageBox Props Reference

### 8.1 Core MessageBox Props

| Prop      | Type                                                        | Default     | Description                                        |
| --------- | ----------------------------------------------------------- | ----------- | -------------------------------------------------- |
| title     | `string`                                                    | -           | Optional heading that summarizes the message       |
| content   | `ReactNode`                                                 | -           | **Required**. Main message content (supports HTML) |
| variant   | `"default" \| "info" \| "success" \| "warning" \| "danger"` | `"default"` | Visual variant and semantic meaning                |
| className | `string`                                                    | -           | Additional CSS classes                             |
| testId    | `string`                                                    | -           | Custom test ID for the component                   |

### 8.2 Display Control Props

| Prop        | Type      | Default | Description                                |
| ----------- | --------- | ------- | ------------------------------------------ |
| showIcon    | `boolean` | `true`  | Whether to display the variant-based icon  |
| fixedHeight | `boolean` | `false` | Always show full content without expansion |
| sticky      | `boolean` | `false` | Stick to top of page/relative parent       |
| dismissible | `boolean` | `false` | Show close button for manual dismissal     |

### 8.3 Timing and Auto-Hide Props

| Prop          | Type                                         | Default | Description                               |
| ------------- | -------------------------------------------- | ------- | ----------------------------------------- |
| autoHide      | `boolean`                                    | `false` | Automatically hide after specified delay  |
| autoHideDelay | `number`                                     | `3000`  | Duration in milliseconds before auto-hide |
| onRemove      | `(message: Record<string, unknown>) => void` | -       | Callback when message is removed          |

### 8.4 Button Props

| Prop   | Type                  | Default | Description                 |
| ------ | --------------------- | ------- | --------------------------- |
| button | `ButtonForMessageBox` | -       | Action button configuration |

#### ButtonForMessageBox Properties

| Property   | Type         | Description                          |
| ---------- | ------------ | ------------------------------------ |
| buttonText | `string`     | **Required**. Button label text      |
| onClick    | `() => void` | **Required**. Click handler function |
| iconLeft   | `TBaseIcons` | Optional left icon                   |
| iconRight  | `TBaseIcons` | Optional right icon                  |
| disabled   | `boolean`    | Whether button is disabled           |
| loading    | `boolean`    | Whether button shows loading state   |

### 8.5 Variant Mappings

| MessageBox Variant | Icon               | Button Variant | Use Case              |
| ------------------ | ------------------ | -------------- | --------------------- |
| `default`          | Circle Info        | `secondary`    | General information   |
| `info`             | Circle Info        | `info`         | Helpful information   |
| `success`          | Circle Check       | `success`      | Positive confirmation |
| `warning`          | Circle Exclamation | `warning`      | Caution messages      |
| `danger`           | Hand Stop          | `danger`       | Critical errors       |

## Step 9: Best Practices

### 9.1 When to Use Each Variant

**Default Messages:**

- General information that doesn't fit other categories
- Neutral system messages
- Basic user guidance

```tsx
// Good: General information
<MessageBox title="Updated" content="Your profile has been updated." variant="default" />
```

**Info Messages:**

- Helpful tips and guidance
- Status updates
- Educational content

```tsx
// Good: Helpful tip
<MessageBox title="Search Tip" content="Use Ctrl+F for faster searching." variant="info" />
```

**Success Messages:**

- Confirmation of completed actions
- Positive outcomes
- Achievement notifications

```tsx
// Good: Action confirmation
<MessageBox title="Changes Saved" content="Your changes have been saved successfully." variant="success" />
```

**Warning Messages:**

- Potential issues that allow continuation
- Important notices
- Caution before actions

```tsx
// Good: Important caution
<MessageBox title="Unsaved Changes" content="Do you really want to leave? Changes will be lost." variant="warning" />
```

**Danger Messages:**

- Critical errors
- Blocking issues
- System failures

```tsx
// Good: Critical error
<MessageBox
  title="System Error"
  content="An internal server error occurred. We're working to resolve it."
  variant="danger"
/>
```

### 9.2 Content Writing Guidelines

**Keep It Clear and Concise:**

```tsx
// Good: Clear and direct
<MessageBox
  title="Save Failed"
  content="Please try again or contact support."
  variant="danger"
/>

// Avoid: Too verbose
<MessageBox
  title="Operation Could Not Be Completed Successfully"
  content="The system encountered an unexpected error while attempting to save your data to the database. Please review your information and attempt the operation again, or contact our technical support team for assistance."
  variant="danger"
/>
```

**Be Specific and Actionable:**

```tsx
// Good: Specific with action
<MessageBox
  title="Contract Created"
  content="Contract #12345697 has been successfully created."
  variant="success"
  button={{
    buttonText: "Download Contract",
    onClick: handleDownload
  }}
/>

// Avoid: Vague messaging
<MessageBox
  title="Success"
  content="Operation completed."
  variant="success"
/>
```

**Match Content to Variant:**

```tsx
// Good: Content matches severity
<MessageBox
  title="Insufficient Permissions"
  content="You don't have permission to perform this action."
  variant="danger"
/>

// Wrong: Mismatch between content and variant
<MessageBox
  title="Success"
  content="An error occurred during processing."
  variant="success"
/>
```

### 9.3 Timing and Persistence Best Practices

**Use Auto-Hide Appropriately:**

- Success confirmations: 3-5 seconds
- Info messages: 5-7 seconds
- Warnings: Don't auto-hide or use longer delays
- Errors: Never auto-hide

```tsx
// Good: Appropriate timing
<MessageBox
  title="Saved"
  content="Changes saved successfully."
  variant="success"
  autoHide={true}
  autoHideDelay={4000}
/>

// Wrong: Error with auto-hide
<MessageBox
  title="Critical Error"
  content="System failure detected."
  variant="danger"
  autoHide={true}  // Don't auto-hide errors
/>
```

**Use Sticky Messages for:**

- System-wide announcements
- Persistent warnings
- Important notices that shouldn't be missed

```tsx
// Good: Important system notice
<MessageBox
  title="System Maintenance"
  content="Scheduled maintenance tonight at 2 AM."
  variant="warning"
  sticky={true}
/>
```

### 9.4 Button Integration Guidelines

**Provide Clear Actions:**

```tsx
// Good: Clear, actionable button
<MessageBox
  title="Save Failed"
  content="Unable to save your changes."
  variant="danger"
  button={{
    buttonText: "Retry Save",
    onClick: handleRetry
  }}
/>

// Good: Helpful action
<MessageBox
  title="Low Storage"
  content="Your storage is almost full."
  variant="warning"
  button={{
    buttonText: "Manage Storage",
    onClick: openStorageManager
  }}
/>
```

**Use Icons Meaningfully:**

```tsx
// Good: Icon enhances action
<MessageBox
  title="Report Ready"
  content="Your monthly report has been generated."
  variant="success"
  button={{
    buttonText: "Download",
    iconRight: baseIcons.downloadSolid,
    onClick: handleDownload,
  }}
/>
```

### 9.5 Accessibility Considerations

- Use clear, descriptive titles and content
- Ensure sufficient color contrast
- Provide keyboard navigation support
- Use semantic HTML in content
- Test with screen readers

```tsx
// Good: Accessible content structure
<MessageBox
  title="Form Validation Error"
  content={
    <>
      <p>Please correct the following errors:</p>
      <ul>
        <li>Email address is required</li>
        <li>Password must be at least 8 characters</li>
      </ul>
    </>
  }
  variant="warning"
/>
```

## Step 10: Common Patterns and Examples

### 10.1 System Status Messages

```tsx
import { MessageBox } from "@neuron/ui";
import { useState, useEffect } from "react";

const SystemStatusMessages = () => {
  const [systemStatus, setSystemStatus] = useState("operational");

  useEffect(() => {
    // Monitor system status
    const checkStatus = () => {
      // Simulate status check
      const statuses = ["operational", "maintenance", "degraded"];
      setSystemStatus(statuses[Math.floor(Math.random() * statuses.length)]);
    };

    const interval = setInterval(checkStatus, 30000);
    return () => clearInterval(interval);
  }, []);

  const getStatusMessage = () => {
    switch (systemStatus) {
      case "maintenance":
        return {
          title: "System Maintenance",
          content: "We're performing scheduled maintenance. Some features may be temporarily unavailable.",
          variant: "warning",
          sticky: true,
        };
      case "degraded":
        return {
          title: "Performance Issues",
          content: "We're experiencing some performance issues. Our team is working to resolve them.",
          variant: "warning",
        };
      default:
        return null;
    }
  };

  const statusMessage = getStatusMessage();

  return (
    <div>
      {statusMessage && (
        <MessageBox
          title={statusMessage.title}
          content={statusMessage.content}
          variant={statusMessage.variant}
          sticky={statusMessage.sticky}
        />
      )}

      <div className="main-content">
        <h1>Application Content</h1>
        <p>Your main application content goes here...</p>
      </div>
    </div>
  );
};
```

### 10.2 Multi-Step Process Feedback

```tsx
import { MessageBox, Button } from "@neuron/ui";
import { useState } from "react";

const MultiStepProcessFeedback = () => {
  const [currentStep, setCurrentStep] = useState(0);
  const [processStatus, setProcessStatus] = useState("idle");

  const steps = ["Validating information", "Processing payment", "Creating account", "Sending confirmation"];

  const startProcess = async () => {
    setProcessStatus("running");

    for (let i = 0; i < steps.length; i++) {
      setCurrentStep(i);
      // Simulate step processing
      await new Promise((resolve) => setTimeout(resolve, 2000));
    }

    setProcessStatus("completed");
  };

  const getProcessMessage = () => {
    if (processStatus === "idle") return null;

    if (processStatus === "running") {
      return {
        title: "Processing...",
        content: `Step ${currentStep + 1} of ${steps.length}: ${steps[currentStep]}`,
        variant: "info",
      };
    }

    if (processStatus === "completed") {
      return {
        title: "Process Complete",
        content: "Your account has been successfully created and activated.",
        variant: "success",
        button: {
          buttonText: "Continue to Dashboard",
          onClick: () => console.log("Navigating to dashboard..."),
        },
      };
    }
  };

  const message = getProcessMessage();

  return (
    <div>
      {message && (
        <MessageBox title={message.title} content={message.content} variant={message.variant} button={message.button} />
      )}

      {processStatus === "idle" && <Button onClick={startProcess}>Start Account Creation</Button>}
    </div>
  );
};
```

## Step 11: Common Mistakes to Avoid

### 11.1 Don't Use Wrong Variants

```tsx
// Wrong: Success variant for error content
<MessageBox
  title="Error Occurred"
  content="Unable to save your changes."
  variant="success"
/>

// Right: Match variant to content
<MessageBox
  title="Save Failed"
  content="Unable to save your changes."
  variant="danger"
/>
```

### 11.2 Don't Auto-Hide Critical Messages

```tsx
// Wrong: Auto-hiding error messages
<MessageBox
  title="Critical System Error"
  content="Database connection failed."
  variant="danger"
  autoHide={true}
/>

// Right: Keep critical messages visible
<MessageBox
  title="Critical System Error"
  content="Database connection failed."
  variant="danger"
  dismissible={true}
/>
```

### 11.3 Don't Write Verbose Content

```tsx
// Wrong: Too much text
<MessageBox
  title="Information About Your Account Status"
  content="We would like to inform you that your account has been successfully updated with the new information that you provided through the form submission process, and all changes have been saved to our secure database system."
  variant="info"
/>

// Right: Concise and clear
<MessageBox
  title="Account Updated"
  content="Your account information has been successfully updated."
  variant="success"
/>
```

### 11.4 Don't Forget Action Context

```tsx
// Wrong: Vague button text
<MessageBox
  title="Error"
  content="Something went wrong."
  variant="danger"
  button={{
    buttonText: "OK",
    onClick: () => {}
  }}
/>

// Right: Specific, actionable button
<MessageBox
  title="Save Failed"
  content="Unable to save your changes. Please try again."
  variant="danger"
  button={{
    buttonText: "Retry Save",
    onClick: handleRetry
  }}
/>
```

## Summary

The Neuron MessageBox component provides a comprehensive, accessible, and consistent system for contextual user feedback. Key points to remember:

1. **Use appropriate variants** based on message type and severity
2. **Keep content clear and concise** with actionable information
3. **Provide meaningful actions** through well-designed buttons
4. **Control timing appropriately** - auto-hide for confirmations, persistent for errors
5. **Support content expansion** for longer messages
6. **Integrate with workflows** for comprehensive user feedback
7. **Follow accessibility guidelines** for inclusive design
8. **Match visual design to content** for consistent user experience

### Key Takeaways

- **Contextual feedback** that provides important information without blocking workflow
- **Automatic content expansion** with "Show more/less" functionality
- **Variant-based styling** with appropriate icons and button colors
- **Flexible timing control** with auto-hide and sticky options
- **Rich content support** including HTML and React components
- **Action integration** with contextually styled buttons
- **Built-in accessibility** features for inclusive design
- **Consistent behavior** across all application contexts

By following these guidelines, you'll create effective, user-friendly feedback systems that enhance the user experience and provide clear, actionable information across your Neuron applications.

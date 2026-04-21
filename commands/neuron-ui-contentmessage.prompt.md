---
agent: agent
---

# AI-Assisted Neuron ContentMessage Component Integration Guide

## For the AI Assistant

Your task is to integrate and configure the Neuron ContentMessage component in React applications. This guide provides comprehensive instructions for implementing ContentMessage components, which serve as versatile message displays with icons, titles, subtitles, tags, and action buttons for various use cases including error pages, empty states, and informational content.

## Sync Metadata

- **Component Version:** v3.1.4
- **Component Source:** `packages/neuron/ui/src/lib/messages/contentMessage/ContentMessage.tsx`
- **Guideline Command:** `/neuron-ui-contentmessage`
- **Related Skill:** `neuron-ui-messages`

## Introduction

The ContentMessage component is a flexible UI element designed to display structured messages with visual hierarchy and optional interactive elements. It serves as a comprehensive solution for error pages, empty data placeholders, informational displays, and status communications within Neuron applications.

### What is the ContentMessage Component?

The ContentMessage component provides structured message display with support for:

- **Title and subtitle** - Hierarchical text content with automatic title uppercasing
- **Icon integration** - Visual enhancement with variant-based icon mapping
- **Tag display** - Optional tag with tooltip support for additional context
- **Action buttons** - Interactive button zone for user actions
- **Variant styling** - Semantic variants (default, info, success, warning, danger)
- **Size options** - Small, large, and ultra size configurations
- **Layout modes** - Vertical (default) and horizontal orientations
- **Accessibility** - Built-in accessibility features and test ID support

### Key Features

- **Structured Content Display**: Hierarchical title/subtitle with consistent formatting
- **Variant-Based Styling**: Semantic variants with automatic icon mapping
- **Flexible Layout**: Vertical and horizontal orientation support
- **Interactive Elements**: Tag display and action button integration
- **Size Configurations**: Multiple size options for different contexts
- **Icon Integration**: Automatic icon selection based on variant or custom icons
- **Accessibility Compliant**: Full keyboard navigation and screen reader support
- **Test Integration**: Built-in test ID support for automated testing

**Storybook Reference:** You can refer to the Storybook documentation for visual examples and interactive demos of the ContentMessage component.

## Step 1: Basic ContentMessage Implementation

### 1.1 Import the ContentMessage Component

```tsx
import { ContentMessage } from "@neuron/ui";
```

### 1.2 Basic ContentMessage Usage

Here's a simple implementation of the ContentMessage component:

```tsx
import { ContentMessage } from "@neuron/ui";

const BasicContentMessageExample = () => {
  return (
    <div>
      {/* Basic content message */}
      <ContentMessage title="Welcome" subtitle="This is a basic content message example" />

      {/* Content message with custom icon */}
      <ContentMessage title="Information" subtitle="Content message with custom icon" icon="infoCircleSolid" />

      {/* Content message without icon */}
      <ContentMessage title="No Icon" subtitle="Content message without icon display" showIcon={false} />
    </div>
  );
};
```

### 1.3 ContentMessage with Tag

Add tags to provide additional context:

```tsx
import { ContentMessage } from "@neuron/ui";

const TaggedContentMessageExample = () => {
  return (
    <div>
      {/* Content message with tag */}
      <ContentMessage
        title="Tagged Message"
        subtitle="This message includes a contextual tag"
        tagText="Important"
        tagTooltip="This message requires attention"
      />

      {/* Content message with tag but no tooltip */}
      <ContentMessage title="Simple Tag" subtitle="Message with simple tag display" tagText="Status: Active" />
    </div>
  );
};
```

## Step 2: ContentMessage Variants

### 2.1 Semantic Variants

Use semantic variants to communicate different types of messages:

```tsx
import { ContentMessage } from "@neuron/ui";

const VariantsExample = () => {
  return (
    <div>
      {/* Default variant */}
      <ContentMessage title="Default Message" subtitle="Standard content message display" variant="default" />

      {/* Info variant */}
      <ContentMessage title="Information" subtitle="Informational content message" variant="info" />

      {/* Success variant */}
      <ContentMessage title="Success" subtitle="Operation completed successfully" variant="success" />

      {/* Warning variant */}
      <ContentMessage title="Warning" subtitle="Please review this information" variant="warning" />

      {/* Danger variant */}
      <ContentMessage title="Error" subtitle="An error occurred during processing" variant="danger" />
    </div>
  );
};
```

### 2.2 Size Configurations

Configure message sizes based on context and importance:

```tsx
import { ContentMessage } from "@neuron/ui";

const SizeConfigurationsExample = () => {
  return (
    <div>
      {/* Small size (default) */}
      <ContentMessage title="Small Message" subtitle="Compact content message display" size="small" />

      {/* Large size */}
      <ContentMessage title="Large Message" subtitle="Prominent content message display" size="large" />

      {/* Ultra large size */}
      <ContentMessage title="Ultra Large Message" subtitle="Maximum prominence content message" size="ultra" />
    </div>
  );
};
```

### 2.3 Layout Orientations

Choose between vertical and horizontal layouts:

```tsx
import { ContentMessage } from "@neuron/ui";

const LayoutOrientationsExample = () => {
  return (
    <div>
      {/* Vertical layout (default) */}
      <ContentMessage title="Vertical Layout" subtitle="Standard vertical content arrangement" tagText="Default" />

      {/* Horizontal layout */}
      <ContentMessage
        title="Horizontal Layout"
        subtitle="Compact horizontal content arrangement"
        tagText="Compact"
        horizontal={true}
      />
    </div>
  );
};
```

## Step 3: Interactive ContentMessage with Buttons

### 3.1 ContentMessage with Action Buttons

Add interactive buttons for user actions:

```tsx
import { ContentMessage } from "@neuron/ui";

const InteractiveContentMessageExample = () => {
  const handlePrimaryAction = () => {
    console.log("Primary action clicked");
  };

  const handleSecondaryAction = () => {
    console.log("Secondary action clicked");
  };

  const buttons = [
    {
      label: "Primary Action",
      onClick: handlePrimaryAction,
      variant: "primary" as const,
    },
    {
      label: "Secondary Action",
      onClick: handleSecondaryAction,
      variant: "secondary" as const,
    },
  ];

  return (
    <div>
      {/* Content message with action buttons */}
      <ContentMessage
        title="Interactive Message"
        subtitle="This message includes action buttons"
        buttons={buttons}
        variant="info"
      />

      {/* Content message with single button */}
      <ContentMessage
        title="Single Action"
        subtitle="Message with one action button"
        buttons={[
          {
            label: "Take Action",
            onClick: handlePrimaryAction,
            variant: "primary" as const,
          },
        ]}
      />
    </div>
  );
};
```

### 3.2 Complete Interactive Example

Combine all features for comprehensive message display:

```tsx
import { ContentMessage } from "@neuron/ui";

const CompleteInteractiveExample = () => {
  const handleRetry = () => {
    console.log("Retry action");
  };

  const handleCancel = () => {
    console.log("Cancel action");
  };

  const actionButtons = [
    {
      label: "Retry",
      onClick: handleRetry,
      variant: "primary" as const,
    },
    {
      label: "Cancel",
      onClick: handleCancel,
      variant: "secondary" as const,
    },
  ];

  return (
    <div>
      <ContentMessage
        title="Operation Failed"
        subtitle="The requested operation could not be completed. Please try again or contact support."
        variant="danger"
        size="large"
        tagText="Error Code: 500"
        tagTooltip="Internal server error occurred"
        buttons={actionButtons}
        icon="exclamationTriangleSolid"
      />
    </div>
  );
};
```

## Step 4: Common Use Cases and Patterns

### 4.1 Error Page Implementation

Use ContentMessage as the main content for error pages:

```tsx
import { ContentMessage } from "@neuron/ui";

const ErrorPageExample = () => {
  const handleGoHome = () => {
    window.location.href = "/";
  };

  const handleGoBack = () => {
    window.history.back();
  };

  const errorButtons = [
    {
      label: "Go Home",
      onClick: handleGoHome,
      variant: "primary" as const,
    },
    {
      label: "Go Back",
      onClick: handleGoBack,
      variant: "secondary" as const,
    },
  ];

  return (
    <div>
      <ContentMessage
        title="Page Not Found"
        subtitle="The page you are looking for might have been removed, had its name changed, or is temporarily unavailable."
        variant="danger"
        size="ultra"
        tagText="Error 404"
        buttons={errorButtons}
      />
    </div>
  );
};
```

### 4.2 Empty State Placeholder

Use ContentMessage for empty data states:

```tsx
import { ContentMessage } from "@neuron/ui";

const EmptyStateExample = () => {
  const handleCreateNew = () => {
    console.log("Create new item");
  };

  const handleImport = () => {
    console.log("Import data");
  };

  const emptyStateButtons = [
    {
      label: "Create New",
      onClick: handleCreateNew,
      variant: "primary" as const,
    },
    {
      label: "Import Data",
      onClick: handleImport,
      variant: "secondary" as const,
    },
  ];

  return (
    <div>
      <ContentMessage
        title="No Data Available"
        subtitle="You haven't created any items yet. Get started by creating your first item or importing existing data."
        variant="info"
        size="large"
        tagText="Empty State"
        buttons={emptyStateButtons}
        icon="folderOpenSolid"
      />
    </div>
  );
};
```

### 4.3 Success Confirmation

Display success messages with follow-up actions:

```tsx
import { ContentMessage } from "@neuron/ui";

const SuccessConfirmationExample = () => {
  const handleViewDetails = () => {
    console.log("View details");
  };

  const handleCreateAnother = () => {
    console.log("Create another");
  };

  const successButtons = [
    {
      label: "View Details",
      onClick: handleViewDetails,
      variant: "primary" as const,
    },
    {
      label: "Create Another",
      onClick: handleCreateAnother,
      variant: "secondary" as const,
    },
  ];

  return (
    <div>
      <ContentMessage
        title="Success"
        subtitle="Your item has been created successfully. You can now view the details or create another item."
        variant="success"
        size="large"
        tagText="Completed"
        buttons={successButtons}
      />
    </div>
  );
};
```

### 4.4 Loading State with Message

Display loading states with informational content:

```tsx
import { ContentMessage } from "@neuron/ui";

const LoadingStateExample = () => {
  return (
    <div>
      <ContentMessage
        title="Processing"
        subtitle="Please wait while we process your request. This may take a few moments."
        variant="info"
        size="large"
        tagText="In Progress"
        icon="spinnerSolid"
        // No buttons during loading state
      />
    </div>
  );
};
```

### 4.5 Warning with Action Required

Display warnings that require user attention:

```tsx
import { ContentMessage } from "@neuron/ui";

const WarningActionExample = () => {
  const handleResolve = () => {
    console.log("Resolve issue");
  };

  const handleDismiss = () => {
    console.log("Dismiss warning");
  };

  const warningButtons = [
    {
      label: "Resolve Now",
      onClick: handleResolve,
      variant: "primary" as const,
    },
    {
      label: "Dismiss",
      onClick: handleDismiss,
      variant: "secondary" as const,
    },
  ];

  return (
    <div>
      <ContentMessage
        title="Action Required"
        subtitle="Your account requires verification to continue using all features. Please complete the verification process."
        variant="warning"
        size="large"
        tagText="Verification Needed"
        tagTooltip="Account verification is required for full access"
        buttons={warningButtons}
      />
    </div>
  );
};
```

## Step 5: Advanced Patterns and Integration

### 5.1 Conditional ContentMessage Display

Implement conditional rendering based on application state:

```tsx
import { ContentMessage } from "@neuron/ui";
import { useState } from "react";

const ConditionalDisplayExample = () => {
  const [dataState, setDataState] = useState<"loading" | "empty" | "error" | "success">("loading");
  const [data, setData] = useState<any[]>([]);

  const handleRetry = () => {
    setDataState("loading");
    // Simulate API call
    setTimeout(() => {
      setDataState("success");
      setData([{ id: 1, name: "Sample Data" }]);
    }, 2000);
  };

  const handleRefresh = () => {
    setDataState("loading");
    setData([]);
    // Simulate refresh
    setTimeout(() => {
      setDataState("empty");
    }, 1000);
  };

  const renderContentMessage = () => {
    switch (dataState) {
      case "loading":
        return (
          <ContentMessage
            title="Loading"
            subtitle="Fetching your data..."
            variant="info"
            icon="spinnerSolid"
            tagText="Please Wait"
          />
        );

      case "empty":
        return (
          <ContentMessage
            title="No Data"
            subtitle="No items found. Try refreshing or check back later."
            variant="info"
            buttons={[
              {
                label: "Refresh",
                onClick: handleRefresh,
                variant: "primary" as const,
              },
            ]}
          />
        );

      case "error":
        return (
          <ContentMessage
            title="Error"
            subtitle="Failed to load data. Please try again."
            variant="danger"
            tagText="Connection Error"
            buttons={[
              {
                label: "Retry",
                onClick: handleRetry,
                variant: "primary" as const,
              },
            ]}
          />
        );

      case "success":
        return null; // Show actual data

      default:
        return null;
    }
  };

  return (
    <div>
      {dataState === "success" ? (
        <div>
          <h3>Data Loaded Successfully</h3>
          <div>
            {data.map((item) => (
              <div key={item.id}>{item.name}</div>
            ))}
          </div>
          <button onClick={() => setDataState("empty")}>Simulate Empty</button>
          <button onClick={() => setDataState("error")}>Simulate Error</button>
        </div>
      ) : (
        renderContentMessage()
      )}
    </div>
  );
};
```

### 5.2 ContentMessage in Modal Context

Use ContentMessage within modal dialogs:

```tsx
import { ContentMessage } from "@neuron/ui";

const ModalContentMessageExample = () => {
  const handleConfirm = () => {
    console.log("Confirmed");
    // Close modal logic
  };

  const handleCancel = () => {
    console.log("Cancelled");
    // Close modal logic
  };

  const confirmationButtons = [
    {
      label: "Confirm",
      onClick: handleConfirm,
      variant: "primary" as const,
    },
    {
      label: "Cancel",
      onClick: handleCancel,
      variant: "secondary" as const,
    },
  ];

  return (
    <div>
      {/* This would typically be inside a Modal component */}
      <ContentMessage
        title="Confirm Action"
        subtitle="Are you sure you want to delete this item? This action cannot be undone."
        variant="warning"
        size="small"
        horizontal={true}
        tagText="Destructive Action"
        buttons={confirmationButtons}
      />
    </div>
  );
};
```

### 5.3 Responsive ContentMessage

Adapt ContentMessage for different screen sizes:

```tsx
import { ContentMessage } from "@neuron/ui";
import { useState, useEffect } from "react";

const ResponsiveContentMessageExample = () => {
  const [isMobile, setIsMobile] = useState(false);

  useEffect(() => {
    const checkScreenSize = () => {
      setIsMobile(window.innerWidth < 768);
    };

    checkScreenSize();
    window.addEventListener("resize", checkScreenSize);
    return () => window.removeEventListener("resize", checkScreenSize);
  }, []);

  const handleAction = () => {
    console.log("Action clicked");
  };

  return (
    <div>
      <ContentMessage
        title="Responsive Message"
        subtitle="This message adapts to different screen sizes"
        variant="info"
        size={isMobile ? "small" : "large"}
        horizontal={isMobile}
        tagText={isMobile ? "Mobile" : "Desktop"}
        buttons={[
          {
            label: isMobile ? "Action" : "Take Action",
            onClick: handleAction,
            variant: "primary" as const,
          },
        ]}
      />
    </div>
  );
};
```

## Step 6: Accessibility and Best Practices

### 6.1 Accessibility Features

The ContentMessage component includes built-in accessibility features:

- **Semantic Structure**: Proper heading hierarchy with title and subtitle
- **ARIA Labels**: Appropriate labeling for screen readers
- **Keyboard Navigation**: Full keyboard support for interactive elements
- **Focus Management**: Proper focus indicators and management
- **Test Integration**: Built-in test ID support for automated testing

```tsx
import { ContentMessage } from "@neuron/ui";

const AccessibilityExample = () => {
  const handleAccessibleAction = () => {
    console.log("Accessible action performed");
  };

  return (
    <div>
      <ContentMessage
        title="Accessible Message"
        subtitle="This message follows accessibility best practices"
        variant="info"
        testId="accessible-content-message"
        buttons={[
          {
            label: "Accessible Action",
            onClick: handleAccessibleAction,
            variant: "primary" as const,
          },
        ]}
      />
    </div>
  );
};
```

### 6.2 Best Practices

**Do:**

- Use semantic variants that match the message intent
- Keep titles concise and descriptive (they are automatically uppercased)
- Provide clear, actionable subtitles
- Use tags for additional context, not primary information
- Choose appropriate sizes based on message importance
- Include meaningful button labels for actions

**Don't:**

- Mix multiple variants in the same context without clear purpose
- Use overly long titles (they will be uppercased)
- Rely solely on color to convey meaning
- Include too many action buttons (limit to 2-3 for clarity)
- Use ContentMessage for simple text display (use other components instead)

### 6.3 Performance Considerations

- ContentMessage components are lightweight with minimal performance impact
- Icon rendering is optimized through the variant-based icon mapping system
- Button rendering is efficient and supports various interaction patterns
- Consider lazy loading for ContentMessage components in large lists or complex layouts

## Summary

The Neuron ContentMessage component provides comprehensive structured message display with support for:

- **Hierarchical Content**: Title and subtitle with automatic formatting
- **Semantic Variants**: Default, info, success, warning, and danger styling
- **Interactive Elements**: Tag display and action button integration
- **Flexible Layout**: Vertical and horizontal orientation options
- **Size Configurations**: Small, large, and ultra size options
- **Icon Integration**: Variant-based automatic mapping or custom icons
- **Accessibility**: Full keyboard navigation and screen reader support

Use ContentMessage strategically for error pages, empty states, informational displays, and any scenario requiring structured message presentation with optional user actions. The component's flexibility makes it suitable for a wide range of communication needs in your Neuron applications.

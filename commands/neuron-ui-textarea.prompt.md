---
agent: agent
---

# Textarea Component AI Guidelines

## For the AI Assistant

Your task is to integrate and configure the Neuron Textarea component in React applications. This guide provides essential instructions for implementing Textarea components, which provide multi-line text input functionality with validation, resizing, character counting, and accessibility features through React Hook Form integration across all Neuron applications.

## Overview

The TextArea component is a multi-line text input field built on PrimeReact InputTextarea, providing validation, character counting, auto-resize functionality, and enhanced UX features through React Hook Form integration. It's ideal for longer text content like descriptions, comments, and messages.

## Core Usage

### Basic Implementation

```tsx
import { Form } from "@neuron/ui";
import { useForm } from "react-hook-form";
import { zodResolver } from "@hookform/resolvers/zod";
import { z } from "zod";

const schema = z.object({
  description: z.string().min(10, { message: "Description must be at least 10 characters" }),
  comments: z.string().max(500, { message: "Comments cannot exceed 500 characters" }),
  feedback: z.string().optional(),
});

const MyForm = () => {
  const { control } = useForm({
    resolver: zodResolver(schema),
    mode: "onChange",
  });

  return (
    <>
      <Form.TextArea
        name="description"
        labelText="Product Description"
        control={control}
        placeholder="Enter detailed product description..."
        required
        requiredFlag={true}
        maxLength={1000}
        autoResize={true}
        description="Provide a comprehensive description of the product"
      />

      <Form.TextArea
        name="comments"
        labelText="Additional Comments"
        control={control}
        placeholder="Any additional comments..."
        maxLength={500}
        restrictMaxLength={true}
        tooltip="Optional comments or special instructions"
      />

      <Form.TextArea
        name="feedback"
        labelText="Customer Feedback"
        control={control}
        placeholder="Share your experience..."
        optional
        maxLength={2000}
        restrictMaxLength={false}
        autoResize={true}
        maxHeight="200px"
      />
    </>
  );
};
```

### Character Counting and Length Restrictions

```tsx
const ContentForm = () => {
  const { control } = useForm();

  return (
    <>
      {/* Strict character limit - prevents typing beyond limit */}
      <Form.TextArea
        name="title"
        labelText="Article Title"
        control={control}
        maxLength={100}
        restrictMaxLength={true}
        placeholder="Enter article title (max 100 characters)"
        description="Title will be used in search results"
      />

      {/* Soft character limit - allows typing beyond limit with warning */}
      <Form.TextArea
        name="excerpt"
        labelText="Article Excerpt"
        control={control}
        maxLength={250}
        restrictMaxLength={false}
        placeholder="Brief excerpt for preview..."
        description="Recommended length: 150-250 characters"
      />

      {/* No character limit */}
      <Form.TextArea
        name="content"
        labelText="Article Content"
        control={control}
        placeholder="Write your article content here..."
        autoResize={true}
        maxHeight="400px"
        description="Full article content with no character restrictions"
      />
    </>
  );
};
```

## Key Props Reference

### Essential Props

- **name**: Field name for form registration
- **control**: React Hook Form control object
- **labelText**: Accessible label text for the textarea
- **placeholder**: Placeholder text for empty textarea

### Character Management

- **maxLength**: Maximum number of characters allowed
- **restrictMaxLength**: `true` prevents typing beyond limit, `false` allows with warning
- Character counter automatically displays when `maxLength` is set

### Layout & Sizing

- **autoResize**: Automatically adjusts height based on content
- **maxHeight**: Maximum height when auto-resize is enabled (e.g., "200px")
- **className**: CSS classes for grid positioning and styling

### Validation & States

- **required**: HTML5 required attribute
- **requiredFlag**: Shows required indicator (\*)
- **optional**: Shows optional indicator
- **isValid**: Manual valid state indicator

### Enhanced Features

- **tooltip**: Help text with tooltip integration
- **description**: Helper text above the textarea
- **descriptionVariant**: `"default"` | `"success"` | `"warning"` | `"danger"` | `"info"`
- **literalView**: Display as read-only text instead of textarea
- **testId**: Custom test identifier for testing

### Interaction & Behavior

- **disabled**: Disable textarea interactions
- **readOnly**: Make textarea read-only
- **onChange**: Custom change event handler

### Access Control

- **readonlyAccess**: Role-based read access control
- **fullAccess**: Role-based full access control

## Validation Patterns

### Zod Schema Validation

```tsx
// Basic length validation
const basicSchema = z.object({
  description: z
    .string()
    .min(10, { message: "Description must be at least 10 characters" })
    .max(1000, { message: "Description cannot exceed 1000 characters" }),

  comments: z.string().max(500, { message: "Comments too long" }).optional(),

  feedback: z
    .string()
    .min(1, { message: "Feedback is required" })
    .refine((val) => val.trim().length > 0, {
      message: "Feedback cannot be empty or just whitespace",
    }),
});

// Advanced content validation
const contentSchema = z.object({
  articleContent: z
    .string()
    .min(100, { message: "Article must be at least 100 characters" })
    .max(10000, { message: "Article too long" })
    .refine(
      (content) => {
        const wordCount = content.trim().split(/\s+/).length;
        return wordCount >= 20;
      },
      { message: "Article must contain at least 20 words" },
    )
    .refine(
      (content) => {
        // Check for prohibited content
        const prohibited = ["spam", "inappropriate"];
        return !prohibited.some((word) => content.toLowerCase().includes(word.toLowerCase()));
      },
      { message: "Content contains prohibited words" },
    ),

  summary: z
    .string()
    .max(250, { message: "Summary too long" })
    .refine(
      (summary) => {
        // Summary should be significantly shorter than main content
        return summary.length > 0;
      },
      { message: "Summary is required" },
    ),
});

// Conditional validation based on other fields
const conditionalSchema = z
  .object({
    messageType: z.enum(["urgent", "normal", "low"]),
    message: z.string(),
    details: z.string().optional(),
  })
  .refine(
    (data) => {
      if (data.messageType === "urgent") {
        return data.message.length >= 50;
      }
      return data.message.length >= 10;
    },
    {
      message: "Urgent messages require detailed explanation (min 50 characters)",
      path: ["message"],
    },
  )
  .refine(
    (data) => {
      if (data.messageType === "urgent") {
        return data.details && data.details.length >= 20;
      }
      return true;
    },
    {
      message: "Urgent messages require additional details",
      path: ["details"],
    },
  );
```

### Custom Validation Functions

```tsx
// Word count validation
const validateWordCount = (text: string, minWords: number, maxWords?: number) => {
  const words = text
    .trim()
    .split(/\s+/)
    .filter((word) => word.length > 0);

  if (words.length < minWords) {
    return `Minimum ${minWords} words required (current: ${words.length})`;
  }

  if (maxWords && words.length > maxWords) {
    return `Maximum ${maxWords} words allowed (current: ${words.length})`;
  }

  return true;
};

// Content quality validation
const validateContentQuality = (text: string) => {
  const trimmed = text.trim();

  if (trimmed.length === 0) return "Content cannot be empty";

  // Check for repeated characters
  if (/(.)\1{10,}/.test(trimmed)) {
    return "Content contains too many repeated characters";
  }

  // Check for minimum sentence structure
  const sentences = trimmed.split(/[.!?]+/).filter((s) => s.trim().length > 0);
  if (sentences.length < 2) {
    return "Content should contain at least 2 sentences";
  }

  return true;
};

// Usage with custom validation
const FormWithCustomValidation = () => {
  const schema = z.object({
    essay: z
      .string()
      .refine((text) => validateWordCount(text, 50, 500))
      .refine(validateContentQuality),

    review: z.string().refine((text) => validateWordCount(text, 10, 100)),
  });

  const { control } = useForm({
    resolver: zodResolver(schema),
    mode: "onChange",
  });

  return (
    <>
      <Form.TextArea
        name="essay"
        labelText="Essay"
        control={control}
        maxLength={3000}
        autoResize={true}
        description="Write a thoughtful essay (50-500 words)"
      />

      <Form.TextArea
        name="review"
        labelText="Product Review"
        control={control}
        maxLength={600}
        restrictMaxLength={false}
        description="Share your experience (10-100 words recommended)"
      />
    </>
  );
};
```

## Advanced Features

### Auto-Resize with Height Limits

```tsx
<Form.TextArea
  name="expandingContent"
  labelText="Expanding Content"
  control={control}
  autoResize={true}
  maxHeight="300px"
  placeholder="This textarea will grow as you type..."
  description="Automatically adjusts height up to 300px"
/>
```

### Custom Change Handling

```tsx
const handleContentChange = useCallback(
  (value: string, event: ChangeEvent<FormControlElement>) => {
    console.log("Content changed:", value.length, "characters");

    // Auto-save draft
    if (value.length > 50) {
      saveDraft(value);
    }

    // Update word count
    const wordCount = value
      .trim()
      .split(/\s+/)
      .filter((w) => w.length > 0).length;
    setWordCount(wordCount);

    // Prevent default if needed
    if (someCondition) {
      event.preventDefault();
    }
  },
  [saveDraft, setWordCount],
);

<Form.TextArea
  name="content"
  labelText="Article Content"
  control={control}
  onChange={handleContentChange}
  maxLength={5000}
  autoResize={true}
/>;
```

### Literal View Mode

```tsx
<Form.TextArea
  name="savedContent"
  labelText="Saved Content"
  control={control}
  literalView={true} // Shows content as read-only formatted text
/>
```

### Character Limit Behaviors

```tsx
const CharacterLimitExamples = () => {
  return (
    <>
      {/* Strict limit - prevents typing beyond maxLength */}
      <Form.TextArea
        name="strictLimit"
        labelText="Strict Character Limit"
        control={control}
        maxLength={100}
        restrictMaxLength={true}
        description="Cannot type beyond 100 characters"
      />

      {/* Soft limit - allows typing with warning */}
      <Form.TextArea
        name="softLimit"
        labelText="Soft Character Limit"
        control={control}
        maxLength={100}
        restrictMaxLength={false}
        description="Can type beyond 100 characters with warning"
      />

      {/* No limit - unlimited typing */}
      <Form.TextArea
        name="noLimit"
        labelText="No Character Limit"
        control={control}
        description="No character restrictions"
      />
    </>
  );
};
```

## Common Use Cases

### Content Management

```tsx
const ContentManagementForm = () => {
  return (
    <>
      <Form.TextArea
        name="title"
        labelText="Article Title"
        control={control}
        maxLength={100}
        restrictMaxLength={true}
        placeholder="Enter compelling article title..."
      />

      <Form.TextArea
        name="excerpt"
        labelText="Article Excerpt"
        control={control}
        maxLength={250}
        restrictMaxLength={false}
        placeholder="Brief summary for preview..."
        description="Used in article previews and search results"
      />

      <Form.TextArea
        name="content"
        labelText="Article Content"
        control={control}
        autoResize={true}
        maxHeight="500px"
        placeholder="Write your article content here..."
        description="Full article content in markdown format"
      />

      <Form.TextArea
        name="metaDescription"
        labelText="Meta Description"
        control={control}
        maxLength={160}
        restrictMaxLength={true}
        placeholder="SEO meta description..."
        description="Used by search engines (max 160 characters)"
      />
    </>
  );
};
```

### Feedback and Reviews

```tsx
const FeedbackForm = () => {
  return (
    <>
      <Form.TextArea
        name="experience"
        labelText="Overall Experience"
        control={control}
        maxLength={1000}
        autoResize={true}
        placeholder="Describe your overall experience..."
        required
        description="Help us understand your experience"
      />

      <Form.TextArea
        name="improvements"
        labelText="Suggested Improvements"
        control={control}
        maxLength={500}
        placeholder="What could we improve?"
        optional
        description="Optional suggestions for improvement"
      />

      <Form.TextArea
        name="additionalComments"
        labelText="Additional Comments"
        control={control}
        placeholder="Any other feedback..."
        optional
        autoResize={true}
        maxHeight="150px"
      />
    </>
  );
};
```

### Communication Forms

```tsx
const MessageForm = () => {
  return (
    <>
      <Form.TextArea
        name="subject"
        labelText="Subject"
        control={control}
        maxLength={100}
        restrictMaxLength={true}
        placeholder="Message subject..."
        required
      />

      <Form.TextArea
        name="message"
        labelText="Message"
        control={control}
        maxLength={2000}
        autoResize={true}
        maxHeight="300px"
        placeholder="Type your message here..."
        required
        description="Detailed message content"
      />

      <Form.TextArea
        name="signature"
        labelText="Signature"
        control={control}
        maxLength={200}
        placeholder="Your signature..."
        optional
        description="Optional signature to append to message"
      />
    </>
  );
};
```

## Role-Based Access Control

```tsx
const { userPermissions } = useAuth();

<Form.TextArea
  name="sensitiveContent"
  labelText="Sensitive Information"
  control={control}
  fullAccess={userPermissions.includes("edit_sensitive") ? "admin" : undefined}
  readonlyAccess={userPermissions.includes("view_sensitive") ? "user" : undefined}
  maxLength={1000}
  description="Restricted access based on user permissions"
/>;
```

## Testing Integration

```tsx
<Form.TextArea
  name="testableTextArea"
  labelText="Testable TextArea"
  control={control}
  maxLength={500}
  testId="feedback-textarea"
/>;

// In tests
describe("TextArea Component", () => {
  it("should enforce character limits", async () => {
    render(<TestForm />);

    const textarea = screen.getByTestId("feedback-textarea");
    const longText = "a".repeat(600);

    fireEvent.change(textarea, { target: { value: longText } });

    await waitFor(() => {
      expect(textarea).toHaveValue("a".repeat(500)); // Truncated to maxLength
    });
  });

  it("should show character counter", async () => {
    render(<TestForm />);

    const textarea = screen.getByTestId("feedback-textarea");
    fireEvent.change(textarea, { target: { value: "Hello" } });

    await waitFor(() => {
      expect(screen.getByText("Character Count (5/500)")).toBeInTheDocument();
    });
  });

  it("should auto-resize when enabled", async () => {
    render(<TestForm autoResize={true} />);

    const textarea = screen.getByTestId("feedback-textarea");
    const multilineText = "Line 1\nLine 2\nLine 3\nLine 4";

    fireEvent.change(textarea, { target: { value: multilineText } });

    await waitFor(() => {
      expect(textarea.style.height).not.toBe("");
    });
  });
});
```

## Performance Considerations

```tsx
const OptimizedTextArea = memo(({ control, maxLength, ...props }) => {
  // Memoize change handler to prevent unnecessary re-renders
  const handleChange = useCallback((value: string) => {
    // Debounce auto-save for performance
    debouncedAutoSave(value);
  }, []);

  return <Form.TextArea control={control} maxLength={maxLength} onChange={handleChange} {...props} />;
});
```

## Troubleshooting

### Common Issues and Solutions

1. **Character counter not showing**

   ```tsx
   // Ensure maxLength is set
   <Form.TextArea maxLength={500} /> // Counter will appear
   ```

2. **Auto-resize not working**

   ```tsx
   // Enable autoResize prop
   <Form.TextArea autoResize={true} maxHeight="300px" />
   ```

3. **Validation not working with character limits**
   ```tsx
   // Ensure Zod schema matches maxLength
   z.string().max(500); // Should match maxLength={500}
   ```

## Version Information

- **Component Version**: v4.2.0
- **Features**: React Hook Form integration, character counting, auto-resize, validation
- **Dependencies**: PrimeReact InputTextarea, React Hook Form, Zod validation

## Sync Metadata

- **Component Version:** v4.2.4
- **Component Source:** `packages/neuron/ui/src/lib/form/textArea/TextArea.tsx`
- **Guideline Command:** `/neuron-ui-textarea`
- **Related Skill:** `neuron-ui-form-core`

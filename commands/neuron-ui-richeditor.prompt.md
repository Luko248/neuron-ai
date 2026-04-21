---
agent: agent
---

# RichEditor Component AI Guidelines

## For the AI Assistant

Your task is to integrate and configure the Neuron RichEditor component in React applications. This guide provides essential instructions for implementing RichEditor components, which provide comprehensive WYSIWYG editing capabilities with custom formatting, variable insertion, XHTML compatibility, and HTML sanitization through React Hook Form integration across all Neuron applications.

## Overview

The RichEditor component is a rich text editor built on PrimeReact Editor with Quill.js, providing WYSIWYG editing capabilities with custom formatting controls, variable insertion, markdown support, and HTML sanitization. It integrates seamlessly with React Hook Form and includes comprehensive validation and security features.

## Core Usage

### Basic Implementation

```tsx
import { Form } from "@neuron/ui";
import { useForm } from "react-hook-form";
import { zodResolver } from "@hookform/resolvers/zod";
import { z } from "zod";

const schema = z.object({
  content: z.string().min(10, { message: "Content must be at least 10 characters" }),
  description: z.string().max(1000, { message: "Description cannot exceed 1000 characters" }),
  notes: z.string().optional(),
});

const MyForm = () => {
  const { control } = useForm({
    resolver: zodResolver(schema),
    mode: "onChange",
  });

  return (
    <>
      <Form.RichEditor
        name="content"
        labelText="Article Content"
        control={control}
        placeholder="Enter your content here..."
        required
        requiredFlag={true}
        showHeader={true}
        description="Rich text content with formatting options"
      />

      <Form.RichEditor
        name="description"
        labelText="Product Description"
        control={control}
        placeholder="Describe the product..."
        allowedFormats={{
          bold: true,
          italic: true,
          underline: false,
          strike: false,
          sub: false,
          super: false,
          variable: false,
        }}
        tooltip="Use basic formatting for product descriptions"
      />

      <Form.RichEditor
        name="notes"
        labelText="Additional Notes"
        control={control}
        placeholder="Optional notes..."
        optional
        dataType="markdown"
        description="Notes will be stored in markdown format"
      />
    </>
  );
};
```

### Markdown Support

```tsx
const MarkdownForm = () => {
  const { control } = useForm();

  return (
    <>
      {/* HTML data type (default) */}
      <Form.RichEditor
        name="htmlContent"
        labelText="HTML Content"
        control={control}
        dataType="html"
        placeholder="Content will be stored as HTML..."
        description="Rich content stored in HTML format"
      />

      {/* Markdown data type */}
      <Form.RichEditor
        name="markdownContent"
        labelText="Markdown Content"
        control={control}
        dataType="markdown"
        placeholder="Content will be converted to markdown..."
        description="Rich content converted to markdown format"
      />

      {/* Auto-detection of markdown */}
      <Form.RichEditor
        name="autoDetectContent"
        labelText="Auto-Detect Content"
        control={control}
        placeholder="Markdown patterns will be auto-detected..."
        description="Automatically detects and converts markdown patterns"
      />
    </>
  );
};
```

## Key Props Reference

### Essential Props

- **name**: Field name for form registration
- **control**: React Hook Form control object
- **labelText**: Accessible label text for the editor
- **placeholder**: Placeholder text for empty editor (takes precedence over default translation text)

### Data Management

- **dataType**: `"html"` (default) | `"markdown"` - Output format
- **value**: Initial content value
- **onTextChange**: Custom change event handler
- **onSelectionChange**: Selection change event handler
- **onLoad**: Editor load event handler

### Formatting Control

- **allowedFormats**: Object controlling which formatting options are available
  - **bold**: Enable bold formatting (default: true)
  - **italic**: Enable italic formatting (default: true)
  - **underline**: Enable underline formatting (default: true)
  - **strike**: Enable strikethrough formatting (default: true)
  - **sub**: Enable subscript formatting (default: true)
  - **super**: Enable superscript formatting (default: true)
  - **variable**: Enable variable insertion (default: true)

### Layout & Display

- **showHeader**: Show/hide the formatting toolbar (default: true)
- **className**: CSS classes for grid positioning and styling
- **literalView**: Display as read-only formatted text instead of editor

### Validation & States

- **required**: HTML5 required attribute
- **requiredFlag**: Shows required indicator (\*)
- **optional**: Shows optional indicator
- **isValid**: Manual valid state indicator

### Enhanced Features

- **tooltip**: Help text with tooltip integration
- **description**: Helper text above the editor
- **descriptionVariant**: `"default"` | `"success"` | `"warning"` | `"danger"` | `"info"`
- **testId**: Custom test identifier for testing

### Interaction & Behavior

- **disabled**: Disable editor interactions
- **readOnly**: Make editor read-only
- **deps**: Dependencies for form field updates

### Access Control

- **readonlyAccess**: Role-based read access control
- **fullAccess**: Role-based full access control

## Formatting Configuration

### Custom Format Restrictions

```tsx
const RestrictedFormattingForm = () => {
  return (
    <>
      {/* Basic formatting only */}
      <Form.RichEditor
        name="basicContent"
        labelText="Basic Formatting"
        control={control}
        allowedFormats={{
          bold: true,
          italic: true,
          underline: false,
          strike: false,
          sub: false,
          super: false,
          variable: false,
        }}
        description="Only bold and italic formatting allowed"
      />

      {/* Scientific content with subscript/superscript */}
      <Form.RichEditor
        name="scientificContent"
        labelText="Scientific Formula"
        control={control}
        allowedFormats={{
          bold: true,
          italic: true,
          underline: false,
          strike: false,
          sub: true,
          super: true,
          variable: false,
        }}
        description="Formatting for scientific formulas and equations"
      />

      {/* Template content with variables */}
      <Form.RichEditor
        name="templateContent"
        labelText="Email Template"
        control={control}
        allowedFormats={{
          bold: true,
          italic: true,
          underline: true,
          strike: false,
          sub: false,
          super: false,
          variable: true,
        }}
        description="Email template with variable placeholders"
      />

      {/* No formatting - plain text only */}
      <Form.RichEditor
        name="plainContent"
        labelText="Plain Text"
        control={control}
        allowedFormats={{
          bold: false,
          italic: false,
          underline: false,
          strike: false,
          sub: false,
          super: false,
          variable: false,
        }}
        showHeader={false}
        description="Plain text input without formatting"
      />
    </>
  );
};
```

### Variable Insertion

```tsx
const TemplateForm = () => {
  const handleVariableInsert = useCallback(() => {
    console.log("Variable inserted: {{variable}}");
  }, []);

  return (
    <Form.RichEditor
      name="emailTemplate"
      labelText="Email Template"
      control={control}
      allowedFormats={{
        bold: true,
        italic: true,
        underline: true,
        variable: true,
      }}
      placeholder="Create your email template with {{variable}} placeholders..."
      description="Use the variable button to insert {{variable}} placeholders"
    />
  );
};
```

## Validation Patterns

### Zod Schema Validation

```tsx
// Basic content validation
const basicSchema = z.object({
  content: z
    .string()
    .min(10, { message: "Content must be at least 10 characters" })
    .max(5000, { message: "Content cannot exceed 5000 characters" }),

  description: z.string().max(1000, { message: "Description too long" }).optional(),

  announcement: z
    .string()
    .min(1, { message: "Announcement is required" })
    .refine(
      (val) => {
        // Remove HTML tags for length validation
        const textOnly = val.replace(/<[^>]*>/g, "");
        return textOnly.trim().length > 0;
      },
      { message: "Announcement cannot be empty" },
    ),
});

// HTML content validation
const htmlContentSchema = z.object({
  richContent: z
    .string()
    .refine(
      (html) => {
        // Validate HTML structure
        const parser = new DOMParser();
        const doc = parser.parseFromString(html, "text/html");
        return !doc.querySelector("parsererror");
      },
      { message: "Invalid HTML content" },
    )
    .refine(
      (html) => {
        // Check for minimum text content
        const textContent = html.replace(/<[^>]*>/g, "").trim();
        return textContent.length >= 20;
      },
      { message: "Content must contain at least 20 characters of text" },
    )
    .refine(
      (html) => {
        // Validate allowed tags only
        const allowedTags = ["b", "i", "u", "s", "sub", "sup"];
        const tagRegex = /<(\/?)([\w]+)[^>]*>/g;
        let match;
        while ((match = tagRegex.exec(html)) !== null) {
          const tagName = match[2].toLowerCase();
          if (!allowedTags.includes(tagName)) {
            return false;
          }
        }
        return true;
      },
      { message: "Content contains unauthorized HTML tags" },
    ),
});

// Markdown content validation
const markdownSchema = z.object({
  markdownContent: z.string().refine(
    (markdown) => {
      // Validate markdown syntax
      const markdownPatterns = [
        /\*\*(.*?)\*\*/, // Bold
        /\*(.*?)\*/, // Italic
        /\+\+(.*?)\+\+/, // Underline
        /~~(.*?)~~/, // Strike
        /~(.*?)~/, // Subscript
        /\^(.*?)\^/, // Superscript
      ];

      // Check for unclosed markdown tags
      for (const pattern of markdownPatterns) {
        const matches = markdown.match(pattern);
        if (matches) {
          // Validate that tags are properly closed
          const openTags = (markdown.match(/\*\*|\*|\+\+|~~|~|\^/g) || []).length;
          if (openTags % 2 !== 0) {
            return false;
          }
        }
      }
      return true;
    },
    { message: "Markdown syntax is invalid - check for unclosed tags" },
  ),
});

// Variable template validation
const templateSchema = z.object({
  template: z
    .string()
    .refine(
      (content) => {
        // Validate variable syntax
        const variableRegex = /\{\{([^}]+)\}\}/g;
        const variables = content.match(variableRegex);

        if (variables) {
          // Check for valid variable names
          return variables.every((variable) => {
            const name = variable.slice(2, -2).trim();
            return /^[a-zA-Z_][a-zA-Z0-9_]*$/.test(name);
          });
        }
        return true;
      },
      { message: "Invalid variable syntax - use {{variableName}} format" },
    )
    .refine(
      (content) => {
        // Ensure at least one variable exists
        return /\{\{[^}]+\}\}/.test(content);
      },
      { message: "Template must contain at least one variable" },
    ),
});
```

### Custom Validation Functions

```tsx
// Content quality validation
const validateRichContent = (html: string) => {
  // Remove HTML tags to get text content
  const textContent = html.replace(/<[^>]*>/g, "").trim();

  if (textContent.length === 0) {
    return "Content cannot be empty";
  }

  // Check for minimum meaningful content
  if (textContent.length < 10) {
    return "Content must be at least 10 characters";
  }

  // Check for excessive formatting
  const formatTags = (html.match(/<[^>]*>/g) || []).length;
  const textLength = textContent.length;

  if (formatTags > textLength / 5) {
    return "Content has excessive formatting";
  }

  return true;
};

// Variable template validation
const validateTemplate = (content: string) => {
  const variableRegex = /\{\{([^}]+)\}\}/g;
  const variables = [];
  let match;

  while ((match = variableRegex.exec(content)) !== null) {
    variables.push(match[1].trim());
  }

  // Check for duplicate variables
  const uniqueVariables = [...new Set(variables)];
  if (variables.length !== uniqueVariables.length) {
    return "Template contains duplicate variables";
  }

  // Validate variable names
  for (const variable of variables) {
    if (!/^[a-zA-Z_][a-zA-Z0-9_]*$/.test(variable)) {
      return `Invalid variable name: ${variable}`;
    }
  }

  return true;
};

// Usage with custom validation
const FormWithCustomValidation = () => {
  const schema = z.object({
    richContent: z.string().refine(validateRichContent),

    emailTemplate: z.string().refine(validateTemplate),
  });

  const { control } = useForm({
    resolver: zodResolver(schema),
    mode: "onChange",
  });

  return (
    <>
      <Form.RichEditor
        name="richContent"
        labelText="Rich Content"
        control={control}
        description="Content with quality validation"
      />

      <Form.RichEditor
        name="emailTemplate"
        labelText="Email Template"
        control={control}
        allowedFormats={{ variable: true }}
        description="Template with variable validation"
      />
    </>
  );
};
```

## Advanced Features

### Custom Event Handling

```tsx
const AdvancedRichEditor = () => {
  const [wordCount, setWordCount] = useState(0);
  const [activeFormats, setActiveFormats] = useState<string[]>([]);

  const handleTextChange = useCallback((event: EditorTextChangeEvent) => {
    const textContent = event.textValue || "";
    const words = textContent
      .trim()
      .split(/\s+/)
      .filter((w) => w.length > 0);
    setWordCount(words.length);

    console.log("Content changed:", {
      htmlValue: event.htmlValue,
      textValue: event.textValue,
      delta: event.delta,
      source: event.source,
      wordCount: words.length,
    });

    // Auto-save functionality
    if (words.length > 10) {
      debouncedAutoSave(event.htmlValue);
    }
  }, []);

  const handleSelectionChange = useCallback((event: { range: any; oldRange: any; source: string }) => {
    console.log("Selection changed:", event);

    // Track active formats for custom UI
    if (event.range) {
      // Get formats at current selection
      // This would be handled internally by the component
    }
  }, []);

  const handleLoad = useCallback((event: any) => {
    console.log("Editor loaded:", event);

    // Custom initialization
    // Set focus, configure custom modules, etc.
  }, []);

  return (
    <Form.RichEditor
      name="advancedContent"
      labelText="Advanced Rich Editor"
      control={control}
      onTextChange={handleTextChange}
      onSelectionChange={handleSelectionChange}
      onLoad={handleLoad}
      description={`Word count: ${wordCount}`}
    />
  );
};
```

### Literal View Mode

When `literalView={true}`, the RichEditor renders a `PlainText` component instead of the editor. The `dataType` and `allowedFormats` props are passed through to `PlainText`, ensuring consistent rendering between edit and read-only modes.

```tsx
{
  /* literalView renders PlainText with the same dataType and allowedFormats */
}
<Form.RichEditor
  name="displayContent"
  labelText="Display Content"
  control={control}
  literalView={true}
  dataType="markdown"
  allowedFormats={{ bold: true, italic: true }}
  description="Content displayed in read-only format using PlainText"
/>;
```

### Data Type Conversion

```tsx
const DataTypeExamples = () => {
  return (
    <>
      {/* HTML output (default) */}
      <Form.RichEditor
        name="htmlContent"
        labelText="HTML Content"
        control={control}
        dataType="html"
        description="Content stored as HTML"
      />

      {/* Markdown output */}
      <Form.RichEditor
        name="markdownContent"
        labelText="Markdown Content"
        control={control}
        dataType="markdown"
        description="Content converted to markdown format"
      />

      {/* Auto-detection */}
      <Form.RichEditor
        name="autoContent"
        labelText="Auto-Detect Content"
        control={control}
        description="Automatically detects markdown patterns"
      />
    </>
  );
};
```

## Common Use Cases

### Content Management System

```tsx
const CMSForm = () => {
  return (
    <>
      <Form.RichEditor
        name="articleTitle"
        labelText="Article Title"
        control={control}
        allowedFormats={{
          bold: true,
          italic: true,
          underline: false,
          strike: false,
          sub: false,
          super: false,
          variable: false,
        }}
        placeholder="Enter article title..."
        description="Article title with basic formatting"
      />

      <Form.RichEditor
        name="articleContent"
        labelText="Article Content"
        control={control}
        allowedFormats={{
          bold: true,
          italic: true,
          underline: true,
          strike: true,
          sub: false,
          super: false,
          variable: false,
        }}
        placeholder="Write your article content..."
        description="Full article content with rich formatting"
      />

      <Form.RichEditor
        name="excerpt"
        labelText="Article Excerpt"
        control={control}
        allowedFormats={{
          bold: true,
          italic: true,
          underline: false,
          strike: false,
          sub: false,
          super: false,
          variable: false,
        }}
        placeholder="Brief article summary..."
        description="Short excerpt for article previews"
      />
    </>
  );
};
```

### Email Templates

```tsx
const EmailTemplateForm = () => {
  return (
    <>
      <Form.RichEditor
        name="subject"
        labelText="Email Subject"
        control={control}
        allowedFormats={{
          bold: false,
          italic: false,
          underline: false,
          strike: false,
          sub: false,
          super: false,
          variable: true,
        }}
        showHeader={true}
        placeholder="Email subject with variables..."
        description="Subject line with variable placeholders"
      />

      <Form.RichEditor
        name="emailBody"
        labelText="Email Body"
        control={control}
        allowedFormats={{
          bold: true,
          italic: true,
          underline: true,
          strike: false,
          sub: false,
          super: false,
          variable: true,
        }}
        placeholder="Email content with formatting and variables..."
        description="Rich email content with variable support"
      />

      <Form.RichEditor
        name="signature"
        labelText="Email Signature"
        control={control}
        allowedFormats={{
          bold: true,
          italic: true,
          underline: false,
          strike: false,
          sub: false,
          super: false,
          variable: true,
        }}
        placeholder="Email signature..."
        description="Signature with basic formatting"
      />
    </>
  );
};
```

### Scientific Documentation

```tsx
const ScientificForm = () => {
  return (
    <>
      <Form.RichEditor
        name="formula"
        labelText="Chemical Formula"
        control={control}
        allowedFormats={{
          bold: false,
          italic: true,
          underline: false,
          strike: false,
          sub: true,
          super: true,
          variable: false,
        }}
        placeholder="Enter chemical formula..."
        description="Chemical formulas with subscript and superscript"
      />

      <Form.RichEditor
        name="methodology"
        labelText="Methodology"
        control={control}
        allowedFormats={{
          bold: true,
          italic: true,
          underline: true,
          strike: false,
          sub: true,
          super: true,
          variable: false,
        }}
        placeholder="Describe the methodology..."
        description="Detailed methodology with scientific notation"
      />

      <Form.RichEditor
        name="results"
        labelText="Results"
        control={control}
        dataType="markdown"
        allowedFormats={{
          bold: true,
          italic: true,
          underline: false,
          strike: false,
          sub: true,
          super: true,
          variable: false,
        }}
        placeholder="Document the results..."
        description="Results in markdown format with scientific notation"
      />
    </>
  );
};
```

### Product Descriptions

```tsx
const ProductForm = () => {
  return (
    <>
      <Form.RichEditor
        name="shortDescription"
        labelText="Short Description"
        control={control}
        allowedFormats={{
          bold: true,
          italic: true,
          underline: false,
          strike: false,
          sub: false,
          super: false,
          variable: false,
        }}
        placeholder="Brief product description..."
        description="Short description for product listings"
      />

      <Form.RichEditor
        name="detailedDescription"
        labelText="Detailed Description"
        control={control}
        allowedFormats={{
          bold: true,
          italic: true,
          underline: true,
          strike: false,
          sub: false,
          super: false,
          variable: false,
        }}
        placeholder="Detailed product information..."
        description="Comprehensive product description"
      />

      <Form.RichEditor
        name="specifications"
        labelText="Technical Specifications"
        control={control}
        allowedFormats={{
          bold: true,
          italic: false,
          underline: true,
          strike: false,
          sub: true,
          super: true,
          variable: false,
        }}
        placeholder="Technical specifications..."
        description="Technical details with scientific notation"
      />
    </>
  );
};
```

## Role-Based Access Control

```tsx
const { userPermissions } = useAuth();

<Form.RichEditor
  name="sensitiveContent"
  labelText="Sensitive Information"
  control={control}
  fullAccess={userPermissions.includes("edit_content") ? "admin" : undefined}
  readonlyAccess={userPermissions.includes("view_content") ? "user" : undefined}
  allowedFormats={{
    bold: true,
    italic: true,
    underline: userPermissions.includes("advanced_formatting"),
    strike: userPermissions.includes("advanced_formatting"),
    sub: userPermissions.includes("scientific_notation"),
    super: userPermissions.includes("scientific_notation"),
    variable: userPermissions.includes("template_editing"),
  }}
  description="Access controlled based on user permissions"
/>;
```

## Testing Integration

```tsx
<Form.RichEditor
  name="testableRichEditor"
  labelText="Testable Rich Editor"
  control={control}
  testId="content-editor"
  allowedFormats={{
    bold: true,
    italic: true,
    variable: true,
  }}
/>;

// In tests
describe("RichEditor Component", () => {
  it("should handle text input and formatting", async () => {
    render(<TestForm />);

    const editor = screen.getByTestId("content-editor");

    // Test text input
    fireEvent.change(editor, { target: { value: "Test content" } });

    await waitFor(() => {
      expect(editor).toHaveValue("Test content");
    });
  });

  it("should apply formatting when buttons are clicked", async () => {
    render(<TestForm />);

    const boldButton = screen.getByRole("button", { name: /bold/i });
    const editor = screen.getByTestId("content-editor");

    // Select text and apply formatting
    fireEvent.click(boldButton);

    await waitFor(() => {
      expect(boldButton).toHaveClass("active");
    });
  });

  it("should insert variables when variable button is clicked", async () => {
    render(<TestForm allowedFormats={{ variable: true }} />);

    const variableButton = screen.getByRole("button", { name: /variable/i });

    fireEvent.click(variableButton);

    await waitFor(() => {
      const editor = screen.getByTestId("content-editor");
      expect(editor).toHaveValue(expect.stringContaining("{{variable}}"));
    });
  });

  it("should convert between HTML and markdown", async () => {
    render(<TestForm dataType="markdown" />);

    const editor = screen.getByTestId("content-editor");

    // Input HTML-like content
    fireEvent.change(editor, {
      target: { value: "<strong>Bold text</strong>" },
    });

    await waitFor(() => {
      // Should convert to markdown format
      expect(editor).toHaveValue("**Bold text**");
    });
  });

  it("should sanitize HTML content", async () => {
    render(<TestForm />);

    const editor = screen.getByTestId("content-editor");

    // Input potentially dangerous HTML
    fireEvent.change(editor, {
      target: { value: '<script>alert("xss")</script><b>Safe content</b>' },
    });

    await waitFor(() => {
      // Should remove script tags but keep allowed formatting
      expect(editor).toHaveValue("<b>Safe content</b>");
    });
  });
});
```

## Performance Considerations

```tsx
const OptimizedRichEditor = memo(({ control, allowedFormats, ...props }) => {
  // Memoize event handlers to prevent unnecessary re-renders
  const handleTextChange = useCallback((event: EditorTextChangeEvent) => {
    // Debounce auto-save for performance
    debouncedAutoSave(event.htmlValue);
  }, []);

  const handleSelectionChange = useCallback((event: any) => {
    // Handle selection changes efficiently
    console.log("Selection changed:", event);
  }, []);

  // Memoize allowed formats to prevent unnecessary header re-renders
  const memoizedFormats = useMemo(() => allowedFormats, [allowedFormats]);

  return (
    <Form.RichEditor
      control={control}
      allowedFormats={memoizedFormats}
      onTextChange={handleTextChange}
      onSelectionChange={handleSelectionChange}
      {...props}
    />
  );
});
```

## Security Considerations

### HTML Sanitization

The RichEditor automatically sanitizes HTML content to prevent XSS attacks:

```tsx
// Content is automatically sanitized based on allowedFormats
<Form.RichEditor
  name="userContent"
  labelText="User Content"
  control={control}
  allowedFormats={{
    bold: true,
    italic: true,
    underline: false,
    strike: false,
    sub: false,
    super: false,
    variable: false,
  }}
  // Only <b> and <i> tags will be allowed in the output
/>
```

### Variable Template Security

```tsx
// Validate variable templates to prevent injection
const templateSchema = z.object({
  template: z.string().refine(
    (content) => {
      // Ensure variables follow safe naming conventions
      const variableRegex = /\{\{([^}]+)\}\}/g;
      const variables = content.match(variableRegex);

      if (variables) {
        return variables.every((variable) => {
          const name = variable.slice(2, -2).trim();
          // Only allow alphanumeric and underscore
          return /^[a-zA-Z_][a-zA-Z0-9_]*$/.test(name);
        });
      }
      return true;
    },
    { message: "Invalid variable syntax" },
  ),
});
```

## Troubleshooting

### Common Issues and Solutions

1. **Formatting buttons not working**

   ```tsx
   // Ensure allowedFormats includes the desired format
   <Form.RichEditor allowedFormats={{ bold: true, italic: true }} />
   ```

2. **Content not saving in correct format**

   ```tsx
   // Specify dataType explicitly
   <Form.RichEditor dataType="markdown" /> // or "html"
   ```

3. **Variable insertion not working**

   ```tsx
   // Enable variable format
   <Form.RichEditor allowedFormats={{ variable: true }} />
   ```

4. **HTML content being stripped**

   ```tsx
   // Check allowedFormats configuration
   <Form.RichEditor
     allowedFormats={{
       bold: true, // Allows <b> tags
       italic: true, // Allows <i> tags
       // etc.
     }}
   />
   ```

5. **Markdown conversion issues**
   ```tsx
   // Ensure content follows supported markdown patterns
   // **bold**, *italic*, ++underline++, ~~strike~~, ~sub~, ^super^
   ```

## Version Information

- **Component Version**: v2.7.2
- **Features**: Rich text editing, markdown support, HTML sanitization, variable insertion, literalView with PlainText integration
- **Recent Change (v2.7.2)**: `literalView` now renders `PlainText` with `dataType` and `allowedFormats` passed through, ensuring consistent markdown/html rendering and sanitization between edit and read-only modes.
- **Dependencies**: PrimeReact Editor, Quill.js, React Hook Form, PlainText (for literalView)
- **Security**: Built-in HTML sanitization and XSS protection via shared `sanitizeHtml` mechanism

## Sync Metadata

- **Component Version:** v2.7.2
- **Component Source:** `packages/neuron/ui/src/lib/form/richEditor/RichEditor.tsx`
- **Guideline Command:** `/neuron-ui-richeditor`
- **Related Skill:** `neuron-ui-form-rich`

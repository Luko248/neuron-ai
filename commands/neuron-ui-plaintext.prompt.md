---
agent: agent
---

# PlainText Component AI Guidelines

## For the AI Assistant

Your task is to integrate and configure the Neuron PlainText component in React applications. This guide provides essential instructions for implementing PlainText components, which provide read-only display functionality for plain text, HTML, or markdown content with intelligent truncation, expand/collapse functionality, and space-efficient content presentation across all Neuron applications.

## Overview

The PlainText component is a read-only display component that renders plain text, HTML, or markdown content with intelligent truncation and expand/collapse functionality. It's designed for displaying formatted content in a space-efficient manner with automatic overflow handling and user-controlled content expansion.

## Core Usage

### Basic Implementation

```tsx
import { PlainText } from "@neuron/ui";

const MyComponent = () => {
  return (
    <>
      {/* Plain text (default) */}
      <PlainText
        value="This is a simple text content that will be displayed as plain text."
        label={{ text: "Description" }}
        rowCount={3}
        testId="description-text"
      />

      {/* HTML content */}
      <PlainText
        value="<p>This is <strong>HTML content</strong> with <em>formatting</em>.</p>"
        dataType="html"
        label={{ text: "Formatted Content" }}
        rowCount={2}
        testId="html-content"
      />

      {/* Markdown content */}
      <PlainText
        value="This is **markdown** with *emphasis* and `code`."
        dataType="markdown"
        label={{ text: "Markdown Content" }}
        rowCount={2}
        testId="markdown-content"
      />
    </>
  );
};
```

### HTML Content Display

```tsx
const HTMLContentExamples = () => {
  return (
    <>
      {/* Rich text content from editor */}
      <PlainText
        value="<p>This content came from a <strong>rich text editor</strong> and contains <em>formatting</em> like <u>underlined text</u> and <s>strikethrough</s>.</p>"
        dataType="html"
        label={{ text: "Rich Text Content" }}
        rowCount={3}
      />

      {/* Sanitized user content */}
      <PlainText
        value="<p>User submitted content with <script>alert('xss')</script> potential security issues that will be <strong>automatically sanitized</strong>.</p>"
        dataType="html"
        label={{ text: "User Content" }}
        rowCount={2}
      />
    </>
  );
};
```

### Markdown Content Display

```tsx
const MarkdownContentExamples = () => {
  return (
    <>
      {/* Markdown with formatting */}
      <PlainText
        value="This is **bold**, *italic*, and `inline code`. Supports common markdown syntax."
        dataType="markdown"
        label={{ text: "Markdown Text" }}
        rowCount={3}
      />

      {/* Markdown with restricted formats */}
      <PlainText
        value="Only **bold** and *italic* are allowed here, ~~strikethrough~~ will be stripped."
        dataType="markdown"
        allowedFormats={["bold", "italic"]}
        label={{ text: "Restricted Markdown" }}
        rowCount={2}
      />
    </>
  );
};
```

## Key Props Reference

### Essential Props

- **value**: `string` — The text, HTML, or markdown content to display (required)
- **label**: `LabelProps` — Label configuration object with text and styling options
- **testId**: `string` — Custom test identifier for testing

### Content Configuration

- **dataType**: `"text" | "html" | "markdown"` — Source format of the provided value (default: `"text"`)
  - `"text"` — Renders as plain text (no HTML interpretation)
  - `"html"` — Sanitizes and renders as HTML
  - `"markdown"` — Converts markdown to HTML, then sanitizes and renders
- **allowedFormats**: `AllowedFormats` — Controls which formatting tags are preserved during sanitization. Uses the same format contract as `RichEditor`.
- **rowCount**: `number` — Number of rows to display when collapsed (default: 3)

### Display Behavior

- Automatically detects content overflow and shows expand/collapse toggle
- Sanitizes HTML and markdown content using the same sanitization mechanism as `RichEditor` (`sanitizeHtml`)
- Markdown conversion uses `markdownToHtml` from `RichEditor`
- Responsive design with automatic resize detection via `ResizeObserver`
- Fallback display of `"-"` for empty/null values

## Content Types and Use Cases

### Plain Text Display

```tsx
const PlainTextExamples = () => {
  return (
    <>
      {/* Simple text content */}
      <PlainText
        value="This is plain text content without any formatting."
        label={{ text: "Plain Text Description" }}
        rowCount={2}
      />

      {/* Multi-line text content */}
      <PlainText
        value={`Line 1: First line of content
Line 2: Second line of content
Line 3: Third line of content`}
        label={{ text: "Multi-line Content" }}
        rowCount={3}
      />
    </>
  );
};
```

### HTML Content Display

```tsx
const HTMLContentExamples = () => {
  return (
    <>
      {/* Rich editor output */}
      <PlainText
        value="<p>Content from rich text editor with <strong>bold text</strong>, <em>italic text</em>, and <u>underlined text</u>.</p>"
        dataType="html"
        label={{ text: "Rich Editor Output" }}
        rowCount={3}
      />

      {/* Potentially unsafe content (automatically sanitized) */}
      <PlainText
        value="<p>Safe content with <strong>formatting</strong></p><script>alert('This will be removed')</script>"
        dataType="html"
        label={{ text: "Sanitized Content" }}
        rowCount={3}
      />
    </>
  );
};
```

## Row Count Configuration

### Different Row Count Examples

```tsx
const RowCountExamples = () => {
  const longContent =
    "Lorem ipsum dolor sit amet, consectetur adipiscing elit. Sed do eiusmod tempor incididunt ut labore et dolore magna aliqua.";

  return (
    <>
      {/* Single row display */}
      <PlainText value={longContent} label={{ text: "Single Row (1)" }} rowCount={1} />

      {/* Default three row display */}
      <PlainText value={longContent} label={{ text: "Three Rows (3) - Default" }} rowCount={3} />

      {/* Five row display */}
      <PlainText value={longContent} label={{ text: "Five Rows (5)" }} rowCount={5} />
    </>
  );
};
```

## Label Configuration

### Label Styling and Options

```tsx
const LabelExamples = () => {
  return (
    <>
      {/* Basic label */}
      <PlainText value="Content with basic label" label={{ text: "Basic Label" }} rowCount={2} />

      {/* Label with tooltip */}
      <PlainText
        value="Content with tooltip label"
        label={{
          text: "Label with Tooltip",
          tooltip: "This label has additional information in a tooltip",
        }}
        rowCount={2}
      />

      {/* No label */}
      <PlainText value="Content without any label" rowCount={2} />
    </>
  );
};
```

## Integration with RichEditor

The `PlainText` component shares sanitization infrastructure with `RichEditor`:

- When used as `literalView` in `RichEditor`, the `dataType` and `allowedFormats` from `RichEditor` are passed through to `PlainText`, ensuring consistent rendering between edit and read-only modes.
- The `allowedFormats` prop uses the same `AllowedFormats` type as `RichEditor`.
- HTML sanitization uses the same `sanitizeHtml` function.
- Markdown conversion uses the same `markdownToHtml` function.

```tsx
{
  /* PlainText as literal view inherits RichEditor's settings */
}
<RichEditor
  dataType="markdown"
  allowedFormats={["bold", "italic", "underline"]}
  literalView={true}
  value="**Bold** and *italic* content"
/>;
```

## Security Features

### HTML Sanitization

The PlainText component automatically sanitizes HTML and markdown content using the same sanitization mechanism as `RichEditor`:

```tsx
const SecurityExamples = () => {
  return (
    <>
      {/* Potentially dangerous content - automatically sanitized */}
      <PlainText
        value="<p>Safe content</p><script>alert('This script will be removed')</script>"
        dataType="html"
        label={{ text: "Sanitized Content" }}
        rowCount={2}
      />

      {/* Content with event handlers - automatically cleaned */}
      <PlainText
        value="<p onclick='maliciousFunction()'>This paragraph had an onclick handler that was removed</p>"
        dataType="html"
        label={{ text: "Cleaned Event Handlers" }}
        rowCount={1}
      />
    </>
  );
};
```

## Testing Integration

```tsx
<PlainText value="Content for testing" label={{ text: "Test Label" }} rowCount={3} testId="test-plaintext" />;

describe("PlainText Component", () => {
  it("should display plain text content", () => {
    render(<PlainText value="Test content" label={{ text: "Test Label" }} testId="test-content" />);

    expect(screen.getByTestId("test-content")).toHaveTextContent("Test content");
    expect(screen.getByText("Test Label")).toBeInTheDocument();
  });

  it("should sanitize HTML content", () => {
    render(
      <PlainText value="<p>Safe content</p><script>alert('xss')</script>" dataType="html" testId="html-content" />,
    );

    const element = screen.getByTestId("html-content");
    expect(element.innerHTML).toContain("<p>Safe content</p>");
    expect(element.innerHTML).not.toContain("<script>");
  });

  it("should render markdown content", () => {
    render(<PlainText value="**Bold text**" dataType="markdown" testId="md-content" />);

    const element = screen.getByTestId("md-content");
    expect(element.innerHTML).toContain("<strong>Bold text</strong>");
  });

  it("should handle empty content gracefully", () => {
    render(<PlainText value="" label={{ text: "Empty Content" }} testId="empty-content" />);

    expect(screen.getByTestId("empty-content")).toHaveTextContent("-");
  });
});
```

## Common Mistakes to Avoid

### ❌ Don't Use the Removed `isHtml` Prop

```tsx
{
  /* Wrong: isHtml was removed in v2.0.0 */
}
<PlainText value="<p>HTML content</p>" isHtml={true} />;

{
  /* Right: Use dataType */
}
<PlainText value="<p>HTML content</p>" dataType="html" />;
```

### ❌ Don't Forget dataType for Non-Text Content

```tsx
{/* Wrong: HTML tags will be rendered as literal text */}
<PlainText value="<p>This won't render as HTML</p>" />

{/* Right: Specify the data type */}
<PlainText value="<p>This renders as HTML</p>" dataType="html" />
<PlainText value="**This renders as markdown**" dataType="markdown" />
```

### ❌ Don't Forget State Management

```tsx
{
  /* Wrong: Hardcoded text */
}
<PlainText title="Delete Item" message="Are you sure?" />;

{
  /* Right: Localized text */
}
<PlainText value={t("content.description")} label={{ text: t("labels.description") }} />;
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

1. **Use `dataType` instead of `isHtml`** — The `isHtml` prop was removed in v2.0.0; use `dataType="html"` or `dataType="markdown"` instead.
2. **Markdown support** — Native markdown rendering via `dataType="markdown"`, converted to HTML and sanitized automatically.
3. **`allowedFormats` for sanitization control** — Same format contract as `RichEditor` to restrict which formatting tags are preserved.
4. **Shared sanitization with RichEditor** — Both components use the same `sanitizeHtml` and `markdownToHtml` functions for consistent behavior.
5. **Automatic truncation** — Content overflow is detected via `ResizeObserver` and toggle is shown automatically.
6. **Security built-in** — All HTML and markdown content is sanitized before rendering.

## Version Information

- **Component Version**: v2.0.1
- **Breaking Change**: `isHtml` prop removed; use `dataType` (`"text"` | `"html"` | `"markdown"`) instead.
- **New Features**: Markdown support via `dataType="markdown"`, `allowedFormats` prop for sanitization control.
- **Dependencies**: `sanitizeHtml` and `markdownToHtml` from `RichEditor`, React i18n
- **Security**: Built-in XSS protection through shared sanitization mechanism with `RichEditor`.

## Sync Metadata

- **Component Version:** v2.0.1
- **Component Source:** `packages/neuron/ui/src/lib/form/plainText/PlainText.tsx`
- **Guideline Command:** `/neuron-ui-plaintext`
- **Related Skill:** `neuron-ui-form-rich`

---
agent: agent
---

# AI-Assisted Neuron Link Component Integration Guide

## For the AI Assistant

Your task is to integrate and configure the Neuron Link components in a React application. This guide provides comprehensive instructions for implementing Link components, which serve as the foundation for navigation and user interactions across all Neuron applications.

## Sync Metadata

- **Component Version:** v3.1.2
- **Component Source:** `packages/neuron/ui/src/lib/misc/link/Link.tsx`
- **Guideline Command:** `/neuron-ui-link`
- **Related Skill:** `neuron-ui-misc`

## Introduction

The Link system is a core part of the Neuron UI framework, designed to create consistent, accessible, and flexible navigation patterns across all Neuron applications.

### What is the Link Component?

The Link component provides standardized navigation rendering for your application with support for:

- **React Router DOM integration** - Seamless navigation within single-page applications
- **External link support** - Standard anchor elements for external URLs
- **Click handler functionality** - Button-like behavior for interactive elements
- **Icon integration** - Left and right icon support using baseIcons
- **Multiple variants** - Primary and secondary styling options
- **Size variations** - Small and large size options
- **Accessibility features** - Proper ARIA support and semantic HTML

### Key Features

- **Flexible Navigation**: Support for React Router, external links, and click handlers
- **Icon Integration**: Left and right icon support with baseIcons
- **Multiple Variants**: Primary and secondary styling with underline options
- **Size Options**: Small and large sizes for different UI contexts
- **Accessibility**: Built-in ARIA attributes and semantic HTML
- **TypeScript Support**: Full type safety with comprehensive props typing
- **Disabled States**: Proper disabled state handling across all link types

**Storybook Reference:** You can refer to the Storybook documentation for visual examples and interactive demos of all available link configurations.

## Step 1: Basic Link Implementation

### 1.1 Import the Link Component

```tsx
import { Link, baseIcons } from "@neuron/ui";
```

### 1.2 Basic Link Usage

Here are the fundamental ways to use the Link component:

```tsx
import { Link } from "@neuron/ui";

const BasicLinks = () => {
  return (
    <div className="link-examples">
      {/* React Router navigation */}
      <Link to="/dashboard">Go to Dashboard</Link>

      {/* External link */}
      <Link href="https://example.com" target="_blank">
        External Link
      </Link>

      {/* Click handler (button behavior) */}
      <Link onClick={(event) => console.info("Link clicked", event)}>Click Handler</Link>
    </div>
  );
};
```

### 1.3 Link Variants

Use different variants based on visual hierarchy and context:

```tsx
import { Link } from "@neuron/ui";

const LinkVariants = () => {
  return (
    <div className="link-variants">
      {/* Primary variant - default, prominent styling */}
      <Link to="/primary" variant="primary">
        Primary Link
      </Link>

      {/* Secondary variant - subtle styling */}
      <Link to="/secondary" variant="secondary">
        Secondary Link
      </Link>
    </div>
  );
};
```

**When to use each variant:**

- **Primary**: Main navigation links, call-to-action links, important navigation
- **Secondary**: Supporting links, less prominent navigation, breadcrumbs

## Step 2: Link Sizes and Visual Options

### 2.1 Link Sizes

Use different sizes based on the UI context:

```tsx
import { Link } from "@neuron/ui";

const LinkSizes = () => {
  return (
    <div className="link-sizes">
      {/* Large size - default, prominent */}
      <Link to="/dashboard" size="large">
        Large Link
      </Link>

      {/* Small size - compact, inline */}
      <Link to="/settings" size="small">
        Small Link
      </Link>
    </div>
  );
};
```

### 2.2 Underline Options

Control underline appearance for different design needs:

```tsx
import { Link } from "@neuron/ui";

const UnderlineOptions = () => {
  return (
    <div className="underline-options">
      {/* Without underline - default */}
      <Link to="/no-underline">No Underline</Link>

      {/* With underline - traditional link appearance */}
      <Link to="/with-underline" underline={true}>
        With Underline
      </Link>
    </div>
  );
};
```

## Step 3: Icon Integration

### 3.1 Links with Icons

Enhance links with icons for better visual communication:

```tsx
import { Link, baseIcons } from "@neuron/ui";

const IconLinks = () => {
  return (
    <div className="icon-links">
      {/* Left icon */}
      <Link to="/dashboard" iconLeft={baseIcons.houseChimneySolid}>
        Dashboard
      </Link>

      {/* Right icon */}
      <Link href="https://example.com" iconRight={baseIcons.arrowUpRightFromSquareSolid} target="_blank">
        External Link
      </Link>

      {/* Both icons */}
      <Link to="/settings" iconLeft={baseIcons.settingSolid} iconRight={baseIcons.chevronRightSolid}>
        Settings
      </Link>
    </div>
  );
};
```

### 3.2 Icon Guidelines for Links

Use appropriate icons based on link purpose:

```tsx
import { Link, baseIcons } from "@neuron/ui";

const SemanticIconLinks = () => {
  return (
    <div className="semantic-icon-links">
      {/* Navigation icons */}
      <Link to="/home" iconLeft={baseIcons.houseChimneySolid}>
        Home
      </Link>

      {/* External link indicators */}
      <Link href="https://docs.example.com" iconRight={baseIcons.arrowUpRightFromSquareSolid}>
        Documentation
      </Link>

      {/* Download links */}
      <Link href="/files/document.pdf" iconLeft={baseIcons.downloadSolid}>
        Download PDF
      </Link>

      {/* Action links */}
      <Link onClick={handleEdit} iconLeft={baseIcons.penSolid}>
        Edit Item
      </Link>
    </div>
  );
};
```

## Step 4: Navigation Types and Usage

### 4.1 React Router Navigation

Use for internal application navigation:

```tsx
import { Link } from "@neuron/ui";

const InternalNavigation = () => {
  return (
    <nav className="internal-nav">
      {/* Simple route navigation */}
      <Link to="/dashboard">Dashboard</Link>
      <Link to="/profile">Profile</Link>
      <Link to="/settings">Settings</Link>

      {/* Relative navigation */}
      <Link to="../parent">Back to Parent</Link>
      <Link to="./child">Go to Child</Link>

      {/* Hash navigation */}
      <Link to="/page#section">Go to Section</Link>
    </nav>
  );
};
```

### 4.2 External Links

Use for links to external websites:

```tsx
import { Link, baseIcons } from "@neuron/ui";

const ExternalLinks = () => {
  return (
    <div className="external-links">
      {/* External website */}
      <Link href="https://www.example.com" target="_blank" iconRight={baseIcons.arrowUpRightFromSquareSolid}>
        Visit Website
      </Link>

      {/* Email link */}
      <Link href="mailto:contact@example.com" iconLeft={baseIcons.envelopeSolid}>
        Send Email
      </Link>

      {/* Phone link */}
      <Link href="tel:+1234567890" iconLeft={baseIcons.phoneSolid}>
        Call Us
      </Link>

      {/* File download */}
      <Link href="/assets/document.pdf" iconLeft={baseIcons.filePdfRegular}>
        Download PDF
      </Link>
    </div>
  );
};
```

### 4.3 Click Handler Links

Use for interactive elements that don't navigate:

```tsx
import { Link, baseIcons } from "@neuron/ui";

const ClickHandlerLinks = () => {
  const handleAction = (event: React.MouseEvent) => {
    console.info("Action triggered", event);
    // Perform action logic here
  };

  const handleToggle = () => {
    console.info("Toggle state");
    // Toggle functionality
  };

  return (
    <div className="click-handler-links">
      {/* Action trigger */}
      <Link onClick={handleAction} iconLeft={baseIcons.boltRegular}>
        Trigger Action
      </Link>

      {/* Toggle functionality */}
      <Link onClick={handleToggle} iconLeft={baseIcons.eyeRegular}>
        Toggle View
      </Link>

      {/* Modal opener */}
      <Link onClick={() => console.info("Open modal")} iconLeft={baseIcons.circlePlusSolid}>
        Add New Item
      </Link>
    </div>
  );
};
```

## Step 5: Disabled States and Accessibility

### 5.1 Disabled Links

Properly handle disabled states for different link types:

```tsx
import { Link, baseIcons } from "@neuron/ui";

const DisabledLinks = () => {
  return (
    <div className="disabled-links">
      {/* Disabled navigation */}
      <Link to="/restricted" disabled={true}>
        Restricted Page
      </Link>

      {/* Disabled external link */}
      <Link href="https://unavailable.com" disabled={true} iconRight={baseIcons.lockSolid}>
        Unavailable Link
      </Link>

      {/* Disabled action */}
      <Link onClick={handleAction} disabled={true} iconLeft={baseIcons.handStopRegular}>
        Disabled Action
      </Link>
    </div>
  );
};
```

### 5.2 Accessibility Considerations

Ensure proper accessibility for all link types:

```tsx
import { Link, baseIcons } from "@neuron/ui";

const AccessibleLinks = () => {
  return (
    <div className="accessible-links">
      {/* Descriptive text for screen readers */}
      <Link
        href="https://example.com"
        target="_blank"
        aria-label="Visit Example.com (opens in new tab)"
        iconRight={baseIcons.arrowUpRightFromSquareSolid}
      >
        Visit Site
      </Link>

      {/* Clear action description */}
      <Link onClick={handleDelete} aria-label="Delete this item" iconLeft={baseIcons.trashCanSolid}>
        Delete
      </Link>

      {/* File download with type indication */}
      <Link href="/document.pdf" aria-label="Download PDF document" iconLeft={baseIcons.filePdfRegular}>
        Download (PDF)
      </Link>
    </div>
  );
};
```

## Step 6: Link Props Reference

### 6.1 Core Link Props

| Prop        | Type                       | Default     | Description                       |
| ----------- | -------------------------- | ----------- | --------------------------------- |
| `to`        | `string`                   | -           | React Router navigation path      |
| `href`      | `string`                   | -           | External URL or anchor href       |
| `onClick`   | `function`                 | -           | Click handler for button behavior |
| `children`  | `ReactNode`                | -           | Link content text or elements     |
| `variant`   | `"primary" \| "secondary"` | `"primary"` | Visual variant of the link        |
| `size`      | `"small" \| "large"`       | `"large"`   | Size of the link                  |
| `disabled`  | `boolean`                  | `false`     | Disabled state                    |
| `underline` | `boolean`                  | `false`     | Show underline decoration         |
| `target`    | `string`                   | -           | Link target (e.g., "\_blank")     |
| `className` | `string`                   | -           | Additional CSS classes            |

### 6.2 Icon Props

| Prop        | Type                           | Default | Description                 |
| ----------- | ------------------------------ | ------- | --------------------------- |
| `iconLeft`  | `TBaseIcons \| IconDefinition` | -       | Icon displayed on the left  |
| `iconRight` | `TBaseIcons \| IconDefinition` | -       | Icon displayed on the right |

### 6.3 Navigation Type Requirements

| Navigation Type | Required Props | Optional Props | Use Case                 |
| --------------- | -------------- | -------------- | ------------------------ |
| React Router    | `to`           | `target`       | Internal app navigation  |
| External Link   | `href`         | `target`       | External websites, files |
| Click Handler   | `onClick`      | -              | Interactive actions      |

**Important**: Only one of `to`, `href`, or `onClick` should be provided at a time.

## Step 7: Best Practices

### 7.1 Link Type Selection

**Choose appropriate link types based on functionality:**

```tsx
{
  /* ✅ CORRECT: React Router for internal navigation */
}
<Link to="/dashboard">Dashboard</Link>;

{
  /* ✅ CORRECT: href for external links */
}
<Link href="https://example.com" target="_blank">
  External Site
</Link>;

{
  /* ✅ CORRECT: onClick for actions */
}
<Link onClick={handleAction}>Perform Action</Link>;

{
  /* ❌ WRONG: Multiple navigation props */
}
<Link to="/page" href="https://example.com">
  Conflicting Props
</Link>;
```

### 7.2 Icon Usage Guidelines

**Use appropriate icons for different link purposes:**

```tsx
{/* ✅ CORRECT: Semantic icon usage */}
<Link to="/home" iconLeft={baseIcons.houseChimneySolid}>Home</Link>
<Link href="mailto:contact@example.com" iconLeft={baseIcons.envelopeSolid}>Email</Link>

{/* ✅ CORRECT: External link indicators */}
<Link href="https://example.com" iconRight={baseIcons.arrowUpRightFromSquareSolid}>
  External Link
</Link>

{/* ❌ WRONG: Misleading icons */}
<Link to="/settings" iconLeft={baseIcons.trashCanSolid}>Settings</Link>
```

### 7.3 Accessibility Guidelines

**Ensure proper accessibility for all users:**

```tsx
{
  /* ✅ CORRECT: Descriptive link text */
}
<Link to="/dashboard">Go to Dashboard</Link>;

{
  /* ✅ CORRECT: External link indication */
}
<Link href="https://example.com" target="_blank" aria-label="Visit Example.com (opens in new tab)">
  Example Site
</Link>;

{
  /* ❌ WRONG: Vague link text */
}
<Link to="/page">Click here</Link>;

{
  /* ❌ WRONG: Missing external link indication */
}
<Link href="https://example.com">Link</Link>;
```

### 7.4 Variant and Size Guidelines

**Use appropriate variants and sizes for context:**

```tsx
{
  /* ✅ CORRECT: Primary for main navigation */
}
<Link to="/dashboard" variant="primary">
  Dashboard
</Link>;

{
  /* ✅ CORRECT: Secondary for supporting links */
}
<Link to="/help" variant="secondary" size="small">
  Help
</Link>;

{
  /* ✅ CORRECT: Small size for inline links */
}
<p>
  Read more about this in our
  <Link to="/docs" size="small" underline={true}>
    documentation
  </Link>
  .
</p>;
```

## Step 8: Common Use Cases and Patterns

### 8.1 Navigation Menus

Links in navigation structures:

```tsx
import { Link, baseIcons } from "@neuron/ui";

const NavigationMenu = () => {
  return (
    <nav className="main-navigation">
      <Link to="/dashboard" iconLeft={baseIcons.houseChimneySolid} variant="primary">
        Dashboard
      </Link>
      <Link to="/projects" iconLeft={baseIcons.briefcaseSolid} variant="primary">
        Projects
      </Link>
      <Link to="/settings" iconLeft={baseIcons.settingSolid} variant="primary">
        Settings
      </Link>
    </nav>
  );
};
```

### 8.2 Content Links

Links within content areas:

```tsx
import { Link, baseIcons } from "@neuron/ui";

const ContentLinks = () => {
  return (
    <div className="content-area">
      <p>
        For more information, visit our{" "}
        <Link
          href="https://docs.example.com"
          target="_blank"
          size="small"
          underline={true}
          iconRight={baseIcons.arrowUpRightFromSquareSolid}
        >
          documentation
        </Link>
        .
      </p>

      <p>
        You can also{" "}
        <Link href="mailto:support@example.com" size="small" underline={true} iconLeft={baseIcons.envelopeSolid}>
          contact our support team
        </Link>{" "}
        for assistance.
      </p>
    </div>
  );
};
```

### 8.3 Action Links

Links that trigger actions:

```tsx
import { Link, baseIcons } from "@neuron/ui";

const ActionLinks = () => {
  const handleExport = () => {
    console.info("Exporting data...");
  };

  const handleRefresh = () => {
    console.info("Refreshing data...");
  };

  return (
    <div className="action-links">
      <Link onClick={handleExport} iconLeft={baseIcons.downloadSolid} variant="secondary">
        Export Data
      </Link>

      <Link onClick={handleRefresh} iconLeft={baseIcons.arrowsRotateSolid} variant="secondary">
        Refresh
      </Link>
    </div>
  );
};
```

## Step 9: Common Mistakes to Avoid

### 9.1 Don't Use Multiple Navigation Props

**Never provide multiple navigation methods in a single Link:**

```tsx
{/* ❌ WRONG: Multiple navigation props */}
<Link to="/page" href="https://example.com" onClick={handleClick}>
  Conflicting Link
</Link>

{/* ✅ CORRECT: Single navigation method */}
<Link to="/page">Internal Link</Link>
<Link href="https://example.com">External Link</Link>
<Link onClick={handleClick}>Action Link</Link>
```

### 9.2 Don't Use FontAwesome Icons Directly

**Always use baseIcons instead of direct FontAwesome imports:**

```tsx
{
  /* ❌ WRONG: Direct FontAwesome usage */
}
import { faHome } from "@fortawesome/free-solid-svg-icons";
<Link to="/home" iconLeft={faHome}>
  Home
</Link>;

{
  /* ✅ CORRECT: Use baseIcons */
}
import { baseIcons } from "@neuron/ui";
<Link to="/home" iconLeft={baseIcons.houseChimneySolid}>
  Home
</Link>;
```

### 9.3 Don't Ignore Accessibility

**Always provide proper accessibility attributes:**

```tsx
{/* ❌ WRONG: Missing accessibility information */}
<Link href="https://example.com">Link</Link>
<Link onClick={handleAction}>Action</Link>

{/* ✅ CORRECT: Proper accessibility */}
<Link
  href="https://example.com"
  target="_blank"
  aria-label="Visit Example.com (opens in new tab)"
>
  External Link
</Link>

<Link
  onClick={handleAction}
  aria-label="Perform specific action"
>
  Action Link
</Link>
```

### 9.4 Don't Use Inappropriate Variants

**Choose variants that match the link's importance:**

```tsx
{/* ❌ WRONG: Primary for supporting links */}
<Link to="/privacy-policy" variant="primary">Privacy Policy</Link>

{/* ❌ WRONG: Secondary for main navigation */}
<Link to="/dashboard" variant="secondary">Dashboard</Link>

{/* ✅ CORRECT: Appropriate variants */}
<Link to="/dashboard" variant="primary">Dashboard</Link>
<Link to="/privacy-policy" variant="secondary" size="small">Privacy Policy</Link>
```

### 9.5 Don't Use Misleading Icons

**Use icons that clearly represent the link's purpose:**

```tsx
{/* ❌ WRONG: Misleading icons */}
<Link to="/settings" iconLeft={baseIcons.trashCanSolid}>Settings</Link>
<Link href="mailto:contact@example.com" iconLeft={baseIcons.phoneSolid}>Email</Link>

{/* ✅ CORRECT: Appropriate icons */}
<Link to="/settings" iconLeft={baseIcons.settingSolid}>Settings</Link>
<Link href="mailto:contact@example.com" iconLeft={baseIcons.envelopeSolid}>Email</Link>
```

## Key Takeaways

The Neuron Link component provides a comprehensive, accessible foundation for navigation and user interactions. Key points to remember:

1. **Choose the right navigation type**: Use `to` for internal routes, `href` for external links, `onClick` for actions
2. **Never combine navigation props**: Only use one of `to`, `href`, or `onClick` per Link
3. **Always use baseIcons**: Never import FontAwesome icons directly
4. **Use appropriate variants**: Primary for main navigation, secondary for supporting links
5. **Consider accessibility**: Provide descriptive text and ARIA labels
6. **Use semantic icons**: Choose icons that clearly represent the link's purpose
7. **Handle disabled states**: Properly implement disabled functionality
8. **Indicate external links**: Use target="\_blank" and appropriate icons for external navigation
9. **Size appropriately**: Use large for navigation, small for inline content links
10. **Maintain consistency**: Follow established patterns across your application

By following these guidelines, you'll create consistent, accessible, and user-friendly navigation experiences across your Neuron applications.

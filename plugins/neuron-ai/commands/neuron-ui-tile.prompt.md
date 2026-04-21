---
agent: agent
---

# AI-Assisted Neuron Tile Component Integration Guide

## For the AI Assistant

Your task is to integrate and configure the Neuron Tile components in a React application. This guide provides comprehensive instructions for implementing the Tile component, which serves as a flexible clickable element that can function as a link, button, or static container across all Neuron applications.

## Sync Metadata

- **Component Version:** v3.4.1
- **Component Source:** `packages/neuron/ui/src/lib/panels/tile/Tile.tsx`
- **Guideline Command:** `/neuron-ui-tile`
- **Related Skill:** `neuron-ui-panels`

## Introduction

The Tile system is a versatile component of the Neuron UI framework, designed to create consistent, accessible, and interactive tile elements that can adapt to different interaction patterns across all Neuron applications.

### What is the Tile System?

The Tile component provides a standardized clickable tile interface for your application with support for:

- Multiple interaction modes (link, button, static)
- Automatic element rendering based on props (anchor, button, div)
- Multiple variants (default, outline)
- Size variations (base, small)
- Active state management
- Disabled state handling
- Flexible width control
- React Router integration
- External link support
- Custom click handling

### Key Features

- **Flexible Interaction**: Automatically renders as anchor, button, or div based on props
- **React Router Integration**: Built-in support for React Router navigation with `to` prop
- **External Links**: Support for external URLs with `href` prop
- **Custom Actions**: Support for custom click handlers with `onClick` prop
- **Active State**: Visual indication of selected/active tiles
- **Responsive Design**: Configurable max-width and size variations
- **Accessibility**: Proper semantic HTML and ARIA attributes
- **TypeScript Support**: Full type safety with comprehensive prop interfaces
- **Disabled State**: Proper disabled state handling with visual feedback

**Storybook Reference:** You can refer to the Storybook documentation for visual examples and interactive demos of the Tile component.

## Step 1: Basic Tile Implementation

### 1.1 Import the Tile Component

```tsx
import { Tile } from "@neuron/ui";
```

### 1.2 Understanding Tile Rendering Modes

**⚠️ CRITICAL: Tile Rendering Logic**

The Tile component automatically renders different HTML elements based on the props provided:

- **`to` prop** → Renders `<a>` element (React Router Link)
- **`href` prop** → Renders `<a>` element (HTML anchor)
- **`onClick` prop** → Renders `<button>` element
- **No interaction props** → Renders `<div>` element (static)

```tsx
import { Tile } from "@neuron/ui";

const TileRenderingModes = () => {
  return (
    <div className="tile-examples">
      {/* Renders as <a> element - for React Router navigation */}
      <Tile to="/dashboard">Navigate to Dashboard</Tile>

      {/* Renders as <a> element - for external links */}
      <Tile href="https://example.com" target="_blank">
        External Link
      </Tile>

      {/* Renders as <button> element - for custom actions */}
      <Tile onClick={() => console.info("Tile clicked")}>Custom Action</Tile>

      {/* Renders as <div> element - for static content */}
      <Tile>Static Content Tile</Tile>
    </div>
  );
};
```

### 1.3 Basic Usage Examples

Here are simple implementations of each tile mode:

```tsx
import { Tile } from "@neuron/ui";

const BasicTileExamples = () => {
  return (
    <div className="basic-tiles">
      {/* Link tile for navigation */}
      <Tile to="/profile">View Profile</Tile>

      {/* Action tile for interactions */}
      <Tile onClick={() => alert("Action performed!")}>Perform Action</Tile>

      {/* Static tile for display */}
      <Tile>Information Display</Tile>
    </div>
  );
};
```

## Step 2: Tile Variants and Styling

### 2.1 Tile Variants

The Tile component supports different visual variants:

```tsx
import { Tile } from "@neuron/ui";

const TileVariants = () => {
  return (
    <div className="tile-variants">
      {/* Default variant - filled appearance */}
      <Tile to="/default">Default Tile</Tile>

      {/* Outline variant - bordered appearance */}
      <Tile variant="outline" to="/outline">
        Outline Tile
      </Tile>
    </div>
  );
};
```

### 2.2 Tile Sizes

Control tile padding with size variations:

```tsx
import { Tile } from "@neuron/ui";

const TileSizes = () => {
  return (
    <div className="tile-sizes">
      {/* Base size - standard padding */}
      <Tile size="base" to="/base">
        Base Size Tile
      </Tile>

      {/* Small size - reduced padding */}
      <Tile size="small" to="/small">
        Small Size Tile
      </Tile>
    </div>
  );
};
```

### 2.3 Custom Width Control

Control tile width with the maxWidth property:

```tsx
import { Tile } from "@neuron/ui";

const TileWidths = () => {
  return (
    <div className="tile-widths">
      {/* Default max width (380px) */}
      <Tile to="/default-width">Default Width Tile</Tile>

      {/* Custom max width */}
      <Tile maxWidth="200px" to="/narrow">
        Narrow Tile
      </Tile>

      {/* Full width */}
      <Tile maxWidth="100%" to="/full-width">
        Full Width Tile
      </Tile>
    </div>
  );
};
```

## Step 3: Tile States

### 3.1 Active State

Use the active state to indicate selected or current tiles:

```tsx
import { Tile } from "@neuron/ui";
import { useState } from "react";

const ActiveTileExample = () => {
  const [activeTile, setActiveTile] = useState("tile1");

  const tiles = [
    { id: "tile1", label: "Dashboard", path: "/dashboard" },
    { id: "tile2", label: "Profile", path: "/profile" },
    { id: "tile3", label: "Settings", path: "/settings" },
  ];

  return (
    <div className="active-tiles">
      {tiles.map((tile) => (
        <Tile key={tile.id} to={tile.path} active={activeTile === tile.id} onClick={() => setActiveTile(tile.id)}>
          {tile.label}
        </Tile>
      ))}
    </div>
  );
};
```

### 3.2 Disabled State

Disable tiles to prevent interaction:

```tsx
import { Tile } from "@neuron/ui";

const DisabledTileExample = () => {
  return (
    <div className="disabled-tiles">
      {/* Disabled link tile */}
      <Tile to="/disabled" disabled>
        Disabled Link Tile
      </Tile>

      {/* Disabled action tile */}
      <Tile onClick={() => {}} disabled>
        Disabled Action Tile
      </Tile>

      {/* Disabled static tile */}
      <Tile disabled>Disabled Static Tile</Tile>
    </div>
  );
};
```

### 3.3 Combined States

Combine different states and variants:

```tsx
import { Tile } from "@neuron/ui";

const CombinedStatesExample = () => {
  return (
    <div className="combined-states">
      {/* Active outline tile */}
      <Tile variant="outline" active to="/active-outline">
        Active Outline
      </Tile>

      {/* Disabled outline tile */}
      <Tile variant="outline" disabled to="/disabled-outline">
        Disabled Outline
      </Tile>

      {/* Small active tile */}
      <Tile size="small" active onClick={() => {}}>
        Small Active
      </Tile>
    </div>
  );
};
```

## Step 4: Navigation and Links

### 4.1 React Router Navigation

Use the `to` prop for internal navigation with React Router:

```tsx
import { Tile } from "@neuron/ui";

const NavigationTiles = () => {
  return (
    <div className="navigation-tiles">
      {/* Simple navigation */}
      <Tile to="/dashboard">Go to Dashboard</Tile>

      {/* Navigation with parameters */}
      <Tile to="/user/123">View User Profile</Tile>

      {/* Navigation with query parameters */}
      <Tile to="/search?q=react">Search Results</Tile>

      {/* Navigation with hash */}
      <Tile to="/docs#installation">Installation Guide</Tile>
    </div>
  );
};
```

### 4.2 External Links

Use the `href` prop for external links:

```tsx
import { Tile } from "@neuron/ui";

const ExternalLinkTiles = () => {
  return (
    <div className="external-link-tiles">
      {/* External link in same tab */}
      <Tile href="https://example.com">Visit Example.com</Tile>

      {/* External link in new tab */}
      <Tile href="https://github.com" target="_blank">
        Open GitHub
      </Tile>

      {/* Email link */}
      <Tile href="mailto:contact@example.com">Send Email</Tile>

      {/* Phone link */}
      <Tile href="tel:+1234567890">Call Us</Tile>
    </div>
  );
};
```

### 4.3 Conditional Navigation

Implement conditional navigation based on application state:

```tsx
import { Tile } from "@neuron/ui";

const ConditionalNavigationTiles = ({ user, isLoggedIn }) => {
  return (
    <div className="conditional-navigation">
      {/* Navigation based on authentication */}
      <Tile to={isLoggedIn ? "/dashboard" : "/login"}>{isLoggedIn ? "Go to Dashboard" : "Please Login"}</Tile>

      {/* Navigation based on user role */}
      <Tile to={user?.role === "admin" ? "/admin" : "/user"} disabled={!user}>
        {user?.role === "admin" ? "Admin Panel" : "User Panel"}
      </Tile>

      {/* External link with conditions */}
      <Tile
        href={user?.isPremium ? "https://premium.example.com" : undefined}
        to={!user?.isPremium ? "/upgrade" : undefined}
      >
        {user?.isPremium ? "Premium Features" : "Upgrade Account"}
      </Tile>
    </div>
  );
};
```

## Step 5: Interactive Tiles

### 5.1 Custom Click Actions

Use the `onClick` prop for custom interactions:

```tsx
import { Tile } from "@neuron/ui";
import { useState } from "react";

const InteractiveTiles = () => {
  const [count, setCount] = useState(0);
  const [showModal, setShowModal] = useState(false);

  return (
    <div className="interactive-tiles">
      {/* Counter tile */}
      <Tile onClick={() => setCount(count + 1)}>Click Count: {count}</Tile>

      {/* Modal trigger tile */}
      <Tile onClick={() => setShowModal(true)}>Open Modal</Tile>

      {/* Confirmation tile */}
      <Tile
        onClick={() => {
          if (confirm("Are you sure?")) {
            console.info("Action confirmed");
          }
        }}
      >
        Confirm Action
      </Tile>

      {/* Async action tile */}
      <Tile
        onClick={async () => {
          try {
            await fetch("/api/action");
            alert("Action completed");
          } catch (error) {
            alert("Action failed");
          }
        }}
      >
        Async Action
      </Tile>
    </div>
  );
};
```

### 5.2 Form Integration

Integrate tiles with form actions:

```tsx
import { Tile } from "@neuron/ui";
import { useForm } from "react-hook-form";

const FormIntegratedTiles = () => {
  const { handleSubmit, reset, formState } = useForm();

  const onSubmit = (data) => {
    console.info("Form submitted:", data);
  };

  return (
    <div className="form-integrated-tiles">
      <form onSubmit={handleSubmit(onSubmit)}>
        {/* Form fields here */}

        <div className="form-actions">
          {/* Submit tile */}
          <Tile onClick={handleSubmit(onSubmit)} disabled={!formState.isValid}>
            Submit Form
          </Tile>

          {/* Reset tile */}
          <Tile variant="outline" onClick={() => reset()}>
            Reset Form
          </Tile>
        </div>
      </form>
    </div>
  );
};
```

### 5.3 State Management Integration

Connect tiles with application state:

```tsx
import { Tile } from "@neuron/ui";
import { useDispatch, useSelector } from "react-redux";

const StateConnectedTiles = () => {
  const dispatch = useDispatch();
  const { items, loading } = useSelector((state) => state.items);

  return (
    <div className="state-connected-tiles">
      {/* Action tiles connected to Redux */}
      <Tile onClick={() => dispatch({ type: "FETCH_ITEMS" })} disabled={loading}>
        {loading ? "Loading..." : "Fetch Items"}
      </Tile>

      <Tile onClick={() => dispatch({ type: "CLEAR_ITEMS" })} disabled={items.length === 0} variant="outline">
        Clear Items ({items.length})
      </Tile>

      <Tile onClick={() => dispatch({ type: "ADD_ITEM" })}>Add New Item</Tile>
    </div>
  );
};
```

## Step 6: Static Content Tiles

### 6.1 Information Display Tiles

Use static tiles for displaying information without interaction:

```tsx
import { Tile } from "@neuron/ui";

const InformationTiles = ({ stats }) => {
  return (
    <div className="information-tiles">
      {/* Statistics display */}
      <Tile>
        <div>
          <h3>Total Users</h3>
          <p>{stats.totalUsers}</p>
        </div>
      </Tile>

      <Tile variant="outline">
        <div>
          <h3>Active Sessions</h3>
          <p>{stats.activeSessions}</p>
        </div>
      </Tile>

      <Tile size="small">
        <div>
          <h4>Last Updated</h4>
          <p>{stats.lastUpdated}</p>
        </div>
      </Tile>
    </div>
  );
};
```

### 6.2 Content Cards

Create content cards using static tiles:

```tsx
import { Tile } from "@neuron/ui";

const ContentCards = ({ articles }) => {
  return (
    <div className="content-cards">
      {articles.map((article) => (
        <Tile key={article.id} maxWidth="300px">
          <div className="article-card">
            <h3>{article.title}</h3>
            <p>{article.excerpt}</p>
            <div className="article-meta">
              <span>By {article.author}</span>
              <span>{article.publishDate}</span>
            </div>
          </div>
        </Tile>
      ))}
    </div>
  );
};
```

### 6.3 Interactive Elements Inside Static Tiles

**⚠️ CRITICAL: When you need interactive elements inside tiles, use static tiles (no `to`, `href`, or `onClick`)**

```tsx
import { Tile, Button } from "@neuron/ui";

const InteractiveContentTiles = ({ items }) => {
  return (
    <div className="interactive-content-tiles">
      {items.map((item) => (
        <Tile key={item.id}>
          {" "}
          {/* Static tile - no interaction props */}
          <div className="item-content">
            <h3>{item.title}</h3>
            <p>{item.description}</p>

            {/* Interactive elements inside */}
            <div className="item-actions">
              <Button size="small" onClick={() => console.info("Edit", item.id)}>
                Edit
              </Button>
              <Button size="small" variant="danger" onClick={() => console.info("Delete", item.id)}>
                Delete
              </Button>
            </div>
          </div>
        </Tile>
      ))}
    </div>
  );
};
```

## Step 7: Tile Props Reference

### 7.1 Core Tile Props

| Prop      | Type                | Default   | Description              |
| --------- | ------------------- | --------- | ------------------------ |
| children  | `ReactNode`         | -         | Tile content             |
| variant   | `"outline"`         | -         | Visual variant style     |
| size      | `"base" \| "small"` | `"base"`  | Tile padding size        |
| disabled  | `boolean`           | `false`   | Disable tile interaction |
| active    | `boolean`           | `false`   | Active state indicator   |
| maxWidth  | `string`            | `"380px"` | Maximum tile width       |
| className | `string`            | -         | Additional CSS classes   |
| testId    | `string`            | -         | Test identifier          |

### 7.2 Navigation Props

| Prop   | Type     | Default | Description                   |
| ------ | -------- | ------- | ----------------------------- |
| to     | `string` | -       | React Router navigation path  |
| href   | `string` | -       | External URL or anchor href   |
| target | `string` | -       | Link target (e.g., "\_blank") |

### 7.3 Interaction Props

| Prop    | Type                                                   | Default | Description            |
| ------- | ------------------------------------------------------ | ------- | ---------------------- |
| onClick | `(event: React.MouseEvent<HTMLButtonElement>) => void` | -       | Click handler function |

### 7.4 Rendering Logic

**⚠️ CRITICAL: Component renders different HTML elements based on props**

| Props Provided | Rendered Element          | Use Case            |
| -------------- | ------------------------- | ------------------- |
| `to`           | `<a>` (React Router Link) | Internal navigation |
| `href`         | `<a>` (HTML anchor)       | External links      |
| `onClick`      | `<button>`                | Custom actions      |
| None of above  | `<div>`                   | Static content      |

## Step 8: Best Practices

### 8.1 When to Use Each Tile Mode

**Link Tiles (to/href):**

- Navigation between pages
- External links
- Downloadable resources
- Email/phone links

```tsx
{/* Good: Navigation tiles */}
<Tile to="/dashboard">Dashboard</Tile>
<Tile href="https://docs.example.com" target="_blank">Documentation</Tile>
```

**Action Tiles (onClick):**

- Form submissions
- Modal triggers
- State changes
- API calls

```tsx
{/* Good: Action tiles */}
<Tile onClick={() => setModalOpen(true)}>Open Settings</Tile>
<Tile onClick={handleSubmit}>Submit Form</Tile>
```

**Static Tiles (no interaction props):**

- Information display
- Content cards
- Containers with internal interactive elements

```tsx
{
  /* Good: Static content tiles */
}
<Tile>
  <div>
    <h3>Statistics</h3>
    <Button onClick={handleRefresh}>Refresh</Button>
  </div>
</Tile>;
```

### 8.2 Prop Combination Rules

**⚠️ CRITICAL: Use only one interaction prop at a time**

```tsx
{/* ❌ INCORRECT: Multiple interaction props */}
<Tile
  to="/dashboard"
  onClick={() => {}}
  href="https://example.com"
>
  Conflicting Actions
</Tile>

{/* ✅ CORRECT: Single interaction prop */}
<Tile to="/dashboard">Navigate</Tile>
<Tile onClick={() => {}}>Action</Tile>
<Tile href="https://example.com">External Link</Tile>
```

### 8.3 Active State Management

- Use active state for navigation tiles to show current page
- Manage active state externally for consistent behavior
- Consider URL-based active state for navigation tiles

```tsx
{
  /* Good: URL-based active state */
}
const NavigationTiles = () => {
  const location = useLocation();

  return (
    <div>
      <Tile to="/dashboard" active={location.pathname === "/dashboard"}>
        Dashboard
      </Tile>
      <Tile to="/profile" active={location.pathname === "/profile"}>
        Profile
      </Tile>
    </div>
  );
};
```

### 8.4 Width and Layout Considerations

- Use default maxWidth (380px) for most cases
- Set maxWidth="100%" for full-width tiles
- Consider responsive design with CSS Grid or Flexbox

```tsx
{
  /* Good: Responsive tile layout */
}
<div
  className="tile-grid"
  style={{
    display: "grid",
    gridTemplateColumns: "repeat(auto-fit, minmax(300px, 1fr))",
    gap: "16px",
  }}
>
  <Tile maxWidth="100%" to="/item1">
    Item 1
  </Tile>
  <Tile maxWidth="100%" to="/item2">
    Item 2
  </Tile>
  <Tile maxWidth="100%" to="/item3">
    Item 3
  </Tile>
</div>;
```

### 8.5 Accessibility Considerations

- Provide meaningful content for screen readers
- Use proper semantic HTML (automatic based on props)
- Ensure sufficient color contrast
- Add proper ARIA attributes when needed

```tsx
{/* Good: Accessible tiles */}
<Tile to="/dashboard" aria-label="Navigate to dashboard">
  <div>
    <h3>Dashboard</h3>
    <p>View your analytics and reports</p>
  </div>
</Tile>

<Tile onClick={handleDelete} aria-label="Delete item">
  <span aria-hidden="true">🗑️</span>
  Delete
</Tile>
```

## Step 9: Common Patterns and Examples

### 9.1 Dashboard Tiles

```tsx
import { Tile } from "@neuron/ui";

const DashboardTiles = ({ stats }) => {
  return (
    <div className="dashboard-tiles">
      <Tile to="/users" active={stats.activeSection === "users"}>
        <div className="stat-tile">
          <h3>Users</h3>
          <p className="stat-number">{stats.totalUsers}</p>
          <p className="stat-change">+{stats.newUsers} this week</p>
        </div>
      </Tile>

      <Tile to="/orders" active={stats.activeSection === "orders"}>
        <div className="stat-tile">
          <h3>Orders</h3>
          <p className="stat-number">{stats.totalOrders}</p>
          <p className="stat-change">+{stats.newOrders} today</p>
        </div>
      </Tile>

      <Tile onClick={() => window.print()}>
        <div className="action-tile">
          <h3>Print Report</h3>
          <p>Generate printable summary</p>
        </div>
      </Tile>
    </div>
  );
};
```

### 9.2 Product Grid

```tsx
import { Tile, Button } from "@neuron/ui";

const ProductGrid = ({ products, onAddToCart }) => {
  return (
    <div className="product-grid">
      {products.map((product) => (
        <Tile key={product.id} maxWidth="280px">
          <div className="product-card">
            <img src={product.image} alt={product.name} />
            <h3>{product.name}</h3>
            <p className="price">${product.price}</p>
            <p className="description">{product.description}</p>

            <div className="product-actions">
              <Button variant="primary" size="small" onClick={() => onAddToCart(product.id)}>
                Add to Cart
              </Button>
              <Button variant="secondary" size="small" onClick={() => window.open(`/product/${product.id}`)}>
                View Details
              </Button>
            </div>
          </div>
        </Tile>
      ))}
    </div>
  );
};
```

### 9.3 Settings Panel

```tsx
import { Tile } from "@neuron/ui";

const SettingsPanel = ({ settings, onToggleSetting }) => {
  return (
    <div className="settings-panel">
      <Tile variant="outline" size="small">
        <div className="setting-item">
          <h4>Email Notifications</h4>
          <p>Receive updates via email</p>
          <label>
            <input
              type="checkbox"
              checked={settings.emailNotifications}
              onChange={() => onToggleSetting("emailNotifications")}
            />
            Enable
          </label>
        </div>
      </Tile>

      <Tile variant="outline" size="small">
        <div className="setting-item">
          <h4>Dark Mode</h4>
          <p>Use dark theme</p>
          <label>
            <input type="checkbox" checked={settings.darkMode} onChange={() => onToggleSetting("darkMode")} />
            Enable
          </label>
        </div>
      </Tile>

      <Tile onClick={() => onToggleSetting("autoSave")} active={settings.autoSave}>
        <div className="setting-item">
          <h4>Auto Save</h4>
          <p>Automatically save changes</p>
          <span>{settings.autoSave ? "Enabled" : "Disabled"}</span>
        </div>
      </Tile>
    </div>
  );
};
```

### 9.4 Navigation Menu

```tsx
import { Tile } from "@neuron/ui";
import { useLocation } from "react-router-dom";

const NavigationMenu = ({ menuItems }) => {
  const location = useLocation();

  return (
    <nav className="navigation-menu">
      {menuItems.map((item) => (
        <Tile key={item.path} to={item.path} active={location.pathname === item.path} size="small">
          <div className="nav-item">
            <span className="nav-icon">{item.icon}</span>
            <span className="nav-label">{item.label}</span>
            {item.badge && <span className="nav-badge">{item.badge}</span>}
          </div>
        </Tile>
      ))}
    </nav>
  );
};
```

## Step 10: Common Mistakes to Avoid

### 10.1 Don't Mix Interaction Props

```tsx
{/* ❌ INCORRECT: Multiple interaction props */}
<Tile to="/page" onClick={() => {}} href="https://example.com">
  Conflicting Actions
</Tile>

{/* ✅ CORRECT: Single interaction prop */}
<Tile to="/page">Navigation</Tile>
<Tile onClick={() => {}}>Action</Tile>
<Tile href="https://example.com">External Link</Tile>
```

### 10.2 Don't Use onClick for Navigation

```tsx
{
  /* ❌ INCORRECT: Using onClick for navigation */
}
<Tile onClick={() => navigate("/dashboard")}>Go to Dashboard</Tile>;

{
  /* ✅ CORRECT: Use to prop for navigation */
}
<Tile to="/dashboard">Go to Dashboard</Tile>;
```

### 10.3 Don't Make Static Tiles Clickable

```tsx
{
  /* ❌ INCORRECT: Static tile with click handler inside */
}
<Tile onClick={() => {}}>
  <div>
    <h3>Content</h3>
    <Button
      onClick={(e) => {
        e.stopPropagation(); // Needed to prevent conflicts
        handleAction();
      }}
    >
      Action
    </Button>
  </div>
</Tile>;

{
  /* ✅ CORRECT: Static tile with internal interactions */
}
<Tile>
  <div>
    <h3>Content</h3>
    <Button onClick={handleAction}>Action</Button>
  </div>
</Tile>;
```

### 10.4 Don't Ignore Disabled State

```tsx
{
  /* ❌ INCORRECT: No disabled state for unavailable actions */
}
<Tile onClick={user ? handleAction : undefined}>{user ? "Perform Action" : "Login Required"}</Tile>;

{
  /* ✅ CORRECT: Proper disabled state */
}
<Tile onClick={handleAction} disabled={!user}>
  {user ? "Perform Action" : "Login Required"}
</Tile>;
```

### 10.5 Don't Forget Accessibility

```tsx
{
  /* ❌ INCORRECT: No accessible content */
}
<Tile onClick={handleDelete}>🗑️</Tile>;

{
  /* ✅ CORRECT: Accessible content */
}
<Tile onClick={handleDelete} aria-label="Delete item">
  <span aria-hidden="true">🗑️</span>
  <span className="sr-only">Delete</span>
</Tile>;
```

## Key Takeaways

The Neuron Tile component system provides a flexible, accessible, and consistent foundation for clickable elements. Key points to remember:

1. **Automatic rendering** - Component renders appropriate HTML element based on props
2. **Single interaction rule** - Use only one of `to`, `href`, or `onClick` per tile
3. **Semantic HTML** - Proper accessibility through automatic element selection
4. **Flexible sizing** - Control width and padding with `maxWidth` and `size` props
5. **State management** - Use `active` and `disabled` states appropriately
6. **Navigation patterns** - Use `to` for internal navigation, `href` for external links
7. **Interactive content** - Use static tiles when you need interactive elements inside
8. **Responsive design** - Consider layout and width in different screen sizes
9. **Accessibility first** - Provide meaningful content and proper ARIA attributes
10. **Performance considerations** - Use appropriate interaction patterns for better UX

By following these guidelines, you'll create consistent, accessible, and user-friendly tile interfaces that enhance your Neuron applications across all interaction patterns.

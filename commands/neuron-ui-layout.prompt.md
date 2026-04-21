---
agent: agent
---

# AI-Assisted Neuron Layout System Integration Guide

## For the AI Assistant

Your task is to integrate and configure the Neuron Layout system in a React application. This guide provides step-by-step instructions for implementing the Layout component, which serves as the foundation for structuring application pages with consistent layout patterns.

## Sync Metadata

- **Component Version:** v4.0.3
- **Component Source:** `packages/neuron/ui/src/lib/layoutSystem/layout/Layout.tsx`
- **Guideline Command:** `/neuron-ui-layout`
- **Related Skill:** `neuron-ui-layout`

## Introduction

The Layout system is a core part of the Neuron UI framework, designed to create consistent, responsive, and accessible user interfaces across all Neuron applications.

### What is the Layout System?

The Layout component provides a structured container for your application with support for:

- Main content area for primary content
- Optional left and right side panels for navigation and contextual information
- Top, bottom, and footer sections for headers, action bars, and footers
- Built-in loading states with skeletons
- Color mode support (light/dark)
- Integrated Dock component for floating actions

### Key Features

- **Consistent Structure**: Standardized layout across applications
- **Responsive Design**: Adapts to different screen sizes
- **Loading States**: Built-in support for loading skeletons
- **Color Mode Support**: Integrated light and dark mode
- **Portal System**: Inject content into layout sections from anywhere
- **Dock Integration**: Built-in support for floating action buttons
- **Grid System**: 12-column grid for responsive layouts

**Storybook Reference:** You can refer to the Storybook documentation for visual examples and interactive demos of the Layout component.

## Step 1: Install Dependencies

Ensure that the `@neuron/ui` package is included in your application's `package.json`. This package contains the Layout component and all related components.

```json
{
  "dependencies": {
    "@neuron/ui": "latest"
  }
}
```

Then, run `npm install` to install the dependency.

## Step 2: Setup Required Providers

### 2.1 PrimeReactProvider

The `PrimeReactProvider` (exported as `PRProvider`) is essential when using Layout components with PrimeReact overlay components such as Dropdown, MultiSelect, Calendar, etc. It must be placed outside the `LayoutProvider`.

```tsx
import { PRProvider, LayoutProvider, Layout } from "@neuron/ui";

const App = () => {
  return (
    <PRProvider>
      <LayoutProvider>
        <Layout>
          <div className="g-col-12 d-grid">
            <Outlet />
          </div>
        </Layout>
      </LayoutProvider>
    </PRProvider>
  );
};
```

The `PRProvider` configures PrimeReact with appropriate settings for the Neuron UI ecosystem:

- Disables ripple effects
- Sets z-index values for different component types
- Configures overlay behaviors
- Sets locale settings

**Important**: Always include `PRProvider` when using Layout components with any PrimeReact components to ensure correct overlay behavior.

### 2.2 LayoutProvider

The `LayoutProvider` is a crucial component that must wrap your Layout components. It manages the state and references for all layout sections.

```tsx
import { LayoutProvider, Layout } from "@neuron/ui";

const App = () => {
  return (
    <LayoutProvider>
      <Layout>{/* Your application content */}</Layout>
    </LayoutProvider>
  );
};
```

The `LayoutProvider` creates a React context that:

- Stores references to each layout section
- Manages loading states
- Provides methods to update content in different layout sections
- Maintains a reference to the dock element

**Important**: Always place the `LayoutProvider` within the `PRProvider` but at a high level in your application, typically in your main App component or around your router.

## Step 3: Basic Layout Implementation

The Layout component is the main container for your application's UI. It provides sections for different parts of your interface.

### 3.1 Basic Usage

Here's a simple implementation of the Layout component:

```tsx
import { Layout, LayoutProvider } from "@neuron/ui";

const App = () => {
  return (
    <LayoutProvider>
      <Layout>
        <div className="g-col-12">
          <h1>My Application</h1>
          <p>Welcome to my application!</p>
        </div>
      </Layout>
    </LayoutProvider>
  );
};
```

### 3.2 Layout Structure

The Layout component is structured as follows:

```
<main class="layout">
  <aside class="layout__left" id="layoutLeft">
    <!-- Left sidebar content -->
  </aside>
  <div class="layout__inner-wrapper">
    <div class="layout__main-wrapper">
      <div class="layout__top" id="layoutTop">
        <!-- Top content (e.g., StickyHeader) -->
      </div>
      <section class="grid" id="layoutMain">
        <!-- Main content -->
      </section>
      <aside class="layout__right" id="layoutRight">
        <!-- Right sidebar content -->
      </aside>
    </div>
    <div class="layout__bottom" id="layoutBottom">
      <!-- Bottom content (e.g., ActionBar) -->
    </div>
    <div class="layout__footer" id="layoutFooter">
      <!-- Footer content -->
    </div>
  </div>
</main>
```

### 3.3 Layout Props

The Layout component accepts the following props:

| Prop                 | Type      | Default  | Description                      |
| -------------------- | --------- | -------- | -------------------------------- |
| children             | ReactNode | required | Main content to render           |
| topContent           | ReactNode | -        | Content for the top section      |
| bottomContent        | ReactNode | -        | Content for the bottom section   |
| leftSide             | ReactNode | -        | Content for the left sidebar     |
| rightSide            | ReactNode | -        | Content for the right sidebar    |
| footerContent        | ReactNode | -        | Content for the footer           |
| isTopContentLoading  | boolean   | false    | Shows skeleton for top content   |
| isRightSideLoading   | boolean   | false    | Shows skeleton for right sidebar |
| isMainContentLoading | boolean   | false    | Shows skeleton for main content  |
| colGap               | boolean   | true     | Enables column gap in layout     |
| background           | boolean   | true     | Shows background color           |
| className            | string    | -        | Additional CSS class             |

## Step 4: Layout Portal System

### 4.1 When to Use Layout Portal vs. Direct Layout Properties

The Layout component provides two ways to add content to different sections:

#### Direct Properties (Recommended for Global Components)

Use direct layout properties (`leftSide`, `topContent`, `footerContent`, etc.) for:

- **Global components** that appear on all pages (like Menu and Footer)
- Components that are essential to your application's structure
- Elements that should be consistent across all pages

Example:

```tsx
<Layout
  leftSide={<AppMenu />} // Menu is global - always visible
  footerContent={<Footer />} // Footer is global - always visible
>
  {children}
</Layout>
```

#### Layout Portal (For Page-Specific Components)

Use Layout Portal for:

- Components that should appear only on **specific pages**
- Dynamic content that changes based on routes
- Components that need to be mounted conditionally
- Components that are defined/rendered deep in the component tree

Example:

```tsx
// This component only appears on the Dashboard page
const DashboardPage = () => {
  return (
    <>
      <LayoutPortal position="top">
        <DashboardHeader /> {/* Only visible on Dashboard */}
      </LayoutPortal>

      <LayoutPortal position="main">
        <DashboardContent /> {/* Dashboard-specific content */}
      </LayoutPortal>
    </>
  );
};
```

### 4.2 Available Portal Positions

- `main`: The main content area
- `left`: The left sidebar
- `right`: The right sidebar
- `top`: The top content area
- `bottom`: The bottom content area
- `footer`: The footer area

### 4.3 Injecting Content with LayoutPortal

```tsx
import { LayoutPortal } from "@neuron/ui";

// Inject a component into the top section (header)
const HeaderComponent = () => {
  return (
    <LayoutPortal position="top">
      <StickyHeader />
    </LayoutPortal>
  );
};

// Inject a component into the left sidebar (menu)
const MenuComponent = () => {
  return (
    <LayoutPortal position="left">
      <Menu />
    </LayoutPortal>
  );
};

// Inject a component into the main content area
const MainComponent = () => {
  return (
    <LayoutPortal position="main">
      <Dashboard />
    </LayoutPortal>
  );
};
```

### 4.3 Complete Example with Multiple Portals

```tsx
import { Layout, LayoutPortal, LayoutProvider, Menu, StickyHeader, Footer } from "@neuron/ui";

const App = () => {
  return (
    <LayoutProvider>
      <Layout>{/* Main content will be empty as we're using portals */}</Layout>

      {/* Inject components into different layout sections */}
      <LayoutPortal position="top">
        <StickyHeader title="My Application" />
      </LayoutPortal>

      <LayoutPortal position="left">
        <Menu items={menuItems} />
      </LayoutPortal>

      <LayoutPortal position="main">
        <div className="g-col-12">
          <h1>Dashboard</h1>
          <p>Welcome to your dashboard!</p>
        </div>
      </LayoutPortal>

      <LayoutPortal position="footer">
        <Footer />
      </LayoutPortal>
    </LayoutProvider>
  );
};
```

### 4.4 How Layout Portal Works

1. The `LayoutPortal` component determines the target DOM element ID based on the specified position
2. It uses React's `useEffect` to find the target element in the DOM
3. Once found, it uses `ReactDOM.createPortal` to render its children into that element
4. If the target element is not found, it logs a warning and renders nothing

This approach allows you to:

- Inject content from any component in your tree
- Keep your component structure clean and modular
- Avoid prop drilling for layout content

## Step 5: Managing Loading States with Skeletons

The Layout component includes built-in support for loading states with skeletons for different sections. This provides a better user experience during data fetching operations.

### 5.1 Available Loading States

- `isMainContentLoading`: Shows skeleton for the main content area
- `isTopContentLoading`: Shows skeleton for the top section
- `isRightSideLoading`: Shows skeleton for the right sidebar

### 5.2 Setting Loading States via Props

You can set loading states directly through props on the Layout component:

```tsx
import { Layout, LayoutProvider } from "@neuron/ui";
import { useState, useEffect } from "react";

const App = () => {
  const [isLoading, setIsLoading] = useState(true);

  useEffect(() => {
    // Simulate data loading
    const timer = setTimeout(() => {
      setIsLoading(false);
    }, 2000);

    return () => clearTimeout(timer);
  }, []);

  return (
    <LayoutProvider>
      <Layout isMainContentLoading={isLoading}>
        <div className="g-col-12">
          <h1>Dashboard</h1>
          <p>This content will be replaced by a skeleton while loading</p>
        </div>
      </Layout>
    </LayoutProvider>
  );
};
```

### 5.3 Setting Loading States via Context

For more dynamic control, especially from child components, use the LayoutContext:

```tsx
import { useContext, useEffect } from "react";
import { LayoutContext } from "@neuron/ui";

const DataComponent = () => {
  const { setIsMainContentLoading } = useContext(LayoutContext);

  useEffect(() => {
    // Set loading state before fetching data
    setIsMainContentLoading(true);

    // Fetch data
    fetchData().finally(() => {
      // Clear loading state when done
      setIsMainContentLoading(false);
    });
  }, [setIsMainContentLoading]);

  return <div>Data Component</div>;
};
```

### 5.4 Skeleton Components

The layout system includes three skeleton components that are automatically displayed when the corresponding loading state is set to `true`:

1. `MainContentSkeleton`: Used for the main content area
2. `TopContentSkeleton`: Used for the top section
3. `RightContentSkeleton`: Used for the right sidebar

These skeletons are designed to match the layout and provide a smooth loading experience.

## Step 6: Color Mode Support

The Layout component includes built-in support for light and dark modes through the `ColorModeProvider`. This provider is integrated into the Layout component, so you don't need to add it separately.

### 6.1 Automatic Color Mode

When using the Layout component with Menu or Footer components from @neuron/ui, the color mode switching is handled automatically. The Menu component typically includes a toggle button for switching between light and dark modes.

### 6.2 Accessing Color Mode

If you need to access or modify the color mode programmatically:

```tsx
import { useColorMode } from "@neuron/ui";

const MyComponent = () => {
  const { colorMode, setColorMode } = useColorMode();

  return (
    <div>
      <p>Current mode: {colorMode}</p>
      <button onClick={() => setColorMode(colorMode === "light" ? "dark" : "light")}>Toggle mode</button>
    </div>
  );
};
```

## Step 7: Using the Dock Component with Layout

The Layout includes an integrated `Dock` component that provides a container for floating action buttons. The Dock is positioned at the edge of the screen and can contain various action buttons.

### 7.1 Understanding the Layout-Dock Integration

When using the Layout component, a Dock component is automatically included. The key aspects of this integration are:

1. You don't need to add a Dock component manually
2. The Dock will only be displayed if it contains at least one button
3. The Layout component manages the Dock element reference through the LayoutContext

### 7.2 Adding Buttons to the Layout's Dock

You can add buttons to the Layout's Dock from anywhere in your application using the `DockButton` component:

```tsx
import { DockButton, baseIcons } from "@neuron/ui";

const MyComponent = () => {
  const handleClick = () => {
    console.log("Button clicked");
  };

  return (
    <>
      {/* Your component content */}

      {/* Add a button to the Layout dock */}
      <DockButton icon={baseIcons.plus} onClick={handleClick} tooltip="Add new item" />

      {/* Add a button with notification count */}
      <DockButton
        icon={baseIcons.bellNotificationSolid}
        onClick={handleClick}
        notificationCount={5}
        tooltip="Notifications"
      />
    </>
  );
};
```

### 7.3 How Layout Manages the Dock

The Layout component:

1. Creates a Dock component internally
2. Stores a reference to the Dock element in the LayoutContext
3. Makes this reference available to DockButton components

This integration ensures that:

- DockButtons can find the Dock element automatically through the LayoutContext
- The Dock is positioned correctly relative to the Layout
- The Dock is only visible when it contains buttons

### 7.4 Important Cleanup Considerations

When using DockButtons in components that may unmount, it's important to handle cleanup properly:

````tsx
import { DockButton, baseIcons } from "@neuron/ui";
import { useRef, useEffect } from "react";

const DynamicComponent = () => {
  // Keep a reference to the button element
  const buttonRef = useRef(null);

  // Cleanup when component unmounts
  useEffect(() => {
    return () => {
      // The DockButton will automatically remove itself from the Dock
      // when it unmounts, so no explicit cleanup is needed
    };
  }, []);

  return (
    <DockButton
      ref={buttonRef}
      icon={baseIcons.plus}
      onClick={() => console.log("Clicked")}
      tooltip="Add item"
    />
  );
};

For more detailed information about the Dock component itself, please refer to the separate Dock guidelines document.

## Step 8: Best Practices

Follow these best practices to ensure optimal implementation of the Layout system:

1. **Provider hierarchy**
   - Always wrap components with the proper provider hierarchy: `Helmet > PRProvider > LayoutProvider > Layout`
   - The Layout component is typically used within the Helmet project
   - Only use one PRProvider and one LayoutProvider per application
   - Place these providers at the top level of your component tree

2. **Prefer LayoutPortal for injecting content**
   - Use LayoutPortal instead of passing content as props when possible
   - This keeps your component structure cleaner and more modular
   - LayoutPortal allows you to inject content from anywhere in your component tree

3. **Handle loading states appropriately**
   - Set loading states when fetching data
   - Clear loading states when data fetching is complete
   - Use the built-in skeleton components for a consistent loading experience

4. **Combine with other Neuron UI components**
   - Use StickyHeader in the top section
   - Use ActionBar in the bottom section
   - Use Menu in the left sidebar
   - Use appropriate components in the right sidebar based on your needs

5. **Avoid direct DOM manipulation**
   - Use the provided APIs (LayoutContext, LayoutPortal) instead of directly manipulating DOM elements
   - This ensures compatibility with future updates to the Layout system

6. **Manage Dock buttons properly**
   - Always clean up Dock buttons when components unmount
   - Use meaningful icons and tooltips for Dock buttons
   - Limit the number of buttons to avoid cluttering the interface

## Step 9: Troubleshooting

### Layout sections not appearing

1. **Check Provider hierarchy**
   - Ensure the Layout component is wrapped in the proper order: `PRProvider > LayoutProvider > Layout`
   - Verify there's only one PRProvider and one LayoutProvider in your application

2. **Check LayoutPortal usage**
   - Ensure the position prop is correct ("main", "top", "bottom", "left", "right", "footer")
   - Check that the Layout component is already mounted before using LayoutPortal

3. **Check console for warnings**
   - Look for warnings like "LayoutPortal: Element with id X not found"
   - This indicates that the target DOM element doesn't exist yet

### Loading states not working

1. **Check prop names**
   - Ensure you're using the correct prop names: `isMainContentLoading`, `isTopContentLoading`, `isRightSideLoading`

2. **Check context usage**
   - If using context, ensure you're calling the setter functions correctly
   - Add the setter functions to your useEffect dependency array

### Dock buttons not appearing

1. **Check Layout mounting**
   - Ensure the Layout component is mounted before adding DockButtons
   - The Dock element needs to be in the DOM before DockButtons can find it

2. **Check DockButton props**
   - Ensure you're providing an icon prop to the DockButton
   - Check that the onClick handler is properly defined

## Step 10: Using the Grid System

The main content area of the Layout uses a 12-column grid system. This is implemented with the CSS class `.grid` on the main content section.

### 10.1 Grid System Basics

The grid system allows you to create responsive layouts by specifying how many columns an element should span at different screen sizes.

```tsx
<div className="g-col-12 g-col-md-6 g-col-lg-4">
  {/* This element will take 12 columns on small screens, 6 on medium, and 4 on large */}
  <p>Responsive grid item</p>
</div>
````

### 10.2 Available Grid Classes

- `g-col-{n}`: Takes n columns (1-12) on all screen sizes
- `g-col-md-{n}`: Takes n columns on medium screens and up
- `g-col-lg-{n}`: Takes n columns on large screens and up
- `g-col-xl-{n}`: Takes n columns on extra-large screens and up

### 10.3 Grid System Example

```tsx
import { Layout, LayoutProvider, LayoutPortal } from "@neuron/ui";

const GridExample = () => {
  return (
    <LayoutProvider>
      <Layout>{/* Empty Layout as we're using LayoutPortal */}</Layout>

      <LayoutPortal position="main">
        <div className="g-col-12">
          <h1>Grid System Example</h1>
        </div>

        {/* Two equal columns on medium screens and up */}
        <div className="g-col-12 g-col-md-6">
          <div className="card">
            <h2>Column 1</h2>
            <p>This takes full width on small screens and half width on medium screens and up.</p>
          </div>
        </div>

        <div className="g-col-12 g-col-md-6">
          <div className="card">
            <h2>Column 2</h2>
            <p>This takes full width on small screens and half width on medium screens and up.</p>
          </div>
        </div>

        {/* Three equal columns on large screens */}
        <div className="g-col-12 g-col-md-6 g-col-lg-4">
          <div className="card">
            <h3>Item 1</h3>
            <p>Responsive grid item</p>
          </div>
        </div>

        <div className="g-col-12 g-col-md-6 g-col-lg-4">
          <div className="card">
            <h3>Item 2</h3>
            <p>Responsive grid item</p>
          </div>
        </div>

        <div className="g-col-12 g-col-md-12 g-col-lg-4">
          <div className="card">
            <h3>Item 3</h3>
            <p>Responsive grid item</p>
          </div>
        </div>
      </LayoutPortal>
    </LayoutProvider>
  );
};
```

## Step 11: Complete Example

Here's a complete example of a typical Layout implementation with all major features:

```tsx
import { useEffect, useState } from "react";
import {
  Helmet,
  PRProvider,
  Layout,
  LayoutProvider,
  LayoutPortal,
  Menu,
  StickyHeader,
  Footer,
  ActionBar,
  DockButton,
  baseIcons,
} from "@neuron/ui";

const App = () => {
  const [isLoading, setIsLoading] = useState(true);

  useEffect(() => {
    // Simulate data loading
    const timer = setTimeout(() => {
      setIsLoading(false);
    }, 2000);

    return () => clearTimeout(timer);
  }, []);

  const handleAddClick = () => {
    console.log("Add button clicked");
  };

  return (
    <Helmet>
      <PRProvider>
        <LayoutProvider>
          {/* Main Layout */}
          <Layout isMainContentLoading={isLoading}>{/* Main content can be empty if using portals */}</Layout>

          {/* Top content - header */}
          <LayoutPortal position="top">
            <StickyHeader title="My Application" />
          </LayoutPortal>

          {/* Left side - menu */}
          <LayoutPortal position="left">
            <Menu items={menuItems} />
          </LayoutPortal>

          {/* Main content */}
          <LayoutPortal position="main">
            <div className="g-col-12">
              <h1>Dashboard</h1>
              <p>Welcome to your dashboard!</p>
            </div>
          </LayoutPortal>

          {/* Bottom content - action bar */}
          <LayoutPortal position="bottom">
            <ActionBar
              primaryActions={[
                { label: "Save", onClick: () => console.log("Save") },
                { label: "Cancel", onClick: () => console.log("Cancel") },
              ]}
            />
          </LayoutPortal>

          {/* Footer */}
          <LayoutPortal position="footer">
            <Footer />
          </LayoutPortal>

          {/* Dock button */}
          <DockButton icon={baseIcons.plus} onClick={handleAddClick} tooltip="Add new item" />
        </LayoutProvider>
      </PRProvider>
    </Helmet>
  );
};

export default App;
```

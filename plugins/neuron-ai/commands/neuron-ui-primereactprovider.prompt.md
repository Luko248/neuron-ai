---
agent: agent
---

# PrimeReactProvider Integration Guidelines

## For the AI Assistant

Your task is to help users properly integrate the PrimeReactProvider (PRProvider) component in Neuron applications. This guide provides step-by-step instructions for implementing PRProvider correctly to ensure all PrimeReact components function properly within the application.

## Sync Metadata

- **Component Version:** v1.0.0
- **Component Source:** `packages/neuron/ui/src/lib/settings/PrimeReactProvider.tsx`
- **Guideline Command:** `/neuron-ui-primereactprovider`
- **Related Skill:** `neuron-ui-layout`

## Introduction

The `PRProvider` component is a crucial wrapper that configures PrimeReact components to work correctly within the Neuron UI ecosystem. It provides a configuration layer that ensures proper behavior of overlay components like Dropdown, MultiSelect, AutoComplete, Calendar, and other PrimeReact components.

### Why PRProvider is Essential

PrimeReact components with overlay functionality require specific configuration settings to:

- Position correctly in the DOM
- Maintain proper z-index hierarchy
- Handle keyboard and focus management
- Support accessibility features
- Work correctly within nested components and portals

Without `PRProvider`, PrimeReact overlay components may exhibit incorrect positioning, focus issues, or z-index conflicts.

## Step 1: Import the PRProvider

Import the `PRProvider` component from `@neuron/ui`:

```tsx
import { PRProvider } from "@neuron/ui";
```

## Step 2: Wrap Your Application Components

Place the `PRProvider` at a high level in your component hierarchy, ensuring it wraps all components that use PrimeReact:

```tsx
import { PRProvider } from "@neuron/ui";

const App = () => {
  return <PRProvider>{/* Your application content */}</PRProvider>;
};
```

## Step 3: Proper Provider Hierarchy

When using with Layout and other Neuron providers, follow this hierarchy:

```tsx
import { Helmet, PRProvider, LayoutProvider, Layout } from "@neuron/ui";

const App = () => {
  return (
    <Helmet>
      <PRProvider>
        <LayoutProvider>
          <Layout>
            <div className="g-col-12 d-grid">
              <Outlet />
            </div>
          </Layout>
        </LayoutProvider>
      </PRProvider>
    </Helmet>
  );
};
```

This hierarchy ensures that:

1. `Helmet` provides global styling and meta information
2. `PRProvider` configures PrimeReact components correctly
3. `LayoutProvider` establishes the layout structure
4. `Layout` renders the UI framework

## Step 4: Advanced Configuration

The `PRProvider` accepts an optional `shadowRoot` prop for applications using Shadow DOM:

```tsx
// With a shadow root
const shadowRoot = element.attachShadow({ mode: "open" });

<PRProvider shadowRoot={{ root: shadowRoot }}>
  <App />
</PRProvider>;
```

## Default Configuration

The `PRProvider` sets these default configurations for PrimeReact:

- Disables ripple effects (`ripple: false`)
- Enables CSS transitions (`cssTransition: true`)
- Disables hiding overlays on document scrolling (`hideOverlaysOnDocumentScrolling: false`)
- Sets predefined z-index values for different component types:
  - Modal: 1000
  - Overlay: 2000
  - Menu: 2000
  - Tooltip: 3000
  - Toast: 4000
- Sets "cs" as default locale
- Disables auto z-indexing (`autoZIndex: false`)
- Sets `document.body` as the append container

## Troubleshooting

### Overlay Components Not Positioning Correctly

1. Ensure `PRProvider` wraps your components
2. Check that `PRProvider` is positioned correctly in the provider hierarchy
3. Verify there's only one instance of `PRProvider` in your application

### Z-index Conflicts

1. Use the predefined z-index system
2. Avoid manual z-index values that could conflict with PrimeReact components
3. If custom z-index values are needed, ensure they follow the hierarchy:
   - Modal components < Overlay components < Tooltip components < Toast components

### Shadow DOM Issues

When using Shadow DOM:

1. Ensure the correct shadowRoot is passed to the provider
2. Consider using a styleContainer if styles need to be injected in a different element

## Best Practices

1. **Single Instance**: Use only one PRProvider per application
2. **Correct Order**: Place PRProvider outside LayoutProvider but inside Helmet
3. **Component Grouping**: Keep components that need to interact with each other under the same PRProvider
4. **Shadow DOM**: When using Shadow DOM, always provide both root and styleContainer
5. **Avoid Overrides**: Don't override the default configuration unless absolutely necessary

## Example: Complete Integration in a Typical App

```tsx
import { useEffect } from "react";
import { Helmet, PRProvider, LayoutProvider, Layout, StickyHeader, Menu, Footer } from "@neuron/ui";
import { BrowserRouter, Routes, Route } from "react-router-dom";

const App = () => {
  return (
    <BrowserRouter>
      <Helmet>
        <PRProvider>
          <LayoutProvider>
            <Layout leftSide={<Menu items={menuItems} />} footerContent={<Footer />}></Layout>
          </LayoutProvider>
        </PRProvider>
      </Helmet>
    </BrowserRouter>
  );
};

export default App;
```

Remember that the correct provider hierarchy is essential for proper functionality of all Neuron UI components. Always follow the `Helmet > PRProvider > LayoutProvider > Layout` structure in your applications.

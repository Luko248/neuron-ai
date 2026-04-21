---
name: neuron-content-helmet
description: Manage document head metadata in Neuron applications. Use when wiring the root app shell, page titles, meta tags, or Helmet placement with PRProvider, LayoutProvider, and Layout.
---

# Helmet Component Integration

Use this skill when you need to place or configure `Helmet` in a Neuron application.

## Core Purpose

`Helmet` is the Neuron wrapper around `react-helmet`. It standardizes:

- page titles
- viewport and theme-color metadata
- favicon and icon links
- shared document head defaults

## Required Placement

Place `Helmet` at the highest meaningful level in the app shell.

Preferred hierarchy:

```tsx
import { Helmet, PRProvider, LayoutProvider, Layout } from "@neuron/ui";

const App = () => {
  return (
    <Helmet title="Dashboard">
      <PRProvider>
        <LayoutProvider>
          <Layout>{/* app content */}</Layout>
        </LayoutProvider>
      </PRProvider>
    </Helmet>
  );
};
```

## Rules

- Use a single main `Helmet` wrapper for the page or app shell.
- Put `Helmet` above `PRProvider`, `LayoutProvider`, and `Layout`.
- Use the `title` prop instead of hand-rolling document title logic.
- Keep page titles concise and business-oriented.
- Do not introduce custom head-management abstractions when `Helmet` already fits.

## Title Behavior

```tsx
<Helmet title="User Profile">{/* page content */}</Helmet>
```

Expected behavior:

- no title prop -> default Neuron title
- title prop provided -> formatted application title with the Neuron prefix

## Typical Usage Patterns

### Root application shell

```tsx
const AppShell = () => {
  return (
    <Helmet title="Dashboard">
      <PRProvider>
        <LayoutProvider>
          <Layout>{/* routes */}</Layout>
        </LayoutProvider>
      </PRProvider>
    </Helmet>
  );
};
```

### Page-level title

```tsx
const UserProfilePage = () => {
  return <Helmet title="User Profile">{/* page content */}</Helmet>;
};
```

### Custom meta tags

If custom meta tags are required, keep the Neuron `Helmet` as the main wrapper and add extra tags in a controlled way within that head-management flow.

## Validation Checklist

- `Helmet` is present at the correct app or page boundary.
- Provider order is correct.
- Title is passed through `title`.
- No duplicate competing head-management wrappers were introduced.
- The final page title matches the current route or feature context.

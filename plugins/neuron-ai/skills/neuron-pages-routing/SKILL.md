---
name: neuron-pages-routing
description: Create pages and configure routing in Neuron applications. Use when adding new pages, defining routes, or setting up navigation. Covers page structure, route definitions, React Router integration, Helmet metadata, lazy loading, and protected routes. Provides step-by-step page creation and routing workflow.
---

# Page Creation & Routing

Create pages and configure routing in Neuron applications.

## Process

1. **Create page component** - Build page with Helmet and lazy loading
2. **Define route constant** - Add route path to constants
3. **Add to routing** - Configure route in Routing.tsx
4. **Add navigation** - Link to page from menu/navigation
5. **Test route** - Verify page loads correctly

## Page Structure

```
src/app/pages/
├── DashboardPage.tsx
├── UserProfilePage.tsx
└── SettingsPage.tsx

src/app/features/
├── dashboard/
│   └── Dashboard.tsx
├── userProfile/
│   └── UserProfile.tsx
└── settings/
    └── Settings.tsx
```

## Page Component Pattern

**Page component (DashboardPage.tsx):**

```tsx
import { Helmet } from "@neuron/ui";
import { lazy, Suspense } from "react";
import { Spinner } from "react-bootstrap";

const Dashboard = lazy(() => import("app/features/dashboard/Dashboard"));

const DashboardPage = () => (
  <Helmet title="Dashboard">
    <Suspense fallback={<Spinner animation="border" />}>
      <Dashboard />
    </Suspense>
  </Helmet>
);

export default DashboardPage;
```

**Feature component (Dashboard.tsx):**

```tsx
export const Dashboard = () => {
  return (
    <div className="grid content-gap">
      <Panel title="Dashboard" className="g-col-12">
        <p>Dashboard content</p>
      </Panel>
    </div>
  );
};

export default Dashboard;
```

## Route Configuration

**Route constants (routes.ts):**

```typescript
export const ROUTES = {
  DASHBOARD: "/dashboard",
  USER_PROFILE: "/user-profile",
  SETTINGS: "/settings",
  POST_LOGOUT: "/logged-out",
};
```

**Routing setup (Routing.tsx):**

```tsx
import { createBrowserRouter, createRoutesFromElements, Route } from "react-router-dom";
import { ProtectedRoute } from "@neuron/auth";
import DefaultLayout from "app/elements/common/layout/components/DefaultLayout";
import DashboardPage from "app/pages/DashboardPage";
import UserProfilePage from "app/pages/UserProfilePage";
import PostLogoutPage from "app/pages/PostLogoutPage";
import { ROUTES } from "constants/routes";

const Routing = () => {
  return createBrowserRouter(
    createRoutesFromElements(
      <>
        {/* PUBLIC ROUTES */}
        <Route path={ROUTES.POST_LOGOUT} element={<PostLogoutPage />} />

        {/* PROTECTED ROUTES */}
        <Route element={<ProtectedRoute redirectToAuthServer={true} />}>
          <Route element={<DefaultLayout />}>
            <Route index element={<DashboardPage />} />
            <Route path={ROUTES.DASHBOARD} element={<DashboardPage />} />
            <Route path={ROUTES.USER_PROFILE} element={<UserProfilePage />} />
            <Route path={ROUTES.SETTINGS} element={<SettingsPage />} />
          </Route>
        </Route>
      </>,
    ),
  );
};

export default Routing;
```

## Navigation Links

**Using route constants:**

```tsx
import { Link } from "react-router-dom";
import { ROUTES } from "constants/routes";

const Navigation = () => (
  <nav>
    <Link to={ROUTES.DASHBOARD}>Dashboard</Link>
    <Link to={ROUTES.USER_PROFILE}>Profile</Link>
    <Link to={ROUTES.SETTINGS}>Settings</Link>
  </nav>
);
```

## Protected Routes

**With authentication:**

```tsx
<Route element={<ProtectedRoute redirectToAuthServer={true} />}>
  <Route element={<DefaultLayout />}>
    <Route path={ROUTES.DASHBOARD} element={<DashboardPage />} />
  </Route>
</Route>
```

**With permissions:**

```tsx
<Route element={<ProtectedRoute requiredPermissions={["ADMIN"]} />}>
  <Route path={ROUTES.ADMIN} element={<AdminPage />} />
</Route>
```

## Examples

**Example 1: Simple page**

```tsx
// DashboardPage.tsx
import { Helmet } from "@neuron/ui";
import { lazy, Suspense } from "react";

const Dashboard = lazy(() => import("app/features/dashboard/Dashboard"));

const DashboardPage = () => (
  <Helmet title="Dashboard">
    <Suspense fallback={<div>Loading...</div>}>
      <Dashboard />
    </Suspense>
  </Helmet>
);

export default DashboardPage;
```

**Example 2: Route with parameter**

```tsx
// routes.ts
export const USER_DETAIL = "/users/:id";

// Routing.tsx
<Route path={ROUTES.USER_DETAIL} element={<UserDetailPage />} />

// Usage
<Link to={`/users/${userId}`}>View User</Link>
```

**Example 3: Nested routes**

```tsx
<Route path="/settings" element={<SettingsLayout />}>
  <Route index element={<GeneralSettings />} />
  <Route path="profile" element={<ProfileSettings />} />
  <Route path="security" element={<SecuritySettings />} />
</Route>
```

## Best Practices

- Use Helmet for page metadata and titles
- Use lazy loading with Suspense for code splitting
- Define route constants in centralized file
- Separate page components from feature components
- Use ProtectedRoute for authenticated pages
- Use DefaultLayout for consistent page structure
- Add fallback UI for Suspense
- Use route parameters for dynamic routes
- Keep routing configuration in Routing.tsx
- Use Link component for navigation

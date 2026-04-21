---
name: neuron-auth
description: Integrate @neuron/auth package for authentication in Neuron applications. Use when setting up authentication, protecting routes, or implementing login/logout flows. Covers Redux integration, ProtectedRoute configuration, component usage patterns, and permission-based access control. Provides complete auth flow with automatic redirects and session management.
---

# Authentication Integration

Integrate @neuron/auth package for authentication in Neuron applications.

## Process

1. **Install dependencies** - Add @neuron/auth package
2. **Configure Redux** - Add authenticationSlice to store
3. **Setup configuration** - Add auth config to app config
4. **Create pages** - Dashboard and post-logout pages
5. **Setup routing** - Configure ProtectedRoute
6. **Use in components** - Access auth state with useAuth hook

## Installation

```bash
npm install @neuron/auth
```

## Redux Integration

**rootReducer.ts:**

```typescript
import { combineReducers } from "@reduxjs/toolkit";
import { authenticationSlice } from "@neuron/auth";
import { insertReducer } from "@neuron/core";

export const rootReducer = combineReducers({
  ...insertReducer(authenticationSlice),
});
```

## Configuration

**appConfig.ts:**

```typescript
import { IAppConfigFw } from "@neuron/core";
import { IAuthConfigFw } from "@neuron/auth";

export interface AppConfigNeuron extends IAppConfigFw<IAppConfigBe>, IAuthConfigFw {}
```

**config-internal.json:**

```json
{
  "security": {
    "applicationId": "Your App Name",
    "clientId": "your-client-id",
    "apiISBaseUri": "https://api-is.dev.example.com",
    "authorizeEndpoint": "/oauth2/authorize",
    "logoutEndpoint": "/oidc/logout",
    "tokenEndpoint": "/oauth2/token",
    "useMockTokens": false
  }
}
```

## Protected Route Setup

**Routing.tsx:**

```typescript
import { createBrowserRouter, createRoutesFromElements, Route } from "react-router-dom";
import { ProtectedRoute } from "@neuron/auth";
import DefaultLayout from "app/elements/common/layout/components/DefaultLayout";
import DashboardPage from "app/pages/DashboardPage";
import PostLogoutPage from "app/pages/PostLogoutPage";

const Routing = () => {
  return createBrowserRouter(
    createRoutesFromElements(
      <>
        {/* PUBLIC ROUTES */}
        <Route path="/logged-out" element={<PostLogoutPage />} />

        {/* PROTECTED ROUTES */}
        <Route element={<ProtectedRoute redirectToAuthServer={true} />}>
          <Route element={<DefaultLayout />}>
            <Route index element={<DashboardPage />} />
            <Route path="/dashboard" element={<DashboardPage />} />
          </Route>
        </Route>
      </>
    )
  );
};
```

## Component Usage

**Dashboard.tsx:**

```typescript
import { useAuth } from "@neuron/auth";
import { Button } from "@neuron/ui";

export const Dashboard = () => {
  const { user, logout, isAuthenticated, isAuthenticating } = useAuth();

  if (isAuthenticating) return <div>Loading...</div>;
  if (!isAuthenticated) return <div>Not authenticated</div>;

  return (
    <div>
      <h1>Welcome, {user?.firstName} {user?.lastName}!</h1>
      <p>Email: {user?.email}</p>
      <p>Department: {user?.department}</p>
      <Button onClick={logout}>Logout</Button>
    </div>
  );
};
```

**PostLogout.tsx:**

```typescript
import { useAuth } from "@neuron/auth";
import { useNavigate } from "react-router-dom";
import { useEffect } from "react";

export const PostLogout = () => {
  const navigate = useNavigate();
  const { isAuthenticated } = useAuth();

  useEffect(() => {
    if (isAuthenticated) navigate("/");
  }, [isAuthenticated, navigate]);

  return (
    <div>
      <h1>You have been logged out</h1>
      <button onClick={() => navigate("/")}>Return to Home</button>
    </div>
  );
};
```

## Layout with Auth

**DefaultLayout.tsx:**

```typescript
import { useAuth } from "@neuron/auth";
import { Outlet } from "react-router-dom";
import { Layout, LayoutProvider, AutomaticLogOutAlert as AutomaticLogOutAlertUI } from "@neuron/ui";
import { AutomaticLogOutAlert } from "@neuron/auth";

export const DefaultLayout = () => {
  const { user } = useAuth();

  return (
    <LayoutProvider>
      <AutomaticLogOutAlert
        AutomaticLogOutAlertElement={AutomaticLogOutAlertUI}
        serverSessionTerminationTime={{ hours: 3, minutes: 1 }}
      />
      <Layout>
        <Outlet />
      </Layout>
    </LayoutProvider>
  );
};
```

## Permission-Based Access

**Route-level permissions:**

```typescript
<Route
  element={
    <ProtectedRoute
      redirectToAuthServer={true}
      requiredPermissions={["CAN_VIEW_ADMIN"]}
    />
  }
>
  <Route path="/admin" element={<AdminPage />} />
</Route>
```

**Component-level permissions:**

```typescript
import { AccessControl } from "@neuron/auth";

const AdminPanel = () => (
  <AccessControl
    fullAccess={["CAN_EDIT_USERS"]}
    readonlyAccess={["CAN_VIEW_USERS"]}
  >
    {({ hasFullAccess, hasReadonlyAccess }) => {
      if (hasFullAccess) return <UserEditor />;
      if (hasReadonlyAccess) return <UserViewer />;
      return <div>No access</div>;
    }}
  </AccessControl>
);
```

## API Integration

**Automatic headers with axiosNeuron:**

```typescript
import { axiosNeuron } from "@neuron/core";

// Authorization header added automatically
const response = await axiosNeuron.get("/api/protected");
```

**Manual headers:**

```typescript
import { prepareAuthorizationHeader } from "@neuron/auth";

const authHeader = prepareAuthorizationHeader(); // "Bearer <token>"

fetch("/api/protected", {
  headers: { Authorization: authHeader },
});
```

## Examples

**Example 1: Basic authentication check**

```typescript
import { useAuth } from "@neuron/auth";

const UserProfile = () => {
  const { user, isAuthenticated } = useAuth();

  if (!isAuthenticated) return <div>Not logged in</div>;

  return <div>Hello {user?.firstName}!</div>;
};
```

**Example 2: Manual login/logout**

```typescript
import { useAuth } from "@neuron/auth";

const AuthControls = () => {
  const { login, logout, isAuthenticated } = useAuth();

  return isAuthenticated ? (
    <button onClick={logout}>Logout</button>
  ) : (
    <button onClick={login}>Login</button>
  );
};
```

**Example 3: Protected route with permissions**

```typescript
<Route
  element={
    <ProtectedRoute
      redirectToAuthServer={true}
      requiredPermissions={["ADMIN"]}
    />
  }
>
  <Route path="/admin" element={<AdminPage />} />
</Route>
```

## Common Issues

**Issue: Navigation not working after login**

- Use `<Route index element={<DashboardPage />} />` pattern
- ProtectedRoute handles post-login redirects automatically

**Issue: Logout shows popup**

- Expected behavior due to ADFS limitations
- Close browser tab to fully log out

**Issue: Certificate errors**

- Open auth server URL in new tab
- Accept security warning
- Reload application

**Issue: Endless redirects**

- Verify redirect URIs match in config and auth server
- Clear browser cookies and local storage

---
name: neuron-api
description: Create API integration packages using RTK Query in Neuron monorepo. Use when setting up new API clients, integrating OpenAPI specs, or creating API packages. Covers package structure, TypeScript configuration, Redux integration, and component usage patterns. Generates type-safe API clients with automatic caching and state management.
---

# API Integration with RTK Query

Create type-safe API integration packages using RTK Query in Neuron monorepo.

## Process

1. **Create package structure** - Set up API package with proper folder organization
2. **Configure TypeScript** - Add path aliases to tsconfig.base.json
3. **Generate API code** - Use OpenAPI spec to generate endpoints and types
4. **Integrate with Redux** - Add reducer and middleware to store
5. **Use in components** - Access hooks through API object

## Package Structure

```
packages/api/[api-name]/
├── package.json
├── project.json
├── scripts/
│   ├── package.json           # {"type": "module"}
│   └── generate-api.js        # Code generation script
├── src/
│   ├── index.ts
│   ├── lib/api/
│   │   ├── index.ts
│   │   ├── [apiName]Api.ts                  # Main API (customizable)
│   │   ├── [apiName]GetBaseQueryConfig.ts   # Base query config
│   │   ├── _gen_[apiName]EmptyApi.ts        # Generated base
│   │   ├── _gen_[apiName]ApiGen.ts          # Generated endpoints
│   │   └── _gen_[apiName]Model/             # Generated types
│   └── resources/
│       └── [apiName]Api.json  # OpenAPI spec
└── tests/
    └── api.test.ts
```

## Configuration

**package.json:**

```json
{
  "name": "@api/[api-name]",
  "scripts": {
    "codegen": "node ./scripts/generate-api.js"
  },
  "dependencies": {
    "@reduxjs/toolkit": "^2.0.0",
    "react": "^18.0.0",
    "react-redux": "^9.0.0"
  }
}
```

**tsconfig.base.json (workspace root):**

```json
{
  "compilerOptions": {
    "paths": {
      "@api/[api-name]": ["packages/api/[api-name]/src/index.ts"]
    }
  }
}
```

## API Exports

**src/lib/api/index.ts:**

```typescript
export * from "./[apiName]Api";
export type * from "./_gen_[apiName]Model";
```

**src/index.ts:**

```typescript
export * from "./lib/api/index";
```

## Custom Endpoints

**src/lib/api/[apiName]Api.ts:**

```typescript
import { __[apiName]ApiGen } from "./_gen_[apiName]ApiGen";
import type { GetUserApiResponse, GetUserApiArg } from "./_gen_[apiName]Model";

export const [apiName]Api = __[apiName]ApiGen.injectEndpoints({
  endpoints: (build) => ({
    getUserWithMeta: build.query<
      { response: GetUserApiResponse; meta: Record<string, unknown> },
      GetUserApiArg
    >({
      query: (queryArg) => ({
        url: `/users/${queryArg.userId}`,
        headers: { "X-Correlation-Id": queryArg["X-Correlation-Id"] },
      }),
      transformResponse: (data, meta) => ({ response: data, meta: meta || {} }),
    }),
  }),
  overrideExisting: true,
});
```

## Redux Integration

**rootReducer.ts:**

```typescript
import { combineReducers } from "redux";
import { insertReducer } from "@neuron/core";
import { [apiName]Api } from "@api/[api-name]";

export const rootReducer = combineReducers({
  ...insertReducer([apiName]Api),
});
```

**configureStore.ts:**

```typescript
import { configureStore } from "@reduxjs/toolkit";
import { [apiName]Api } from "@api/[api-name]";

export const store = configureStore({
  reducer: rootReducer,
  middleware: (getDefaultMiddleware) =>
    getDefaultMiddleware()
      .concat([apiName]Api.middleware),
});
```

## Component Usage

**CRITICAL: Always import API object, never individual hooks**

```typescript
// ✅ CORRECT
import { [apiName]Api } from "@api/[api-name]";

const { data, isLoading } = [apiName]Api.useGetUsersQuery({...});
const [createUser] = [apiName]Api.useCreateUserMutation();

// ❌ WRONG
import { useGetUsersQuery } from "@api/[api-name]";
```

## Testing

**tests/api.test.ts:**

```typescript
import { [apiName]Api } from '../src';

describe('[API Name] API', () => {
  it('should be defined', () => {
    expect([apiName]Api).toBeDefined();
    expect([apiName]Api.reducerPath).toBe('[apiName]Api');
  });

  it('should have generated endpoints', () => {
    expect([apiName]Api.endpoints).toBeDefined();
    expect(Object.keys([apiName]Api.endpoints).length).toBeGreaterThan(0);
  });

  it('should have reducer and middleware', () => {
    expect([apiName]Api.reducer).toBeDefined();
    expect([apiName]Api.middleware).toBeDefined();
  });
});
```

## Examples

**Example 1: Basic query**

```typescript
import { [apiName]Api } from "@api/[api-name]";

const UsersList = () => {
  const { data, isLoading } = [apiName]Api.useGetUsersQuery({});

  if (isLoading) return <div>Loading...</div>;
  return <div>{data?.map(user => <div key={user.id}>{user.name}</div>)}</div>;
};
```

**Example 2: Mutation**

```typescript
import { [apiName]Api } from "@api/[api-name]";

const CreateUser = () => {
  const [createUser, { isLoading }] = [apiName]Api.useCreateUserMutation();

  const handleSubmit = async (userData) => {
    await createUser(userData).unwrap();
  };

  return <button onClick={handleSubmit}>Create</button>;
};
```

**Example 3: Custom endpoint**

```typescript
// In [apiName]Api.ts
export const [apiName]Api = __[apiName]ApiGen.injectEndpoints({
  endpoints: (build) => ({
    getActiveUsers: build.query({
      query: () => '/users?status=active',
    }),
  }),
});
```

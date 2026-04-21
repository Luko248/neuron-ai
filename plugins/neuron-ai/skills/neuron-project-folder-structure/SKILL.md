---
name: neuron-project-folder-structure
description: Understand folder structure patterns for Neuron React projects. Use when creating files, organizing components, or structuring features. Covers application structure (elements, features, pages, routing, store), package organization, and file placement rules. Provides clear guidelines for where to place different types of files.
---

# Project Folder Structure

Understand folder structure patterns for Neuron React projects.

## Root Structure

```
project-root/
├── apps/[app-name]/          # Applications
├── packages/[package-name]/  # Shared packages
├── docs/                     # Documentation
└── scripts/                  # Build scripts
```

## Application Structure

```
apps/[app-name]/src/
├── app/
│   ├── elements/             # Reusable UI elements
│   │   ├── common/           # Shared components
│   │   └── specific/         # Feature-specific
│   ├── features/             # Feature components
│   ├── pages/                # Page components
│   ├── routing/              # Routing config
│   ├── store/                # Redux store
│   └── App.tsx               # Root component
├── assets/                   # Static assets
├── config/                   # App configuration
├── constants/                # Constants
├── i18n/                     # Translations
└── index.tsx                 # Entry point
```

## File Placement Rules

**Pages (apps/[app-name]/src/app/pages/):**

- Page wrapper components
- Use Helmet for metadata
- Lazy load feature components
- Example: `DashboardPage.tsx`, `UserProfilePage.tsx`

**Features (apps/[app-name]/src/app/features/):**

- Main feature implementation
- Business logic
- Feature-specific components
- Example: `dashboard/Dashboard.tsx`, `userProfile/UserProfile.tsx`

**Elements (apps/[app-name]/src/app/elements/):**

- Reusable UI components
- Common: Shared across app
- Specific: Feature-specific reusable components
- Example: `common/Header.tsx`, `specific/UserCard.tsx`

**Routing (apps/[app-name]/src/app/routing/):**

- Route configuration
- Protected routes
- Example: `Routing.tsx`, `ProtectedRoutes.tsx`

**Store (apps/[app-name]/src/app/store/):**

- Redux store configuration
- Root reducer
- Middleware setup
- Example: `configureStore.ts`, `rootReducer.ts`

**Constants (apps/[app-name]/src/constants/):**

- Route constants
- Configuration constants
- Example: `routes.ts`, `config.ts`

**i18n (apps/[app-name]/src/i18n/):**

- Translation files
- i18n configuration
- Example: `cs/common/actions.json`, `index.ts`

## Package Structure

```
packages/[category]/[package-name]/
├── src/
│   ├── index.ts              # Package entry
│   └── lib/                  # Implementation
├── package.json
├── project.json
└── tsconfig.json
```

**Categories:**

- `api/` - API packages
- `domains/` - Domain packages
- `neuron/` - Framework packages

## Examples

**Example 1: Create new page**

```
1. Create page: src/app/pages/SettingsPage.tsx
2. Create feature: src/app/features/settings/Settings.tsx
3. Add route: src/constants/routes.ts
4. Configure routing: src/app/routing/Routing.tsx
```

**Example 2: Create reusable component**

```
1. Common component: src/app/elements/common/Button.tsx
2. Feature-specific: src/app/elements/specific/UserAvatar.tsx
```

**Example 3: Add translations**

```
1. Common: src/i18n/cs/common/actions.json
2. Feature: src/i18n/cs/features/settings.json
```

## Best Practices

- Place pages in `pages/` directory
- Place features in `features/` directory
- Place reusable components in `elements/common/`
- Place feature-specific components in `elements/specific/`
- Place routing in `routing/` directory
- Place store configuration in `store/` directory
- Place constants in `constants/` directory
- Place translations in `i18n/` directory
- Use lazy loading for feature components
- Keep consistent folder structure across projects

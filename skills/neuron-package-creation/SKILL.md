---
name: neuron-package-creation
description: Create packages in Nx workspace with proper structure and configuration. Use when creating new packages under packages/ directory. Covers package structure, configuration files (package.json, project.json, tsconfig), Nx integration, and TypeScript path aliases. Provides step-by-step package creation workflow.
---

# Package Creation in Nx Workspace

Create packages in Nx workspace with proper structure and configuration.

## Process

1. **Create directory structure** - Set up package folders
2. **Configure package.json** - Define package metadata and dependencies
3. **Configure project.json** - Set up Nx project configuration
4. **Configure TypeScript** - Set up tsconfig files
5. **Add to workspace** - Update tsconfig.base.json with path alias
6. **Create source files** - Add index.ts and lib/ structure

## Package Structure

```
packages/[category]/[package-name]/
├── package.json
├── project.json
├── tsconfig.json
├── tsconfig.lib.json
├── tsconfig.spec.json
├── jest.config.ts
├── README.md
└── src/
    ├── index.ts
    └── lib/
        └── [implementation files]
```

**Categories:**

- `api/` - API packages
- `domains/` - Domain-specific packages

## Configuration Files

**package.json:**

```json
{
  "name": "@category/package-name",
  "version": "1.0.0",
  "type": "commonjs",
  "main": "./src/index.ts",
  "types": "./src/index.ts"
}
```

**project.json:**

```json
{
  "name": "category-package-name",
  "$schema": "../../node_modules/nx/schemas/project-schema.json",
  "sourceRoot": "packages/category/package-name/src",
  "projectType": "library",
  "tags": ["scope:category", "type:library"],
  "targets": {
    "build": {
      "executor": "@nx/js:tsc",
      "options": {
        "outputPath": "dist/packages/category/package-name",
        "main": "packages/category/package-name/src/index.ts",
        "tsConfig": "packages/category/package-name/tsconfig.lib.json"
      }
    }
  }
}
```

**tsconfig.json:**

```json
{
  "extends": "../../../tsconfig.base.json",
  "references": [{ "path": "./tsconfig.lib.json" }, { "path": "./tsconfig.spec.json" }]
}
```

## Workspace Integration

**Add to tsconfig.base.json (workspace root):**

```json
{
  "compilerOptions": {
    "paths": {
      "@category/package-name": ["packages/category/package-name/src/index.ts"]
    }
  }
}
```

## Source Files

**src/index.ts:**

```typescript
export * from "./lib";
```

**src/lib/index.ts:**

```typescript
export const exampleFunction = () => {
  return "Hello from package";
};
```

## Examples

**Example 1: Create API package**

```bash
# Directory structure
packages/api/my-api/
├── package.json
├── project.json
├── src/
│   ├── index.ts
│   └── lib/
│       └── api/
│           └── myApi.ts
```

**Example 2: Create domain package**

```bash
# Directory structure
packages/domains/user-management/
├── package.json
├── project.json
├── src/
│   ├── index.ts
│   └── lib/
│       ├── types.ts
│       └── utils.ts
```

**Example 3: Import from package**

```typescript
// After adding to tsconfig.base.json
import { exampleFunction } from "@category/package-name";

const result = exampleFunction();
```

## Best Practices

- Use consistent naming: `@category/package-name`
- Add package to tsconfig.base.json paths
- Use proper Nx project configuration
- Include all required config files
- Set up proper TypeScript configuration
- Use src/index.ts as entry point
- Organize code in src/lib/ directory
- Add README.md with documentation
- Configure jest for testing
- Use proper tags in project.json

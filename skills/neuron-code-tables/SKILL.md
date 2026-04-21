---
name: neuron-code-tables
description: Create and manage static code tables in Neuron applications. Use when implementing dropdowns, status indicators, or predefined value lists. Covers code table structure, SCT enum definition, data creation, registration, and UI component integration. Provides type-safe centralized data management for consistent application data.
---

# Static Code Tables

Create and manage static code tables for predefined values in Neuron applications.

## Process

1. **Define SCT enum** - Add code table type to enum
2. **Create data file** - Define code table items
3. **Register code table** - Add to registration helper
4. **Use in components** - Integrate with UI components

## Code Table Structure

```typescript
interface ICodeTableItem {
  id: string;
  code: string;
  valid: boolean;
  name: string;
  description: string;
  type: "string" | "number" | "boolean";
  group?: string;
  order?: number;
  active: boolean;
}
```

## File Structure

```
src/codeTables/
├── index.ts
├── static/
│   ├── index.ts
│   ├── staticCodeTableConstants.ts  # SCT enum
│   ├── registerStaticCodeTables.ts
│   └── data/
│       ├── yesNo.codeTable.ts
│       └── index.ts
└── registerCodeTables.ts
```

## Implementation

**1. Define SCT enum (staticCodeTableConstants.ts):**

```typescript
export enum SCT {
  YES_NO = "yesNo",
  STATUS = "status",
  COUNTRY = "country",
}
```

**2. Create data file (yesNo.codeTable.ts):**

```typescript
import { createStaticCodeTable, ICodeTableItem } from "@neuron/core";
import { SCT } from "../staticCodeTableConstants";

const yesNoData: ICodeTableItem[] = [
  { id: "yes", code: "yes", valid: true, name: "Yes", description: "Yes", type: "string", active: true },
  { id: "no", code: "no", valid: true, name: "No", description: "No", type: "string", active: true },
];

export const ctYesNo = createStaticCodeTable(SCT.YES_NO, yesNoData);
```

**3. Export from data/index.ts:**

```typescript
export { ctYesNo } from "./yesNo.codeTable";
```

**4. Register (registerStaticCodeTables.ts):**

```typescript
import { registerCodeTable } from "@neuron/core";
import { ctYesNo } from "./data";

export const registerStaticCodeTables = () => {
  registerCodeTable(ctYesNo);
};
```

**5. Main registration (registerCodeTables.ts):**

```typescript
import { registerStaticCodeTables } from "./static";

export const registerCodeTables = () => {
  registerStaticCodeTables();
};
```

**6. Initialize in AppRoot:**

```typescript
import { useEffect, useRef } from "react";
import { registerCodeTables } from "./codeTables";

export const AppRoot = () => {
  const isInitialized = useRef(false);

  useEffect(() => {
    if (!isInitialized.current) {
      isInitialized.current = true;
      registerCodeTables();
    }
  }, []);

  return <App />;
};
```

## Component Usage

**RadioSet:**

```tsx
import { RadioSet } from "@neuron/ui";
import { SCT } from "./codeTables/static/staticCodeTableConstants";

<RadioSet name="yesNoInput" codeTableName={SCT.YES_NO} />;
```

**Dropdown:**

```tsx
import { Dropdown } from "@neuron/ui";
import { SCT } from "./codeTables/static/staticCodeTableConstants";

<Dropdown name="status" codeTableName={SCT.STATUS} />;
```

## Examples

**Example 1: Simple Yes/No code table**

```typescript
const yesNoData: ICodeTableItem[] = [
  { id: "yes", code: "yes", valid: true, name: "Yes", description: "Yes", type: "string", active: true },
  { id: "no", code: "no", valid: true, name: "No", description: "No", type: "string", active: true },
];

export const ctYesNo = createStaticCodeTable(SCT.YES_NO, yesNoData);
```

**Example 2: Status code table with ordering**

```typescript
const statusData: ICodeTableItem[] = [
  {
    id: "active",
    code: "ACTIVE",
    valid: true,
    name: "Active",
    description: "Active status",
    type: "string",
    active: true,
    order: 1,
  },
  {
    id: "inactive",
    code: "INACTIVE",
    valid: true,
    name: "Inactive",
    description: "Inactive status",
    type: "string",
    active: true,
    order: 2,
  },
  {
    id: "pending",
    code: "PENDING",
    valid: true,
    name: "Pending",
    description: "Pending status",
    type: "string",
    active: true,
    order: 3,
  },
];

export const ctStatus = createStaticCodeTable(SCT.STATUS, statusData);
```

**Example 3: Using in RadioSet component**

```tsx
import { RadioSet } from "@neuron/ui";
import { SCT } from "./codeTables/static/staticCodeTableConstants";

const MyForm = () => (
  <form>
    <RadioSet name="answer" codeTableName={SCT.YES_NO} />
  </form>
);
```

## Best Practices

- Use UPPER_SNAKE_CASE for SCT enum keys
- Mark codes as inactive instead of deleting
- Include order property for consistent sorting
- Always set active status
- Avoid hardcoding values in components

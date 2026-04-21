---
agent: agent
---

# Tree Component Guidelines

## For the AI Assistant

Use this guide to integrate and configure the Neuron `Tree` component in enterprise React applications. Follow the documented patterns for hierarchical data presentation, selection management, filtering, and lazy loading to maintain consistency with the Neuron UI framework.

**Tree Component (v1.0.0)**: Enterprise hierarchical navigator built on PrimeReact `Tree` with Neuron-specific templates, managed selection helpers, and accessibility-focused defaults.

## 🚨 CRITICAL Implementation Rules

### ALWAYS DO

- **Use `TreeItemNode` interfaces** for all data passed into the component, even when converting from PrimeReact `TreeNode`.
- **Localize UI strings** using `t("neuron.ui.tree.filterPlaceholder")` and translation keys for empty states.
- **Control selection state** via `selectionKeys` and `onSelectionChange` or wrap handlers with `createManagedHandlers()`.
- **Provide stable `key` values** by setting `key` on each node and `dataKey` when consuming selection events.
- **Handle loading and empty states** using `loading`, `filter`, and localized empty messages.
- **Leverage `notificationBadge` and `icon` props** for richer nodes instead of custom markup.
- **Stop propagation inside action handlers** when triggering node actions to prevent unwanted selection changes.

### NEVER DO

- **Do not mutate PrimeReact event payloads**—clone or derive new state before updates.
- **Do not bypass `TreeItemContent`** with raw HTML; use `nodeTemplate` only for advanced scenarios.
- **Do not render uncontrolled state** for selection or expanded keys in production flows.
- **Do not hardcode text** directly inside nodes—localize labels.
- **Do not mix asynchronous loading** with static nodes without handling `loading` states and expanded key updates.

## 📋 Core Architecture

### Component Structure

```tsx
import { Tree, TreeItemNode } from "@neuron/ui";

interface IBusinessUnit extends TreeItemNode {
  key: string;
  label: string;
  url?: string;
  children?: IBusinessUnit[];
}

const businessUnits: IBusinessUnit[] = [
  {
    key: "hq",
    label: t("navigation.businessUnits.hq"),
    icon: { iconDef: baseIcons.buildingRegular },
    children: [
      {
        key: "hq-claims",
        label: t("navigation.businessUnits.claims"),
        action: () => openSection("claims"),
        notificationBadge: { count: 3, variant: "info" },
      },
    ],
  },
];

const BusinessTree = () => {
  return (
    <Tree
      items={businessUnits}
      selectionMode="single"
      selectionKeys={selectedKey}
      onSelectionChange={handleSelectionChange}
      expandedKeys={expandedKeys}
      onToggle={handleToggle}
      filter={true}
      filterBy="label"
      testId="business-tree"
    />
  );
};
```

### Data Normalisation

- Use `toTreeItems()` to convert PrimeReact `TreeNode[]` into Neuron `TreeItemNode[]` when migrating existing data structures.
- `Tree` internally normalises incoming `items`, so providing mixed node formats is supported but TypeScript safety improves with `TreeItemNode` typing.

## Step 1: Basic Tree Usage

```tsx
import { Tree, TreeItemNode } from "@neuron/ui";

const documents: TreeItemNode[] = [
  {
    key: "reports",
    label: t("documents.reports"),
    children: [
      {
        key: "reports-q1",
        label: t("documents.reportsQ1"),
        url: "/reports/q1",
      },
    ],
  },
];

<Tree items={documents} />;
```

- **Localization**: Always wrap labels and empty messages with `t()`.
- **Keys**: Provide deterministic `key` values to ensure selection and toggle state consistency.

## Step 2: Node Content & Actions

`TreeItemContent` provides rich node rendering without custom templates.

```tsx
const underwritingNodes: TreeItemNode[] = [
  {
    key: "worklist",
    label: t("underwriting.worklist"),
    icon: { iconDef: baseIcons.folderRegular },
    notificationBadge: { count: 5, variant: "warning" },
    children: [
      {
        key: "worklist-high",
        label: t("underwriting.highRisk"),
        action: () => navigateTo("high-risk"),
      },
    ],
  },
];

const UnderwritingTree = () => (
  <Tree
    items={underwritingNodes}
    onSelect={(event) => console.info(event.node)}
    onUnselect={(event) => console.info(event.node)}
  />
);
```

### Action vs URL

- **URL**: Provide `url` for anchor navigation. The label becomes a silent `Link`.
- **Action**: Provide `action` for callable behaviour. `TreeItemContent` wraps it in a button-like `Link` that stops propagation.
- **Disabled**: Set `disabled: true` to prevent interaction while preserving visual presence.

## Step 3: Selection Modes

### Single Selection

```tsx
const [selectedKey, setSelectedKey] = useState<TreeProps["selectionKeys"]>(null);

<Tree
  items={nodes}
  selectionMode="single"
  selectionKeys={selectedKey}
  onSelectionChange={(event) => setSelectedKey(event.value)}
/>;
```

### Multiple Selection

```tsx
const [selectedKeys, setSelectedKeys] = useState<Record<string, boolean>>({});

<Tree
  items={nodes}
  selectionMode="multiple"
  selectionKeys={selectedKeys}
  onSelectionChange={(event) => setSelectedKeys(event.value)}
/>;
```

### Checkbox Selection with Tristate

```tsx
const [selectedKeys, setSelectedKeys] = useState<Record<string, { checked: boolean; partialChecked: boolean }>>({});

<Tree
  items={nodes}
  selectionMode="checkbox"
  selectionKeys={selectedKeys}
  onSelectionChange={(event) => setSelectedKeys(event.value)}
/>;
```

- Always initialise state with `{}` or `null` to avoid uncontrolled state warnings.
- PrimeReact expects immutable updates; clone the payload before mutating.

## Step 4: Managed Selection & Toggle State

Use `createManagedHandlers()` to sync internal state and forward events.

```tsx
import { createManagedHandlers, reduceSelectionKeys, reduceToggleKeys } from "@neuron/ui";

const TreeWithManagedState = ({ items }: { items: TreeItemNode[] }) => {
  const [state, setState] = useState({
    selectionKeys: null as TreeProps["selectionKeys"],
    expandedKeys: null as TreeProps["expandedKeys"],
  });

  const managedHandlers = useMemo(() => {
    return createManagedHandlers(
      {
        onSelectionChange: (event) => console.info("Selection", event.value),
        onToggle: (event) => console.info("Toggled", event.value),
      },
      (nextState) => setState((prev) => ({ ...prev, ...nextState })),
    );
  }, []);

  return (
    <Tree
      items={items}
      selectionMode="checkbox"
      selectionKeys={reduceSelectionKeys(state.selectionKeys)}
      expandedKeys={reduceToggleKeys(state.expandedKeys)}
      {...managedHandlers}
    />
  );
};
```

- `reduceSelectionKeys()` and `reduceToggleKeys()` normalise `undefined` to `null` for PrimeReact compatibility.
- Provide `customEvent` callback to `createManagedHandlers()` when analytics or logging is required.

## Step 5: Filtering & Empty States

```tsx
<Tree
  items={nodes}
  filter={true}
  filterBy="label"
  filterPlaceholder={t("neuron.ui.tree.filterPlaceholder")}
  emptyMessage={t("neuron.ui.tree.empty")}
/>
```

- `Tree` injects a magnifying glass icon automatically when `filter` is enabled.
- Use `resolveTreeEmptyMessageTitle()` when composing your own `emptyMessage` wrappers.
- Provide `filterPlaceholder` to override the default translation when needed.

## Step 6: Disabled Trees & Nodes

```tsx
<Tree items={nodes} disabled={true} loading={isLoading} />
```

- `disabled` flag prevents interaction and sets `data-disabled` for styling.
- Individual nodes support `disabled: true` to disable specific branches while keeping the tree interactive.

## Step 7: Large Dataset Optimisation

- Enable `scrollHeight` and wrap the tree with constrained containers for large datasets.
- Use lazy loading with `onExpand` to fetch child nodes on demand.
- Memoise `items` with `useMemo` when derived from API payloads to avoid unnecessary re-renders.

```tsx
const handleLazyLoad = async (event: TreeEventNodeExpandParams) => {
  if (!event.node || event.node.children) {
    return;
  }

  const children = await fetchChildren(event.node.key);
  setNodes((prev) => assignChildren(prev, event.node.key, children));
};

<Tree items={nodes} onExpand={handleLazyLoad} />;
```

## Step 8: Custom Node Templates

Only override `nodeTemplate` for advanced scenarios.

```tsx
const renderCustomNode: TreeProps["nodeTemplate"] = (node) => {
  return (
    <TreeItemContent
      item={{
        label: node.label,
        icon: { iconDef: baseIcons.fileRegular },
        notificationBadge: node.badge,
      }}
      fallbackLabel={node.label}
    />
  );
};

<Tree items={nodes} nodeTemplate={renderCustomNode} />;
```

- Call `TreeItemContent` inside custom templates to keep consistent styling.
- Ensure you pass through `fallbackLabel` to maintain accessibility for nodes without labels.

## Step 9: Prop Reference

| Prop                | Type                                   | Default                                 | Description                               |
| ------------------- | -------------------------------------- | --------------------------------------- | ----------------------------------------- |
| `items`             | `TreeItemNode[] \| TreeNode[]`         | –                                       | Hierarchical dataset rendered in the tree |
| `selectionMode`     | `"single" \| "multiple" \| "checkbox"` | `undefined`                             | Enables selection behaviour               |
| `selectionKeys`     | `TreeProps["selectionKeys"]`           | `null`                                  | Controlled selection state                |
| `expandedKeys`      | `TreeProps["expandedKeys"]`            | `null`                                  | Controlled expanded nodes state           |
| `nodeTemplate`      | `(node: TreeNode) => ReactNode`        | `TreeItemContent`                       | Custom renderer per node                  |
| `filter`            | `boolean`                              | `false`                                 | Enables search filter input               |
| `filterPlaceholder` | `string`                               | `t("neuron.ui.tree.filterPlaceholder")` | Placeholder for filter input              |
| `emptyMessage`      | `ReactNode`                            | Localized ContentMessage                | Custom empty state content                |
| `disabled`          | `boolean`                              | `false`                                 | Disables the entire tree                  |
| `testId`            | `string`                               | –                                       | Sets `data-testid` attribute              |

## Step 10: Common Mistakes

- **Missing keys**: Forgetting to set unique `key` values breaks selection and toggle state.
- **Mutating selection payloads**: Always clone the `event.value` before modifications.
- **Hardcoded text**: Every label must use localization.
- **Ignoring disabled state**: Use `disabled` on the tree or nodes during async operations to prevent duplicate actions.
- **Overriding nodeTemplate unnecessarily**: Use built-in `TreeItemContent` unless bespoke layout is required.

## ✅ Summary Checklist

- **Define typed `TreeItemNode` data** with localized labels.
- **Control selection and expanded state** using managed handlers or controlled props.
- **Leverage built-in rendering** (`TreeItemContent`) for icons, badges, actions, and links.
- **Enable filtering and empty states** with localized placeholders and messages.
- **Optimise large datasets** with lazy loading and memoised data structures.
- **Document and test** selection flows, ensuring PrimeReact events propagate correctly.

Follow these guidelines to integrate the Tree component consistently across Neuron applications while maintaining enterprise-grade UX, accessibility, and performance.

## Sync Metadata

- **Component Version:** v1.0.3
- **Component Source:** `packages/neuron/ui/src/lib/data/tree/Tree.tsx`
- **Guideline Command:** `/neuron-ui-tree`
- **Related Skill:** `neuron-ui-data`

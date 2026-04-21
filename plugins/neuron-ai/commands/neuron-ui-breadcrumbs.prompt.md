---
agent: agent
---

# AI-Assisted Neuron BreadCrumbs Component Integration Guide

## For the AI Assistant

Your task is to integrate and configure the Neuron BreadCrumbs component in a React application. This guide provides comprehensive instructions for implementing BreadCrumbs navigation, which serves as hierarchical navigation that must be placed in the Layout top content area across all Neuron applications.

---

## ⚠️ CRITICAL MANDATORY RULES - READ FIRST

**🚨 THESE RULES ARE NON-NEGOTIABLE:**

1. **BreadCrumbs MUST be placed in Layout top content ONLY** - Never anywhere else
2. **BreadCrumbs provide hierarchical navigation** - Show user's current location in app structure
3. **By default BreadCrumbs are NOT sticky** - Optional sticky behavior available
4. **NO exceptions to placement rule** - This applies to every page that needs navigation context

**If you violate these rules, the implementation is WRONG and must be corrected.**

---

## Sync Metadata

- **Component Version:** v4.3.2
- **Component Source:** `packages/neuron/ui/src/lib/menus/breadCrumbs/BreadCrumbs.tsx`
- **Guideline Command:** `/neuron-ui-breadcrumbs`
- **Related Skill:** `neuron-ui-menu`

## Introduction

The BreadCrumbs component is a specialized navigation component designed to display hierarchical navigation paths that show users their current location within the application structure. It serves as a critical interface element for providing navigation context and quick access to parent pages within Neuron applications.

### 🚨 CRITICAL: BreadCrumbs Placement Rule

**⚠️ MANDATORY NEURON PATTERN: BreadCrumbs MUST ALWAYS be placed in Layout top content, NEVER anywhere else. This is not optional.**

**ALWAYS in Layout top content:**

- **Navigation breadcrumbs** showing current page hierarchy
- **Path indicators** for multi-level navigation
- **Context navigation** for complex application structures

**NEVER place BreadCrumbs:**

- ❌ In main content area
- ❌ In other layout sections (bottom, left, right)
- ❌ Inline with page content
- ❌ Inside modals or sidesheets
- ❌ Floating or positioned elsewhere

**This rule applies to ALL pages that need hierarchical navigation context.**

### What is the BreadCrumbs Component?

The BreadCrumbs component provides standardized navigation breadcrumbs with support for:

- **Hierarchical navigation** - Clear path from root to current page
- **Responsive behavior** - Automatic scrolling and overflow handling
- **Sticky functionality** - Optional always-visible navigation (disabled by default)
- **Icon support** - Optional icons for breadcrumb items
- **Root path configuration** - Customizable home/root navigation
- **Active state indication** - Visual indication of current page
- **Layout integration** - Designed specifically for Layout top content placement

### Key Features

- **Mandatory Layout Placement**: Must be placed in Layout top content - no other placement is correct
- **Hierarchical Navigation**: Shows clear path from root to current page
- **Responsive Design**: Automatic horizontal scrolling for long breadcrumb paths
- **Optional Sticky Behavior**: Can be made always visible (disabled by default)
- **Icon Support**: Optional icons for breadcrumb items using baseIcons
- **Root Path Configuration**: Customizable home icon and root navigation
- **Active State**: Visual indication of current page in breadcrumb path
- **TypeScript Support**: Full type safety with comprehensive prop definitions

**Storybook Reference:** You can refer to the Storybook documentation for visual examples and interactive demos of the BreadCrumbs component.

## Step 1: Basic BreadCrumbs Implementation

### 1.1 Import the BreadCrumbs Component

```tsx
import { BreadCrumbs } from "@neuron/ui";
```

### 1.2 CRITICAL: Layout Placement Requirement

**🚨 MANDATORY RULES - NO EXCEPTIONS:**

1. **BreadCrumbs MUST be placed in Layout top content ONLY** - Any other placement is WRONG
2. **BreadCrumbs provide navigation context** - Show hierarchical path to current page
3. **This applies to EVERY page** that needs navigation context

**⚠️ VIOLATION OF THESE RULES = INCORRECT IMPLEMENTATION**

The BreadCrumbs component is specifically designed for the Layout top content area and serves as the navigation context for the entire application.

```tsx
import { Layout, LayoutProvider, LayoutPortal, BreadCrumbs } from "@neuron/ui";

const App = () => {
  return (
    <LayoutProvider>
      <Layout>
        {/* Main content */}
        <div className="g-col-12">
          <h1>Current Page Content</h1>
        </div>
      </Layout>

      {/* CORRECT: BreadCrumbs in Layout top content */}
      <LayoutPortal position="top">
        <BreadCrumbs
          items={[
            {
              labelText: "Products",
              href: "/products",
            },
            {
              labelText: "Insurance",
              href: "/products/insurance",
            },
            {
              labelText: "Car Insurance",
              href: "/products/insurance/car",
            },
          ]}
          rootPath="/"
        />
      </LayoutPortal>
    </LayoutProvider>
  );
};
```

### 1.3 Alternative: Direct Layout Props

You can also place BreadCrumbs using direct Layout props:

```tsx
import { Layout, LayoutProvider, BreadCrumbs } from "@neuron/ui";

const App = () => {
  const breadcrumbsComponent = (
    <BreadCrumbs
      items={[
        {
          labelText: "User Management",
          href: "/users",
        },
        {
          labelText: "Create User",
          href: "/users/create",
        },
      ]}
      rootPath="/"
    />
  );

  return (
    <LayoutProvider>
      <Layout topContent={breadcrumbsComponent}>
        {/* Main content */}
        <div className="g-col-12">
          <h1>Create New User</h1>
        </div>
      </Layout>
    </LayoutProvider>
  );
};
```

## ⚠️ CRITICAL: What NOT to Do - Common Violations

### ❌ WRONG: BreadCrumbs in Main Content

```tsx
// ❌ THIS IS COMPLETELY WRONG - NEVER DO THIS
const WrongBreadCrumbsPlacement = () => {
  return (
    <div className="g-col-12">
      {/* ❌ WRONG: BreadCrumbs in main content - NEVER DO THIS */}
      <BreadCrumbs
        items={[
          { labelText: "Products", href: "/products" },
          { labelText: "Details", href: "/products/details" },
        ]}
        rootPath="/"
      />

      <h1>Page Content</h1>
      <p>Main content here...</p>
    </div>
  );
};
```

### ❌ WRONG: BreadCrumbs in Other Layout Sections

```tsx
// ❌ THIS IS COMPLETELY WRONG - NEVER DO THIS
const WrongLayoutPlacement = () => {
  return (
    <>
      {/* ❌ WRONG: BreadCrumbs in bottom content - NEVER DO THIS */}
      <LayoutPortal position="bottom">
        <BreadCrumbs items={breadcrumbItems} rootPath="/" />
      </LayoutPortal>

      {/* ❌ WRONG: BreadCrumbs in left sidebar - NEVER DO THIS */}
      <LayoutPortal position="left">
        <BreadCrumbs items={breadcrumbItems} rootPath="/" />
      </LayoutPortal>
    </>
  );
};
```

### ✅ CORRECT: BreadCrumbs in Layout Top Content

```tsx
// ✅ THIS IS THE ONLY CORRECT WAY
const CorrectBreadCrumbsImplementation = () => {
  return (
    <>
      {/* Content area - NO BREADCRUMBS */}
      <div className="g-col-12">
        <h1>Current Page</h1>
        <p>Page content without navigation breadcrumbs...</p>
      </div>

      {/* BREADCRUMBS IN LAYOUT TOP CONTENT - THIS IS MANDATORY */}
      <LayoutPortal position="top">
        <BreadCrumbs
          items={[
            { labelText: "Dashboard", href: "/dashboard" },
            { labelText: "Reports", href: "/dashboard/reports" },
            { labelText: "Monthly Report", href: "/dashboard/reports/monthly" },
          ]}
          rootPath="/"
        />
      </LayoutPortal>
    </>
  );
};
```

**🚨 REMEMBER: If you see BreadCrumbs anywhere other than Layout top content, the implementation is WRONG and must be fixed.**

## Step 2: BreadCrumb Items Configuration

### 2.1 Basic Item Structure

BreadCrumb items consist of label text and navigation href:

```tsx
import { BreadCrumbs } from "@neuron/ui";

const BasicBreadCrumbs = () => {
  const breadcrumbItems = [
    {
      labelText: "Products",
      href: "/products",
    },
    {
      labelText: "Insurance",
      href: "/products/insurance",
    },
    {
      labelText: "Car Insurance",
      href: "/products/insurance/car",
    },
    {
      labelText: "Policy Details",
      href: "/products/insurance/car/policy",
    },
  ];

  return (
    <LayoutPortal position="top">
      <BreadCrumbs items={breadcrumbItems} rootPath="/" />
    </LayoutPortal>
  );
};
```

### 2.2 BreadCrumbs with Icons

Add icons to breadcrumb items for better visual context:

```tsx
import { BreadCrumbs, baseIcons } from "@neuron/ui";

const IconBreadCrumbs = () => {
  const breadcrumbItems = [
    {
      labelText: "Documents",
      href: "/documents",
      iconDef: baseIcons.folderSolid,
    },
    {
      labelText: "Contracts",
      href: "/documents/contracts",
      iconDef: baseIcons.fileContractSolid,
    },
    {
      labelText: "Insurance Contract",
      href: "/documents/contracts/insurance",
      iconDef: baseIcons.fileTextSolid,
    },
  ];

  return (
    <LayoutPortal position="top">
      <BreadCrumbs items={breadcrumbItems} rootPath="/" />
    </LayoutPortal>
  );
};
```

### 2.3 Custom Root Path Configuration

Configure the root path for the home icon:

```tsx
import { BreadCrumbs } from "@neuron/ui";

const CustomRootBreadCrumbs = () => {
  return (
    <LayoutPortal position="top">
      <BreadCrumbs
        items={[
          { labelText: "Admin Panel", href: "/admin" },
          { labelText: "User Management", href: "/admin/users" },
          { labelText: "Create User", href: "/admin/users/create" },
        ]}
        rootPath="/admin" // Custom root path
      />
    </LayoutPortal>
  );
};
```

## Step 3: Sticky Behavior Configuration

### 3.1 Default Behavior (Non-Sticky)

**By default, BreadCrumbs are NOT sticky** and scroll with the page content:

```tsx
import { BreadCrumbs } from "@neuron/ui";

const DefaultBreadCrumbs = () => {
  return (
    <LayoutPortal position="top">
      <BreadCrumbs
        items={breadcrumbItems}
        rootPath="/"
        // isSticky is false by default - breadcrumbs scroll with content
      />
    </LayoutPortal>
  );
};
```

### 3.2 Sticky Behavior (Always Visible)

Enable sticky behavior to keep breadcrumbs always visible:

```tsx
import { BreadCrumbs } from "@neuron/ui";

const StickyBreadCrumbs = () => {
  return (
    <LayoutPortal position="top">
      <BreadCrumbs
        items={[
          { labelText: "Dashboard", href: "/dashboard" },
          { labelText: "Analytics", href: "/dashboard/analytics" },
          { labelText: "Reports", href: "/dashboard/analytics/reports" },
        ]}
        rootPath="/"
        isSticky={true} // Makes breadcrumbs always visible
      />
    </LayoutPortal>
  );
};
```

### 3.3 When to Use Sticky Breadcrumbs

**Use sticky breadcrumbs when:**

- Users need constant navigation context
- Pages have long scrollable content
- Navigation hierarchy is complex
- Users frequently navigate between levels

**Use default (non-sticky) breadcrumbs when:**

- Pages have short content
- Navigation context is simple
- Screen real estate is limited
- Breadcrumbs would interfere with content

```tsx
// Good: Sticky breadcrumbs for long content pages
const LongContentPage = () => {
  return (
    <>
      <div className="g-col-12">
        <h1>Long Article</h1>
        <div className="long-content">{/* Very long scrollable content */}</div>
      </div>

      <LayoutPortal position="top">
        <BreadCrumbs
          items={breadcrumbItems}
          rootPath="/"
          isSticky={true} // Keep navigation always visible
        />
      </LayoutPortal>
    </>
  );
};

// Good: Non-sticky breadcrumbs for short content
const ShortContentPage = () => {
  return (
    <>
      <div className="g-col-12">
        <h1>Simple Form</h1>
        <form>{/* Short form content */}</form>
      </div>

      <LayoutPortal position="top">
        <BreadCrumbs
          items={breadcrumbItems}
          rootPath="/"
          // isSticky defaults to false - scrolls with content
        />
      </LayoutPortal>
    </>
  );
};
```

## Step 4: Real Application Example

### 4.1 Complete Application Layout with BreadCrumbs

Based on the real application example from the starter app:

```tsx
import { useAuth } from "@neuron/auth";
import { Outlet } from "react-router-dom";
import { BreadCrumbs, Footer, Helmet, Layout, LayoutProvider, PRProvider } from "@neuron/ui";
import { AppMenu } from "./components/AppMenu";

export const AppLayout = () => {
  const { user } = useAuth();
  const menu = user && <AppMenu />;

  return (
    <Helmet>
      <PRProvider>
        <LayoutProvider>
          <Layout
            leftSide={menu}
            topContent={
              <BreadCrumbs
                items={[
                  {
                    labelText: "User Creation",
                    href: "/user-creation",
                  },
                  {
                    labelText: "Action Bar",
                    href: "/action-bar",
                  },
                  {
                    labelText: "Sticky Header",
                    href: "/sticky-header",
                  },
                  {
                    labelText: "Sidebar",
                    href: "/side-bar",
                  },
                ]}
                rootPath="/"
                isSticky={true} // Always visible in this app
              />
            }
            footerContent={<Footer />}
          >
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

### 4.2 Dynamic BreadCrumbs Based on Route

Generate breadcrumbs dynamically based on current route:

```tsx
import { BreadCrumbs } from "@neuron/ui";
import { useLocation } from "react-router-dom";
import { useMemo } from "react";

const DynamicBreadCrumbs = () => {
  const location = useLocation();

  const breadcrumbItems = useMemo(() => {
    const pathSegments = location.pathname.split("/").filter(Boolean);

    return pathSegments.map((segment, index) => {
      const href = "/" + pathSegments.slice(0, index + 1).join("/");
      const labelText = segment.charAt(0).toUpperCase() + segment.slice(1);

      return {
        labelText,
        href,
      };
    });
  }, [location.pathname]);

  return (
    <LayoutPortal position="top">
      <BreadCrumbs items={breadcrumbItems} rootPath="/" isSticky={true} />
    </LayoutPortal>
  );
};
```

### 4.3 Page-Specific BreadCrumbs

Different pages can have different breadcrumb configurations:

```tsx
// User management page
const UserManagementBreadCrumbs = () => (
  <LayoutPortal position="top">
    <BreadCrumbs
      items={[
        { labelText: "Administration", href: "/admin" },
        { labelText: "User Management", href: "/admin/users" },
      ]}
      rootPath="/"
      isSticky={true}
    />
  </LayoutPortal>
);

// Product details page
const ProductDetailsBreadCrumbs = () => (
  <LayoutPortal position="top">
    <BreadCrumbs
      items={[
        { labelText: "Products", href: "/products", iconDef: baseIcons.boxSolid },
        { labelText: "Electronics", href: "/products/electronics", iconDef: baseIcons.laptopSolid },
        { labelText: "Laptops", href: "/products/electronics/laptops" },
        { labelText: "MacBook Pro", href: "/products/electronics/laptops/macbook-pro" },
      ]}
      rootPath="/"
    />
  </LayoutPortal>
);
```

## Step 5: BreadCrumbs Props Reference

### 5.1 Core BreadCrumbs Props

| Prop      | Type               | Default | Description                                        |
| --------- | ------------------ | ------- | -------------------------------------------------- |
| items     | `BreadCrumbItem[]` | -       | **Required**. Array of breadcrumb navigation items |
| rootPath  | `string`           | `"/"`   | Path for the root/home icon navigation             |
| isSticky  | `boolean`          | `false` | Enable sticky behavior (always visible on scroll)  |
| className | `string`           | -       | Additional CSS classes                             |
| testId    | `string`           | -       | Custom test ID for the component                   |

### 5.2 BreadCrumbItem Properties

| Property    | Type                           | Description                                          |
| ----------- | ------------------------------ | ---------------------------------------------------- |
| labelText   | `string`                       | **Required**. Display text for the breadcrumb item   |
| href        | `string`                       | **Required**. Navigation URL for the breadcrumb item |
| iconDef     | `IconDefinition \| TBaseIcons` | Optional icon for the breadcrumb item                |
| isCollapsed | `boolean`                      | Optional flag for collapsed state (internal use)     |

### 5.3 Sticky Behavior Options

| Value             | Behavior                  | Use Case                       |
| ----------------- | ------------------------- | ------------------------------ |
| `false` (default) | Scrolls with page content | Short pages, simple navigation |
| `true`            | Always visible at top     | Long pages, complex navigation |

### 5.4 Layout Placement Rules

| Placement             | Correct          | Description                            |
| --------------------- | ---------------- | -------------------------------------- |
| Layout top content    | ✅ **MANDATORY** | Only correct placement for BreadCrumbs |
| Layout main content   | ❌ **WRONG**     | Never place in main content area       |
| Layout bottom content | ❌ **WRONG**     | Never place in bottom content area     |
| Layout left/right     | ❌ **WRONG**     | Never place in sidebar areas           |

## Step 6: Best Practices

### 6.1 CRITICAL: BreadCrumbs Placement Rule

**🚨 ALWAYS place BreadCrumbs in Layout top content:**

```tsx
// ✅ CORRECT: BreadCrumbs in Layout top content
<LayoutPortal position="top">
  <BreadCrumbs items={breadcrumbItems} rootPath="/" />
</LayoutPortal>

// ❌ WRONG: BreadCrumbs anywhere else
<div className="g-col-12">
  <BreadCrumbs items={breadcrumbItems} rootPath="/" />  {/* WRONG */}
</div>
```

### 6.2 Breadcrumb Hierarchy Guidelines

**Create logical navigation hierarchies:**

- Start with broad categories
- Progress to specific items
- Keep hierarchy depth reasonable (3-5 levels max)
- Use meaningful, descriptive labels

```tsx
// Good: Logical hierarchy
const goodHierarchy = [
  { labelText: "E-commerce", href: "/ecommerce" },
  { labelText: "Products", href: "/ecommerce/products" },
  { labelText: "Electronics", href: "/ecommerce/products/electronics" },
  { labelText: "Laptops", href: "/ecommerce/products/electronics/laptops" },
];

// Avoid: Too deep or illogical hierarchy
const badHierarchy = [
  { labelText: "Home", href: "/" },
  { labelText: "Category", href: "/cat" },
  { labelText: "Sub", href: "/cat/sub" },
  { labelText: "Sub-sub", href: "/cat/sub/subsub" },
  { labelText: "Sub-sub-sub", href: "/cat/sub/subsub/subsubsub" },
  { labelText: "Final", href: "/cat/sub/subsub/subsubsub/final" }, // Too deep
];
```

### 6.3 Icon Usage Guidelines

**Use icons meaningfully:**

- Add icons to enhance navigation context
- Use consistent icon themes
- Don't overuse icons - not every item needs one

```tsx
// Good: Meaningful icons
const meaningfulIcons = [
  { labelText: "Dashboard", href: "/dashboard", iconDef: baseIcons.chartLineSolid },
  { labelText: "Users", href: "/users", iconDef: baseIcons.usersSolid },
  { labelText: "John Doe", href: "/users/john-doe" }, // No icon for specific user
];

// Avoid: Random or meaningless icons
const randomIcons = [
  { labelText: "Settings", href: "/settings", iconDef: baseIcons.carSolid }, // Wrong icon
  { labelText: "Profile", href: "/profile", iconDef: baseIcons.pizzaSolid }, // Meaningless
];
```

### 6.4 Sticky Behavior Best Practices

**Choose sticky behavior appropriately:**

```tsx
// Good: Sticky for long content pages
const LongFormPage = () => (
  <>
    <div className="g-col-12">
      <form className="long-form">{/* Many form fields requiring scrolling */}</form>
    </div>

    <LayoutPortal position="top">
      <BreadCrumbs
        items={breadcrumbItems}
        rootPath="/"
        isSticky={true} // Keep navigation visible
      />
    </LayoutPortal>
  </>
);

// Good: Non-sticky for short content
const SimpleDialog = () => (
  <>
    <div className="g-col-12">
      <div className="simple-content">
        <p>Short content that doesn't require scrolling</p>
      </div>
    </div>

    <LayoutPortal position="top">
      <BreadCrumbs
        items={breadcrumbItems}
        rootPath="/"
        // isSticky defaults to false
      />
    </LayoutPortal>
  </>
);
```

## Step 7: Common Mistakes to Avoid

### 7.1 Don't Place BreadCrumbs Outside Layout Top Content

```tsx
// ❌ WRONG: BreadCrumbs in main content
<div className="g-col-12">
  <BreadCrumbs items={breadcrumbItems} rootPath="/" />  {/* WRONG */}
  <h1>Page Content</h1>
</div>

// ❌ WRONG: BreadCrumbs in bottom content
<LayoutPortal position="bottom">
  <BreadCrumbs items={breadcrumbItems} rootPath="/" />  {/* WRONG */}
</LayoutPortal>

// ✅ RIGHT: BreadCrumbs in top content
<LayoutPortal position="top">
  <BreadCrumbs items={breadcrumbItems} rootPath="/" />  {/* CORRECT */}
</LayoutPortal>
```

### 7.2 Don't Create Illogical Hierarchies

```tsx
// ❌ WRONG: Illogical breadcrumb hierarchy
const wrongHierarchy = [
  { labelText: "Settings", href: "/settings" },
  { labelText: "Products", href: "/products" }, // Doesn't follow from Settings
  { labelText: "User Profile", href: "/profile" }, // Doesn't follow from Products
];

// ✅ RIGHT: Logical breadcrumb hierarchy
const rightHierarchy = [
  { labelText: "Administration", href: "/admin" },
  { labelText: "User Management", href: "/admin/users" },
  { labelText: "User Profile", href: "/admin/users/profile" },
];
```

### 7.3 Don't Overuse Sticky Behavior

```tsx
// ❌ WRONG: Sticky breadcrumbs on short content
const ShortPage = () => (
  <>
    <div className="g-col-12">
      <p>Just a short paragraph of content.</p>
    </div>

    <LayoutPortal position="top">
      <BreadCrumbs
        items={breadcrumbItems}
        rootPath="/"
        isSticky={true} // Unnecessary for short content
      />
    </LayoutPortal>
  </>
);

// ✅ RIGHT: Non-sticky for short content
const ShortPageCorrect = () => (
  <>
    <div className="g-col-12">
      <p>Just a short paragraph of content.</p>
    </div>

    <LayoutPortal position="top">
      <BreadCrumbs
        items={breadcrumbItems}
        rootPath="/"
        // isSticky defaults to false - appropriate for short content
      />
    </LayoutPortal>
  </>
);
```

### 7.4 Don't Forget Root Path Configuration

```tsx
// ❌ WRONG: Incorrect root path for sub-applications
<BreadCrumbs
  items={adminBreadcrumbs}
  rootPath="/"  // Wrong: should be "/admin" for admin section
/>

// ✅ RIGHT: Correct root path for context
<BreadCrumbs
  items={adminBreadcrumbs}
  rootPath="/admin"  // Correct: matches admin section context
/>
```

## Summary

The Neuron BreadCrumbs component provides a comprehensive, responsive, and consistent system for hierarchical navigation. Key points to remember:

1. **MANDATORY Layout placement** - Must be in Layout top content, no exceptions
2. **Hierarchical navigation** - Show clear path from root to current page
3. **Default non-sticky behavior** - Scrolls with content unless explicitly made sticky
4. **Optional sticky functionality** - Can be always visible for long content pages
5. **Icon support** - Optional icons for enhanced visual context
6. **Root path configuration** - Customizable home navigation
7. **Responsive design** - Automatic horizontal scrolling for long paths
8. **Integration with Layout system** - Use LayoutPortal or direct Layout props

### Key Takeaways

- **Layout top placement only** - Component designed specifically for this location
- **Navigation context provision** - Shows users where they are in app hierarchy
- **Default non-sticky behavior** - Scrolls with content for better space utilization
- **Optional sticky mode** - Always visible navigation for complex or long pages
- **Logical hierarchy creation** - Clear, meaningful navigation paths
- **Icon enhancement** - Optional visual context using baseIcons
- **Root path customization** - Flexible home navigation configuration
- **Consistent behavior** - Standardized across all Neuron applications

By following these guidelines, you'll create effective, user-friendly navigation interfaces that provide clear hierarchical context and enhance user orientation within your Neuron applications.

---

## 🚨 FINAL REMINDER: Critical Rules Checklist

Before implementing any BreadCrumbs, verify these mandatory requirements:

### ✅ BreadCrumbs Placement Checklist

- [ ] BreadCrumbs is placed in Layout top content using `LayoutPortal position="top"` or `Layout topContent` prop
- [ ] BreadCrumbs is NOT placed in main content, bottom, left, right, or any other location
- [ ] BreadCrumbs is NOT placed inline with page content

### ✅ Navigation Hierarchy Checklist

- [ ] Breadcrumb items follow logical hierarchy from broad to specific
- [ ] Each item has meaningful labelText and correct href
- [ ] Hierarchy depth is reasonable (3-5 levels maximum)
- [ ] Root path is configured appropriately for the application context

### ✅ Sticky Behavior Checklist

- [ ] Sticky behavior is disabled by default (isSticky=false)
- [ ] Sticky behavior is only enabled for long content pages that benefit from persistent navigation
- [ ] Short content pages use default non-sticky behavior

### ✅ Implementation Checklist

- [ ] All breadcrumb items have meaningful labels and correct navigation URLs
- [ ] Icons are used meaningfully and consistently (if used)
- [ ] Root path matches the application or section context
- [ ] Responsive behavior works correctly for long breadcrumb paths

**If any checklist item is not met, the implementation violates Neuron patterns and must be corrected.**

---

## ⚠️ Zero Tolerance Policy

**There are NO exceptions to these rules. Every violation must be fixed:**

- **BreadCrumbs placed outside Layout top content** = WRONG implementation
- **BreadCrumbs in main content or other layout sections** = WRONG implementation
- **Illogical or meaningless navigation hierarchy** = WRONG implementation

**These are not suggestions - they are mandatory Neuron UI patterns that ensure consistency, proper navigation context, and optimal user experience across all applications.**

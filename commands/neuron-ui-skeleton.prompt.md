---
agent: agent
---

# AI-Assisted Neuron Skeleton Component Integration Guide

## For the AI Assistant

Your task is to integrate and configure the Neuron Skeleton components in a React application. This guide provides comprehensive instructions for implementing the Skeleton component, which serves as a loading placeholder to improve perceived performance and user experience across all Neuron applications.

## Sync Metadata

- **Component Version:** v3.0.1
- **Component Source:** `packages/neuron/ui/src/lib/misc/skeleton/Skeleton.tsx`
- **Guideline Command:** `/neuron-ui-skeleton`
- **Related Skill:** `neuron-ui-misc`

## Introduction

The Skeleton system is a loading state component of the Neuron UI framework, designed to create consistent, accessible, and smooth loading experiences that maintain layout stability while content is being fetched across all Neuron applications.

### What is the Skeleton System?

The Skeleton component provides a standardized loading placeholder interface for your application with support for:

- Multiple shapes (rectangle, circle, square)
- Flexible sizing (width, height, size)
- Border radius customization
- Automatic Layout integration
- Manual implementation for custom cases
- Responsive design patterns
- Accessibility features
- Performance optimization

### Key Features

- **Built-in Layout Integration**: Automatic skeleton display in Layout components during loading states
- **Flexible Shapes**: Rectangle (default), circle, and square shapes for different content types
- **Responsive Sizing**: Support for fixed dimensions, percentages, and viewport units
- **Border Radius Control**: Customizable border radius for rounded corners
- **PrimeReact Foundation**: Built on PrimeReact Skeleton with Neuron styling
- **Accessibility**: Proper ARIA attributes and screen reader support
- **Performance**: Lightweight and optimized for smooth animations
- **TypeScript Support**: Full type safety with PrimeReact prop interfaces

**Storybook Reference:** You can refer to the Storybook documentation for visual examples and interactive demos of the Skeleton component.

### Figma Code Connect Integration

**🎨 Design-to-Code Connection Available**

## Step 1: Basic Skeleton Implementation

### 1.1 Import the Skeleton Component

```tsx
import { Skeleton } from "@neuron/ui";
```

### 1.2 Basic Usage

Here's a simple implementation of the Skeleton component:

```tsx
import { Skeleton } from "@neuron/ui";

const MyComponent = () => {
  return (
    <div className="skeleton-examples">
      {/* Basic rectangle skeleton */}
      <Skeleton />

      {/* Skeleton with custom width */}
      <Skeleton width="10rem" />

      {/* Skeleton with custom height */}
      <Skeleton height="2rem" />

      {/* Skeleton with both width and height */}
      <Skeleton width="10rem" height="4rem" />
    </div>
  );
};
```

### 1.3 Layout Integration (Automatic)

**⚠️ IMPORTANT: Layout component automatically shows skeletons during loading states**

```tsx
import { Layout } from "@neuron/ui";

const MyPage = () => {
  const [isLoading, setIsLoading] = useState(true);

  return (
    <Layout isMainContentLoading={isLoading} isTopContentLoading={isLoading} isRightSideLoading={isLoading}>
      {/* Content will be shown when isLoading is false */}
      {/* Skeletons are automatically displayed when isLoading is true */}
      <div>Your main content here</div>
    </Layout>
  );
};
```

## Step 2: Skeleton Shapes

### 2.1 Rectangle Skeletons (Default)

Rectangle skeletons are the default and most common type:

```tsx
import { Skeleton } from "@neuron/ui";

const RectangleSkeletons = () => {
  return (
    <div className="rectangle-skeletons">
      {/* Default rectangle */}
      <Skeleton className="mb-2" />

      {/* Various widths */}
      <Skeleton width="10rem" className="mb-2" />
      <Skeleton width="5rem" className="mb-2" />

      {/* Custom height */}
      <Skeleton height="2rem" className="mb-2" />

      {/* Custom width and height */}
      <Skeleton width="10rem" height="4rem" />
    </div>
  );
};
```

### 2.2 Circle Skeletons

Use circle skeletons for profile pictures, avatars, and circular content:

```tsx
import { Skeleton } from "@neuron/ui";

const CircleSkeletons = () => {
  return (
    <div className="circle-skeletons">
      {/* Small circle */}
      <Skeleton shape="circle" size="2rem" className="me-2" />

      {/* Medium circle */}
      <Skeleton shape="circle" size="3rem" className="me-2" />

      {/* Large circle */}
      <Skeleton shape="circle" size="4rem" className="me-2" />

      {/* Extra large circle */}
      <Skeleton shape="circle" size="5rem" />
    </div>
  );
};
```

### 2.3 Square Skeletons

Use square skeletons for thumbnails, icons, and square content:

```tsx
import { Skeleton } from "@neuron/ui";

const SquareSkeletons = () => {
  return (
    <div className="square-skeletons">
      {/* Small square */}
      <Skeleton size="2rem" className="me-2" />

      {/* Medium square */}
      <Skeleton size="3rem" className="me-2" />

      {/* Large square */}
      <Skeleton size="4rem" className="me-2" />

      {/* Extra large square */}
      <Skeleton size="5rem" />
    </div>
  );
};
```

## Step 3: Rounded Skeletons

### 3.1 Custom Border Radius

Add rounded corners to skeletons for modern designs:

```tsx
import { Skeleton } from "@neuron/ui";

const RoundedSkeletons = () => {
  return (
    <div className="rounded-skeletons">
      {/* Slightly rounded */}
      <Skeleton className="mb-2" borderRadius="4px" />

      {/* Medium rounded */}
      <Skeleton width="10rem" className="mb-2" borderRadius="8px" />

      {/* Highly rounded */}
      <Skeleton width="5rem" borderRadius="16px" className="mb-2" />

      {/* Custom dimensions with rounding */}
      <Skeleton width="10rem" height="4rem" borderRadius="12px" />
    </div>
  );
};
```

### 3.2 Pill-Shaped Skeletons

Create pill-shaped skeletons for tags, badges, and buttons:

```tsx
import { Skeleton } from "@neuron/ui";

const PillSkeletons = () => {
  return (
    <div className="pill-skeletons">
      {/* Small pill */}
      <Skeleton width="4rem" height="1.5rem" borderRadius="0.75rem" className="me-2" />

      {/* Medium pill */}
      <Skeleton width="6rem" height="2rem" borderRadius="1rem" className="me-2" />

      {/* Large pill */}
      <Skeleton width="8rem" height="2.5rem" borderRadius="1.25rem" />
    </div>
  );
};
```

## Step 4: Content-Specific Skeletons

### 4.1 Text Content Skeletons

Create skeletons that mimic text content:

```tsx
import { Skeleton } from "@neuron/ui";

const TextSkeletons = () => {
  return (
    <div className="text-skeletons">
      {/* Title skeleton */}
      <Skeleton width="60%" height="2rem" className="mb-3" />

      {/* Paragraph skeletons */}
      <Skeleton width="100%" height="1rem" className="mb-2" />
      <Skeleton width="95%" height="1rem" className="mb-2" />
      <Skeleton width="80%" height="1rem" className="mb-2" />
      <Skeleton width="70%" height="1rem" className="mb-3" />

      {/* Short text skeleton */}
      <Skeleton width="40%" height="1rem" />
    </div>
  );
};
```

### 4.2 Card Content Skeletons

Create skeletons for card-based layouts:

```tsx
import { Skeleton } from "@neuron/ui";

const CardSkeletons = () => {
  return (
    <div className="card-skeletons">
      {/* Card header */}
      <div className="d-flex align-items-center mb-3">
        <Skeleton shape="circle" size="3rem" className="me-3" />
        <div className="flex-grow-1">
          <Skeleton width="60%" height="1.2rem" className="mb-1" />
          <Skeleton width="40%" height="1rem" />
        </div>
      </div>

      {/* Card image */}
      <Skeleton width="100%" height="12rem" borderRadius="8px" className="mb-3" />

      {/* Card content */}
      <Skeleton width="100%" height="1rem" className="mb-2" />
      <Skeleton width="85%" height="1rem" className="mb-2" />
      <Skeleton width="60%" height="1rem" />
    </div>
  );
};
```

### 4.3 List Item Skeletons

Create skeletons for list-based content:

```tsx
import { Skeleton } from "@neuron/ui";

const ListSkeletons = () => {
  const renderListItem = (index: number) => (
    <div key={index} className="d-flex align-items-center mb-3">
      <Skeleton shape="circle" size="2.5rem" className="me-3" />
      <div className="flex-grow-1">
        <Skeleton width="70%" height="1rem" className="mb-1" />
        <Skeleton width="50%" height="0.8rem" />
      </div>
      <Skeleton width="4rem" height="1.5rem" borderRadius="0.75rem" />
    </div>
  );

  return <div className="list-skeletons">{Array.from({ length: 5 }, (_, index) => renderListItem(index))}</div>;
};
```

## Step 5: Loading State Patterns

### 5.1 Conditional Skeleton Display

Show skeletons while content is loading:

```tsx
import { Skeleton } from "@neuron/ui";
import { useState, useEffect } from "react";

const ConditionalSkeletons = () => {
  const [isLoading, setIsLoading] = useState(true);
  const [data, setData] = useState(null);

  useEffect(() => {
    // Simulate data loading
    setTimeout(() => {
      setData({ title: "Loaded Content", description: "This is the actual content" });
      setIsLoading(false);
    }, 2000);
  }, []);

  if (isLoading) {
    return (
      <div className="loading-skeletons">
        <Skeleton width="60%" height="2rem" className="mb-3" />
        <Skeleton width="100%" height="1rem" className="mb-2" />
        <Skeleton width="80%" height="1rem" />
      </div>
    );
  }

  return (
    <div className="loaded-content">
      <h2>{data.title}</h2>
      <p>{data.description}</p>
    </div>
  );
};
```

### 5.2 Progressive Loading Skeletons

Show different skeleton states as content loads progressively:

```tsx
import { Skeleton } from "@neuron/ui";
import { useState, useEffect } from "react";

const ProgressiveSkeletons = () => {
  const [loadingStates, setLoadingStates] = useState({
    header: true,
    content: true,
    sidebar: true,
  });

  useEffect(() => {
    // Simulate progressive loading
    setTimeout(() => setLoadingStates((prev) => ({ ...prev, header: false })), 1000);
    setTimeout(() => setLoadingStates((prev) => ({ ...prev, content: false })), 2000);
    setTimeout(() => setLoadingStates((prev) => ({ ...prev, sidebar: false })), 3000);
  }, []);

  return (
    <div className="progressive-skeletons">
      {/* Header */}
      {loadingStates.header ? (
        <Skeleton width="50%" height="2rem" className="mb-4" />
      ) : (
        <h1 className="mb-4">Page Title</h1>
      )}

      {/* Content */}
      {loadingStates.content ? (
        <div className="mb-4">
          <Skeleton width="100%" height="1rem" className="mb-2" />
          <Skeleton width="90%" height="1rem" className="mb-2" />
          <Skeleton width="75%" height="1rem" />
        </div>
      ) : (
        <p className="mb-4">Main content has loaded successfully.</p>
      )}

      {/* Sidebar */}
      {loadingStates.sidebar ? (
        <Skeleton width="200px" height="300px" borderRadius="8px" />
      ) : (
        <div className="sidebar">Sidebar content loaded</div>
      )}
    </div>
  );
};
```

### 5.3 Error State with Skeletons

Handle error states gracefully with skeleton fallbacks:

```tsx
import { Skeleton } from "@neuron/ui";
import { useState, useEffect } from "react";

const ErrorStateSkeletons = () => {
  const [state, setState] = useState({ loading: true, error: false, data: null });

  useEffect(() => {
    // Simulate API call with potential error
    setTimeout(() => {
      const hasError = Math.random() > 0.7; // 30% chance of error

      if (hasError) {
        setState({ loading: false, error: true, data: null });
      } else {
        setState({ loading: false, error: false, data: { title: "Success!" } });
      }
    }, 2000);
  }, []);

  if (state.loading) {
    return (
      <div className="error-state-skeletons">
        <Skeleton width="60%" height="2rem" className="mb-3" />
        <Skeleton width="100%" height="8rem" borderRadius="8px" />
      </div>
    );
  }

  if (state.error) {
    return (
      <div className="error-message">
        <p>Failed to load content. Please try again.</p>
      </div>
    );
  }

  return (
    <div className="success-content">
      <h2>{state.data.title}</h2>
      <div>Content loaded successfully!</div>
    </div>
  );
};
```

## Step 6: Advanced Skeleton Patterns

### 6.1 Table Skeletons

Create skeletons for table-based content:

```tsx
import { Skeleton } from "@neuron/ui";

const TableSkeletons = () => {
  const renderTableRow = (index: number) => (
    <tr key={index}>
      <td>
        <Skeleton width="80%" height="1rem" />
      </td>
      <td>
        <Skeleton width="60%" height="1rem" />
      </td>
      <td>
        <Skeleton width="40%" height="1rem" />
      </td>
      <td>
        <Skeleton width="5rem" height="1.5rem" borderRadius="0.75rem" />
      </td>
    </tr>
  );

  return (
    <table className="table">
      <thead>
        <tr>
          <th>
            <Skeleton width="60%" height="1rem" />
          </th>
          <th>
            <Skeleton width="50%" height="1rem" />
          </th>
          <th>
            <Skeleton width="40%" height="1rem" />
          </th>
          <th>
            <Skeleton width="30%" height="1rem" />
          </th>
        </tr>
      </thead>
      <tbody>{Array.from({ length: 5 }, (_, index) => renderTableRow(index))}</tbody>
    </table>
  );
};
```

### 6.2 Form Skeletons

Create skeletons for form layouts:

```tsx
import { Skeleton } from "@neuron/ui";

const FormSkeletons = () => {
  return (
    <div className="form-skeletons">
      {/* Form title */}
      <Skeleton width="40%" height="2rem" className="mb-4" />

      {/* Form fields */}
      <div className="mb-3">
        <Skeleton width="20%" height="1rem" className="mb-2" />
        <Skeleton width="100%" height="2.5rem" borderRadius="4px" />
      </div>

      <div className="mb-3">
        <Skeleton width="25%" height="1rem" className="mb-2" />
        <Skeleton width="100%" height="2.5rem" borderRadius="4px" />
      </div>

      <div className="mb-3">
        <Skeleton width="30%" height="1rem" className="mb-2" />
        <Skeleton width="100%" height="6rem" borderRadius="4px" />
      </div>

      {/* Form buttons */}
      <div className="d-flex gap-2">
        <Skeleton width="6rem" height="2.5rem" borderRadius="4px" />
        <Skeleton width="5rem" height="2.5rem" borderRadius="4px" />
      </div>
    </div>
  );
};
```

### 6.3 Dashboard Skeletons

Create skeletons for dashboard layouts:

```tsx
import { Skeleton } from "@neuron/ui";

const DashboardSkeletons = () => {
  return (
    <div className="dashboard-skeletons">
      {/* Dashboard header */}
      <div className="d-flex justify-content-between align-items-center mb-4">
        <Skeleton width="30%" height="2rem" />
        <Skeleton width="8rem" height="2rem" borderRadius="4px" />
      </div>

      {/* Stats cards */}
      <div className="row g-3 mb-4">
        {Array.from({ length: 4 }, (_, index) => (
          <div key={index} className="col-md-3">
            <div className="card p-3">
              <Skeleton width="60%" height="1rem" className="mb-2" />
              <Skeleton width="40%" height="2rem" className="mb-1" />
              <Skeleton width="80%" height="0.8rem" />
            </div>
          </div>
        ))}
      </div>

      {/* Chart area */}
      <div className="row g-3">
        <div className="col-md-8">
          <Skeleton width="100%" height="20rem" borderRadius="8px" />
        </div>
        <div className="col-md-4">
          <Skeleton width="100%" height="20rem" borderRadius="8px" />
        </div>
      </div>
    </div>
  );
};
```

## Step 7: Skeleton Props Reference

### 7.1 Core Skeleton Props (from PrimeReact)

| Prop         | Type                      | Default       | Description                       |
| ------------ | ------------------------- | ------------- | --------------------------------- |
| shape        | `"rectangle" \| "circle"` | `"rectangle"` | Shape of the skeleton             |
| size         | `string`                  | -             | Size for square/circle shapes     |
| width        | `string`                  | `"100%"`      | Width of the skeleton             |
| height       | `string`                  | `"1rem"`      | Height of the skeleton            |
| borderRadius | `string`                  | -             | Border radius for rounded corners |
| className    | `string`                  | -             | Additional CSS classes            |

### 7.2 Layout Integration Props

| Prop                 | Type      | Default | Description                         |
| -------------------- | --------- | ------- | ----------------------------------- |
| isMainContentLoading | `boolean` | `false` | Show skeleton in main content area  |
| isTopContentLoading  | `boolean` | `false` | Show skeleton in top content area   |
| isRightSideLoading   | `boolean` | `false` | Show skeleton in right sidebar area |

### 7.3 Common Size Values

**Fixed Sizes:**

- `"1rem"`, `"2rem"`, `"3rem"` - Relative units
- `"16px"`, `"32px"`, `"48px"` - Pixel values
- `"2em"`, `"3em"` - Em units

**Responsive Sizes:**

- `"100%"`, `"50%"`, `"25%"` - Percentage values
- `"10vw"`, `"20vh"` - Viewport units
- `"10svb"`, `"25svb"` - Small viewport units

## Step 8: Best Practices

### 8.1 When to Use Skeletons

**Use Skeletons for:**

- Content loading states
- API data fetching
- Image loading
- Progressive content loading
- Layout preservation during loading

```tsx
{
  /* Good: Loading state for content */
}
{
  isLoading ? <Skeleton width="100%" height="2rem" /> : <h2>{data.title}</h2>;
}
```

**Don't use Skeletons for:**

- Very short loading times (< 200ms)
- Error states (use error messages)
- Empty states (use empty state components)

### 8.2 Layout Integration Best Practices

**⚠️ IMPORTANT: Prefer Layout integration over manual skeletons**

```tsx
{
  /* ✅ CORRECT: Use Layout integration */
}
<Layout isMainContentLoading={isLoading}>
  <YourContent />
</Layout>;

{
  /* ⚠️ MANUAL: Only when Layout integration isn't sufficient */
}
{
  isLoading ? <Skeleton /> : <YourContent />;
}
```

### 8.3 Skeleton Sizing Guidelines

- **Match content dimensions** as closely as possible
- **Use consistent spacing** with your actual content
- **Consider responsive behavior** across screen sizes

```tsx
{
  /* Good: Matches actual content size */
}
{
  isLoading ? <Skeleton width="60%" height="2rem" className="mb-3" /> : <h2 className="mb-3">{title}</h2>;
}
```

### 8.4 Animation and Performance

- **Use default animations** provided by PrimeReact
- **Avoid complex custom animations** that impact performance
- **Limit the number of skeletons** on screen simultaneously

```tsx
{
  /* Good: Simple, performant skeleton */
}
<Skeleton width="100%" height="1rem" className="mb-2" />;

{
  /* Avoid: Too many complex skeletons */
}
{
  Array.from({ length: 100 }, (_, i) => <Skeleton key={i} borderRadius="50px" className="complex-animation" />);
}
```

### 8.5 Accessibility Considerations

- **Skeletons are automatically accessible** through PrimeReact
- **Provide loading announcements** for screen readers when needed
- **Ensure proper focus management** during loading transitions

```tsx
{
  /* Good: Accessible loading pattern */
}
<div role="status" aria-live="polite">
  {isLoading ? (
    <>
      <span className="sr-only">Loading content...</span>
      <Skeleton width="100%" height="2rem" />
    </>
  ) : (
    <div>{content}</div>
  )}
</div>;
```

## Step 9: Common Mistakes to Avoid

### 9.1 Don't Ignore Layout Integration

```tsx
{
  /* ❌ INCORRECT: Manual skeleton when Layout integration available */
}
<Layout>{isLoading ? <Skeleton /> : <Content />}</Layout>;

{
  /* ✅ CORRECT: Use Layout integration */
}
<Layout isMainContentLoading={isLoading}>
  <Content />
</Layout>;
```

### 9.2 Don't Mismatch Skeleton and Content Sizes

```tsx
{
  /* ❌ INCORRECT: Size mismatch */
}
{
  isLoading ? <Skeleton width="50%" height="1rem" /> : <h1 className="display-1">Large Title</h1>;
}

{
  /* ✅ CORRECT: Matching sizes */
}
{
  isLoading ? <Skeleton width="60%" height="3rem" /> : <h1 className="display-1">Large Title</h1>;
}
```

### 9.3 Don't Overuse Skeletons

```tsx
{
  /* ❌ INCORRECT: Too many skeletons */
}
<div>
  {Array.from({ length: 50 }, (_, i) => (
    <Skeleton key={i} className="mb-1" />
  ))}
</div>;

{
  /* ✅ CORRECT: Reasonable number of skeletons */
}
<div>
  {Array.from({ length: 5 }, (_, i) => (
    <Skeleton key={i} className="mb-2" />
  ))}
</div>;
```

### 9.4 Don't Forget Loading States

```tsx
{
  /* ❌ INCORRECT: No loading feedback */
}
const MyComponent = () => {
  const [data, setData] = useState(null);

  return <div>{data?.title}</div>; // Shows nothing while loading
};

{
  /* ✅ CORRECT: Proper loading state */
}
const MyComponent = () => {
  const [data, setData] = useState(null);
  const [isLoading, setIsLoading] = useState(true);

  return <div>{isLoading ? <Skeleton width="60%" height="2rem" /> : <div>{data?.title}</div>}</div>;
};
```

## Key Takeaways

The Neuron Skeleton component system provides a comprehensive, accessible, and performant foundation for loading states. Key points to remember:

1. **Layout integration first** - Use Layout component loading props when possible
2. **Manual implementation** - Use Skeleton component for custom loading scenarios
3. **Match content dimensions** - Skeleton sizes should closely match actual content
4. **Shape variety** - Use appropriate shapes (rectangle, circle, square) for content type
5. **Border radius control** - Customize rounding to match your design system
6. **Performance optimization** - Limit skeleton complexity and quantity
7. **Accessibility compliance** - Built-in accessibility through PrimeReact foundation
8. **Progressive loading** - Support different loading states for complex content
9. **Error handling** - Combine with proper error states for robust UX
10. **Responsive design** - Consider skeleton behavior across different screen sizes

By following these guidelines, you'll create smooth, accessible, and performant loading experiences that enhance your Neuron applications' perceived performance and user satisfaction.

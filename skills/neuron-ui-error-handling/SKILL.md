---
name: neuron-ui-error-handling
description: Implement error handling using ErrorBoundary and ErrorPage components in Neuron applications. Use when adding error boundaries, creating error pages, or handling component errors. Covers ErrorBoundary setup, ErrorPage component, useErrorBoundary hook, and error logging integration. Provides robust error handling patterns.
---

# Error Handling

Implement error handling using ErrorBoundary and ErrorPage components.

## Process

1. **Add ErrorBoundary** - Wrap app or sections with ErrorBoundary
2. **Configure fallback** - Set up ErrorPage or custom fallback UI
3. **Use hook** - Access useErrorBoundary for programmatic errors
4. **Log errors** - Integrate with LoggerService
5. **Test error handling** - Verify error boundaries work

## ErrorBoundary Component

**Basic usage:**

```tsx
import { ErrorBoundary } from "@neuron/ui";

const App = () => (
  <ErrorBoundary>
    <YourApp />
  </ErrorBoundary>
);
```

**With custom fallback:**

```tsx
import { ErrorBoundary, ErrorPage } from "@neuron/ui";

const App = () => (
  <ErrorBoundary fallback={<ErrorPage />}>
    <YourApp />
  </ErrorBoundary>
);
```

**With error callback:**

```tsx
<ErrorBoundary
  onError={(error, errorInfo) => {
    console.error("Error caught:", error);
    loggerService.error(error, errorInfo);
  }}
>
  <YourApp />
</ErrorBoundary>
```

## ErrorPage Component

**Default error page:**

```tsx
import { ErrorPage } from "@neuron/ui";

<ErrorPage />;
```

**Custom error page:**

```tsx
<ErrorPage
  title="Something went wrong"
  message="We're working on fixing this issue."
  showRetry={true}
  onRetry={() => window.location.reload()}
/>
```

## useErrorBoundary Hook

**Programmatic error handling:**

```tsx
import { useErrorBoundary } from "@neuron/ui";

const MyComponent = () => {
  const { showBoundary } = useErrorBoundary();

  const handleError = async () => {
    try {
      await riskyOperation();
    } catch (error) {
      showBoundary(error);
    }
  };

  return <button onClick={handleError}>Trigger Error</button>;
};
```

## Error Logging

**With LoggerService:**

```tsx
import { LoggerService } from "@neuron/core";

const loggerService = new LoggerService();

<ErrorBoundary
  onError={(error, errorInfo) => {
    loggerService.error("Component error", {
      error: error.message,
      stack: error.stack,
      componentStack: errorInfo.componentStack,
    });
  }}
>
  <YourApp />
</ErrorBoundary>;
```

## Examples

**Example 1: Root-level error boundary**

```tsx
import { ErrorBoundary, ErrorPage } from "@neuron/ui";

const AppRoot = () => (
  <ErrorBoundary fallback={<ErrorPage />}>
    <App />
  </ErrorBoundary>
);
```

**Example 2: Section-specific error boundary**

```tsx
const Dashboard = () => (
  <div>
    <Header />
    <ErrorBoundary fallback={<div>Failed to load dashboard</div>}>
      <DashboardContent />
    </ErrorBoundary>
  </div>
);
```

**Example 3: Programmatic error handling**

```tsx
const DataFetcher = () => {
  const { showBoundary } = useErrorBoundary();

  useEffect(() => {
    fetchData().catch((error) => showBoundary(error));
  }, []);

  return <div>Loading data...</div>;
};
```

## Best Practices

- Use ErrorBoundary at root level for global error handling
- Add section-specific ErrorBoundaries for isolated error handling
- Use ErrorPage component for consistent error UI
- Integrate with LoggerService for error tracking
- Use useErrorBoundary hook for async error handling
- Provide meaningful error messages to users
- Add retry functionality when appropriate
- Test error boundaries with intentional errors
- Log errors with context information
- Don't catch errors in event handlers (use try-catch instead)

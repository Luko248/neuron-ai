---
name: neuron-hooks
description: Use Neuron framework global hooks for common functionality. Use when implementing responsive layouts, scroll tracking, debouncing, or app context. MANDATORY: Always use framework hooks instead of custom implementations. Covers useResponsiveMediaQuery, useScrollPosition, useDebounce, useAppContextUrl, and useAppConfig hooks with usage patterns.
---

# Neuron Framework Hooks

Use Neuron framework global hooks for common functionality.

**CRITICAL: Always use framework hooks instead of custom implementations.**

## Available Hooks

1. **useResponsiveMediaQuery** - Responsive breakpoint detection
2. **useScrollPosition** - Scroll position tracking
3. **useDebounce** - Debounce values and callbacks
4. **useAppContextUrl** - App context and URL management
5. **useAppConfig** - Access app configuration

## 1. useResponsiveMediaQuery

**Import:** `import { useResponsiveMediaQuery } from "@neuron/core";`

**Purpose:** Responsive breakpoint detection for conditional rendering.

**Breakpoints:** `mobile-s`, `mobile`, `tablet`, `desktop-s`, `desktop-l`, `desktop-hd`, `desktop-full-hd`, `desktop-2k`, `desktop-4k`

**Example:**

```tsx
import { useResponsiveMediaQuery } from "@neuron/core";

const ResponsiveComponent = () => {
  const isMobile = useResponsiveMediaQuery("mobile");
  const isDesktop = useResponsiveMediaQuery("desktop-l");

  return (
    <div>
      {isMobile && <MobileNavigation />}
      {isDesktop && <DesktopSidebar />}
    </div>
  );
};
```

## 2. useScrollPosition

**Import:** `import { useScrollPosition } from "@neuron/core";`

**Purpose:** Track scroll position with performance optimization.

**Example:**

````tsx
import { useScrollPosition } from "@neuron/core";
import { useRef } from "react";

// Window scroll
const ScrollIndicator = () => {
  const scrollPosition = useScrollPosition();
  return <div>{scrollPosition.y > 100 && <BackToTopButton />}</div>;
};

// Element scroll
const CustomScrollContainer = () => {
  const containerRef = useRef<HTMLDivElement>(null);
  const scrollPosition = useScrollPosition(containerRef);

  return (
    <div ref={containerRef} style={{ height: "300px", overflow: "auto" }}>
      <p>Scroll: {scrollPosition.y}</p>
    </div>
  );

## 3. useDebounce

**Import:** `import { useDebounce } from "@neuron/core";`

**Purpose:** Debounce values or callbacks to optimize performance.

**Example:**
```tsx
import { useDebounce } from "@neuron/core";
import { useState } from "react";

const SearchComponent = () => {
  const [searchTerm, setSearchTerm] = useState("");
  const debouncedSearchTerm = useDebounce(searchTerm, 500);

  // API call only triggers after 500ms of no typing
  useEffect(() => {
    if (debouncedSearchTerm) {
      fetchSearchResults(debouncedSearchTerm);
    }
  }, [debouncedSearchTerm]);

  return <Input value={searchTerm} onChange={(e) => setSearchTerm(e.target.value)} />;
};
````

## 4. useAppContextUrl

**Import:** `import { useAppContextUrl } from "@neuron/core";`

**Purpose:** Access app context and URL management.

**Example:**

```tsx
import { useAppContextUrl } from "@neuron/core";

const NavigationComponent = () => {
  const { completeUrl, contextUrl, baseUrl } = useAppContextUrl();

  return (
    <nav>
      <a href={completeUrl("/dashboard")}>Dashboard</a>
      <a href={completeUrl("/settings")}>Settings</a>
    </nav>
  );
};
```

## 5. useAppConfig

**Import:** `import { useAppConfig } from "@neuron/core";`

**Purpose:** Access application configuration.

**Example:**

```tsx
import { useAppConfig } from "@neuron/core";

const ApiComponent = () => {
  const config = useAppConfig();
  const apiUrl = config.be.baseUrl;

  const fetchData = async () => {
    const response = await fetch(`${apiUrl}/api/data`);
    return response.json();
  };

  return <div>API URL: {apiUrl}</div>;
};
```

## Examples

**Example 1: Responsive layout**

```tsx
const ResponsiveLayout = () => {
  const isMobile = useResponsiveMediaQuery("mobile");
  return isMobile ? <MobileView /> : <DesktopView />;
};
```

**Example 2: Scroll-based header**

```tsx
const Header = () => {
  const scrollPosition = useScrollPosition();
  const isSticky = scrollPosition.y > 100;
  return <header className={isSticky ? "sticky" : ""}>{/* content */}</header>;
};
```

**Example 3: Debounced search**

```tsx
const Search = () => {
  const [query, setQuery] = useState("");
  const debouncedQuery = useDebounce(query, 300);

  useEffect(() => {
    if (debouncedQuery) search(debouncedQuery);
  }, [debouncedQuery]);

  return <Input value={query} onChange={(e) => setQuery(e.target.value)} />;
};
```

## Best Practices

- Always use framework hooks instead of custom implementations
- Use `useResponsiveMediaQuery` for breakpoint detection
- Use `useScrollPosition` for scroll tracking
- Use `useDebounce` for search inputs and API calls
- Use `useAppContextUrl` for URL management
- Use `useAppConfig` for accessing configuration
- Avoid creating custom hooks for functionality already provided
- Import hooks from `@neuron/core`

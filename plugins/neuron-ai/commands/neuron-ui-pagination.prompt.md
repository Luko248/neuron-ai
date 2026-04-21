---
agent: agent
---

# Neuron Pagination Component Guidelines

## For the AI Assistant

This document provides comprehensive guidelines for implementing and configuring the Neuron Pagination component. It explains proper usage, configuration options, responsive behavior, and integration patterns for data pagination.

## Sync Metadata

- **Component Version:** v3.3.2
- **Component Source:** `packages/neuron/ui/src/lib/data/pagination/Pagination.tsx`
- **Guideline Command:** `/neuron-ui-pagination`
- **Related Skill:** `neuron-ui-data`

## Introduction

The Pagination component provides navigation controls for large datasets by breaking content into manageable pages. It includes configurable page size selection, navigation controls, and responsive behavior for optimal user experience across different screen sizes.

## Component Structure

The Pagination component consists of:

```
Pagination
├── Pagination Info Text (optional)
└── Pagination Controls (optional)
    ├── Navigation Controls (First, Previous, Page Links, Next, Last)
    └── Page Size Selector (optional)
```

## Basic Usage

### 1. Import the Pagination Component

```tsx
import { Pagination } from "@neuron/ui";
```

### 2. Basic Pagination Implementation

Here's a simple implementation of the Pagination component:

```tsx
import { Pagination } from "@neuron/ui";
import { useState } from "react";

const BasicPaginationExample = () => {
  const [pagination, setPagination] = useState({
    offset: 0,
    limit: 25,
    total: 100,
  });

  const handlePaginationChange = ({ offset, limit }: { offset: number; limit: number }) => {
    setPagination((prev) => ({ ...prev, offset, limit }));
    // Fetch new data based on offset and limit
  };

  return <Pagination pagination={pagination} onChange={handlePaginationChange} />;
};
```

## Pagination Configuration

### Pagination Object Structure

The pagination prop requires an object with three properties:

```tsx
interface PaginationData {
  offset: number; // Starting index (0-based)
  limit: number; // Items per page
  total: number; // Total number of items
}
```

### Basic Configuration Examples

```tsx
import { Pagination } from "@neuron/ui";

// Standard pagination
const standardPagination = {
  offset: 0, // First page (0-based)
  limit: 25, // 25 items per page
  total: 100, // 100 total items
};

// Second page
const secondPagePagination = {
  offset: 25, // Second page starts at index 25
  limit: 25, // 25 items per page
  total: 100, // 100 total items
};

// Custom page size
const customSizePagination = {
  offset: 0, // First page
  limit: 50, // 50 items per page
  total: 200, // 200 total items
};
```

## Page Size Configuration

### Default Page Sizes

```tsx
// Default page sizes: [10, 20, 50]
<Pagination
  pagination={pagination}
  onChange={handleChange}
/>

// Custom page sizes
<Pagination
  pagination={pagination}
  pageSizes={[25, 100, 200]}
  onChange={handleChange}
/>
```

### Hiding Page Size Selector

```tsx
// Hide page size selector completely
<Pagination
  pagination={pagination}
  isPageSizeSelectorHidden={true}
  onChange={handleChange}
/>

// Alternative using pagination settings
<Pagination
  pagination={pagination}
  paginationSettings={{
    hidePageSizeSelector: true
  }}
  onChange={handleChange}
/>
```

## Advanced Configuration with Pagination Settings

### Comprehensive Settings Object

```tsx
interface PaginationSettings {
  showPaginationForSinglePage?: boolean; // Show pagination when only 1 page (default: true)
  hidePagination?: boolean; // Hide entire pagination component
  hidePaginationControls?: boolean; // Hide navigation controls only
  hidePageSizeSelector?: boolean; // Hide page size dropdown
  hidePaginationText?: boolean; // Hide "Showing X to Y of Z records" text
}
```

### Settings Examples

```tsx
import { Pagination } from "@neuron/ui";

// Minimal pagination (only showing info text)
<Pagination
  pagination={pagination}
  paginationSettings={{
    hidePaginationControls: true,
    hidePageSizeSelector: true
  }}
  onChange={handleChange}
/>

// Hide pagination for single page
<Pagination
  pagination={pagination}
  paginationSettings={{
    showPaginationForSinglePage: false
  }}
  onChange={handleChange}
/>

// Compact pagination (no text, no page size selector)
<Pagination
  pagination={pagination}
  paginationSettings={{
    hidePaginationText: true,
    hidePageSizeSelector: true
  }}
  onChange={handleChange}
/>
```

## Responsive Behavior

### Responsive Mode Configuration

```tsx
// Viewport-based responsive behavior (default)
<Pagination
  pagination={pagination}
  responsiveMode="viewport"
  onChange={handleChange}
/>

// Container-based responsive behavior
<Pagination
  pagination={pagination}
  responsiveMode="container"
  onChange={handleChange}
/>
```

### Responsive Integration Example

```tsx
import { Pagination, useResponsiveMediaQuery } from "@neuron/ui";

const ResponsivePaginationExample = () => {
  const isMobile = useResponsiveMediaQuery("mobile");

  return (
    <Pagination
      pagination={pagination}
      responsiveMode="viewport"
      paginationSettings={{
        hidePaginationText: isMobile, // Hide text on mobile
        hidePageSizeSelector: isMobile, // Hide page size selector on mobile
      }}
      onChange={handleChange}
    />
  );
};
```

## Data Integration Patterns

### With API Data Fetching

```tsx
import { Pagination } from "@neuron/ui";
import { useState, useEffect } from "react";

const DataTableWithPagination = () => {
  const [data, setData] = useState([]);
  const [pagination, setPagination] = useState({
    offset: 0,
    limit: 25,
    total: 0,
  });
  const [loading, setLoading] = useState(false);

  const fetchData = async (offset: number, limit: number) => {
    setLoading(true);
    try {
      const response = await fetch(`/api/data?offset=${offset}&limit=${limit}`);
      const result = await response.json();

      setData(result.items);
      setPagination((prev) => ({
        ...prev,
        offset,
        limit,
        total: result.total,
      }));
    } catch (error) {
      console.error("Failed to fetch data:", error);
    } finally {
      setLoading(false);
    }
  };

  useEffect(() => {
    fetchData(pagination.offset, pagination.limit);
  }, []);

  const handlePaginationChange = ({ offset, limit }: { offset: number; limit: number }) => {
    fetchData(offset, limit);
  };

  return (
    <div>
      {loading && <div>Loading...</div>}

      {/* Data display */}
      <div>
        {data.map((item, index) => (
          <div key={index}>{/* Render item */}</div>
        ))}
      </div>

      {/* Pagination */}
      <Pagination
        pagination={pagination}
        pageSizes={[25, 50, 100]}
        onChange={handlePaginationChange}
        disabled={loading}
      />
    </div>
  );
};
```

### With DataTable Integration

```tsx
import { Pagination, DataTable } from "@neuron/ui";

const DataTablePaginationExample = () => {
  const [pagination, setPagination] = useState({
    offset: 0,
    limit: 25,
    total: 200,
  });

  // Calculate current page data
  const currentPageData = allData.slice(pagination.offset, pagination.offset + pagination.limit);

  return (
    <div>
      <DataTable
        value={currentPageData}
        // Other DataTable props
      />

      <Pagination
        pagination={pagination}
        pageSizes={[25, 50, 100]}
        onChange={({ offset, limit }) => {
          setPagination((prev) => ({ ...prev, offset, limit }));
        }}
      />
    </div>
  );
};
```

## State Management Patterns

### Local State Management

```tsx
import { Pagination } from "@neuron/ui";
import { useState } from "react";

const LocalStatePaginationExample = () => {
  const [paginationState, setPaginationState] = useState({
    offset: 0,
    limit: 25,
    total: 150,
  });

  const handlePaginationChange = ({ offset, limit }: { offset: number; limit: number }) => {
    setPaginationState((prev) => ({
      ...prev,
      offset,
      limit,
    }));

    // Trigger data refetch or update
    // fetchData(offset, limit);
  };

  return <Pagination pagination={paginationState} onChange={handlePaginationChange} />;
};
```

### URL State Synchronization

```tsx
import { Pagination } from "@neuron/ui";
import { useSearchParams } from "react-router-dom";
import { useEffect, useState } from "react";

const UrlSyncPaginationExample = () => {
  const [searchParams, setSearchParams] = useSearchParams();
  const [pagination, setPagination] = useState({
    offset: parseInt(searchParams.get("offset") || "0", 10),
    limit: parseInt(searchParams.get("limit") || "25", 10),
    total: 0,
  });

  const handlePaginationChange = ({ offset, limit }: { offset: number; limit: number }) => {
    // Update URL parameters
    setSearchParams({
      offset: offset.toString(),
      limit: limit.toString(),
    });

    // Update local state
    setPagination((prev) => ({ ...prev, offset, limit }));
  };

  useEffect(() => {
    // Fetch data when URL parameters change
    // fetchData(pagination.offset, pagination.limit);
  }, [pagination.offset, pagination.limit]);

  return <Pagination pagination={pagination} onChange={handlePaginationChange} />;
};
```

## Accessibility and UX

### Disabled State

```tsx
// Disable pagination during loading
<Pagination pagination={pagination} disabled={isLoading} onChange={handleChange} />
```

### Test ID Configuration

```tsx
// Custom test ID for testing
<Pagination pagination={pagination} testId="data-table-pagination" onChange={handleChange} />
```

### ARIA and Accessibility

The Pagination component includes built-in accessibility features:

- Proper ARIA labels for navigation
- Role="navigation" for screen readers
- Keyboard navigation support
- Focus management

```tsx
// Accessibility is built-in
<Pagination
  pagination={pagination}
  onChange={handleChange}
  // Component automatically includes:
  // - role="navigation"
  // - aria-label="Pagination"
  // - Proper keyboard navigation
/>
```

## Common Use Cases

### 1. Standard Data Table Pagination

```tsx
const StandardTablePagination = () => {
  return (
    <Pagination
      pagination={{ offset: 0, limit: 25, total: 100 }}
      pageSizes={[25, 50, 100]}
      onChange={handlePaginationChange}
    />
  );
};
```

### 2. Mobile-Optimized Pagination

```tsx
const MobilePagination = () => {
  const isMobile = useResponsiveMediaQuery("mobile");

  return (
    <Pagination
      pagination={pagination}
      pageSizes={[10, 25, 50]}
      paginationSettings={{
        hidePaginationText: isMobile,
        hidePageSizeSelector: isMobile,
      }}
      onChange={handleChange}
    />
  );
};
```

### 3. Minimal Pagination (Info Only)

```tsx
const InfoOnlyPagination = () => {
  return (
    <Pagination
      pagination={pagination}
      paginationSettings={{
        hidePaginationControls: true,
        hidePageSizeSelector: true,
      }}
      onChange={handleChange}
    />
  );
};
```

### 4. Search Results Pagination

```tsx
const SearchResultsPagination = () => {
  return (
    <Pagination
      pagination={{ offset: 0, limit: 20, total: searchResultsTotal }}
      pageSizes={[10, 20, 50]}
      paginationSettings={{
        showPaginationForSinglePage: false, // Hide if only one page of results
      }}
      onChange={handleSearchPagination}
    />
  );
};
```

## Integration with Forms and Filters

### Filter Reset on Page Size Change

```tsx
const FilterablePaginationExample = () => {
  const [filters, setFilters] = useState({});
  const [pagination, setPagination] = useState({
    offset: 0,
    limit: 25,
    total: 0,
  });

  const handlePaginationChange = ({ offset, limit }: { offset: number; limit: number }) => {
    // Reset to first page when changing page size
    const newOffset = pagination.limit !== limit ? 0 : offset;

    setPagination((prev) => ({
      ...prev,
      offset: newOffset,
      limit,
    }));

    // Refetch data with new pagination and current filters
    fetchData(newOffset, limit, filters);
  };

  return <Pagination pagination={pagination} onChange={handlePaginationChange} />;
};
```

## Pagination Props Reference

| Prop                     | Type                                                      | Required | Description                                    |
| ------------------------ | --------------------------------------------------------- | -------- | ---------------------------------------------- |
| pagination               | `{ offset: number, limit: number, total: number }`        | Yes      | Current pagination state                       |
| onChange                 | `(pagination: { offset: number, limit: number }) => void` | Yes      | Callback when pagination changes               |
| pageSizes                | `number[]`                                                | No       | Available page sizes (default: [10, 20, 50])   |
| isPageSizeSelectorHidden | `boolean`                                                 | No       | Hide page size selector                        |
| disabled                 | `boolean`                                                 | No       | Disable all pagination controls                |
| testId                   | `string`                                                  | No       | Custom test ID for testing                     |
| responsiveMode           | `"viewport" \| "container"`                               | No       | Responsive behavior mode (default: "viewport") |
| paginationSettings       | `PaginationSettings`                                      | No       | Advanced configuration options                 |

## Best Practices

1. **Consistent Page Sizes**: Use standard page sizes across your application (e.g., 25, 50, 100)

2. **Reset on Filter Changes**: Always reset to page 1 when filters change

3. **URL Synchronization**: Sync pagination state with URL for bookmarkable pages

4. **Loading States**: Disable pagination during data loading

5. **Mobile Optimization**: Consider hiding text and page size selector on mobile

6. **Accessibility**: Ensure proper keyboard navigation and screen reader support

7. **Performance**: Implement server-side pagination for large datasets

8. **User Feedback**: Show loading indicators during pagination changes

## Common Mistakes to Avoid

❌ **Wrong offset calculation**:

```tsx
// Wrong - 1-based page number
const wrongOffset = currentPage * limit;

// Correct - 0-based offset
const correctOffset = (currentPage - 1) * limit;
```

❌ **Not resetting offset when changing page size**:

```tsx
// Wrong - might cause out-of-bounds
onChange({ offset: currentOffset, limit: newLimit });

// Correct - reset to first page
onChange({ offset: 0, limit: newLimit });
```

❌ **Missing total update**:

```tsx
// Wrong - total might be outdated
setPagination({ offset, limit, total: oldTotal });

// Correct - update total from API response
setPagination({ offset, limit, total: apiResponse.total });
```

## Summary

The Neuron Pagination component provides comprehensive pagination functionality with:

- **Flexible Configuration**: Page sizes, settings, responsive behavior
- **Built-in Accessibility**: ARIA labels, keyboard navigation, screen reader support
- **Responsive Design**: Adaptive behavior for different screen sizes
- **Easy Integration**: Works with DataTable, forms, and API data
- **State Management**: Supports local state, URL sync, and global state management
- **Customization Options**: Hide/show individual parts, custom styling support

Use Pagination strategically for large datasets to improve user experience and application performance. Always implement server-side pagination for optimal performance with large data sets.

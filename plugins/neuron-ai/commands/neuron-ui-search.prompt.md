---
agent: agent
---

# Search Component AI Guidelines

## For the AI Assistant

Your task is to integrate and configure the Neuron Search component in React applications. This guide provides essential instructions for implementing Search components, which provide specialized search input functionality with format validation, real-time filtering, search icon integration, and comprehensive validation through React Hook Form integration across all Neuron applications.

## Sync Metadata

- **Component Version:** v4.1.0
- **Component Source:** `packages/neuron/ui/src/lib/form/search/Search.tsx`
- **Guideline Command:** `/neuron-ui-search`
- **Related Skill:** `neuron-ui-form-core`

## Overview

The Search component is a specialized input field built on the native HTML `input[type="search"]` element with enhanced functionality including format validation, real-time input filtering, and integrated search icon. It provides a consistent search experience with React Hook Form integration and comprehensive validation support.

## Core Usage

### Basic Implementation

```tsx
import { Form } from "@neuron/ui";
import { useForm } from "react-hook-form";
import { zodResolver } from "@hookform/resolvers/zod";
import { z } from "zod";

const schema = z.object({
  searchQuery: z.string().min(1, { message: "Search query is required" }),
  productSearch: z.string().max(50, { message: "Search query too long" }),
  userSearch: z.string().optional(),
});

const SearchForm = () => {
  const { control, handleSubmit } = useForm({
    resolver: zodResolver(schema),
    mode: "onChange",
  });

  const onSubmit = (data) => {
    console.log("Search data:", data);
  };

  return (
    <form onSubmit={handleSubmit(onSubmit)}>
      <Form.Search
        name="searchQuery"
        labelText="Search Products"
        control={control}
        placeholder="Enter product name or SKU..."
        required
        requiredFlag={true}
        testId="product-search"
      />

      <Form.Search
        name="productSearch"
        labelText="Quick Search"
        control={control}
        placeholder="Search..."
        autoComplete="off"
        testId="quick-search"
      />

      <Form.Search
        name="userSearch"
        labelText="User Search"
        control={control}
        placeholder="Search users..."
        optional
        description="Search by name, email, or ID"
        testId="user-search"
      />
    </form>
  );
};
```

### Format Validation

```tsx
const FormatValidatedSearch = () => {
  const { control } = useForm();

  return (
    <>
      {/* Numeric search with optional wildcard */}
      <Form.Search
        name="numericSearch"
        labelText="Product Code Search"
        control={control}
        format="^(?:\\d{0,9}\\*|\\d{1,10})$"
        placeholder="Enter up to 10 digits or 9 digits + *"
        description="Use * as wildcard (e.g., 12345*)"
        testId="numeric-search"
      />

      {/* Alphanumeric search */}
      <Form.Search
        name="alphanumericSearch"
        labelText="Reference Number"
        control={control}
        format="^[A-Za-z0-9]{0,20}$"
        placeholder="Letters and numbers only"
        description="Maximum 20 alphanumeric characters"
        testId="alphanumeric-search"
      />

      {/* Email format search */}
      <Form.Search
        name="emailSearch"
        labelText="Email Search"
        control={control}
        format="^[a-zA-Z0-9._%+-]*@?[a-zA-Z0-9.-]*\\.?[a-zA-Z]*$"
        placeholder="Start typing email address..."
        description="Search for email addresses"
        testId="email-search"
      />

      {/* Phone number search */}
      <Form.Search
        name="phoneSearch"
        labelText="Phone Number Search"
        control={control}
        format="^[\\d\\s\\-\\+\\(\\)]*$"
        placeholder="Enter phone number..."
        description="Numbers, spaces, dashes, and parentheses allowed"
        testId="phone-search"
      />
    </>
  );
};
```

## Key Props Reference

### Essential Props

- **name**: Field name for form registration (required)
- **control**: React Hook Form control object
- **labelText**: Accessible label text for the search field
- **placeholder**: Placeholder text for empty search field

### Search Configuration

- **format**: `RegExp | string` - Real-time input validation pattern
- **pattern**: `string` - HTML5 pattern attribute for validation
- **autoComplete**: `string` - Browser autocomplete behavior (default: "off")

### Validation & States

- **required**: HTML5 required attribute
- **requiredFlag**: Shows required indicator (\*)
- **optional**: Shows optional indicator
- **isValid**: Manual valid state indicator

### Enhanced Features

- **tooltip**: Help text with tooltip integration
- **description**: Helper text above the search field
- **descriptionVariant**: `"default"` | `"success"` | `"warning"` | `"danger"` | `"info"`
- **literalView**: Display as read-only text instead of search input
- **testId**: Custom test identifier for testing

### Layout & Styling

- **className**: CSS classes for grid positioning and styling
- **inline**: Display search field inline
- **leftAddonContent**: Content to display on the left side
- **rightAddonContent**: Content to display on the right side (e.g., search button)

### Interaction & Behavior

- **disabled**: Disable search field interactions
- **readOnly**: Make search field read-only
- **onChange**: Custom change event handler
- **deps**: Dependencies for form field updates

### Access Control

- **readonlyAccess**: Role-based read access control
- **fullAccess**: Role-based full access control

## Format Validation Patterns

### Common Search Patterns

```tsx
const SearchPatterns = () => {
  return (
    <>
      {/* Product SKU search */}
      <Form.Search
        name="skuSearch"
        labelText="Product SKU"
        control={control}
        format="^[A-Z]{2,3}\\d{4,8}$"
        placeholder="e.g., AB1234 or ABC12345678"
        description="2-3 letters followed by 4-8 digits"
      />

      {/* Order number search */}
      <Form.Search
        name="orderSearch"
        labelText="Order Number"
        control={control}
        format="^ORD\\d{6,10}$"
        placeholder="e.g., ORD123456"
        description="ORD followed by 6-10 digits"
      />

      {/* Customer ID search */}
      <Form.Search
        name="customerSearch"
        labelText="Customer ID"
        control={control}
        format="^C\\d{5,8}$"
        placeholder="e.g., C12345"
        description="C followed by 5-8 digits"
      />

      {/* Invoice number search */}
      <Form.Search
        name="invoiceSearch"
        labelText="Invoice Number"
        control={control}
        format="^INV-\\d{4}-\\d{6}$"
        placeholder="e.g., INV-2024-123456"
        description="Format: INV-YYYY-XXXXXX"
      />
    </>
  );
};
```

### Advanced Format Validation

```tsx
const AdvancedSearchPatterns = () => {
  return (
    <>
      {/* Flexible text search with length limit */}
      <Form.Search
        name="flexibleSearch"
        labelText="General Search"
        control={control}
        format="^.{0,100}$"
        placeholder="Search anything..."
        description="Maximum 100 characters"
      />

      {/* Search with special characters allowed */}
      <Form.Search
        name="specialCharSearch"
        labelText="Advanced Search"
        control={control}
        format="^[\\w\\s\\-\\.@#&]*$"
        placeholder="Letters, numbers, spaces, and common symbols"
        description="Allows letters, numbers, spaces, -, ., @, #, &"
      />

      {/* Date-based search */}
      <Form.Search
        name="dateSearch"
        labelText="Date Search"
        control={control}
        format="^\\d{0,4}-?\\d{0,2}-?\\d{0,2}$"
        placeholder="YYYY-MM-DD or partial date"
        description="Enter full or partial date (2024, 2024-03, 2024-03-15)"
      />

      {/* Version number search */}
      <Form.Search
        name="versionSearch"
        labelText="Version Search"
        control={control}
        format="^v?\\d{1,2}\\.?\\d{0,2}\\.?\\d{0,2}$"
        placeholder="e.g., v1.2.3 or 1.2"
        description="Version format: v1.2.3 or 1.2"
      />
    </>
  );
};
```

## Addon Content Integration

### Search with Action Buttons

```tsx
const SearchWithButtons = () => {
  const { control, watch } = useForm();
  const searchValue = watch("searchQuery");

  const handleSearch = () => {
    console.log("Searching for:", searchValue);
  };

  const handleClear = () => {
    setValue("searchQuery", "");
  };

  return (
    <>
      {/* Search with search button */}
      <Form.Search
        name="searchQuery"
        labelText="Product Search"
        control={control}
        placeholder="Enter search terms..."
        rightAddonContent={
          <Button size="small" onClick={handleSearch} disabled={!searchValue}>
            Search
          </Button>
        }
      />

      {/* Search with clear button */}
      <Form.Search
        name="clearableSearch"
        labelText="Clearable Search"
        control={control}
        placeholder="Type to search..."
        rightAddonContent={
          searchValue && (
            <Button size="small" variant="ghost" onClick={handleClear} icon={{ iconDef: baseIcons.xMarkSolid }} />
          )
        }
      />

      {/* Search with filter dropdown */}
      <Form.Search
        name="filteredSearch"
        labelText="Filtered Search"
        control={control}
        placeholder="Search with filters..."
        leftAddonContent={
          <Select
            name="searchFilter"
            control={control}
            options={[
              { value: "all", label: "All" },
              { value: "products", label: "Products" },
              { value: "users", label: "Users" },
            ]}
            size="small"
          />
        }
      />
    </>
  );
};
```

### Search with Icons and Indicators

```tsx
const SearchWithIcons = () => {
  return (
    <>
      {/* Search with loading indicator */}
      <Form.Search
        name="loadingSearch"
        labelText="Search with Loading"
        control={control}
        placeholder="Searching..."
        rightAddonContent={<Icon iconDef={baseIcons.spinnerSolid} className="animate-spin" />}
      />

      {/* Search with result count */}
      <Form.Search
        name="countSearch"
        labelText="Search with Results"
        control={control}
        placeholder="Search products..."
        rightAddonContent={<span className="text-sm text-muted">{resultCount} results</span>}
      />

      {/* Search with advanced options */}
      <Form.Search
        name="advancedSearch"
        labelText="Advanced Search"
        control={control}
        placeholder="Search with options..."
        rightAddonContent={
          <Button
            size="small"
            variant="ghost"
            icon={{ iconDef: baseIcons.cogSolid }}
            tooltip="Advanced search options"
          />
        }
      />
    </>
  );
};
```

## Validation Patterns

### Zod Schema Validation

```tsx
// Basic search validation
const basicSearchSchema = z.object({
  searchQuery: z
    .string()
    .min(1, { message: "Search query cannot be empty" })
    .max(100, { message: "Search query too long" }),

  productSearch: z
    .string()
    .min(2, { message: "Enter at least 2 characters" })
    .max(50, { message: "Search query too long" })
    .optional(),

  userSearch: z.string().refine((val) => val.trim().length > 0, {
    message: "Search query cannot be empty or just whitespace",
  }),
});

// Advanced search validation
const advancedSearchSchema = z.object({
  skuSearch: z.string().regex(/^[A-Z]{2,3}\d{4,8}$/, {
    message: "Invalid SKU format (e.g., AB1234)",
  }),

  emailSearch: z.string().email({ message: "Invalid email format" }).or(z.string().length(0)), // Allow empty for optional search

  phoneSearch: z
    .string()
    .regex(/^[\d\s\-\+\(\)]*$/, {
      message: "Invalid phone number format",
    })
    .refine(
      (phone) => {
        const digitsOnly = phone.replace(/\D/g, "");
        return digitsOnly.length === 0 || digitsOnly.length >= 10;
      },
      { message: "Phone number must have at least 10 digits" },
    ),

  dateSearch: z
    .string()
    .regex(/^\d{4}-\d{2}-\d{2}$/, {
      message: "Date must be in YYYY-MM-DD format",
    })
    .refine(
      (date) => {
        const parsed = new Date(date);
        return !isNaN(parsed.getTime());
      },
      { message: "Invalid date" },
    ),
});

// Conditional search validation
const conditionalSearchSchema = z
  .object({
    searchType: z.enum(["product", "user", "order"]),
    searchQuery: z.string(),
  })
  .refine(
    (data) => {
      if (data.searchType === "product") {
        return /^[A-Za-z0-9\s\-]{2,50}$/.test(data.searchQuery);
      }
      if (data.searchType === "user") {
        return /^[A-Za-z\s@\.]{2,100}$/.test(data.searchQuery);
      }
      if (data.searchType === "order") {
        return /^ORD\d{6,10}$/.test(data.searchQuery);
      }
      return true;
    },
    {
      message: "Invalid search format for selected type",
      path: ["searchQuery"],
    },
  );
```

### Custom Validation Functions

```tsx
// Search query quality validation
const validateSearchQuality = (query: string) => {
  const trimmed = query.trim();

  if (trimmed.length === 0) return "Search query cannot be empty";

  if (trimmed.length < 2) return "Search query too short (minimum 2 characters)";

  if (trimmed.length > 100) return "Search query too long (maximum 100 characters)";

  // Check for meaningful content
  if (/^[\s\-\.\*]+$/.test(trimmed)) {
    return "Search query must contain meaningful characters";
  }

  // Check for excessive special characters
  const specialCharCount = (trimmed.match(/[^\w\s]/g) || []).length;
  if (specialCharCount > trimmed.length / 2) {
    return "Search query contains too many special characters";
  }

  return true;
};

// Format-specific validation
const validateProductCode = (code: string) => {
  if (!code) return true; // Allow empty for optional fields

  // Product code format: 2-3 letters + 4-8 digits
  if (!/^[A-Z]{2,3}\d{4,8}$/.test(code)) {
    return "Product code must be 2-3 letters followed by 4-8 digits";
  }

  // Check for valid prefix
  const validPrefixes = ["AB", "CD", "EF", "ABC", "DEF"];
  const prefix = code.match(/^[A-Z]{2,3}/)?.[0];
  if (prefix && !validPrefixes.includes(prefix)) {
    return `Invalid product code prefix: ${prefix}`;
  }

  return true;
};

// Usage with custom validation
const CustomValidatedSearch = () => {
  const schema = z.object({
    generalSearch: z.string().refine(validateSearchQuality),
    productCode: z.string().refine(validateProductCode),
  });

  const { control } = useForm({
    resolver: zodResolver(schema),
    mode: "onChange",
  });

  return (
    <>
      <Form.Search
        name="generalSearch"
        labelText="General Search"
        control={control}
        placeholder="Enter search terms..."
        description="Minimum 2 characters, maximum 100"
      />

      <Form.Search
        name="productCode"
        labelText="Product Code"
        control={control}
        format="^[A-Z]{2,3}\\d{4,8}$"
        placeholder="e.g., AB1234"
        description="2-3 letters followed by 4-8 digits"
      />
    </>
  );
};
```

## Common Use Cases

### E-commerce Search

```tsx
const EcommerceSearch = () => {
  return (
    <>
      {/* Product search */}
      <Form.Search
        name="productSearch"
        labelText="Product Search"
        control={control}
        placeholder="Search products by name, SKU, or description..."
        rightAddonContent={<Button size="small">Search</Button>}
        description="Search across all product fields"
      />

      {/* Category search */}
      <Form.Search
        name="categorySearch"
        labelText="Category Search"
        control={control}
        format="^[A-Za-z\\s\\-]{0,50}$"
        placeholder="Search categories..."
        description="Letters, spaces, and hyphens only"
      />

      {/* SKU search with format validation */}
      <Form.Search
        name="skuSearch"
        labelText="SKU Search"
        control={control}
        format="^[A-Z]{2,3}\\d{4,8}$"
        placeholder="Enter product SKU..."
        description="Format: AB1234 or ABC12345678"
      />

      {/* Price range search */}
      <Form.Search
        name="priceSearch"
        labelText="Price Search"
        control={control}
        format="^\\d{0,6}(\\.\\d{0,2})?(-\\d{0,6}(\\.\\d{0,2})?)?$"
        placeholder="e.g., 10.99 or 10-50"
        description="Single price or range (10-50)"
      />
    </>
  );
};
```

### User Management Search

```tsx
const UserManagementSearch = () => {
  return (
    <>
      {/* User search */}
      <Form.Search
        name="userSearch"
        labelText="User Search"
        control={control}
        placeholder="Search by name, email, or ID..."
        description="Search across user profiles"
      />

      {/* Email search */}
      <Form.Search
        name="emailSearch"
        labelText="Email Search"
        control={control}
        format="^[a-zA-Z0-9._%+-]*@?[a-zA-Z0-9.-]*\\.?[a-zA-Z]*$"
        placeholder="Enter email address..."
        description="Partial email addresses supported"
      />

      {/* Role search */}
      <Form.Search
        name="roleSearch"
        labelText="Role Search"
        control={control}
        format="^[A-Za-z\\s]{0,30}$"
        placeholder="Search user roles..."
        description="Letters and spaces only"
      />

      {/* Department search */}
      <Form.Search
        name="departmentSearch"
        labelText="Department Search"
        control={control}
        placeholder="Search departments..."
        leftAddonContent={<Icon iconDef={baseIcons.buildingSolid} />}
      />
    </>
  );
};
```

### Document Search

```tsx
const DocumentSearch = () => {
  return (
    <>
      {/* Document title search */}
      <Form.Search
        name="titleSearch"
        labelText="Document Title"
        control={control}
        placeholder="Search document titles..."
        description="Search in document titles and metadata"
      />

      {/* Document ID search */}
      <Form.Search
        name="docIdSearch"
        labelText="Document ID"
        control={control}
        format="^DOC\\d{6,10}$"
        placeholder="e.g., DOC123456"
        description="Format: DOC followed by 6-10 digits"
      />

      {/* Tag search */}
      <Form.Search
        name="tagSearch"
        labelText="Tag Search"
        control={control}
        format="^[A-Za-z0-9\\s\\-,]{0,100}$"
        placeholder="Enter tags separated by commas..."
        description="Use commas to separate multiple tags"
      />

      {/* Content search */}
      <Form.Search
        name="contentSearch"
        labelText="Content Search"
        control={control}
        placeholder="Search within document content..."
        rightAddonContent={<Button size="small" variant="ghost" icon={{ iconDef: baseIcons.magnifyingGlassSolid }} />}
      />
    </>
  );
};
```

### System Administration Search

```tsx
const AdminSearch = () => {
  return (
    <>
      {/* Log search */}
      <Form.Search
        name="logSearch"
        labelText="Log Search"
        control={control}
        placeholder="Search system logs..."
        format="^[\\w\\s\\-\\.@:]{0,200}$"
        description="Search in log messages and metadata"
      />

      {/* IP address search */}
      <Form.Search
        name="ipSearch"
        labelText="IP Address Search"
        control={control}
        format="^\\d{0,3}\\.?\\d{0,3}\\.?\\d{0,3}\\.?\\d{0,3}$"
        placeholder="e.g., 192.168.1.1"
        description="Full or partial IP addresses"
      />

      {/* Session ID search */}
      <Form.Search
        name="sessionSearch"
        labelText="Session ID"
        control={control}
        format="^[A-Za-z0-9]{0,32}$"
        placeholder="Enter session ID..."
        description="Alphanumeric session identifier"
      />

      {/* Error code search */}
      <Form.Search
        name="errorSearch"
        labelText="Error Code Search"
        control={control}
        format="^[A-Z]{2,4}\\d{3,6}$"
        placeholder="e.g., ERR404 or HTTP500"
        description="Error code format: 2-4 letters + 3-6 digits"
      />
    </>
  );
};
```

## Real-time Search Implementation

### Debounced Search

```tsx
const DebouncedSearch = () => {
  const { control, watch } = useForm();
  const [searchResults, setSearchResults] = useState([]);
  const [isSearching, setIsSearching] = useState(false);

  const searchQuery = watch("searchQuery");

  // Debounced search function
  const debouncedSearch = useCallback(
    debounce(async (query: string) => {
      if (query.length < 2) {
        setSearchResults([]);
        return;
      }

      setIsSearching(true);
      try {
        const results = await searchAPI(query);
        setSearchResults(results);
      } catch (error) {
        console.error("Search error:", error);
      } finally {
        setIsSearching(false);
      }
    }, 300),
    [],
  );

  useEffect(() => {
    debouncedSearch(searchQuery);
  }, [searchQuery, debouncedSearch]);

  return (
    <>
      <Form.Search
        name="searchQuery"
        labelText="Real-time Search"
        control={control}
        placeholder="Start typing to search..."
        rightAddonContent={isSearching && <Icon iconDef={baseIcons.spinnerSolid} className="animate-spin" />}
        description={`${searchResults.length} results found`}
      />

      {/* Search results display */}
      {searchResults.length > 0 && (
        <div className="search-results">
          {searchResults.map((result) => (
            <div key={result.id} className="search-result-item">
              {result.title}
            </div>
          ))}
        </div>
      )}
    </>
  );
};
```

## Role-Based Access Control

```tsx
const { userPermissions } = useAuth();

<Form.Search
  name="sensitiveSearch"
  labelText="Sensitive Data Search"
  control={control}
  fullAccess={userPermissions.includes("search_sensitive") ? "admin" : undefined}
  readonlyAccess={userPermissions.includes("view_search") ? "user" : undefined}
  placeholder="Search sensitive information..."
  description="Restricted access based on user permissions"
/>;
```

## Testing Integration

```tsx
<Form.Search
  name="testableSearch"
  labelText="Testable Search"
  control={control}
  testId="search-input"
  format="^[A-Za-z0-9\\s]{0,50}$"
/>;

// In tests
describe("Search Component", () => {
  it("should accept valid input", async () => {
    render(<TestForm />);

    const searchInput = screen.getByTestId("search-input");

    fireEvent.change(searchInput, { target: { value: "valid search" } });

    await waitFor(() => {
      expect(searchInput).toHaveValue("valid search");
    });
  });

  it("should prevent invalid input based on format", async () => {
    render(<TestForm />);

    const searchInput = screen.getByTestId("search-input");

    // Try to enter invalid characters
    fireEvent.keyDown(searchInput, { key: "@", code: "Digit2" });

    // Should prevent the invalid character
    expect(searchInput).toHaveValue("");
  });

  it("should show search icon", () => {
    render(<TestForm />);

    expect(screen.getByRole("img", { hidden: true })).toBeInTheDocument();
  });

  it("should handle addon content", () => {
    render(
      <Form.Search
        name="searchWithButton"
        control={control}
        rightAddonContent={<button>Search</button>}
        testId="search-with-addon"
      />,
    );

    expect(screen.getByRole("button", { name: "Search" })).toBeInTheDocument();
  });
});
```

## Performance Considerations

```tsx
const OptimizedSearch = memo(({ control, format, ...props }) => {
  // Memoize format regex to prevent recreation
  const formatRegex = useMemo(() => {
    if (!format) return null;
    return format instanceof RegExp ? format : new RegExp(format);
  }, [format]);

  // Memoize change handler
  const handleChange = useCallback((value: string, event) => {
    // Custom search logic
    console.log("Search value:", value);
  }, []);

  return <Form.Search control={control} format={formatRegex} onChange={handleChange} {...props} />;
});
```

## Troubleshooting

### Common Issues and Solutions

1. **Format validation not working**

   ```tsx
   // Ensure format prop is properly set
   <Form.Search format="^\\d{0,10}$" /> // Correct regex format
   ```

2. **Search icon not appearing**

   ```tsx
   // Icon is automatically included - check CSS styling
   // Ensure proper imports and theme setup
   ```

3. **Addon content not displaying**

   ```tsx
   // Ensure addon content is properly wrapped
   <Form.Search rightAddonContent={<Button>Search</Button>} />
   ```

4. **AutoComplete not working as expected**
   ```tsx
   // Set autoComplete explicitly
   <Form.Search autoComplete="off" /> // or "on" for browser suggestions
   ```

## Version Information

- **Component Version**: v4.0.0
- **Features**: Format validation, real-time input filtering, addon content support
- **Dependencies**: React Hook Form, native HTML search input
- **Browser Support**: Modern browsers with HTML5 search input support

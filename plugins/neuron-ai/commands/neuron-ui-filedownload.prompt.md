---
agent: agent
---

# AI-Assisted Neuron FileDownload Component Integration Guide

## For the AI Assistant

Your task is to integrate and configure the Neuron FileDownload component in a React application. This guide provides
comprehensive instructions for implementing the FileDownload component, which handles downloading and displaying lists
of document files across all Neuron applications.

## Sync Metadata

- **Component Version:** v1.3.2
- **Component Source:** `packages/neuron/ui/src/lib/file/fileDownload/FileDownload.tsx`
- **Guideline Command:** `/neuron-ui-filedownload`
- **Related Skill:** `neuron-ui-file`

## Introduction

The FileDownload component is a specialized UI component of the Neuron framework, designed to provide a standardized
interface for file downloading functionality across all Neuron applications.

### What is the FileDownload Component?

The FileDownload component provides a standardized interface for downloading document files with support for:

- Document-based file organization
- Public and private download modes
- Lazy and immediate download strategies
- Multiple display representations (row, link, button, icon)
- Loading states and error handling
- Multiple file display and download
- Integration with authentication systems
- Redux state management integration
- TypeScript support

### Key Features

- **Document Integration**: Files are organized by document ID for structured access
- **Download Modes**: Support for public downloads and authenticated access
- **Loading Strategies**: Lazy loading or immediate caching of file content
- **Display Representations**: Four visual styles - row (default), link, button, or icon
- **State Management**: Built-in Redux integration for file state handling
- **Error Handling**: Comprehensive error states and user feedback
- **Authentication**: Automatic handling of authentication requirements
- **Responsive Display**: Adaptive layout for single or multiple files
- **TypeScript Support**: Full type safety with comprehensive interfaces
- **Test Support**: Built-in testId support for automated testing

**Storybook Reference:** You can refer to the Storybook documentation for visual examples and interactive demos of the
FileDownload component.

## Step 1: Basic FileDownload Implementation

### 1.1 Import the FileDownload Component

```tsx
import { FileDownload } from "@neuron/ui";
```

### 1.2 Basic Usage

Here's a simple implementation of the FileDownload component:

```tsx
import { FileDownload } from "@neuron/ui";

const MyComponent = () => {
  return <FileDownload documentId="9b558dde-0605-4a2b-97b5-441dd160f074" />;
};
```

### 1.3 Data Representation Variants

The FileDownload component supports four visual representations:

```tsx
import { FileDownload } from "@neuron/ui";

const DataRepresentationVariants = () => {
  return (
    <div className="representation-examples">
      {/* Row representation (default) - Full download UI with file details */}
      <FileDownload documentId="document-123" dataRepresentation="row" />

      {/* Link representation - Simple text link style */}
      <FileDownload documentId="document-456" dataRepresentation="link" />

      {/* Button representation - Button-style download trigger */}
      <FileDownload documentId="document-789" dataRepresentation="button" />

      {/* Icon representation - Icon-only download trigger */}
      <FileDownload documentId="document-012" dataRepresentation="icon" />
    </div>
  );
};
```

**When to Use Each Representation:**

- **row** (default): Best for primary download areas where users need full file information (name, size, type)
- **link**: Ideal for inline text contexts where a simple link is more appropriate
- **button**: Perfect for action-focused interfaces or when downloads are a primary action
- **icon**: Use in compact spaces, toolbars, or when space is limited (e.g., table cells)

### 1.4 Download Modes

The FileDownload component supports different download modes:

```tsx
import { FileDownload } from "@neuron/ui";

const DownloadModes = () => {
  return (
    <div className="download-examples">
      {/* Standard authenticated download */}
      <FileDownload documentId="document-123" />

      {/* Public download - no authentication required */}
      <FileDownload documentId="public-document-456" isPublicDownload={true} />

      {/* Lazy download - file downloaded only when clicked */}
      <FileDownload documentId="large-document-789" isLazyDownload={true} />

      {/* Combined public and lazy download */}
      <FileDownload documentId="public-lazy-document-012" isPublicDownload={true} isLazyDownload={true} />

      {/* Button representation with lazy loading */}
      <FileDownload documentId="large-document-345" dataRepresentation="button" isLazyDownload={true} />
    </div>
  );
};
```

## Step 2: Advanced Configuration

### 2.1 Styling and Custom Classes

Apply custom styling to the FileDownload component:

```tsx
import { FileDownload } from "@neuron/ui";

const StyledFileDownload = () => {
  return (
    <div>
      {/* Default styling */}
      <FileDownload documentId="document-1" />

      {/* With custom CSS class */}
      <FileDownload documentId="document-2" className="custom-file-download my-4 shadow-sm" />

      {/* Multiple custom classes */}
      <FileDownload documentId="document-3" className="file-download-bordered file-download-highlighted p-3" />
    </div>
  );
};
```

### 2.2 Testing Integration

Implement proper test identification:

```tsx
import { FileDownload } from "@neuron/ui";

const TestableFileDownload = () => {
  return (
    <div>
      {/* Default testId (will be "fDown") */}
      <FileDownload documentId="test-document-1" />

      {/* Custom testId */}
      <FileDownload documentId="test-document-2" testId="custom-download-component" />

      {/* TestId for automated testing scenarios */}
      <FileDownload documentId="e2e-document" testId="e2e-file-download-primary" />
    </div>
  );
};
```

## Step 3: State Management Integration

### 3.1 Using Redux Selectors

Access file download state using provided hooks:

```tsx
import { FileDownload } from "@neuron/ui";
import { useDocumentFiles, useDocumentMetaData } from "@api/file-download";

const FileDownloadWithState = () => {
  const documentId = "document-123";

  // Access files and metadata using hooks
  const files = useDocumentFiles(documentId);
  const metadata = useDocumentMetaData(documentId);

  return (
    <div>
      <div className="file-info">
        <p>Files available: {files.length}</p>
        {metadata && <p>Document metadata: {metadata.length} items</p>}
      </div>

      <FileDownload documentId={documentId} isLazyDownload={true} />
    </div>
  );
};
```

### 3.2 Dynamic Document Handling

Handle dynamic document IDs and conditional rendering:

```tsx
import { FileDownload } from "@neuron/ui";
import { useState, useEffect } from "react";

const DynamicFileDownload = () => {
  const [currentDocumentId, setCurrentDocumentId] = useState<string | null>(null);
  const [isLoading, setIsLoading] = useState(true);

  useEffect(() => {
    // Simulate document ID loading
    setTimeout(() => {
      setCurrentDocumentId("loaded-document-456");
      setIsLoading(false);
    }, 1000);
  }, []);

  if (isLoading) {
    return <div>Loading document...</div>;
  }

  if (!currentDocumentId) {
    return <div>No document available</div>;
  }

  return <FileDownload documentId={currentDocumentId} isLazyDownload={true} className="dynamic-file-download" />;
};
```

## Step 4: Authentication Integration

### 4.1 Public vs Authenticated Downloads

Configure downloads based on authentication requirements:

```tsx
import { FileDownload } from "@neuron/ui";
import { useAuthenticationData } from "@neuron/auth";

const AuthAwareFileDownload = () => {
  const { isAuthenticated } = useAuthenticationData();

  return (
    <div>
      {isAuthenticated ? (
        <FileDownload documentId="private-document-123" className="authenticated-download" />
      ) : (
        <FileDownload documentId="public-document-456" isPublicDownload={true} className="public-download" />
      )}
    </div>
  );
};
```

### 4.2 Conditional Public Access

Handle documents that may require different access modes:

```tsx
import { FileDownload } from "@neuron/ui";
import { useAuthenticationData } from "@neuron/auth";

const ConditionalAccessDownload = ({
  documentId,
  requiresAuth = false,
}: {
  documentId: string;
  requiresAuth?: boolean;
}) => {
  const { isAuthenticated } = useAuthenticationData();

  // If authentication is required but user is not authenticated
  if (requiresAuth && !isAuthenticated) {
    return <div className="auth-required-message">Authentication required to download this document.</div>;
  }

  return (
    <FileDownload
      documentId={documentId}
      isPublicDownload={!requiresAuth}
      isLazyDownload={true}
      className={`conditional-download ${requiresAuth ? "auth-required" : "public-access"}`}
    />
  );
};
```

## Step 5: Error Handling and Loading States

### 5.1 Error State Handling

The component automatically handles loading and error states:

```tsx
import { FileDownload } from "@neuron/ui";

const ErrorHandlingExample = () => {
  return (
    <div className="download-section">
      <h3>Document Downloads</h3>

      {/* Component handles loading and error states internally */}
      <FileDownload documentId="potential-error-document" className="download-with-error-handling" />

      {/* Additional error context can be provided through wrapper */}
      <div className="download-wrapper">
        <p>If download fails, please contact support.</p>
        <FileDownload documentId="fallback-document" isLazyDownload={true} />
      </div>
    </div>
  );
};
```

### 5.2 Loading State Management

Handle different loading scenarios:

```tsx
import { FileDownload } from "@neuron/ui";
import { useDocumentFiles } from "@api/file-download";
import { useState } from "react";

const LoadingStateExample = () => {
  const [documentId, setDocumentId] = useState("initial-document");
  const files = useDocumentFiles(documentId);

  return (
    <div className="loading-example">
      <div className="controls">
        <button onClick={() => setDocumentId("document-1")}>Load Document 1</button>
        <button onClick={() => setDocumentId("document-2")}>Load Document 2</button>
      </div>

      {/* Show file count when available */}
      {files.length > 0 && <p className="file-count">{files.length} files available</p>}

      <FileDownload documentId={documentId} isLazyDownload={true} className="managed-loading-download" />
    </div>
  );
};
```

## Step 6: Layout and Responsive Behavior

### 6.1 Single vs Multiple File Display

The component automatically adapts its layout based on file count:

```tsx
import { FileDownload } from "@neuron/ui";

const ResponsiveLayoutExample = () => {
  return (
    <div className="layout-examples">
      {/* Single file - horizontal layout */}
      <div className="single-file-example">
        <h4>Single File Document</h4>
        <FileDownload documentId="single-file-document" className="single-file-download" />
      </div>

      {/* Multiple files - vertical layout */}
      <div className="multiple-files-example">
        <h4>Multiple Files Document</h4>
        <FileDownload documentId="multiple-files-document" className="multiple-files-download" />
      </div>
    </div>
  );
};
```

### 6.2 Custom Layout Integration

Integrate with custom layouts and grid systems:

```tsx
import { FileDownload, Panel } from "@neuron/ui";

const LayoutIntegrationExample = () => {
  return (
    <div className="grid">
      <Panel title="Document Files" className="g-col-12 g-col-md-6">
        <FileDownload documentId="panel-document-1" isLazyDownload={true} className="panel-file-download" />
      </Panel>

      <Panel title="Public Downloads" className="g-col-12 g-col-md-6">
        <FileDownload documentId="panel-document-2" isPublicDownload={true} className="panel-public-download" />
      </Panel>
    </div>
  );
};
```

## Step 7: Performance Optimization

### 7.1 Lazy Loading Strategy

Optimize performance with lazy loading:

```tsx
import { FileDownload } from "@neuron/ui";

const PerformanceOptimizedDownloads = () => {
  return (
    <div className="optimized-downloads">
      {/* Large documents - always use lazy loading */}
      <FileDownload documentId="large-document-1" isLazyDownload={true} className="large-document-download" />

      {/* Small, frequently accessed documents - immediate loading */}
      <FileDownload documentId="small-document-1" isLazyDownload={false} className="small-document-download" />

      {/* Public documents with lazy loading */}
      <FileDownload
        documentId="public-large-document"
        isPublicDownload={true}
        isLazyDownload={true}
        className="public-lazy-download"
      />
    </div>
  );
};
```

## Step 8: Integration Patterns

### 8.1 Table Integration

Integrate with data tables using compact representations:

```tsx
import { FileDownload, Table } from "@neuron/ui";

const TableIntegrationExample = () => {
  const tableData = [
    {
      id: "doc-1",
      name: "Document 1",
      documentId: "document-id-1",
    },
    {
      id: "doc-2",
      name: "Document 2",
      documentId: "document-id-2",
    },
  ];

  const columns = [
    {
      key: "name",
      header: "Document Name",
      render: (item: (typeof tableData)[0]) => item.name,
    },
    {
      key: "download",
      header: "Download",
      render: (item: (typeof tableData)[0]) => (
        // Use icon representation for compact table cells
        <FileDownload
          documentId={item.documentId}
          dataRepresentation="icon"
          isLazyDownload={true}
          className="table-cell-download"
        />
      ),
    },
  ];

  return <Table data={tableData} columns={columns} className="documents-table" />;
};
```

**Alternative Table Representations:**

```tsx
// Link representation for table cells
<FileDownload documentId={item.documentId} dataRepresentation="link" isLazyDownload={true} />

// Button representation for action-focused tables
<FileDownload documentId={item.documentId} dataRepresentation="button" isLazyDownload={true} />
```

### 8.2 Form Integration

Integrate with form components:

```tsx
import { FileDownload, Panel, Button } from "@neuron/ui";
import { useState } from "react";

const FormIntegrationExample = () => {
  const [selectedDocumentId, setSelectedDocumentId] = useState<string>("");

  const handleDocumentSelect = (documentId: string) => {
    setSelectedDocumentId(documentId);
  };

  return (
    <Panel title="Document Management">
      <div className="form-section">
        <div className="document-selector">
          <Button onClick={() => handleDocumentSelect("form-document-1")} variant="secondary">
            Load Document 1
          </Button>
          <Button onClick={() => handleDocumentSelect("form-document-2")} variant="secondary">
            Load Document 2
          </Button>
        </div>

        {selectedDocumentId && (
          <div className="selected-document">
            <h4>Selected Document Files:</h4>
            <FileDownload documentId={selectedDocumentId} isLazyDownload={true} className="form-document-download" />
          </div>
        )}
      </div>
    </Panel>
  );
};
```

## Step 9: Data Representation Use Cases

### 9.1 Row Representation (Default)

Best for primary download sections with full file information:

```tsx
import { FileDownload, Panel } from "@neuron/ui";

const RowRepresentationExample = () => {
  return (
    <Panel title="Download Documents">
      {/* Full file details with metadata */}
      <FileDownload documentId="detailed-document-1" dataRepresentation="row" />
    </Panel>
  );
};
```

### 9.2 Link Representation

Ideal for inline text and minimal UI contexts:

```tsx
import { FileDownload } from "@neuron/ui";

const LinkRepresentationExample = () => {
  return (
    <div className="document-section">
      <p>
        You can download the contract document{" "}
        <FileDownload documentId="contract-123" dataRepresentation="link" isLazyDownload={true} /> to review the terms.
      </p>
    </div>
  );
};
```

### 9.3 Button Representation

Perfect for action-oriented interfaces:

```tsx
import { FileDownload, ActionBar } from "@neuron/ui";

const ButtonRepresentationExample = () => {
  return (
    <ActionBar>
      <FileDownload documentId="report-2024" dataRepresentation="button" isLazyDownload={true} />
    </ActionBar>
  );
};
```

### 9.4 Icon Representation

Use in compact spaces and data tables:

```tsx
import { FileDownload, Tile } from "@neuron/ui";

const IconRepresentationExample = () => {
  return (
    <Tile>
      <div className="d-flex justify-content-between align-items-center">
        <span>Document Title</span>
        <FileDownload documentId="compact-doc-1" dataRepresentation="icon" isLazyDownload={true} />
      </div>
    </Tile>
  );
};
```

## Best Practices

### 1. Data Representation Selection

- Use **row** for primary download areas requiring full file information
- Use **link** for inline text contexts and minimal UI
- Use **button** for action-focused interfaces and clear call-to-actions
- Use **icon** for compact spaces like tables, tiles, and toolbars
- Consider user context and space constraints when choosing representation

### 2. Document ID Management

- Always ensure valid document IDs are provided
- Handle cases where document ID might be null or undefined
- Use meaningful document IDs that correspond to your data structure

### 3. Performance Considerations

- Use `isLazyDownload={true}` for large documents or when bandwidth is a concern
- Use immediate loading for small, frequently accessed files
- Consider the user experience when choosing loading strategies
- Combine compact representations (icon, link) with lazy loading for optimal performance in lists and tables

### 4. Security and Access Control

- Use `isPublicDownload={true}` only for truly public documents
- Ensure proper authentication is in place for private documents
- Consider implementing additional access control checks at the application level

### 5. Error Handling

- The component handles most error states automatically
- Provide user-friendly error messages in your application context
- Consider fallback options when downloads fail

### 6. Testing

- Always provide meaningful values for automated testing `testId`
- Test both public and authenticated download scenarios
- Verify error states and loading behavior

### 7. Accessibility

- The component includes built-in accessibility features
- Ensure proper labeling and context in your application
- Test with screen readers and keyboard navigation

## Troubleshooting

### Common Issues

1. **Files not loading**: Verify document ID and authentication status
2. **Public downloads failing**: Ensure `isPublicDownload={true}` is set correctly
3. **Performance issues**: Consider using lazy loading for large files
4. **Authentication errors**: Verify user is properly authenticated for private documents

### Debug Tips

- Check the Redux state for file download data
- Verify network requests in browser developer tools
- Ensure proper document metadata is available
- Test with different document types and sizes

This guide provides comprehensive instructions for implementing the FileDownload component effectively across your
Neuron applications, ensuring consistent behavior and optimal user experience.

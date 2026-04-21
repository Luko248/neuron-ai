---
agent: agent
---

# AI-Assisted Neuron FileUpload Component Integration Guide

## For the AI Assistant

Your task is to integrate and configure the Neuron FileUpload component in a React application. This guide provides
comprehensive instructions for implementing the FileUpload component, which handles file uploads with document
processing, progress tracking, and error handling across all Neuron applications.

## Sync Metadata

- **Component Version:** v2.0.9
- **Component Source:** `packages/neuron/ui/src/lib/file/fileUpload/FileUpload.tsx`
- **Guideline Command:** `/neuron-ui-fileupload`
- **Related Skill:** `neuron-ui-file`

## Introduction

The FileUpload component is a comprehensive file upload solution in the Neuron UI framework, designed to handle complex
document upload workflows with backend processing, progress tracking, and error management.

### What is the FileUpload System?

The FileUpload component provides a standardized file upload interface with support for:

- Multiple file uploads with progress tracking
- Backend document processing and status polling
- Drag and drop functionality
- File validation (size, type, extensions)
- Error handling with detailed feedback
- Redux state management for upload tracking
- Access control integration
- Custom file representations (chip or row)

### Key Features

- **Document Processing Workflow**: Full document lifecycle from upload to processing completion
- **Progress Tracking**: Real-time upload progress for individual files
- **Error Management**: Comprehensive error handling with user feedback
- **Redux Integration**: Built-in state management for document and file tracking
- **Access Control**: Built-in permission handling for upload restrictions
- **Validation**: Frontend and backend validation with detailed error messages
- **Responsive Design**: Adaptive layout for different screen sizes
- **TypeScript Support**: Full type safety with comprehensive interfaces
- **Polling Mechanism**: Automatic status polling for document processing
- **File Representations**: Support for chip and row display variants

You can refer to the Storybook documentation for visual examples and interactive demos of the FileUpload component. \*
\*Storybook Reference:\*\*

## Step 1: Basic FileUpload Implementation

### 1.1 Import the FileUpload Component

```tsx
import { FileUpload } from "@neuron/ui";
```

### 1.2 Basic Usage

Here's a simple implementation of the FileUpload component:

```tsx
import { FileUpload } from "@neuron/ui";

const MyUploadComponent = () => {
  return (
    <FileUpload
      documentAreaCode="DOCUMENTS"
      documentTypeCode="GENERAL"
      multiple={true}
      labelText="Upload Files"
      description="Select files to upload"
      onSuccess={(documentId) => {
        console.log(`Upload completed for document: ${documentId}`);
      }}
      onError={(documentId, errors) => {
        console.error(`Upload failed for document: ${documentId}`, errors);
      }}
    />
  );
};
```

### 1.3 FileUpload Variants

The FileUpload component supports different variants and data representations:

```tsx
import { FileUpload } from "@neuron/ui";

const FileUploadVariants = () => {
  return (
    <div className="upload-variants">
      {/* Basic variant with drag and drop */}
      <FileUpload
        variant="basic"
        dragAndDrop={true}
        dataRepresentation="row"
        documentAreaCode="DOCUMENTS"
        documentTypeCode="GENERAL"
        labelText="Drag & Drop Upload"
      />

      {/* Advanced variant with chip representation */}
      <FileUpload
        variant="advanced"
        dataRepresentation="chip"
        documentAreaCode="DOCUMENTS"
        documentTypeCode="GENERAL"
        labelText="Advanced Upload"
        multiple={true}
      />
    </div>
  );
};
```

### 1.4 File Validation and Limits

Configure file upload limits and validation:

```tsx
import { FileUpload } from "@neuron/ui";

const ValidatedUpload = () => {
  return (
    <FileUpload
      documentAreaCode="DOCUMENTS"
      documentTypeCode="GENERAL"
      multiple={true}
      limit={{
        all: {
          mimeTypes: ["image/jpeg", "image/png", "application/pdf"],
          extensions: [".jpg", ".jpeg", ".png", ".pdf"],
          size: 50 * 1024 * 1024, // 50MB total
          count: 10, // Maximum 10 files
        },
        single: {
          size: 10 * 1024 * 1024, // 10MB per file
        },
      }}
      labelText="Upload Images or PDFs"
      description="Maximum 10 files, 10MB each, 50MB total"
    />
  );
};
```

## Step 2: Advanced FileUpload Configuration

### 2.1 Upload Callbacks and Event Handling

Handle various upload events and status changes:

```tsx
import { FileUpload } from "@neuron/ui";
import { useState } from "react";

const AdvancedFileUpload = () => {
  const [uploadStatus, setUploadStatus] = useState<string>("ready");

  return (
    <FileUpload
      documentAreaCode="DOCUMENTS"
      documentTypeCode="GENERAL"
      multiple={true}
      pollingInterval={2000} // Poll every 2 seconds
      pollingTimeout={300000} // 5 minute timeout
      onInitialized={(documentId) => {
        console.log("Document initialized:", documentId);
        setUploadStatus("initialized");
      }}
      onSuccess={(documentId) => {
        console.log("Upload successful:", documentId);
        setUploadStatus("completed");
      }}
      onError={(documentId, errors) => {
        console.error("Upload failed:", documentId, errors);
        setUploadStatus("failed");
      }}
      onDocumentFilesChanged={(documentId, files) => {
        console.log("Files changed:", documentId, files);
      }}
      onDocumentStatusChanged={(documentId, previousStatus, currentStatus) => {
        console.log("Status changed:", {
          documentId,
          from: previousStatus,
          to: currentStatus,
        });
      }}
      onDocumentFileUploadProgressChanged={(documentId, uploadKey, progress) => {
        console.log("Progress update:", { documentId, uploadKey, progress });
      }}
      labelText="Advanced Upload"
      description={`Status: ${uploadStatus}`}
    />
  );
};
```

### 2.2 Access Control Integration

Implement access control for upload functionality:

```tsx
import { FileUpload } from "@neuron/ui";

const AccessControlledUpload = () => {
  return (
    <FileUpload
      documentAreaCode="DOCUMENTS"
      documentTypeCode="CONFIDENTIAL"
      readonlyAccess="DOCUMENT_READ"
      fullAccess="DOCUMENT_WRITE"
      fallback={<div>You don't have permission to upload files</div>}
      labelText="Confidential Documents"
      description="Requires write permissions"
    />
  );
};
```

### 2.3 Using FileUpload Ref

Access upload state and methods through component ref:

```tsx
import { FileUpload, FileUploadRef } from "@neuron/ui";
import { useRef, useEffect } from "react";

const RefBasedUpload = () => {
  const fileUploadRef = useRef<FileUploadRef>(null);

  useEffect(() => {
    // Access upload information through ref
    const checkUploadStatus = () => {
      if (fileUploadRef.current) {
        const { documentId, documentStatus, errors, hasErrors } = fileUploadRef.current;
        console.log("Upload info:", {
          documentId,
          status: documentStatus,
          hasErrors,
          errors,
        });
      }
    };

    const interval = setInterval(checkUploadStatus, 5000);
    return () => clearInterval(interval);
  }, []);

  return (
    <FileUpload
      ref={fileUploadRef}
      documentAreaCode="DOCUMENTS"
      documentTypeCode="GENERAL"
      labelText="Monitored Upload"
    />
  );
};
```

## Step 3: Redux Integration and Selectors

### 3.1 Using FileUpload Redux Selectors

The FileUpload component uses Redux for state management. Here are the available selectors and hooks:

```tsx
import {
  useDocument,
  useDocumentFiles,
  useDocumentStatus,
  useFilesProgress,
  useFileUploadProgress,
  useGetDocumentStatusIsPolling,
} from "@neuron/ui/file-upload";

const FileUploadMonitor = ({ documentId }: { documentId: string }) => {
  // Get complete document information
  const document = useDocument(documentId);

  // Get document files
  const documentFiles = useDocumentFiles(documentId);

  // Get document status
  const documentStatus = useDocumentStatus(documentId);

  // Get files upload progress
  const filesProgress = useFilesProgress(documentId);

  // Get specific file progress
  const fileProgress = useFileUploadProgress(documentId, "file-upload-key");

  // Check if status polling is active
  const isPolling = useGetDocumentStatusIsPolling(documentId);

  return (
    <div>
      <h3>Upload Monitor</h3>
      <p>Status: {documentStatus}</p>
      <p>Files: {documentFiles?.length || 0}</p>
      <p>Is Polling: {isPolling ? "Yes" : "No"}</p>

      {filesProgress && (
        <div>
          <h4>Upload Progress</h4>
          {Object.entries(filesProgress).map(([uploadKey, progress]) => (
            <div key={uploadKey}>
              {uploadKey}: {progress}%
            </div>
          ))}
        </div>
      )}
    </div>
  );
};
```

### 3.2 Redux State Structure

Understanding the FileUpload Redux state structure:

```tsx
// FileUpload slice state structure
interface FileUploadSliceState {
  [documentId: string]: {
    documentFiles: FileUploadItemExtended[];
    documentUploadStatus: DocumentStatus;
    uploadFilesProgress: Record<string, number>;
    isPolling: boolean;
  };
}

// Document statuses
type DocumentStatus =
  | "INITIALIZED"
  | "VALIDATING"
  | "VALIDATION_FAILED"
  | "PREPARED"
  | "UPLOADING"
  | "UPLOAD_FAILED"
  | "PROCESSING"
  | "DONE_SUCCESS"
  | "DONE_FAILURE";
```

### 3.3 Custom Redux Selectors

Create custom selectors for specific use cases:

```tsx
import { createSelector } from "@reduxjs/toolkit";
import { selectFileUploadSlice } from "@neuron/ui/file-upload";

// Custom selector for completed uploads
const selectCompletedDocuments = createSelector(selectFileUploadSlice, (slice) => {
  if (!slice) return [];
  return Object.entries(slice)
    .filter(([, document]) => document.documentUploadStatus === "DONE_SUCCESS")
    .map(([documentId]) => documentId);
});

// Custom selector for failed uploads
const selectFailedDocuments = createSelector(selectFileUploadSlice, (slice) => {
  if (!slice) return [];
  return Object.entries(slice)
    .filter(
      ([, document]) =>
        document.documentUploadStatus === "DONE_FAILURE" || document.documentUploadStatus === "UPLOAD_FAILED",
    )
    .map(([documentId]) => documentId);
});

// Custom selector for total upload progress
const selectTotalUploadProgress = (documentId: string) =>
  createSelector(selectFileUploadSlice, (slice) => {
    const document = slice?.[documentId];
    if (!document?.uploadFilesProgress) return 0;

    const progresses = Object.values(document.uploadFilesProgress);
    if (progresses.length === 0) return 0;

    return progresses.reduce((sum, progress) => sum + progress, 0) / progresses.length;
  });
```

## Step 4: Error Handling and Validation

### 4.1 Frontend Validation

Handle client-side validation errors:

```tsx
import { FileUpload } from "@neuron/ui";

const ValidatedFileUpload = () => {
  const handleError = (documentId: string, errors: readonly any[]) => {
    errors.forEach((error) => {
      switch (error.code) {
        case "INVALID_MIME_TYPE":
          console.error("Invalid file type:", error.fileName);
          break;
        case "SIZE_LIMIT_EXCEEDED":
          console.error("File too large:", error.fileName);
          break;
        case "FILES_COUNT_LIMIT_EXCEEDED":
          console.error("Too many files selected");
          break;
        default:
          console.error("Upload error:", error);
      }
    });
  };

  return (
    <FileUpload
      documentAreaCode="DOCUMENTS"
      documentTypeCode="GENERAL"
      limit={{
        all: {
          mimeTypes: ["image/jpeg", "application/pdf"],
          count: 5,
          size: 25 * 1024 * 1024, // 25MB total
        },
        single: {
          size: 5 * 1024 * 1024, // 5MB per file
        },
      }}
      onError={handleError}
      labelText="Validated Upload"
      description="JPG or PDF files only, max 5 files, 5MB each"
    />
  );
};
```

### 4.2 Backend Error Handling

Handle server-side processing errors:

```tsx
import { FileUpload } from "@neuron/ui";

const BackendIntegratedUpload = () => {
  const handleError = (documentId: string, errors: readonly any[]) => {
    errors.forEach((error) => {
      if (error.errorType === "BUSINESS") {
        // Handle business logic errors
        console.error("Business error:", error.detail);
      } else if (error.errorType === "SYSTEM") {
        // Handle system errors
        console.error("System error:", error.detail);
      } else if (error.errorType === "VALIDATION") {
        // Handle validation errors
        console.error("Validation error:", error.detail);
      }
    });
  };

  return (
    <FileUpload
      documentAreaCode="DOCUMENTS"
      documentTypeCode="GENERAL"
      apiEndpointUrlVariant="internal" // or "external"
      pollingInterval={3000}
      pollingTimeout={600000} // 10 minutes
      onError={handleError}
      labelText="Backend Integrated Upload"
    />
  );
};
```

## Step 5: Best Practices and Patterns

### 5.1 Component Composition

Compose FileUpload with other components:

```tsx
import { FileUpload, Card, Button } from "@neuron/ui";
import { useState } from "react";

const FileUploadCard = () => {
  const [isUploading, setIsUploading] = useState(false);

  return (
    <Card>
      <Card.Header>
        <h3>Document Upload</h3>
      </Card.Header>
      <Card.Body>
        <FileUpload
          documentAreaCode="DOCUMENTS"
          documentTypeCode="GENERAL"
          multiple={true}
          dragAndDrop={true}
          onInitialized={() => setIsUploading(true)}
          onSuccess={() => setIsUploading(false)}
          onError={() => setIsUploading(false)}
          labelText="Upload Documents"
          disabled={isUploading}
        />
      </Card.Body>
      <Card.Footer>
        <Button disabled={isUploading}>{isUploading ? "Uploading..." : "Continue"}</Button>
      </Card.Footer>
    </Card>
  );
};
```

### 5.2 Performance Optimization

Optimize FileUpload performance:

```tsx
import { FileUpload } from "@neuron/ui";
import { memo, useCallback } from "react";

const OptimizedFileUpload = memo(() => {
  const handleSuccess = useCallback((documentId: string) => {
    console.log("Upload completed:", documentId);
  }, []);

  const handleError = useCallback((documentId: string, errors: any[]) => {
    console.error("Upload failed:", documentId, errors);
  }, []);

  return (
    <FileUpload
      documentAreaCode="DOCUMENTS"
      documentTypeCode="GENERAL"
      pollingInterval={5000} // Longer interval for better performance
      onSuccess={handleSuccess}
      onError={handleError}
      labelText="Optimized Upload"
    />
  );
});
```

## Common Integration Patterns

### Pattern 1: Multi-Step Upload Process

```tsx
import { FileUpload } from "@neuron/ui";
import { useState } from "react";

const MultiStepUpload = () => {
  const [step, setStep] = useState(1);
  const [documentId, setDocumentId] = useState<string | null>(null);

  return (
    <div>
      {step === 1 && (
        <FileUpload
          documentAreaCode="DOCUMENTS"
          documentTypeCode="GENERAL"
          onInitialized={(id) => setDocumentId(id)}
          onSuccess={() => setStep(2)}
          labelText="Step 1: Upload Files"
        />
      )}
      {step === 2 && (
        <div>
          <h3>Step 2: Processing Complete</h3>
          <p>Document ID: {documentId}</p>
        </div>
      )}
    </div>
  );
};
```

### Pattern 2: Batch Upload Management

```tsx
import { FileUpload } from "@neuron/ui";
import { useState } from "react";

const BatchUploadManager = () => {
  const [uploads, setUploads] = useState<string[]>([]);

  const addUpload = () => {
    const uploadId = `upload-${Date.now()}`;
    setUploads((prev) => [...prev, uploadId]);
  };

  return (
    <div>
      <Button onClick={addUpload}>Add Upload</Button>
      {uploads.map((uploadId) => (
        <FileUpload
          key={uploadId}
          documentAreaCode="DOCUMENTS"
          documentTypeCode="GENERAL"
          labelText={`Upload ${uploadId}`}
          onSuccess={(documentId) => {
            console.log(`Upload ${uploadId} completed:`, documentId);
          }}
        />
      ))}
    </div>
  );
};
```

## Integration Checklist

When integrating FileUpload component, ensure:

- Document area and type codes are correctly specified
- File validation limits are appropriate for your use case
- Error handling callbacks are implemented
- Access control permissions are configured
- Redux selectors are used for state monitoring
- Progress tracking is implemented if needed
- Polling intervals are optimized for performance
- Component refs are used when direct access is required
- Proper TypeScript types are imported and used
- Component is properly memoized for performance if needed

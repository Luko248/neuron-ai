---
agent: agent
---

# RadioSet Component AI Guidelines

## For the AI Assistant

Your task is to integrate and configure the Neuron RadioSet component in React applications. This guide provides essential instructions for implementing RadioSet components, which provide comprehensive single-selection functionality with validation, accessibility, and enhanced UX features through React Hook Form integration across all Neuron applications.

## Overview

The RadioSet component is a comprehensive single-selection input field built on PrimeReact RadioButton, providing validation, accessibility, and enhanced UX features through React Hook Form integration. It supports both static options and dynamic code table integration for scalable data-driven applications.

## Core Usage

### Basic Implementation with Static Options

```tsx
import { Form } from "@neuron/ui";
import { useForm } from "react-hook-form";
import { zodResolver } from "@hookform/resolvers/zod";
import { z } from "zod";

const schema = z.object({
  priority: z.enum(["high", "medium", "low"], {
    required_error: "Please select a priority level",
  }),
  theme: z.string().min(1, { message: "Theme selection is required" }),
  notifications: z.enum(["all", "important", "none"]).optional(),
});

const priorityOptions = [
  { value: "high", labelText: "High Priority" },
  { value: "medium", labelText: "Medium Priority" },
  { value: "low", labelText: "Low Priority" },
];

const themeOptions = [
  { value: "light", labelText: "Light Mode" },
  { value: "dark", labelText: "Dark Mode" },
  { value: "auto", labelText: "Auto (System)" },
];

const MyForm = () => {
  const { control } = useForm({
    resolver: zodResolver(schema),
    mode: "onChange",
  });

  return (
    <>
      <Form.RadioSet
        name="priority"
        labelText="Task Priority"
        control={control}
        options={priorityOptions}
        required
        requiredFlag={true}
      />

      <Form.RadioSet
        name="theme"
        labelText="Theme Preference"
        control={control}
        options={themeOptions}
        inline={true}
        description="Choose your preferred interface theme"
      />

      <Form.RadioSet
        name="notifications"
        labelText="Notification Level"
        control={control}
        options={notificationOptions}
        tooltip="Control which notifications you receive"
      />
    </>
  );
};
```

### Code Table Integration

```tsx
import { Form } from "@neuron/ui";
import { CT } from "app/common/codetables";

const MyForm = () => {
  const { control } = useForm({
    resolver: zodResolver(schema),
    mode: "onChange",
  });

  return (
    <>
      <Form.RadioSet
        codeTableName={CT.CT_PRIORITIES}
        name="priority"
        labelText="Task Priority"
        control={control}
        required
      />

      <Form.RadioSet
        codeTableName={CT.CT_DEPARTMENTS}
        name="department"
        labelText="Department"
        control={control}
        handleCodeTableFilter={(items) => items.filter((item) => item.valid && item.primaryCategory === "active")}
        inline={true}
      />
    </>
  );
};
```

## Key Props Reference

### Essential Props

- **name**: Field name for form registration
- **control**: React Hook Form control object
- **labelText**: Accessible label text for the radio set
- **options**: Static array of options (alternative to codeTableName)
- **codeTableName**: Code table identifier for dynamic options

### Data Source Props

- **options**: Static options array with `value` and `labelText` properties
- **codeTableName**: String identifier for code table (e.g., `CT.CT_PRIORITIES`)
- **handleCodeTableFilter**: Function to filter code table results

### Validation & States

- **required**: HTML5 required attribute
- **requiredFlag**: Shows required indicator (\*)
- **optional**: Shows optional indicator
- **isValid**: Manual valid state indicator

### Enhanced Features

- **tooltip**: Help text with tooltip integration
- **description**: Helper text above the radio set
- **descriptionVariant**: `"default"` | `"success"` | `"warning"` | `"danger"` | `"info"`
- **literalView**: Display as read-only text instead of radio buttons
- **testId**: Custom test identifier for testing

### Layout & Styling

- **className**: CSS classes for grid positioning and custom styling
- **inline**: Display radio buttons horizontally instead of vertically
- **disabled**: Disable all radio button interactions
- **readOnly**: Make all radio buttons read-only

### Access Control

- **readonlyAccess**: Role-based read access control
- **fullAccess**: Role-based full access control

## Code Table Integration Showcase

### Static Code Table Examples

#### Priority Levels

```tsx
// app/common/codetables/codeTableTypes.ts
export const CT = {
  CT_PRIORITIES: "priorities" as const,
  CT_STATUS_LEVELS: "statusLevels" as const,
  CT_USER_ROLES: "userRoles" as const,
  CT_DEPARTMENTS: "departments" as const,
} as const;

// app/common/codetables/defs/priorities.codeTable.ts
import { createStaticCodeTable, ICodeTableItem } from "@neuron/core";
import { CT } from "../codeTableTypes";

const priorityData: ICodeTableItem[] = [
  {
    id: { code: "urgent" },
    code: "urgent",
    name: "Urgent",
    description: "Requires immediate attention",
    valid: true,
    primaryCategory: "critical",
    extendedData: [
      { name: "color", value: "#dc3545" },
      { name: "icon", value: "exclamation-triangle" },
    ],
  },
  {
    id: { code: "high" },
    code: "high",
    name: "High Priority",
    description: "Important tasks with tight deadlines",
    valid: true,
    primaryCategory: "important",
    extendedData: [
      { name: "color", value: "#fd7e14" },
      { name: "icon", value: "arrow-up" },
    ],
  },
  {
    id: { code: "medium" },
    code: "medium",
    name: "Medium Priority",
    description: "Standard priority tasks",
    valid: true,
    primaryCategory: "standard",
    extendedData: [
      { name: "color", value: "#ffc107" },
      { name: "icon", value: "minus" },
    ],
  },
  {
    id: { code: "low" },
    code: "low",
    name: "Low Priority",
    description: "Tasks that can be completed when time allows",
    valid: true,
    primaryCategory: "flexible",
    extendedData: [
      { name: "color", value: "#28a745" },
      { name: "icon", value: "arrow-down" },
    ],
  },
];

export const ctPrioritiesDef = createStaticCodeTable(CT.CT_PRIORITIES, priorityData);

// Usage in component
<Form.RadioSet
  codeTableName={CT.CT_PRIORITIES}
  name="taskPriority"
  labelText="Task Priority Level"
  control={control}
  required
  description="Select the urgency level for this task"
/>;
```

#### User Roles

```tsx
// app/common/codetables/defs/userRoles.codeTable.ts
const userRolesData: ICodeTableItem[] = [
  {
    id: { code: "admin" },
    code: "admin",
    name: "Administrator",
    description: "Full system access with all permissions",
    valid: true,
    primaryCategory: "system",
    extendedData: [
      { name: "permissions", value: "all" },
      { name: "level", value: "5" },
    ],
  },
  {
    id: { code: "manager" },
    code: "manager",
    name: "Manager",
    description: "Department management with team oversight",
    valid: true,
    primaryCategory: "management",
    extendedData: [
      { name: "permissions", value: "team,reports" },
      { name: "level", value: "4" },
    ],
  },
  {
    id: { code: "user" },
    code: "user",
    name: "Standard User",
    description: "Regular user with standard permissions",
    valid: true,
    primaryCategory: "standard",
    extendedData: [
      { name: "permissions", value: "basic" },
      { name: "level", value: "2" },
    ],
  },
  {
    id: { code: "guest" },
    code: "guest",
    name: "Guest User",
    description: "Limited read-only access",
    valid: true,
    primaryCategory: "limited",
    extendedData: [
      { name: "permissions", value: "read" },
      { name: "level", value: "1" },
    ],
  },
];

export const ctUserRolesDef = createStaticCodeTable(CT.CT_USER_ROLES, userRolesData);

<Form.RadioSet
  codeTableName={CT.CT_USER_ROLES}
  name="userRole"
  labelText="User Role"
  control={control}
  required
  inline={true}
  handleCodeTableFilter={(items) => items.filter((item) => item.valid && item.primaryCategory !== "system")}
/>;
```

### Remote Code Table Examples

#### Departments from API

```tsx
// app/common/codetables/defs/departments.codeTable.ts
import { createRemoteCodeTable, registerCodeTable } from "@neuron/core";
import { CT } from "../codeTableTypes";
import { DepartmentsAdapter } from "../adapter/departments.adapter";

export const ctDepartmentsDef = createRemoteCodeTable(CT.CT_DEPARTMENTS, "/api/departments", {
  adapter: new DepartmentsAdapter(),
  queryParams: {
    active: "true",
    includeSubdepartments: "false",
  },
  publicFacing: false,
  baseUrl: "https://api.company.com",
});

// Register to generate hooks and selectors
export const { useDepartmentsQuery, selectDepartments, selectDepartmentss, departmentsEndpoint } =
  registerCodeTable(ctDepartmentsDef);

// Custom adapter for departments
export class DepartmentsAdapter implements ICodeTableAdapter<DepartmentRemoteResponse, ICodeTableItem> {
  public processItems = (input: DepartmentRemoteResponse): ICodeTableItem[] => {
    return input.departments.map(this.processItem);
  };

  public processItem = (input: DepartmentRemoteItem): ICodeTableItem => ({
    id: { code: input.deptCode },
    code: input.deptCode,
    name: input.departmentName,
    description: input.description,
    validFrom: input.effectiveDate,
    validTo: input.expiryDate,
    valid: input.isActive,
    primaryCategory: input.category,
    extendedData: [
      { name: "manager", value: input.managerName },
      { name: "location", value: input.location },
      { name: "budget", value: input.budgetCode },
    ],
  });
}

// Usage with enhanced filtering
const handleDepartmentFilter = useCallback((items: ICodeTableItem[]) => {
  return items
    .filter((item) => item.valid && !item.deleted)
    .sort((a, b) => a.name?.localeCompare(b.name || "") || 0)
    .map((item) => ({
      ...item,
      labelText: `${item.name} (${item.code})`,
      disabled: item.primaryCategory === "restricted",
    }));
}, []);

<Form.RadioSet
  codeTableName={CT.CT_DEPARTMENTS}
  name="department"
  labelText="Department"
  control={control}
  handleCodeTableFilter={handleDepartmentFilter}
  required
  description="Select your department"
/>;
```

#### Status Levels with Complex Logic

```tsx
// app/common/codetables/defs/statusLevels.codeTable.ts
export const ctStatusLevelsDef = createRemoteCodeTable(CT.CT_STATUS_LEVELS, "/api/status-levels", {
  adapter: new StatusLevelsAdapter(),
  queryParams: {
    version: "latest",
    includeInactive: "false",
  },
  publicFacing: false,
});

// Usage with conditional filtering based on user role
const { watch } = useForm({ control });
const userRole = watch("userRole");

const handleStatusFilter = useCallback(
  (items: ICodeTableItem[]) => {
    return items.filter((item) => {
      if (!item.valid) return false;

      // Filter based on user role
      switch (userRole) {
        case "admin":
          return true; // Admin can see all statuses
        case "manager":
          return item.primaryCategory !== "system";
        case "user":
          return ["standard", "user"].includes(item.primaryCategory || "");
        default:
          return item.primaryCategory === "public";
      }
    });
  },
  [userRole],
);

<Form.RadioSet
  codeTableName={CT.CT_STATUS_LEVELS}
  name="status"
  labelText="Status Level"
  control={control}
  handleCodeTableFilter={handleStatusFilter}
  deps={["userRole"]} // Revalidate when userRole changes
/>;
```

### Advanced Code Table Filtering

#### Multi-Criteria Filtering

```tsx
const handleAdvancedFilter = useCallback((items: ICodeTableItem[]) => {
  const currentDate = new Date();

  return items
    .filter((item) => {
      // Basic validity check
      if (!item.valid || item.deleted) return false;

      // Date range validation
      if (item.validFrom && new Date(item.validFrom) > currentDate) return false;
      if (item.validTo && new Date(item.validTo) < currentDate) return false;

      // Category-based filtering
      const allowedCategories = ["standard", "premium", "enterprise"];
      if (item.primaryCategory && !allowedCategories.includes(item.primaryCategory)) {
        return false;
      }

      return true;
    })
    .sort((a, b) => {
      // Sort by priority (from extendedData), then by name
      const aPriority = parseInt(a.extendedData?.find((d) => d.name === "priority")?.value || "0");
      const bPriority = parseInt(b.extendedData?.find((d) => d.name === "priority")?.value || "0");

      if (aPriority !== bPriority) {
        return bPriority - aPriority; // Higher priority first
      }

      return a.name?.localeCompare(b.name || "") || 0;
    })
    .map((item) => ({
      ...item,
      // Enhance display with additional information
      labelText: `${item.name}${item.description ? ` - ${item.description}` : ""}`,
      disabled: item.primaryCategory === "maintenance",
    }));
}, []);

<Form.RadioSet
  codeTableName={CT.CT_COMPLEX_OPTIONS}
  name="complexSelection"
  labelText="Advanced Options"
  control={control}
  handleCodeTableFilter={handleAdvancedFilter}
  description="Options are filtered based on current date and user permissions"
/>;
```

## Validation Patterns

### Zod Schema Validation

```tsx
// Enum validation for predefined values
const configSchema = z.object({
  environment: z.enum(["development", "staging", "production"], {
    required_error: "Please select an environment",
  }),

  logLevel: z.enum(["debug", "info", "warn", "error"]),

  deployment: z.string().min(1, { message: "Deployment type is required" }),
});

// Union type validation with custom messages
const accessSchema = z.object({
  accessLevel: z.union([z.literal("read"), z.literal("write"), z.literal("admin")]).refine((val) => val !== undefined, {
    message: "Access level must be selected",
  }),
});

// Conditional validation based on other fields
const subscriptionSchema = z
  .object({
    plan: z.enum(["basic", "premium", "enterprise"]),
    billingCycle: z.enum(["monthly", "yearly"]).optional(),
  })
  .refine(
    (data) => {
      // Enterprise plans require yearly billing
      if (data.plan === "enterprise") {
        return data.billingCycle === "yearly";
      }
      return true;
    },
    {
      message: "Enterprise plans require yearly billing",
      path: ["billingCycle"],
    },
  );
```

### Complex Business Logic Validation

```tsx
const projectSchema = z
  .object({
    priority: z.enum(["low", "medium", "high", "urgent"]),
    department: z.string(),
    assignee: z.string().optional(),
  })
  .refine(
    (data) => {
      // High/urgent priority tasks require assignee
      if (["high", "urgent"].includes(data.priority)) {
        return data.assignee && data.assignee.length > 0;
      }
      return true;
    },
    {
      message: "High and urgent priority tasks must have an assignee",
      path: ["assignee"],
    },
  )
  .refine(
    (data) => {
      // IT department can only have urgent or high priority
      if (data.department === "IT") {
        return ["high", "urgent"].includes(data.priority);
      }
      return true;
    },
    {
      message: "IT department tasks must be high or urgent priority",
      path: ["priority"],
    },
  );
```

## Advanced Features

### Custom Change Handling with Business Logic

```tsx
const handlePriorityChange = useCallback(
  (value: string, event: ChangeEvent<HTMLInputElement>) => {
    console.log("Priority changed:", value);

    // Auto-assign based on priority
    if (value === "urgent") {
      setValue("assignee", "emergency-team");
      setValue("notifications", "all");
    } else if (value === "low") {
      setValue("notifications", "none");
    }

    // Update deadline based on priority
    const deadlines = {
      urgent: new Date(Date.now() + 24 * 60 * 60 * 1000), // 1 day
      high: new Date(Date.now() + 3 * 24 * 60 * 60 * 1000), // 3 days
      medium: new Date(Date.now() + 7 * 24 * 60 * 60 * 1000), // 1 week
      low: new Date(Date.now() + 14 * 24 * 60 * 60 * 1000), // 2 weeks
    };

    if (deadlines[value as keyof typeof deadlines]) {
      setValue("deadline", deadlines[value as keyof typeof deadlines].toISOString().split("T")[0]);
    }

    // Prevent default if needed
    if (someCondition) {
      event.preventDefault();
    }
  },
  [setValue],
);

<Form.RadioSet
  name="priority"
  labelText="Task Priority"
  control={control}
  codeTableName={CT.CT_PRIORITIES}
  onChange={handlePriorityChange}
/>;
```

### Literal View Mode

```tsx
<Form.RadioSet
  name="selectedStatus"
  labelText="Current Status"
  control={control}
  codeTableName={CT.CT_STATUS_LEVELS}
  literalView={true} // Shows selected option text
/>
```

### Inline Layout with Description

```tsx
<Form.RadioSet
  name="theme"
  labelText="Interface Theme"
  control={control}
  options={themeOptions}
  inline={true}
  description="Choose your preferred interface appearance"
  descriptionVariant="info"
/>
```

## Role-Based Access Control

### Dynamic Access Control

```tsx
const { userPermissions } = useAuth();

<Form.RadioSet
  name="securityLevel"
  labelText="Security Level"
  control={control}
  codeTableName={CT.CT_SECURITY_LEVELS}
  fullAccess={userPermissions.includes("security_admin") ? "admin" : undefined}
  readonlyAccess={userPermissions.includes("security_read") ? "user" : undefined}
  handleCodeTableFilter={(items) =>
    items.filter((item) => {
      // Filter based on user permissions
      if (userPermissions.includes("security_admin")) return true;
      if (userPermissions.includes("security_user")) {
        return item.primaryCategory !== "admin";
      }
      return item.primaryCategory === "public";
    })
  }
/>;
```

## Testing Integration

### Comprehensive Testing Patterns

```tsx
<Form.RadioSet
  name="testableField"
  labelText="Testable radio set"
  control={control}
  codeTableName={CT.CT_TEST_OPTIONS}
  testId="priority-selection"
/>;

// In tests
describe("RadioSet Component", () => {
  it("should render with code table options", async () => {
    render(<TestForm />);

    const radioSet = screen.getByTestId("priority-selection");
    expect(radioSet).toBeInTheDocument();

    // Wait for code table to load
    await waitFor(() => {
      expect(screen.getByLabelText("High Priority")).toBeInTheDocument();
    });
  });

  it("should handle selection changes", async () => {
    const onSubmit = jest.fn();
    render(<TestForm onSubmit={onSubmit} />);

    const highPriority = screen.getByLabelText("High Priority");
    fireEvent.click(highPriority);

    const submitButton = screen.getByRole("button", { name: "Submit" });
    fireEvent.click(submitButton);

    await waitFor(() => {
      expect(onSubmit).toHaveBeenCalledWith(expect.objectContaining({ priority: "high" }));
    });
  });

  it("should apply code table filtering", async () => {
    render(<TestForm />);

    await waitFor(() => {
      // Should show filtered options only
      expect(screen.getByLabelText("High Priority")).toBeInTheDocument();
      expect(screen.queryByLabelText("Disabled Option")).not.toBeInTheDocument();
    });
  });
});
```

## Performance Considerations

### Optimized Code Table Usage

```tsx
const RadioSetGroup = memo(({ control }: { control: Control }) => {
  // Memoize filter function to prevent unnecessary re-renders
  const handleFilter = useCallback((items: ICodeTableItem[]) => {
    return items.filter((item) => item.valid && !item.deleted).sort((a, b) => a.name?.localeCompare(b.name || "") || 0);
  }, []);

  // Memoize options transformation
  const enhancedFilter = useMemo(
    () => (items: ICodeTableItem[]) => {
      return handleFilter(items).map((item) => ({
        ...item,
        labelText: `${item.name} (${item.code})`,
        disabled: item.primaryCategory === "maintenance",
      }));
    },
    [handleFilter],
  );

  return (
    <Form.RadioSet
      name="optimizedField"
      labelText="Optimized Radio Set"
      control={control}
      codeTableName={CT.CT_LARGE_DATASET}
      handleCodeTableFilter={enhancedFilter}
    />
  );
});
```

## Troubleshooting

### Common Issues and Solutions

1. **Code Table Not Loading**

   ```tsx
   // Check code table registration
   const { data, error, isLoading } = useDepartmentsQuery();
   console.log("Code table state:", { data, error, isLoading });
   ```

2. **Validation Not Working**

   ```tsx
   // Ensure proper Zod enum setup
   const schema = z.object({
     status: z.enum(["active", "inactive"], {
       required_error: "Status is required",
     }),
   });
   ```

3. **Filtering Not Applied**
   ```tsx
   // Ensure filter function returns array
   const handleFilter = useCallback((items: ICodeTableItem[]) => {
     return items.filter((item) => item.valid); // Must return array
   }, []);
   ```

### Debug Utilities

```tsx
// Debug component state
const DebugRadioSet = ({ name, control, ...props }) => {
  const { watch } = useForm({ control });
  const value = watch(name);

  useEffect(() => {
    console.log(`RadioSet ${name} value:`, value);
  }, [name, value]);

  return <Form.RadioSet name={name} control={control} {...props} />;
};
```

## Version Information

- **Component Version**: v4.0.0
- **Features**: React Hook Form integration, code table support, role-based access control
- **Dependencies**: PrimeReact RadioButton, React Hook Form, Zod validation, @neuron/core code tables

## Sync Metadata

- **Component Version:** v4.0.3
- **Component Source:** `packages/neuron/ui/src/lib/form/radioSet/RadioSet.tsx`
- **Guideline Command:** `/neuron-ui-radioset`
- **Related Skill:** `neuron-ui-form-choice`

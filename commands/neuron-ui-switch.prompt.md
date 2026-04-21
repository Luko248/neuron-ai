---
agent: agent
---

# Switch Component AI Guidelines

## For the AI Assistant

Your task is to integrate and configure the Neuron Switch component in React applications. This guide provides essential instructions for implementing Switch components, which provide intuitive toggle controls for boolean values with validation, accessibility features, focus state improvements, and role-based access control through React Hook Form integration across all Neuron applications.

## Overview

The Switch component is a toggle input control built on PrimeReact InputSwitch that provides an intuitive on/off interface for boolean values. It integrates seamlessly with React Hook Form and includes comprehensive validation, accessibility features, and role-based access control.

## Core Usage

### Basic Implementation

```tsx
import { Form } from "@neuron/ui";
import { useForm } from "react-hook-form";
import { zodResolver } from "@hookform/resolvers/zod";
import { z } from "zod";

const schema = z.object({
  notifications: z.boolean(),
  emailAlerts: z.boolean().optional(),
  darkMode: z.boolean().default(false),
  termsAccepted: z.boolean().refine((val) => val === true, {
    message: "You must accept the terms and conditions",
  }),
});

const SettingsForm = () => {
  const { control, handleSubmit } = useForm({
    resolver: zodResolver(schema),
    mode: "onChange",
    defaultValues: {
      notifications: true,
      emailAlerts: false,
      darkMode: false,
      termsAccepted: false,
    },
  });

  const onSubmit = (data) => {
    console.log("Settings:", data);
  };

  return (
    <form onSubmit={handleSubmit(onSubmit)}>
      <Form.Switch
        name="notifications"
        labelText="Enable Notifications"
        control={control}
        description="Receive push notifications for important updates"
        testId="notifications-switch"
      />

      <Form.Switch
        name="emailAlerts"
        labelText="Email Alerts"
        control={control}
        description="Get email notifications for critical events"
        testId="email-alerts-switch"
      />

      <Form.Switch
        name="darkMode"
        labelText="Dark Mode"
        control={control}
        description="Use dark theme for better visibility in low light"
        testId="dark-mode-switch"
      />

      <Form.Switch
        name="termsAccepted"
        labelText="I accept the terms and conditions"
        control={control}
        required
        description="Required to proceed with registration"
        testId="terms-switch"
      />
    </form>
  );
};
```

### Controlled vs Uncontrolled Usage

```tsx
const SwitchExamples = () => {
  const { control, watch, setValue } = useForm({
    defaultValues: {
      controlledSwitch: false,
      dependentSwitch: false,
      masterSwitch: true,
    },
  });

  const masterSwitchValue = watch("masterSwitch");
  const controlledValue = watch("controlledSwitch");

  return (
    <>
      {/* Basic controlled switch */}
      <Form.Switch
        name="controlledSwitch"
        labelText="Controlled Switch"
        control={control}
        description="This switch is controlled by React Hook Form"
      />

      {/* Switch with external checked prop */}
      <Form.Switch
        name="externalSwitch"
        labelText="External Control"
        control={control}
        checked={controlledValue}
        description="Value controlled externally"
      />

      {/* Master switch controlling others */}
      <Form.Switch
        name="masterSwitch"
        labelText="Master Switch"
        control={control}
        description="Controls availability of other options"
      />

      {/* Dependent switch */}
      <Form.Switch
        name="dependentSwitch"
        labelText="Dependent Option"
        control={control}
        disabled={!masterSwitchValue}
        description="Only available when master switch is enabled"
      />
    </>
  );
};
```

## Key Props Reference

### Essential Props

- **name**: Field name for form registration (required)
- **control**: React Hook Form control object
- **labelText**: Label text displayed next to the switch
- **testId**: Custom test identifier for testing

### Switch Configuration

- **checked**: External control of switch state (overrides form value)
- **onChange**: Custom change event handler
- **disabled**: Disable switch interactions
- **readOnly**: Make switch read-only (same effect as disabled)

### Validation & States

- **required**: Indicates the switch must be checked (useful for terms acceptance)
- **deps**: Dependencies for form field updates

### Enhanced Features

- **description**: Helper text below the switch
- **descriptionVariant**: `"default"` | `"success"` | `"warning"` | `"danger"` | `"info"`
- **literalView**: Display as read-only text instead of interactive switch

### Layout & Styling

- **className**: CSS classes for the switch wrapper
- **formGroupClassName**: CSS classes for the form group container

### Access Control

- **readonlyAccess**: Role-based read access control
- **fullAccess**: Role-based full access control

## Validation Patterns

### Zod Schema Validation

```tsx
// Basic boolean validation
const basicSwitchSchema = z.object({
  isEnabled: z.boolean(),
  isActive: z.boolean().optional(),
  isRequired: z.boolean().refine((val) => val === true, {
    message: "This option must be enabled",
  }),
});

// Advanced switch validation
const advancedSwitchSchema = z
  .object({
    notifications: z.boolean(),
    emailNotifications: z.boolean(),
    smsNotifications: z.boolean(),
    privacyAccepted: z.boolean(),
    termsAccepted: z.boolean(),
  })
  .refine(
    (data) => {
      // At least one notification method must be enabled
      return data.emailNotifications || data.smsNotifications || !data.notifications;
    },
    {
      message: "At least one notification method must be enabled when notifications are on",
      path: ["emailNotifications"],
    },
  )
  .refine(
    (data) => {
      // Privacy and terms must both be accepted
      return data.privacyAccepted && data.termsAccepted;
    },
    {
      message: "Both privacy policy and terms must be accepted",
      path: ["termsAccepted"],
    },
  );

// Conditional switch validation
const conditionalSwitchSchema = z
  .object({
    accountType: z.enum(["basic", "premium", "enterprise"]),
    advancedFeatures: z.boolean(),
    premiumSupport: z.boolean(),
    customIntegrations: z.boolean(),
  })
  .refine(
    (data) => {
      // Advanced features only available for premium+ accounts
      if (data.advancedFeatures && data.accountType === "basic") {
        return false;
      }
      return true;
    },
    {
      message: "Advanced features require premium or enterprise account",
      path: ["advancedFeatures"],
    },
  )
  .refine(
    (data) => {
      // Premium support only for premium+ accounts
      if (data.premiumSupport && data.accountType === "basic") {
        return false;
      }
      return true;
    },
    {
      message: "Premium support requires premium or enterprise account",
      path: ["premiumSupport"],
    },
  );

// Usage with validation
const ValidatedSwitchForm = () => {
  const {
    control,
    formState: { errors },
  } = useForm({
    resolver: zodResolver(advancedSwitchSchema),
    mode: "onChange",
  });

  return (
    <>
      <Form.Switch
        name="notifications"
        labelText="Enable Notifications"
        control={control}
        description="Master notification setting"
      />

      <Form.Switch
        name="emailNotifications"
        labelText="Email Notifications"
        control={control}
        description="Receive notifications via email"
      />

      <Form.Switch
        name="smsNotifications"
        labelText="SMS Notifications"
        control={control}
        description="Receive notifications via SMS"
      />

      <Form.Switch
        name="privacyAccepted"
        labelText="I accept the privacy policy"
        control={control}
        required
        descriptionVariant={errors.privacyAccepted ? "danger" : "default"}
        description="Required for account creation"
      />

      <Form.Switch
        name="termsAccepted"
        labelText="I accept the terms of service"
        control={control}
        required
        descriptionVariant={errors.termsAccepted ? "danger" : "default"}
        description="Required for account creation"
      />
    </>
  );
};
```

### Custom Validation Functions

```tsx
// Terms acceptance validation
const validateTermsAcceptance = (value: boolean, context: any) => {
  if (!value) {
    return "You must accept the terms and conditions to continue";
  }
  return true;
};

// Age verification validation
const validateAgeVerification = (value: boolean, formData: any) => {
  if (formData.requiresAgeVerification && !value) {
    return "Age verification is required for this service";
  }
  return true;
};

// Feature compatibility validation
const validateFeatureCompatibility = (value: boolean, formData: any) => {
  if (value && formData.accountType === "basic") {
    return "This feature is not available for basic accounts";
  }
  return true;
};

// Usage with custom validation
const CustomValidatedSwitches = () => {
  const schema = z.object({
    termsAccepted: z.boolean().refine(validateTermsAcceptance),
    ageVerified: z.boolean().refine((val, ctx) => validateAgeVerification(val, ctx.parent)),
    premiumFeature: z.boolean().refine((val, ctx) => validateFeatureCompatibility(val, ctx.parent)),
    accountType: z.enum(["basic", "premium"]),
    requiresAgeVerification: z.boolean(),
  });

  const { control } = useForm({
    resolver: zodResolver(schema),
    mode: "onChange",
  });

  return (
    <>
      <Form.Switch
        name="termsAccepted"
        labelText="Accept Terms and Conditions"
        control={control}
        required
        description="Required to create account"
      />

      <Form.Switch
        name="ageVerified"
        labelText="I am 18 years or older"
        control={control}
        description="Age verification for restricted content"
      />

      <Form.Switch
        name="premiumFeature"
        labelText="Enable Premium Features"
        control={control}
        description="Advanced functionality (premium accounts only)"
      />
    </>
  );
};
```

## Common Use Cases

### User Preferences and Settings

```tsx
const UserPreferences = () => {
  const { control, watch } = useForm({
    defaultValues: {
      notifications: true,
      emailNotifications: true,
      pushNotifications: false,
      darkMode: false,
      autoSave: true,
      showTutorials: true,
      analyticsOptIn: false,
    },
  });

  const notificationsEnabled = watch("notifications");

  return (
    <>
      {/* Master notification toggle */}
      <Form.Switch
        name="notifications"
        labelText="Enable Notifications"
        control={control}
        description="Master setting for all notification types"
      />

      {/* Dependent notification options */}
      <Form.Switch
        name="emailNotifications"
        labelText="Email Notifications"
        control={control}
        disabled={!notificationsEnabled}
        description="Receive notifications via email"
      />

      <Form.Switch
        name="pushNotifications"
        labelText="Push Notifications"
        control={control}
        disabled={!notificationsEnabled}
        description="Receive browser push notifications"
      />

      {/* UI preferences */}
      <Form.Switch
        name="darkMode"
        labelText="Dark Mode"
        control={control}
        description="Use dark theme for better visibility"
      />

      <Form.Switch
        name="autoSave"
        labelText="Auto-save Changes"
        control={control}
        description="Automatically save your work"
      />

      <Form.Switch
        name="showTutorials"
        labelText="Show Tutorial Tips"
        control={control}
        description="Display helpful tips and tutorials"
      />

      {/* Privacy settings */}
      <Form.Switch
        name="analyticsOptIn"
        labelText="Share Usage Analytics"
        control={control}
        description="Help improve the product by sharing anonymous usage data"
      />
    </>
  );
};
```

### Feature Toggles and Permissions

```tsx
const FeatureToggles = () => {
  const { control, watch } = useForm({
    defaultValues: {
      betaFeatures: false,
      advancedMode: false,
      debugMode: false,
      experimentalUI: false,
      apiAccess: false,
      adminMode: false,
    },
  });

  const advancedModeEnabled = watch("advancedMode");

  return (
    <>
      {/* Beta features */}
      <Form.Switch
        name="betaFeatures"
        labelText="Enable Beta Features"
        control={control}
        description="Access to experimental features (may be unstable)"
        descriptionVariant="warning"
      />

      {/* Advanced mode */}
      <Form.Switch
        name="advancedMode"
        labelText="Advanced Mode"
        control={control}
        description="Show advanced options and controls"
      />

      {/* Debug mode (dependent on advanced mode) */}
      <Form.Switch
        name="debugMode"
        labelText="Debug Mode"
        control={control}
        disabled={!advancedModeEnabled}
        description="Enable debugging tools and verbose logging"
      />

      {/* Experimental UI */}
      <Form.Switch
        name="experimentalUI"
        labelText="Experimental UI"
        control={control}
        description="Try the new user interface design"
        descriptionVariant="info"
      />

      {/* API access */}
      <Form.Switch
        name="apiAccess"
        labelText="API Access"
        control={control}
        description="Enable programmatic access via API"
      />

      {/* Admin mode */}
      <Form.Switch
        name="adminMode"
        labelText="Administrator Mode"
        control={control}
        description="Access administrative functions"
        descriptionVariant="danger"
      />
    </>
  );
};
```

### Terms and Agreements

```tsx
const TermsAndAgreements = () => {
  const {
    control,
    watch,
    formState: { errors },
  } = useForm({
    resolver: zodResolver(
      z.object({
        termsAccepted: z.boolean().refine((val) => val === true, {
          message: "You must accept the terms of service",
        }),
        privacyAccepted: z.boolean().refine((val) => val === true, {
          message: "You must accept the privacy policy",
        }),
        marketingOptIn: z.boolean().optional(),
        ageVerification: z.boolean().refine((val) => val === true, {
          message: "You must confirm you are 18 or older",
        }),
        dataProcessingConsent: z.boolean().refine((val) => val === true, {
          message: "Consent for data processing is required",
        }),
      }),
    ),
    mode: "onChange",
  });

  const allRequiredAccepted = watch(["termsAccepted", "privacyAccepted", "ageVerification", "dataProcessingConsent"]);
  const canProceed = allRequiredAccepted.every(Boolean);

  return (
    <>
      {/* Required agreements */}
      <Form.Switch
        name="termsAccepted"
        labelText="I accept the Terms of Service"
        control={control}
        required
        description="Please read and accept our terms of service"
        descriptionVariant={errors.termsAccepted ? "danger" : "default"}
      />

      <Form.Switch
        name="privacyAccepted"
        labelText="I accept the Privacy Policy"
        control={control}
        required
        description="Please read and accept our privacy policy"
        descriptionVariant={errors.privacyAccepted ? "danger" : "default"}
      />

      <Form.Switch
        name="ageVerification"
        labelText="I am 18 years of age or older"
        control={control}
        required
        description="Age verification is required to use this service"
        descriptionVariant={errors.ageVerification ? "danger" : "default"}
      />

      <Form.Switch
        name="dataProcessingConsent"
        labelText="I consent to data processing"
        control={control}
        required
        description="Required for account creation and service provision"
        descriptionVariant={errors.dataProcessingConsent ? "danger" : "default"}
      />

      {/* Optional marketing consent */}
      <Form.Switch
        name="marketingOptIn"
        labelText="I would like to receive marketing communications"
        control={control}
        description="Optional: Receive updates about new features and promotions"
        descriptionVariant="info"
      />

      {/* Status indicator */}
      <div className={`status-indicator ${canProceed ? "ready" : "pending"}`}>
        {canProceed ? "✓ Ready to proceed" : "Please accept all required terms"}
      </div>
    </>
  );
};
```

### System Configuration

```tsx
const SystemConfiguration = () => {
  const { control, watch } = useForm({
    defaultValues: {
      maintenanceMode: false,
      debugLogging: false,
      cacheEnabled: true,
      compressionEnabled: true,
      sslRequired: true,
      backupEnabled: true,
      monitoringEnabled: true,
      alertsEnabled: true,
    },
  });

  const maintenanceModeEnabled = watch("maintenanceMode");
  const debugLoggingEnabled = watch("debugLogging");

  return (
    <>
      {/* System status */}
      <Form.Switch
        name="maintenanceMode"
        labelText="Maintenance Mode"
        control={control}
        description="Temporarily disable public access for maintenance"
        descriptionVariant="warning"
      />

      {/* Performance settings */}
      <Form.Switch
        name="cacheEnabled"
        labelText="Enable Caching"
        control={control}
        disabled={maintenanceModeEnabled}
        description="Improve performance with intelligent caching"
      />

      <Form.Switch
        name="compressionEnabled"
        labelText="Enable Compression"
        control={control}
        description="Reduce bandwidth usage with content compression"
      />

      {/* Security settings */}
      <Form.Switch
        name="sslRequired"
        labelText="Require SSL/TLS"
        control={control}
        description="Force secure connections (recommended)"
        descriptionVariant="success"
      />

      {/* Backup and monitoring */}
      <Form.Switch
        name="backupEnabled"
        labelText="Automatic Backups"
        control={control}
        description="Regularly backup system data"
      />

      <Form.Switch
        name="monitoringEnabled"
        labelText="System Monitoring"
        control={control}
        description="Monitor system health and performance"
      />

      <Form.Switch
        name="alertsEnabled"
        labelText="System Alerts"
        control={control}
        description="Send notifications for system events"
      />

      {/* Debug settings */}
      <Form.Switch
        name="debugLogging"
        labelText="Debug Logging"
        control={control}
        description="Enable detailed logging (may impact performance)"
        descriptionVariant={debugLoggingEnabled ? "warning" : "default"}
      />
    </>
  );
};
```

### E-commerce Settings

```tsx
const EcommerceSettings = () => {
  const { control, watch } = useForm({
    defaultValues: {
      storeEnabled: true,
      inventoryTracking: true,
      autoRestock: false,
      discountsEnabled: true,
      loyaltyProgram: false,
      guestCheckout: true,
      emailReceipts: true,
      smsNotifications: false,
      reviewsEnabled: true,
      wishlistEnabled: true,
    },
  });

  const storeEnabled = watch("storeEnabled");
  const inventoryTracking = watch("inventoryTracking");

  return (
    <>
      {/* Store status */}
      <Form.Switch
        name="storeEnabled"
        labelText="Store Online"
        control={control}
        description="Make your store available to customers"
        descriptionVariant="success"
      />

      {/* Inventory management */}
      <Form.Switch
        name="inventoryTracking"
        labelText="Track Inventory"
        control={control}
        disabled={!storeEnabled}
        description="Monitor stock levels and availability"
      />

      <Form.Switch
        name="autoRestock"
        labelText="Auto-restock Notifications"
        control={control}
        disabled={!inventoryTracking}
        description="Get notified when items need restocking"
      />

      {/* Promotions and loyalty */}
      <Form.Switch
        name="discountsEnabled"
        labelText="Enable Discounts"
        control={control}
        disabled={!storeEnabled}
        description="Allow discount codes and promotions"
      />

      <Form.Switch
        name="loyaltyProgram"
        labelText="Loyalty Program"
        control={control}
        disabled={!storeEnabled}
        description="Reward repeat customers with points"
      />

      {/* Checkout options */}
      <Form.Switch
        name="guestCheckout"
        labelText="Guest Checkout"
        control={control}
        disabled={!storeEnabled}
        description="Allow purchases without account creation"
      />

      {/* Communication */}
      <Form.Switch
        name="emailReceipts"
        labelText="Email Receipts"
        control={control}
        description="Send purchase confirmations via email"
      />

      <Form.Switch
        name="smsNotifications"
        labelText="SMS Order Updates"
        control={control}
        description="Send order status updates via SMS"
      />

      {/* Customer features */}
      <Form.Switch
        name="reviewsEnabled"
        labelText="Product Reviews"
        control={control}
        disabled={!storeEnabled}
        description="Allow customers to leave product reviews"
      />

      <Form.Switch
        name="wishlistEnabled"
        labelText="Wishlist Feature"
        control={control}
        disabled={!storeEnabled}
        description="Let customers save items for later"
      />
    </>
  );
};
```

## Literal View Mode

### Read-only Display

```tsx
const LiteralViewExamples = () => {
  const { control } = useForm({
    defaultValues: {
      readOnlySwitch: true,
      displaySwitch: false,
      statusSwitch: true,
    },
  });

  return (
    <>
      {/* Switch in literal view mode */}
      <Form.Switch
        name="readOnlySwitch"
        labelText="Read-only Setting"
        control={control}
        literalView={true}
        description="This setting is displayed in read-only mode"
      />

      {/* Conditional literal view */}
      <Form.Switch
        name="displaySwitch"
        labelText="Display Setting"
        control={control}
        literalView={userRole === "viewer"}
        description="Editable for admins, read-only for viewers"
      />

      {/* Status display */}
      <Form.Switch
        name="statusSwitch"
        labelText="System Status"
        control={control}
        literalView={true}
        description="Current system status (read-only)"
      />
    </>
  );
};
```

## Role-Based Access Control

```tsx
const { userPermissions, userRole } = useAuth();

const AccessControlledSwitches = () => {
  return (
    <>
      {/* Admin-only switch */}
      <Form.Switch
        name="adminSetting"
        labelText="Administrator Setting"
        control={control}
        fullAccess={userPermissions.includes("admin_settings") ? "admin" : undefined}
        readonlyAccess={userPermissions.includes("view_settings") ? "user" : undefined}
        description="Administrative configuration option"
      />

      {/* Role-based literal view */}
      <Form.Switch
        name="userSetting"
        labelText="User Setting"
        control={control}
        literalView={userRole === "viewer"}
        disabled={userRole === "guest"}
        description="User-specific configuration"
      />

      {/* Permission-based availability */}
      <Form.Switch
        name="featureToggle"
        labelText="Feature Toggle"
        control={control}
        disabled={!userPermissions.includes("manage_features")}
        description="Toggle advanced features"
      />
    </>
  );
};
```

## Testing Integration

```tsx
<Form.Switch
  name="testableSwitch"
  labelText="Testable Switch"
  control={control}
  testId="test-switch"
  description="Switch for testing purposes"
/>;

// In tests
describe("Switch Component", () => {
  it("should toggle switch state", async () => {
    render(<TestForm />);

    const switchElement = screen.getByTestId("test-switch");

    // Initially unchecked
    expect(switchElement).not.toBeChecked();

    // Click to toggle
    fireEvent.click(switchElement);

    await waitFor(() => {
      expect(switchElement).toBeChecked();
    });
  });

  it("should display label text", () => {
    render(<Form.Switch name="labeledSwitch" labelText="Test Label" control={control} testId="labeled-switch" />);

    expect(screen.getByText("Test Label")).toBeInTheDocument();
  });

  it("should be disabled when disabled prop is true", () => {
    render(<Form.Switch name="disabledSwitch" control={control} disabled={true} testId="disabled-switch" />);

    const switchElement = screen.getByTestId("disabled-switch");
    expect(switchElement).toBeDisabled();
  });

  it("should show literal view when literalView is true", () => {
    render(
      <Form.Switch
        name="literalSwitch"
        labelText="Literal Switch"
        control={control}
        literalView={true}
        checked={true}
        testId="literal-switch"
      />,
    );

    expect(screen.getByText("Yes")).toBeInTheDocument(); // or localized equivalent
  });

  it("should handle external checked prop", async () => {
    const TestComponent = () => {
      const [externalChecked, setExternalChecked] = useState(false);

      return (
        <>
          <Form.Switch name="externalSwitch" control={control} checked={externalChecked} testId="external-switch" />
          <button onClick={() => setExternalChecked(!externalChecked)}>Toggle External</button>
        </>
      );
    };

    render(<TestComponent />);

    const switchElement = screen.getByTestId("external-switch");
    const toggleButton = screen.getByText("Toggle External");

    expect(switchElement).not.toBeChecked();

    fireEvent.click(toggleButton);

    await waitFor(() => {
      expect(switchElement).toBeChecked();
    });
  });
});
```

## Performance Considerations

```tsx
const OptimizedSwitch = memo(({ control, onChange, ...props }) => {
  // Memoize change handler to prevent unnecessary re-renders
  const handleChange = useCallback(
    async (checked: boolean, event) => {
      // Custom logic
      console.log("Switch changed:", checked);

      // Call external handler if provided
      await onChange?.(checked, event);
    },
    [onChange],
  );

  return <Form.Switch control={control} onChange={handleChange} {...props} />;
});
```

## Accessibility Features

### Screen Reader Support

```tsx
const AccessibleSwitches = () => {
  return (
    <>
      {/* Switch with descriptive label */}
      <Form.Switch
        name="accessibleSwitch"
        labelText="Enable Screen Reader Announcements"
        control={control}
        description="Improve accessibility with audio feedback"
      />

      {/* Switch with ARIA attributes */}
      <Form.Switch
        name="ariaSwitch"
        labelText="High Contrast Mode"
        control={control}
        description="Increase visual contrast for better readability"
        // Additional ARIA attributes are handled internally
      />
    </>
  );
};
```

## Troubleshooting

### Common Issues and Solutions

1. **Switch not updating when form value changes**

   ```tsx
   // Ensure proper form integration
   <Form.Switch name="switchName" control={control} />
   ```

2. **External checked prop not working**

   ```tsx
   // Use checked prop for external control
   <Form.Switch checked={externalValue} control={control} />
   ```

3. **Switch appearing as checkbox**

   ```tsx
   // Ensure proper CSS imports and theme setup
   // Check that PrimeReact styles are loaded
   ```

4. **Literal view not showing correct text**

   ```tsx
   // Ensure i18n translations are available
   // Check translation keys: neuron.ui.checkBox.literalViewTrue/False
   ```

5. **Validation not working**
   ```tsx
   // Ensure proper Zod schema for boolean validation
   z.boolean().refine((val) => val === true, { message: "Required" });
   ```

## Version Information

- **Component Version**: v4.0.0
- **Features**: Boolean toggle input, literal view mode, role-based access control
- **Dependencies**: PrimeReact InputSwitch, React Hook Form, React i18n
- **Accessibility**: Full keyboard navigation and screen reader support

## Sync Metadata

- **Component Version:** v4.0.3
- **Component Source:** `packages/neuron/ui/src/lib/form/switch/Switch.tsx`
- **Guideline Command:** `/neuron-ui-switch`
- **Related Skill:** `neuron-ui-form-choice`

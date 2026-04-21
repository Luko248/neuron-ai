---
agent: agent
---

# AI-Assisted Neuron Steps Component Integration Guide

## For the AI Assistant

Your task is to integrate and configure the Neuron Steps components in a React application. This guide provides comprehensive instructions for implementing the Steps component, which serves as a workflow indicator and navigation system for multi-step processes across all Neuron applications.

## Sync Metadata

- **Component Version:** v5.0.6
- **Component Source:** `packages/neuron/ui/src/lib/menus/steps/Steps.tsx`
- **Guideline Command:** `/neuron-ui-steps`
- **Related Skill:** `neuron-ui-menu`

## Introduction

The Steps system is a navigation and progress component of the Neuron UI framework, designed to create consistent, accessible, and responsive step-by-step workflow indicators that adapt to different screen sizes and interaction patterns across all Neuron applications.

### What is the Steps System?

The Steps component provides a standardized workflow navigation interface for your application with support for:

- Horizontal and vertical orientations
- Responsive behavior with navigation buttons
- Multiple size variations (small, medium, large)
- Semantic variants for step states (default, info, success, warning, danger)
- Custom line colors between steps
- Controlled and uncontrolled modes
- Custom icons and interactive content
- Tooltip integration
- Disabled states
- Container and viewport responsive modes

### Key Features

- **Responsive Design**: Automatically shows navigation arrows on tablet and smaller devices
- **Flexible Orientation**: Support for both horizontal and vertical layouts
- **State Management**: Controlled and uncontrolled component patterns
- **Semantic Variants**: Visual indicators for different step states
- **Custom Content**: Support for interactive elements, links, and buttons within steps
- **Navigation Control**: Optional navigation buttons for responsive behavior
- **Container Queries**: Support for container-based responsive behavior
- **Accessibility**: Proper ARIA attributes and keyboard navigation
- **TypeScript Support**: Full type safety with comprehensive prop interfaces

**Storybook Reference:** You can refer to the Storybook documentation for visual examples and interactive demos of the Steps component.

## Step 1: Basic Steps Implementation

### 1.1 Import the Steps Component

```tsx
import { Steps } from "@neuron/ui";
```

### 1.2 Basic Usage

Here's a simple implementation of the Steps component:

```tsx
import { Steps } from "@neuron/ui";
import { useState } from "react";

const MyComponent = () => {
  const [activeIndex, setActiveIndex] = useState(0);

  const stepsModel = [
    { index: 1, setActiveIndex, label: "Step 1" },
    { index: 2, setActiveIndex, label: "Step 2" },
    { index: 3, setActiveIndex, label: "Step 3" },
    { index: 4, setActiveIndex, label: "Step 4" },
  ];

  return <Steps model={stepsModel} activeIndex={activeIndex} setActiveIndex={setActiveIndex} />;
};
```

### 1.3 Steps with Descriptions

Add descriptions to provide more context for each step:

```tsx
import { Steps } from "@neuron/ui";

const StepsWithDescriptions = () => {
  const stepsModel = [
    {
      index: 1,
      setActiveIndex: () => {},
      label: "Personal Info",
      children: "Enter your personal details",
    },
    {
      index: 2,
      setActiveIndex: () => {},
      label: "Address",
      children: "Provide your address information",
    },
    {
      index: 3,
      setActiveIndex: () => {},
      label: "Payment",
      children: "Choose your payment method",
    },
    {
      index: 4,
      setActiveIndex: () => {},
      label: "Confirmation",
      children: "Review and confirm your order",
    },
  ];

  return <Steps model={stepsModel} activeIndex={1} size="medium" />;
};
```

## Step 2: Responsive Behavior

### 2.1 Default Responsive Mode

The Steps component automatically shows navigation arrows on tablet and smaller devices:

```tsx
import { Steps } from "@neuron/ui";

const ResponsiveSteps = () => {
  const stepsModel = [
    { index: 1, setActiveIndex: () => {}, label: "Step 1" },
    { index: 2, setActiveIndex: () => {}, label: "Step 2" },
    { index: 3, setActiveIndex: () => {}, label: "Step 3" },
    { index: 4, setActiveIndex: () => {}, label: "Step 4" },
    { index: 5, setActiveIndex: () => {}, label: "Step 5" },
  ];

  return (
    <Steps
      model={stepsModel}
      activeIndex={2}
      responsiveMode="viewport" // Default - responds to viewport size
      showNavigationButtons={true} // Default - shows arrows on mobile
    />
  );
};
```

### 2.2 Container-Based Responsive Mode

Use container queries for responsive behavior based on parent container:

```tsx
import { Steps } from "@neuron/ui";

const ContainerResponsiveSteps = () => {
  const stepsModel = [
    { index: 1, setActiveIndex: () => {}, label: "Setup" },
    { index: 2, setActiveIndex: () => {}, label: "Configuration" },
    { index: 3, setActiveIndex: () => {}, label: "Testing" },
    { index: 4, setActiveIndex: () => {}, label: "Deployment" },
  ];

  return (
    <div style={{ width: "400px" }}>
      {" "}
      {/* Container width controls responsive behavior */}
      <Steps
        model={stepsModel}
        activeIndex={1}
        responsiveMode="container" // Responds to container size
        showNavigationButtons={true}
      />
    </div>
  );
};
```

### 2.3 Disabling Navigation Buttons

For simple workflows where all steps are always visible:

```tsx
import { Steps } from "@neuron/ui";

const SimpleSteps = () => {
  const stepsModel = [
    { index: 1, setActiveIndex: () => {}, label: "Start" },
    { index: 2, setActiveIndex: () => {}, label: "Process" },
    { index: 3, setActiveIndex: () => {}, label: "Complete" },
  ];

  return (
    <Steps
      model={stepsModel}
      activeIndex={1}
      showNavigationButtons={false} // No arrows - always shows all steps
    />
  );
};
```

## Step 3: Orientation and Sizing

### 3.1 Horizontal vs Vertical Orientation

Choose orientation based on your layout needs:

```tsx
import { Steps } from "@neuron/ui";

const OrientationSteps = () => {
  const stepsModel = [
    { index: 1, setActiveIndex: () => {}, label: "Step 1", children: "Description 1" },
    { index: 2, setActiveIndex: () => {}, label: "Step 2", children: "Description 2" },
    { index: 3, setActiveIndex: () => {}, label: "Step 3", children: "Description 3" },
  ];

  return (
    <div>
      {/* Horizontal orientation (default) */}
      <Steps model={stepsModel} activeIndex={1} vertical={false} />

      {/* Vertical orientation */}
      <Steps model={stepsModel} activeIndex={1} vertical={true} />
    </div>
  );
};
```

### 3.2 Size Variations

Control the size of steps based on your design needs:

```tsx
import { Steps } from "@neuron/ui";

const SizedSteps = () => {
  const stepsModel = [
    { index: 1, setActiveIndex: () => {}, label: "Small Step" },
    { index: 2, setActiveIndex: () => {}, label: "Medium Step" },
    { index: 3, setActiveIndex: () => {}, label: "Large Step" },
  ];

  return (
    <div>
      {/* Small size */}
      <Steps model={stepsModel} activeIndex={0} size="small" />

      {/* Medium size (default) */}
      <Steps model={stepsModel} activeIndex={1} size="medium" />

      {/* Large size */}
      <Steps model={stepsModel} activeIndex={2} size="large" />
    </div>
  );
};
```

## Step 4: State Variants and Visual Indicators

### 4.1 Semantic Step Variants

Use semantic variants to indicate step states:

```tsx
import { Steps } from "@neuron/ui";

const VariantSteps = () => {
  const stepsModel = [
    {
      index: 1,
      setActiveIndex: () => {},
      label: "Completed",
      variant: "success",
      children: "This step is completed",
    },
    {
      index: 2,
      setActiveIndex: () => {},
      label: "In Progress",
      variant: "info",
      children: "Currently working on this step",
    },
    {
      index: 3,
      setActiveIndex: () => {},
      label: "Warning",
      variant: "warning",
      children: "This step needs attention",
    },
    {
      index: 4,
      setActiveIndex: () => {},
      label: "Error",
      variant: "danger",
      children: "This step has an error",
    },
    {
      index: 5,
      setActiveIndex: () => {},
      label: "Pending",
      variant: "default",
      children: "This step is pending",
    },
  ];

  return <Steps model={stepsModel} activeIndex={1} />;
};
```

### 4.2 Custom Line Colors

Customize the connecting lines between steps:

```tsx
import { Steps } from "@neuron/ui";

const CustomLineSteps = () => {
  const stepsModel = [
    {
      index: 1,
      setActiveIndex: () => {},
      label: "Step 1",
      variant: "success",
      lineVariant: "success", // Green connecting line
      children: "Completed step",
    },
    {
      index: 2,
      setActiveIndex: () => {},
      label: "Step 2",
      variant: "info",
      lineVariant: "danger", // Red connecting line
      children: "Current step",
    },
    {
      index: 3,
      setActiveIndex: () => {},
      label: "Step 3",
      variant: "default",
      lineVariant: "warning", // Yellow connecting line
      children: "Future step",
    },
  ];

  return <Steps model={stepsModel} activeIndex={1} />;
};
```

## Step 5: Interactive Content and Custom Icons

### 5.1 Custom Icons

Add custom icons to enhance step meaning:

```tsx
import { Steps, baseIcons } from "@neuron/ui";

const IconSteps = () => {
  const stepsModel = [
    {
      index: 1,
      setActiveIndex: () => {},
      label: "User Info",
      icon: { iconDef: baseIcons.userSolid },
      variant: "success",
    },
    {
      index: 2,
      setActiveIndex: () => {},
      label: "Edit Details",
      icon: { iconDef: baseIcons.penRegular },
      variant: "info",
    },
    {
      index: 3,
      setActiveIndex: () => {},
      label: "Verify",
      icon: { iconDef: baseIcons.checkSolid },
      variant: "success",
    },
    {
      index: 4,
      setActiveIndex: () => {},
      label: "Error Check",
      icon: { iconDef: baseIcons.xmarkLargeRegular },
      variant: "danger",
    },
  ];

  return <Steps model={stepsModel} activeIndex={2} />;
};
```

### 5.2 Interactive Step Content

Include interactive elements within steps:

```tsx
import { Steps, Link, Button } from "@neuron/ui";

const InteractiveSteps = () => {
  const stepsModel = [
    {
      index: 1,
      setActiveIndex: () => {},
      children: <Link onClick={() => console.info("Step 1 clicked")}>Go to Step 1</Link>,
      icon: { iconDef: baseIcons.linkSolid },
    },
    {
      index: 2,
      setActiveIndex: () => {},
      children: "Step 2 - Information",
      variant: "info",
    },
    {
      index: 3,
      setActiveIndex: () => {},
      children: (
        <Button onClick={() => console.info("Step 3 action")} size="small">
          Execute Step 3
        </Button>
      ),
      variant: "success",
    },
  ];

  return <Steps model={stepsModel} activeIndex={1} />;
};
```

## Step 6: Controlled Steps

### 6.1 External State Control

Control step navigation from outside the component:

```tsx
import { Steps, Button } from "@neuron/ui";
import { useState } from "react";

const ControlledSteps = () => {
  const [activeStep, setActiveStep] = useState(0);

  const stepsModel = [
    { index: 1, setActiveIndex: () => {}, label: "Step 1" },
    { index: 2, setActiveIndex: () => {}, label: "Step 2" },
    { index: 3, setActiveIndex: () => {}, label: "Step 3" },
    { index: 4, setActiveIndex: () => {}, label: "Step 4" },
  ];

  const handleNext = () => {
    setActiveStep(Math.min(stepsModel.length - 1, activeStep + 1));
  };

  const handlePrevious = () => {
    setActiveStep(Math.max(0, activeStep - 1));
  };

  const handleStepSelect = (index: number) => {
    setActiveStep(index);
  };

  return (
    <div>
      <div className="mb-3">
        <Button onClick={handlePrevious} disabled={activeStep === 0}>
          Previous
        </Button>
        <Button onClick={handleNext} disabled={activeStep === stepsModel.length - 1}>
          Next
        </Button>
      </div>

      <Steps
        model={stepsModel}
        activeIndex={activeStep}
        setActiveIndex={setActiveStep}
        onSelect={(e) => handleStepSelect(e.index)}
      />

      <div className="mt-3">
        <p>Current Step: {activeStep + 1}</p>
      </div>
    </div>
  );
};
```

### 6.2 Form Wizard Integration

Integrate with multi-step forms:

```tsx
import { Steps, Button, Panel } from "@neuron/ui";
import { useState } from "react";

const FormWizard = () => {
  const [currentStep, setCurrentStep] = useState(0);
  const [formData, setFormData] = useState({});

  const stepsModel = [
    {
      index: 1,
      setActiveIndex: () => {},
      label: "Personal Info",
      variant: currentStep > 0 ? "success" : "default",
    },
    {
      index: 2,
      setActiveIndex: () => {},
      label: "Address",
      variant: currentStep > 1 ? "success" : currentStep === 1 ? "info" : "default",
    },
    {
      index: 3,
      setActiveIndex: () => {},
      label: "Review",
      variant: currentStep === 2 ? "info" : "default",
    },
  ];

  const handleStepComplete = (stepData) => {
    setFormData({ ...formData, ...stepData });
    setCurrentStep(currentStep + 1);
  };

  const renderStepContent = () => {
    switch (currentStep) {
      case 0:
        return <PersonalInfoForm onComplete={handleStepComplete} />;
      case 1:
        return <AddressForm onComplete={handleStepComplete} />;
      case 2:
        return <ReviewForm data={formData} onComplete={handleStepComplete} />;
      default:
        return <div>Form completed!</div>;
    }
  };

  return (
    <div>
      <Steps model={stepsModel} activeIndex={currentStep} showNavigationButtons={false} />

      <Panel className="mt-4">{renderStepContent()}</Panel>
    </div>
  );
};
```

## Step 7: Tooltips and Accessibility

### 7.1 Step Tooltips

Add tooltips for additional context:

```tsx
import { Steps } from "@neuron/ui";

const TooltipSteps = () => {
  const stepsModel = [
    {
      index: 1,
      setActiveIndex: () => {},
      label: "Setup",
      tooltip: "Configure your initial settings",
      variant: "success",
    },
    {
      index: 2,
      setActiveIndex: () => {},
      label: "Configuration",
      tooltip: "Customize your preferences and options",
      variant: "info",
    },
    {
      index: 3,
      setActiveIndex: () => {},
      label: "Testing",
      tooltip: "Verify everything works correctly",
      variant: "warning",
    },
    {
      index: 4,
      setActiveIndex: () => {},
      label: "Deploy",
      tooltip: "Launch your application to production",
      variant: "default",
    },
  ];

  return <Steps model={stepsModel} activeIndex={1} />;
};
```

### 7.2 Disabled Steps

Handle disabled states for restricted navigation:

```tsx
import { Steps } from "@neuron/ui";

const DisabledSteps = () => {
  const stepsModel = [
    {
      index: 1,
      setActiveIndex: () => {},
      label: "Available Step",
      variant: "success",
    },
    {
      index: 2,
      setActiveIndex: () => {},
      label: "Current Step",
      variant: "info",
    },
    {
      index: 3,
      setActiveIndex: () => {},
      label: "Locked Step",
      disabled: true, // User cannot navigate to this step
      variant: "default",
    },
    {
      index: 4,
      setActiveIndex: () => {},
      label: "Future Step",
      disabled: true,
      variant: "default",
    },
  ];

  return (
    <Steps
      model={stepsModel}
      activeIndex={1}
      readOnly={false} // Allow interaction with enabled steps
    />
  );
};
```

### 7.3 Read-Only Mode

Display steps without interaction:

```tsx
import { Steps } from "@neuron/ui";

const ReadOnlySteps = () => {
  const stepsModel = [
    { index: 1, setActiveIndex: () => {}, label: "Completed", variant: "success" },
    { index: 2, setActiveIndex: () => {}, label: "In Progress", variant: "info" },
    { index: 3, setActiveIndex: () => {}, label: "Pending", variant: "default" },
  ];

  return (
    <Steps
      model={stepsModel}
      activeIndex={1}
      readOnly={true} // No interaction allowed
    />
  );
};
```

## Step 8: Steps Props Reference

### 8.1 Core Steps Props

| Prop           | Type                               | Default    | Description                                |
| -------------- | ---------------------------------- | ---------- | ------------------------------------------ |
| model          | `StepsItemProps[]`                 | -          | Array of step definitions (required)       |
| activeIndex    | `number`                           | `0`        | Currently active step index                |
| setActiveIndex | `Dispatch<SetStateAction<number>>` | -          | Function to control active step externally |
| onSelect       | `(event: SelectEvent) => void`     | -          | Callback when step is selected             |
| size           | `"small" \| "medium" \| "large"`   | `"medium"` | Size of the steps component                |
| vertical       | `boolean`                          | `false`    | Vertical orientation                       |
| className      | `string`                           | -          | Additional CSS classes                     |
| testId         | `string`                           | -          | Test identifier                            |

### 8.2 Responsive Props

| Prop                  | Type                        | Default      | Description                      |
| --------------------- | --------------------------- | ------------ | -------------------------------- |
| responsiveMode        | `"viewport" \| "container"` | `"viewport"` | Responsive behavior mode         |
| showNavigationButtons | `boolean`                   | `true`       | Show navigation arrows on mobile |

### 8.3 Interaction Props

| Prop     | Type      | Default | Description                   |
| -------- | --------- | ------- | ----------------------------- |
| readOnly | `boolean` | `false` | Disable all step interactions |

### 8.4 StepsItem Props

| Prop           | Type                                                        | Default     | Description                         |
| -------------- | ----------------------------------------------------------- | ----------- | ----------------------------------- |
| index          | `number`                                                    | -           | Step index (required)               |
| setActiveIndex | `function`                                                  | -           | Step activation function (required) |
| label          | `string`                                                    | -           | Step label text                     |
| children       | `ReactNode`                                                 | -           | Step content/description            |
| variant        | `"default" \| "info" \| "success" \| "warning" \| "danger"` | `"default"` | Step visual variant                 |
| lineVariant    | `"default" \| "info" \| "success" \| "warning" \| "danger"` | `"default"` | Connecting line color               |
| icon           | `IconProps`                                                 | -           | Custom step icon                    |
| tooltip        | `string`                                                    | -           | Step tooltip text                   |
| disabled       | `boolean`                                                   | `false`     | Disable step interaction            |

## Step 9: Best Practices

### 9.1 When to Use Each Orientation

**Horizontal Steps:**

- Multi-step forms and wizards
- Process workflows
- Progress indicators
- When you have 3-7 steps

```tsx
{
  /* Good: Form wizard */
}
<Steps model={formSteps} activeIndex={currentStep} vertical={false} />;
```

**Vertical Steps:**

- Sidebar navigation
- Detailed process descriptions
- When you have many steps
- Limited horizontal space

```tsx
{
  /* Good: Detailed process */
}
<Steps model={detailedSteps} activeIndex={currentStep} vertical={true} />;
```

### 9.2 Responsive Behavior Guidelines

**Use `showNavigationButtons={true}` when:**

- You have many steps (5+)
- Steps might not fit on mobile screens
- Users need to navigate sequentially

**Use `showNavigationButtons={false}` when:**

- You have few steps (2-4)
- All steps should always be visible
- Steps are for display/status only

```tsx
{
  /* Good: Many steps with navigation */
}
<Steps model={manySteps} showNavigationButtons={true} responsiveMode="viewport" />;

{
  /* Good: Few steps, always visible */
}
<Steps model={fewSteps} showNavigationButtons={false} />;
```

### 9.3 State Management Best Practices

- Use controlled mode for complex workflows
- Provide clear visual feedback for step states
- Handle disabled states appropriately
- Use semantic variants meaningfully

```tsx
{
  /* Good: Controlled with clear states */
}
const [activeStep, setActiveStep] = useState(0);

const stepsModel = steps.map((step, index) => ({
  ...step,
  variant: index < activeStep ? "success" : index === activeStep ? "info" : "default",
  disabled: index > activeStep + 1, // Only allow next step
}));
```

### 9.4 Content Guidelines

- Keep step labels concise and descriptive
- Use descriptions for additional context
- Provide meaningful tooltips
- Use icons to enhance understanding

```tsx
{
  /* Good: Clear, descriptive steps */
}
const stepsModel = [
  {
    label: "Account Setup",
    children: "Create your account and verify email",
    tooltip: "This step takes about 2 minutes",
    icon: { iconDef: baseIcons.userSolid },
  },
];
```

### 9.5 Accessibility Considerations

- Provide meaningful labels and descriptions
- Use proper semantic variants
- Ensure keyboard navigation works
- Add tooltips for additional context

```tsx
{
  /* Good: Accessible steps */
}
<Steps
  model={accessibleSteps}
  activeIndex={currentStep}
  onSelect={(e) => {
    // Announce step change to screen readers
    announceStepChange(e.index);
    setCurrentStep(e.index);
  }}
/>;
```

## Step 10: Common Mistakes to Avoid

### 10.1 Don't Forget Required Props

```tsx
{
  /* ❌ INCORRECT: Missing required props */
}
const stepsModel = [
  { label: "Step 1" }, // Missing index and setActiveIndex
  { label: "Step 2" },
];

{
  /* ✅ CORRECT: All required props */
}
const stepsModel = [
  { index: 1, setActiveIndex: () => {}, label: "Step 1" },
  { index: 2, setActiveIndex: () => {}, label: "Step 2" },
];
```

### 10.2 Don't Misuse Responsive Settings

```tsx
{
  /* ❌ INCORRECT: Navigation buttons for few steps */
}
<Steps
  model={[step1, step2]} // Only 2 steps
  showNavigationButtons={true} // Unnecessary
/>;

{
  /* ✅ CORRECT: No navigation for few steps */
}
<Steps model={[step1, step2]} showNavigationButtons={false} />;
```

### 10.3 Don't Ignore State Management

```tsx
{
  /* ❌ INCORRECT: No state management */
}
<Steps model={steps} />; // No activeIndex control

{
  /* ✅ CORRECT: Proper state management */
}
<Steps
  model={steps}
  activeIndex={currentStep}
  setActiveIndex={setCurrentStep}
  onSelect={(e) => handleStepChange(e.index)}
/>;
```

### 10.4 Don't Overuse Variants

```tsx
{
  /* ❌ INCORRECT: Random variant usage */
}
const stepsModel = [
  { label: "Step 1", variant: "danger" }, // Doesn't match state
  { label: "Step 2", variant: "success" }, // Confusing
];

{
  /* ✅ CORRECT: Meaningful variant usage */
}
const stepsModel = [
  { label: "Completed", variant: "success" },
  { label: "In Progress", variant: "info" },
  { label: "Error", variant: "danger" },
];
```

## Key Takeaways

The Neuron Steps component system provides a comprehensive, accessible, and responsive foundation for workflow navigation. Key points to remember:

1. **Responsive by default** - automatically adapts to screen size with navigation buttons
2. **Flexible orientation** - choose horizontal or vertical based on layout needs
3. **State management** - use controlled mode for complex workflows
4. **Semantic variants** - use meaningful colors for step states
5. **Navigation control** - disable navigation buttons for simple workflows
6. **Custom content** - support for interactive elements and custom icons
7. **Container queries** - responsive behavior based on container or viewport
8. **Accessibility compliance** - proper ARIA attributes and keyboard navigation
9. **Performance optimization** - efficient rendering and state updates
10. **User experience** - clear visual feedback and intuitive navigation

By following these guidelines, you'll create consistent, accessible, and user-friendly step navigation interfaces that enhance your Neuron applications' workflow experiences across all device types.

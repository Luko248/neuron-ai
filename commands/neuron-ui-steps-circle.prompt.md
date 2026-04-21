---
agent: agent
---

# AI-Assisted Neuron StepsCircle Component Integration Guide

## For the AI Assistant

Your task is to integrate and configure the Neuron StepsCircle component in a React application. This guide provides comprehensive instructions for implementing StepsCircle, which is used to visualize progress in multi-step processes.

## Sync Metadata

- **Component Version:** v1.0.1
- **Component Source:** `packages/neuron/ui/src/lib/misc/stepsCircle/StepsCircle.tsx`
- **Guideline Command:** `/neuron-ui-steps-circle`
- **Related Skill:** `neuron-ui-misc`

## Introduction

The StepsCircle component is a part of the Neuron UI framework, designed to provide a clear and circular visual indicator of progress through a series of steps.

### What is StepsCircle?

StepsCircle is a presentational component that displays the current step number relative to the total number of steps in a circular format. It also supports a progress ring visualization that fills up as steps are completed.

### Key Features

- **Circular Progress Indicator**: Visualizes progress as a ring that fills proportionally to the current step.
- **Step Counter**: Displays the current step and total steps (e.g., "2/5") in the center.
- **Customizable Content**: Supports rendering children (e.g., title, description) alongside the circle.
- **Smooth Animation**: Progress ring transitions smoothly between steps.
- **Controlled Component**: Fully controlled via props, making it easy to integrate with any state management logic.

## Step 1: Basic StepsCircle Implementation

### 1.1 Import the StepsCircle Component

```tsx
import { StepsCircle } from "@neuron/ui";
```

### 1.2 Basic Usage

Here's a simple implementation of the StepsCircle component:

```tsx
import { StepsCircle } from "@neuron/ui";

const MyProgress = () => {
  return <StepsCircle currentStep={1} stepsCount={5} />;
};
```

## Step 2: Adding Content

You can render content alongside the StepsCircle by passing children. This is useful for displaying the title or description of the current step.

```tsx
import { StepsCircle } from "@neuron/ui";

const MyProgressWithContent = () => {
  return (
    <StepsCircle currentStep={2} stepsCount={5}>
      <div className="d-flex flex-column">
        <h3>Step 2: Personal Details</h3>
        <span>Please enter your information</span>
      </div>
    </StepsCircle>
  );
};
```

## Step 3: Dynamic Usage (State Management)

Since `StepsCircle` is a controlled component, you need to manage the `currentStep` state in a parent component.

### 3.1 Using React State

```tsx
import { useState } from "react";
import { Button, StepsCircle } from "@neuron/ui";

const DynamicSteps = () => {
  const [currentStep, setCurrentStep] = useState(1);
  const totalSteps = 5;

  const handleNext = () => {
    if (currentStep < totalSteps) {
      setCurrentStep((prev) => prev + 1);
    }
  };

  const handlePrev = () => {
    if (currentStep > 1) {
      setCurrentStep((prev) => prev - 1);
    }
  };

  return (
    <div className="d-flex flex-column gap-16">
      <StepsCircle currentStep={currentStep} stepsCount={totalSteps}>
        <h3>Step {currentStep}</h3>
      </StepsCircle>

      <div className="d-flex gap-8">
        <Button onClick={handlePrev} disabled={currentStep === 1} size="small">
          Previous
        </Button>
        <Button onClick={handleNext} disabled={currentStep === totalSteps} size="small">
          Next
        </Button>
      </div>
    </div>
  );
};
```

## Step 4: Props Reference

| Prop          | Type        | Default | Description                                                                                             |
| :------------ | :---------- | :------ | :------------------------------------------------------------------------------------------------------ |
| `currentStep` | `number`    | -       | **Required**. The current active step (1-based index).                                                  |
| `stepsCount`  | `number`    | -       | The total number of steps. Defaults to `undefined` (optional but recommended for progress calculation). |
| `children`    | `ReactNode` | -       | Optional content to render alongside the circle.                                                        |

## Step 5: Best Practices

1.  **State Management**: Always control `currentStep` from a parent component or store. Do not try to make `StepsCircle` manage its own state.
2.  **Validation**: Ensure `currentStep` does not exceed `stepsCount` and is not less than 1 in your logic.
3.  **Content**: Use the `children` prop to provide context about what the current step represents.
4.  **Accessibility**: Ensure the content passed as children is accessible. The component itself provides visual feedback, but screen reader users rely on the text content.

## Common Patterns

### 5.1 Wizard Layout

Use `StepsCircle` in a wizard-style layout to show overall progress while the user fills out forms.

```tsx
const Wizard = () => {
  const [step, setStep] = useState(1);
  // ... form logic ...

  return (
    <div className="wizard-container">
      <div className="wizard-header">
        <StepsCircle currentStep={step} stepsCount={4}>
          <h2>{getStepTitle(step)}</h2>
        </StepsCircle>
      </div>
      <div className="wizard-content">{/* Render form for current step */}</div>
    </div>
  );
};
```

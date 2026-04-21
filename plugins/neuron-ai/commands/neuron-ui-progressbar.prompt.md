---
agent: agent
---

# AI-Assisted Neuron ProgressBar Component Integration Guide

## For the AI Assistant

Your task is to integrate and configure the Neuron ProgressBar component in a React application. This guide provides instructions for implementing ProgressBar for linear loading states and multi-step flows where progress must be visualized with a simple horizontal bar.

## Sync Metadata

- **Component Version:** v1.0.1
- **Component Source:** `packages/neuron/ui/src/lib/helpers/progressBar/ProgressBar.tsx`
- **Workflow:** `/neuron-ui-progressbar`
- **Skill:** `neuron-ui-helpers`

## Introduction

The ProgressBar is a lightweight helper component for displaying progress in two common representations:

- **Percentage mode** for async loading or upload-like progress from `0` to `100`
- **Steps mode** for finite process flows such as onboarding, wizard steps, or guided setup

The component is intentionally controlled from above. It does not manage its own state and does not render navigation controls internally. Parent components are responsible for changing `currentStep`.

### Key Features

- **Controlled progress state** driven by props from the parent
- **Two display modes**: `"percentage"` and `"steps"`
- **Optional textual label** showing either `50%` or `2/4`
- **Optional step title** rendered below the bar on the left
- **Accessible naming** through `ariaLabel` or fallback metadata
- **Consistent helper styling** shared across onboarding and generic loading use cases
- **Accessible progress semantics** via `role="progressbar"` and ARIA values

**Storybook Reference:** Refer to Storybook for examples covering labeled, unlabeled, step-based, and loading simulation variants.

## Step 1: Basic ProgressBar Implementation

### 1.1 Import the ProgressBar Component

```tsx
import { ProgressBar } from "@neuron/ui";
```

### 1.2 Percentage Progress

Use percentage mode when progress maps naturally to a `0..100` range.

```tsx
import { ProgressBar } from "@neuron/ui";

const LoadingState = () => {
  return <ProgressBar currentStep={65} displayMode="percentage" stepsCount={100} />;
};
```

### 1.3 Step-Based Progress

Use step mode for guided flows and wizards.

```tsx
import { ProgressBar } from "@neuron/ui";

const StepProgress = () => {
  return <ProgressBar currentStep={2} displayMode="steps" stepsCount={4} />;
};
```

## Step 2: Labels and Titles

### 2.1 Show Progress Label

Use `isProgressLabelVisible` when the UI should show the textual value under the bar.

```tsx
import { ProgressBar } from "@neuron/ui";

const LabeledProgress = () => {
  return <ProgressBar currentStep={50} displayMode="percentage" isProgressLabelVisible={true} stepsCount={100} />;
};
```

### 2.2 Show Step Title

Use `stepTitle` when the current phase or step should be named.

```tsx
import { ProgressBar } from "@neuron/ui";

const TitledStepProgress = () => {
  return (
    <ProgressBar
      currentStep={1}
      displayMode="steps"
      isProgressLabelVisible={true}
      stepTitle="Modelace"
      stepsCount={4}
    />
  );
};
```

### 2.3 Hide Optional Metadata

Leave `stepTitle` as `null` and `isProgressLabelVisible` as `false` for the cleanest visual form.

```tsx
import { ProgressBar } from "@neuron/ui";

const MinimalProgress = () => {
  return <ProgressBar currentStep={32} displayMode="percentage" stepTitle={null} stepsCount={100} />;
};
```

### 2.4 Accessibility Label

Use `ariaLabel` when the surrounding layout does not clearly describe what the progress refers to.

```tsx
import { ProgressBar } from "@neuron/ui";

const AccessibleProgress = () => {
  return (
    <ProgressBar ariaLabel="Průběh nahrávání souboru" currentStep={72} displayMode="percentage" stepsCount={100} />
  );
};
```

## Step 3: Parent-Controlled State

The ProgressBar does not provide internal previous/next behavior. Parent components must own the current value and update it from external UI.

```tsx
import { Button, ProgressBar } from "@neuron/ui";
import { useState } from "react";

const WizardProgress = () => {
  const [currentStep, setCurrentStep] = useState(2);
  const stepsCount = 5;

  return (
    <div className="d-grid gap-8">
      <div className="d-flex gap-8">
        <Button
          disabled={currentStep <= 1}
          size="small"
          type="button"
          variant="secondary"
          onClick={() => setCurrentStep((step) => Math.max(step - 1, 1))}
        >
          Previous
        </Button>
        <Button
          disabled={currentStep >= stepsCount}
          size="small"
          type="button"
          onClick={() => setCurrentStep((step) => Math.min(step + 1, stepsCount))}
        >
          Next
        </Button>
      </div>

      <ProgressBar
        currentStep={currentStep}
        displayMode="steps"
        isProgressLabelVisible={true}
        stepTitle="Kontrola"
        stepsCount={stepsCount}
      />
    </div>
  );
};
```

## Step 4: File Upload and Async Loading Patterns

Use percentage mode for request progress, uploads, or simulated loading states.

```tsx
import { ProgressBar } from "@neuron/ui";

type UploadProgressProps = {
  progress: number;
};

const UploadProgress = ({ progress }: UploadProgressProps) => {
  return (
    <ProgressBar
      currentStep={progress}
      displayMode="percentage"
      isProgressLabelVisible={false}
      stepTitle={null}
      stepsCount={100}
    />
  );
};
```

## Step 5: Props Reference

| Prop                     | Type                      | Default        | Description                                       |
| ------------------------ | ------------------------- | -------------- | ------------------------------------------------- |
| `currentStep`            | `number`                  | -              | Current value or active step                      |
| `stepsCount`             | `number`                  | `100`          | Total step count or maximum progress value        |
| `displayMode`            | `"percentage" \| "steps"` | `"percentage"` | Controls textual representation and value meaning |
| `ariaLabel`              | `string`                  | -              | Optional accessible label for the progress bar    |
| `stepTitle`              | `string \| null`          | `null`         | Optional title rendered below the bar on the left |
| `isProgressLabelVisible` | `boolean`                 | `false`        | Shows the textual progress label below the bar    |
| `className`              | `string`                  | -              | Optional extra class for wrapper styling          |

## Best Practices

1. **Control progress from parent state** rather than expecting the component to handle step transitions internally.
2. **Use percentage mode for loading** and step mode for finite workflows.
3. **Keep labels optional** in dense layouts such as upload rows where the surrounding UI already communicates state.
4. **Use meaningful step titles** for guided flows so users understand what the current stage represents.
5. **Provide `ariaLabel`** when the visible context around the component is not descriptive enough.
6. **Clamp values before passing when needed**, especially if progress comes from external APIs or async events.

## Common Mistakes

1. **Do not expect built-in navigation**. ProgressBar is display-only.
2. **Do not pass arbitrary max values without `stepsCount`** if percentage mode should represent `0..100`.
3. **Do not duplicate labels nearby** when the layout already contains a separate visible status string.
4. **Do not use step mode for indefinite loading** where there is no stable total number of steps.

## Summary

The Neuron ProgressBar is a small controlled helper for rendering linear progress in either percentage or step form. Use it when you need a consistent horizontal progress track, optional title and label metadata, and parent-owned state transitions without embedding navigation logic into the component itself.

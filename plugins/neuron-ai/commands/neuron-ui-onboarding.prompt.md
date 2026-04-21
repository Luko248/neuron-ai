---
agent: agent
---

# Neuron Onboarding Component Guidelines

## For the AI Assistant

This document provides comprehensive guidelines for implementing the Neuron Onboarding component. It explains proper usage, step configuration, targeting via data attributes, scroll behaviour, "don't show again" persistence, and best practices for creating accessible guided tours.

## Sync Metadata

- **Component Version:** v1.0.1
- **Component Source:** `packages/neuron/ui/src/lib/overlays/onboarding/Onboarding.tsx`
- **Guideline Command:** `/neuron-ui-onboarding`
- **Related Skill:** `neuron-ui-overlays`

## Overview

The Onboarding component guides users through a step-by-step tour of the UI. It highlights target elements using a clip-path overlay cutout, displays a popup with step content, and supports smooth scrolling between steps. The component is marked as **Work in Progress**.

### Key Features

- **Data-attribute targeting** — Add `data-onboarding-step="N"` to any element. No wrappers needed.
- **Clip-path overlay** — Full-viewport dark overlay with a transparent cutout around the highlighted element.
- **Smooth scroll** — Automatic `scrollIntoView` between steps with manual scroll blocking during the tour.
- **CSS Anchor Positioning** — Popup positioned via `position-area: top center` with `flip-block` fallback.
- **"Don't show again"** — Persists user preference to `localStorage`.
- **Footer trigger** — Automatically portals a "Show guide" link into the footer actions area.
- **Manual trigger** — `onShowGuideTriggerReady` callback provides a function to open the tour from anywhere.
- **Progress indicator** — Linear progress bar and step counter (e.g. "2 / 5").
- **Localization** — All UI labels use `react-i18next` with Czech translations.

## Core Usage

### Basic Implementation

```tsx
import { Onboarding } from "@neuron/ui";

const MyPage = () => {
  const [isOnboardingOpen, setIsOnboardingOpen] = useState(false);

  const steps = [
    {
      step: 1,
      title: "Vyhledávání",
      content: <p>Zde můžete vyhledat záznamy podle různých kritérií.</p>,
    },
    {
      step: 2,
      title: "Tabulka výsledků",
      content: <p>Výsledky vyhledávání se zobrazí v této tabulce.</p>,
    },
    {
      step: 3,
      title: "Detail záznamu",
      content: <p>Kliknutím na řádek otevřete detail záznamu.</p>,
    },
  ];

  return (
    <>
      {/* Mark target elements with data-onboarding-step */}
      <Form.Input data-onboarding-step="1" name="search" labelText="Vyhledávání" />
      <Table data-onboarding-step="2" ... />
      <Panel data-onboarding-step="3" ... />

      <Onboarding
        steps={steps}
        isShown={isOnboardingOpen}
        onClose={() => setIsOnboardingOpen(false)}
      />
    </>
  );
};
```

### Auto-Show on Page Load

```tsx
<Onboarding
  steps={steps}
  isShownOnPageLoad={true}
  onClose={() => {
    /* cleanup */
  }}
/>
```

### Manual Trigger from Custom Button

```tsx
const MyPage = () => {
  const triggerRef = useRef<(() => void) | null>(null);

  return (
    <>
      <Button onClick={() => triggerRef.current?.()}>Spustit průvodce</Button>

      <Onboarding
        steps={steps}
        onClose={() => {}}
        onShowGuideTriggerReady={(trigger) => {
          triggerRef.current = trigger;
        }}
      />
    </>
  );
};
```

### Custom localStorage Key

```tsx
<Onboarding steps={steps} onClose={handleClose} dontShowAgainStorageKey="myApp.onboarding.featureX.dismissed" />
```

## Props Reference

### OnboardingProps

| Prop                      | Type                            | Default                            | Description                                              |
| ------------------------- | ------------------------------- | ---------------------------------- | -------------------------------------------------------- |
| `steps`                   | `OnboardingStepConfig[]`        | (required)                         | Ordered array of guidance steps.                         |
| `isShownOnPageLoad`       | `boolean`                       | `false`                            | Auto-show onboarding when the component mounts.          |
| `isShown`                 | `boolean`                       | `false`                            | Controlled visibility of the onboarding tour.            |
| `onClose`                 | `() => void`                    | (required)                         | Called when the tour is closed or completed.             |
| `className`               | `string`                        | —                                  | Additional CSS class for the root element.               |
| `onShowGuideTriggerReady` | `(trigger: () => void) => void` | —                                  | Callback receiving a function to manually open the tour. |
| `dontShowAgainStorageKey` | `string`                        | `"neuron.ui.onboarding.dismissed"` | localStorage key for "don't show again" preference.      |

### OnboardingStepConfig

| Prop      | Type                | Description                                                         |
| --------- | ------------------- | ------------------------------------------------------------------- |
| `step`    | `number`            | Matches the `data-onboarding-step` value on the target DOM element. |
| `title`   | `string` (optional) | Title displayed in the popup header.                                |
| `content` | `ReactNode`         | Content displayed in the popup body.                                |

## Step Targeting

Steps are matched to DOM elements via the `data-onboarding-step` attribute:

```tsx
// In the DOM — add the data attribute to any element:
<Form.Input data-onboarding-step="1" ... />
<div data-onboarding-step="2">...</div>
<Table data-onboarding-step="3" ... />

// In the config — step number matches the attribute value:
const steps: OnboardingStepConfig[] = [
  { step: 1, title: "Input", content: <p>Fill in this field.</p> },
  { step: 2, title: "Section", content: <p>This section contains...</p> },
  { step: 3, title: "Table", content: <p>Results appear here.</p> },
];
```

- No wrappers or refs needed — just the data attribute.
- Steps don't have to be sequential (e.g. 1, 3, 5 is valid).
- The element receives `data-onboarding-active` when it is the current step.

## Scroll Behaviour

- Manual scrolling (wheel, touch) is **blocked** while the tour is active.
- When navigating to a step that is not fully visible, the component:
  1. Hides the clip-path overlay
  2. Calls `scrollIntoView({ behavior: "smooth", block: "center" })`
  3. Listens for the native `scrollend` event
  4. Restores the overlay once scrolling completes
- Top 24% of the viewport is reserved for the popup — elements in that zone are scrolled further down.

## "Don't Show Again" Persistence

- The popup includes a checkbox: "Příště nezobrazovat" (Don't show again).
- Preference is stored in `localStorage` under the configurable key.
- When dismissed, the tour won't auto-show on subsequent page loads.
- The manual trigger (`onShowGuideTriggerReady`) resets the preference and re-opens the tour.

## Footer Trigger

When `isShownOnPageLoad` is `false`, the component automatically portals a "Zobrazit průvodce" (Show guide) link into the footer actions area (`.footer__actions > ul`). This uses a `MutationObserver` to wait for the footer element to appear in the DOM.

## Trigger Modes

### Auto-show on page load (`isShownOnPageLoad={true}`)

The tour opens automatically when the component mounts. The footer trigger link is **not** injected in this mode.

```tsx
<Onboarding
  steps={steps}
  isShownOnPageLoad={true}
  dontShowAgainStorageKey="storybook.onboarding.dismissed"
  onClose={() => {}}
/>
```

### Manual trigger only (`isShownOnPageLoad={false}`)

The tour does **not** auto-open. Instead:

- A "Zobrazit průvodce" link is portaled into the footer actions area automatically.
- Use `onShowGuideTriggerReady` to obtain a trigger function for custom buttons/links.
- Both manual triggers reset the "don't show again" preference before opening.

```tsx
<Onboarding
  steps={steps}
  isShownOnPageLoad={false}
  dontShowAgainStorageKey="storybook.onboarding.manual.dismissed"
  onClose={() => {}}
  onShowGuideTriggerReady={(trigger) => {
    // Store trigger for custom button
    triggerRef.current = trigger;
  }}
/>
```

## Steps Without Titles

Titles are optional. When omitted, only the content and close button are shown in the popup header:

```tsx
const steps = [
  { step: 1, content: <p>This step has no title — only content is shown.</p> },
  { step: 2, content: <p>Step two. Still no title.</p> },
  { step: 3, content: <p>Last step. Click Finish to close.</p> },
];
```

## Rich Step Content

Step content accepts `ReactNode`, so you can use formatted JSX:

```tsx
const steps = [
  {
    step: 1,
    title: "Full Name",
    content: <p>Enter the user's full name. This field is required and must be unique across the system.</p>,
  },
  {
    step: 2,
    title: "Email Address",
    content: <p>Provide a valid email address. It will be used for login and notifications.</p>,
  },
  {
    step: 3,
    title: "Submit or Reset",
    content: (
      <p>
        Click <strong>Submit</strong> to save, or <strong>Reset</strong> to clear all fields and start over.
      </p>
    ),
  },
];
```

## Best Practices

1. **Keep steps focused** — Each step should explain one UI element or concept.
2. **Use concise content** — The popup has limited space; prefer short paragraphs or bullet points.
3. **Order steps logically** — Follow the natural user workflow (top-to-bottom, left-to-right).
4. **Provide titles** — Titles give users context about what they're looking at.
5. **Use unique storage keys** — If you have multiple onboarding tours in the same app, use different `dontShowAgainStorageKey` values.
6. **Don't target hidden elements** — Ensure all `data-onboarding-step` elements are rendered and visible when the tour is active.
7. **Localize content** — Use `t()` for all user-visible text in step content.

## Common Mistakes to Avoid

### Don't forget to add data-onboarding-step attributes

```tsx
{
  /* Wrong: No data attribute — step won't find its target */
}
<Form.Input name="search" />;

{
  /* Right: Target element marked with data attribute */
}
<Form.Input data-onboarding-step="1" name="search" />;
```

### Don't mismatch step numbers

```tsx
{
  /* Wrong: Step config says 1, but DOM element has 2 */
}
<div data-onboarding-step="2" />;
const steps = [{ step: 1, content: <p>...</p> }]; // won't match!

{
  /* Right: Numbers must match */
}
<div data-onboarding-step="1" />;
const steps = [{ step: 1, content: <p>...</p> }];
```

### Don't use without onClose

```tsx
{
  /* Wrong: No way to close the tour */
}
<Onboarding steps={steps} isShown={true} onClose={undefined} />;

{
  /* Right: Always provide onClose */
}
<Onboarding steps={steps} isShown={true} onClose={() => setIsShown(false)} />;
```

## Technical Notes

- The component renders via `createPortal` into `document.body`.
- The overlay uses clip-path with SVG path for the cutout (non-zero winding rule).
- CSS anchor positioning is used for popup placement (`anchor-name` via CSS, `position-area: top center`).
- `ResizeObserver` keeps the cutout rect in sync when the target element resizes.
- The popup is a memoized subcomponent (`OnboardingPopup`) for performance.
- Progress indicator uses a CSS custom property (`--onboarding-progress-percent`).

## Version Information

- **Component Status**: Work in Progress
- **Features**: Step-by-step guided tour, clip-path overlay, smooth scrolling, CSS anchor positioning, localStorage persistence
- **Dependencies**: React (createPortal, useLayoutEffect), react-i18next, ResizeObserver, MutationObserver

## Sync Metadata

- **Component Source:** `packages/neuron/ui/src/lib/overlays/onboarding/Onboarding.tsx`
- **Guideline Command:** `/neuron-ui-onboarding`
- **Related Skill:** `neuron-ui-overlays`

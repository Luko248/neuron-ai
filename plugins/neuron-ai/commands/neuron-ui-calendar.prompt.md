---
agent: agent
---

# Calendar Component Guidelines

## Overview

The **Calendar** component (`@neuron/ui/Calendar`) is a Neuron wrapper around [FullCalendar React](https://fullcalendar.io/docs/react) that provides a standardized, enterprise-grade calendar UI with a custom responsive header, a selection-detail side sheet, event tag rendering, and a ref-based API for programmatic control.

**Version:** v1.0.0

### Key Capabilities

- Month / week / day view switching with a responsive header (ButtonGroup on desktop, Dropdown on tablet and smaller)
- Date/time slot click opens a `SideSheet` side panel listing overlapping events
- `CalendarSelectionEvent` objects rendered as `Tag` badges inside the calendar grid
- Full access to the underlying FullCalendar API via `ref.getApi()`
- Czech locale (`cs-CZ`) out of the box with i18n-ready button labels

---

## Component Architecture

```
Calendar (forwardRef)
├── CalendarHeader           — custom toolbar (prev/next/today + view switcher)
├── CalendarSelectionDetail  — SideSheet panel for the clicked date range
│   └── CalendarEventDetails — individual event card (badge, time, title, action button)
└── FullCalendar             — underlying calendar grid (headerToolbar disabled)
```

### Internal hooks and helpers

| Symbol                           | Purpose                                                            |
| -------------------------------- | ------------------------------------------------------------------ |
| `useCalendarEventTags`           | Renders `Tag` badges inside the FullCalendar event elements        |
| `resolveCalendarApi`             | Safely resolves `CalendarApi` from a component ref                 |
| `resolveCalendarApiFromInstance` | Safely resolves `CalendarApi` from a component instance            |
| `groupEventsBy`                  | Groups `CalendarSelectionEvent[]` by an arbitrary key function     |
| `toEventTagEntry`                | Maps a FullCalendar `EventApi` to a `CalendarEventTagEntry`        |
| `clearFullCalendarEventStyles`   | Removes FullCalendar default event inline styles for Tag rendering |

---

## Basic Usage

```tsx
import { Calendar } from "@neuron/ui";

export const MyPage = () => (
  <Calendar initialView="dayGridMonth" initialEvents={[{ id: "1", title: "Meeting", start: "2024-06-10T10:00:00" }]} />
);
```

The component accepts **all standard FullCalendar `CalendarOptions`** props plus Neuron-specific additions. Do **not** pass `headerToolbar` — the Neuron custom header manages navigation.

---

## CalendarSelectionEvent — Data Structure

`CalendarSelectionEvent` is the lightweight Neuron event model used for the selection-detail panel and Tag rendering. It is separate from FullCalendar's `EventInput`.

```ts
type CalendarSelectionEvent = {
  id?: string; // optional — used for stable edit matching
  title: string; // event title (required)
  description?: string; // shown below title in the detail panel
  date: Date; // event start (required)
  end?: Date; // event end — used for time-range overlap filtering
  type?: TagProps["variant"]; // "info" | "danger" | "warning" | "success" | "default" | …
  buttonProps?: Partial<ButtonProps>; // overrides for the edit button in CalendarEventDetails
};
```

### Important: Keep FullCalendar events and selectionDetailEvents in sync

When adding or editing events you must update **both** the FullCalendar event store (via `calendarRef.current?.getApi()?.addEvent(...)`) and your `selectionDetailEvents` state array. They are independent — the calendar grid reads FullCalendar's internal store while the side panel reads `selectionDetailEvents`.

```tsx
// Add to FullCalendar grid
calendarRef.current?.getApi()?.addEvent({
  id,
  title,
  start,
  end,
  allDay,
  extendedProps: { description, type },
});

// Add to Neuron selection detail panel
setSelectionDetailEvents((prev) => [...prev, { id, title, description, type, date: start, end }]);
```

---

## Calendar Views

Three built-in views are available. The header ButtonGroup/Dropdown uses these FullCalendar view names:

| Label (default) | FullCalendar view name |
| --------------- | ---------------------- |
| Month           | `dayGridMonth`         |
| Week            | `timeGridWeek`         |
| Day             | `timeGridDay`          |

Set the initial view with `initialView`:

```tsx
<Calendar initialView="timeGridWeek" />
```

Change view programmatically via the ref:

```tsx
const calendarRef = useRef<CalendarRef>(null);

calendarRef.current?.getApi()?.changeView("timeGridDay");
```

---

## Event Management — Full Create / Edit Flow

```tsx
import {
  Calendar,
  type CalendarRef,
  type CalendarSelection,
  type CalendarSelectionEvent,
  Form,
  SideSheet,
  Button,
} from "@neuron/ui";
import { useCallback, useMemo, useRef, useState } from "react";
import { useForm } from "react-hook-form";

type EventForm = {
  title: string;
  description: string;
  type: CalendarSelectionEvent["type"];
  fullDay: boolean;
  dateFrom: Date | null;
  dateTo: Date | null;
};

const DEFAULT_VALUES: EventForm = {
  title: "",
  description: "",
  type: "info",
  fullDay: false,
  dateFrom: null,
  dateTo: null,
};

export const EventCalendar = () => {
  const calendarRef = useRef<CalendarRef>(null);
  const [isOpen, setIsOpen] = useState(false);
  const [editingId, setEditingId] = useState<string | null>(null);
  const [selection, setSelection] = useState<CalendarSelection | null>(null);
  const [selectionDetailEvents, setSelectionDetailEvents] = useState<CalendarSelectionEvent[]>([]);

  const isEditMode = editingId !== null;

  const { control, handleSubmit, reset, watch } = useForm<EventForm>({
    defaultValues: DEFAULT_VALUES,
  });
  const isFullDay = watch("fullDay");

  // Open the SideSheet for a NEW event (called from onAddEventClick)
  const openForNew = useCallback(
    (sel: CalendarSelection) => {
      setEditingId(null);
      setSelection(sel);
      reset({ ...DEFAULT_VALUES, fullDay: sel.allDay, dateFrom: sel.start, dateTo: sel.end });
      setIsOpen(true);
    },
    [reset],
  );

  // Open the SideSheet for an EXISTING event (called from buttonProps.onClick)
  const openForEdit = useCallback(
    (event: CalendarSelectionEvent) => {
      setEditingId(event.id ?? null);
      setSelection({ start: event.date, end: event.end ?? event.date, allDay: false });
      reset({
        ...DEFAULT_VALUES,
        title: event.title,
        description: event.description ?? "",
        type: event.type ?? "info",
        dateFrom: event.date,
        dateTo: event.end ?? null,
      });
      setIsOpen(true);
    },
    [reset],
  );

  const close = useCallback(() => {
    setIsOpen(false);
    setSelection(null);
    setEditingId(null);
    reset(DEFAULT_VALUES);
  }, [reset]);

  const onSubmit = handleSubmit((values) => {
    const title = values.title.trim() || "New event";
    const start = values.fullDay ? (selection?.start ?? new Date()) : (values.dateFrom ?? new Date());
    const end = values.fullDay
      ? (() => {
          const d = new Date(start);
          d.setDate(d.getDate() + 1);
          return d;
        })()
      : (values.dateTo ?? start);
    const allDay = values.fullDay;

    if (isEditMode) {
      // Update FullCalendar grid event
      const fcEvent = calendarRef.current?.getApi()?.getEventById(editingId);
      fcEvent?.setProp("title", title);
      fcEvent?.setExtendedProp("type", values.type);
      fcEvent?.setDates(start, end, { allDay });

      // Update selection detail panel
      setSelectionDetailEvents((prev) =>
        prev.map((e) =>
          e.id === editingId
            ? { ...e, title, description: values.description, type: values.type, date: start, end }
            : e,
        ),
      );
    } else {
      const id = String(Date.now());

      calendarRef.current?.getApi()?.addEvent({
        id,
        title,
        start,
        end,
        allDay,
        extendedProps: { description: values.description, type: values.type },
      });

      setSelectionDetailEvents((prev) => [
        ...prev,
        { id, title, description: values.description, type: values.type, date: start, end },
      ]);
    }

    close();
  });

  // Attach edit button to each event — wire openForEdit via buttonProps
  const eventsWithButtons = useMemo(
    () =>
      selectionDetailEvents.map((e) => ({
        ...e,
        buttonProps: { onClick: () => openForEdit(e) },
      })),
    [selectionDetailEvents, openForEdit],
  );

  return (
    <>
      <Calendar
        ref={calendarRef}
        initialView="dayGridMonth"
        selectionDetailEvents={eventsWithButtons}
        onAddEventClick={openForNew}
      />

      <SideSheet
        title={isEditMode ? "Edit event" : "Create event"}
        width="narrow"
        show={isOpen}
        onHide={close}
        closeOnBackdropClick
        closeOnEscape
        fixedFooter
        rightActionsZone={
          <div className="d-flex gap-8">
            <Button variant="secondary" onClick={close}>
              Cancel
            </Button>
            <Button variant="primary" onClick={onSubmit}>
              {isEditMode ? "Save" : "Create"}
            </Button>
          </div>
        }
      >
        <div className="d-flex flex-column gap-16">
          <Form.Input name="title" labelText="Title" control={control} />
          <Form.TextArea name="description" labelText="Description" control={control} autoResize />
          <Form.Select
            name="type"
            labelText="Type"
            control={control}
            options={[
              { labelText: "Info", value: "info" },
              { labelText: "Danger", value: "danger" },
              { labelText: "Warning", value: "warning" },
              { labelText: "Success", value: "success" },
            ]}
          />
          <Form.Switch name="fullDay" labelText="Full day" control={control} />
          <Form.DatePicker name="dateFrom" labelText="From" control={control} showTime disabled={isFullDay} />
          <Form.DatePicker name="dateTo" labelText="To" control={control} showTime disabled={isFullDay} />
        </div>
      </SideSheet>
    </>
  );
};
```

---

## Selection Detail Panel

When the user clicks a date/time slot, `CalendarSelectionDetail` opens as a `SideSheet` and lists all `selectionDetailEvents` that overlap the clicked range.

- Events are filtered with `eventStart < selectionEnd && eventEnd > selectionStart` (open-interval overlap).
- The panel shows a `ContentMessage` empty state when no events overlap.
- The "Add event" button in the panel footer calls `onAddEventClick` with the `CalendarSelection` and closes the panel.
- Each event card is rendered via `CalendarEventDetails` which includes a variant badge, time range, title, optional description, and an action button.

### Wiring the edit button

`CalendarSelectionEvent.buttonProps` is forwarded directly to the `Button` inside `CalendarEventDetails`. Use this to open an edit form:

```tsx
const eventsWithButtons = useMemo(
  () =>
    selectionDetailEvents.map((event) => ({
      ...event,
      buttonProps: { onClick: () => openForEdit(event) },
    })),
  [selectionDetailEvents, openForEdit],
);

<Calendar selectionDetailEvents={eventsWithButtons} onAddEventClick={openForNew} />;
```

---

## Calendar Ref API

Expose the FullCalendar API for programmatic navigation and event manipulation:

```tsx
const calendarRef = useRef<CalendarRef>(null);

// Navigate
calendarRef.current?.getApi()?.prev();
calendarRef.current?.getApi()?.next();
calendarRef.current?.getApi()?.today();

// Change view
calendarRef.current?.getApi()?.changeView("timeGridWeek");

// Add event
calendarRef.current?.getApi()?.addEvent({ id: "x", title: "Hello", start: new Date() });

// Get and mutate existing event
const fcEvent = calendarRef.current?.getApi()?.getEventById("x");
fcEvent?.setProp("title", "Updated title");
fcEvent?.setExtendedProp("type", "success");
fcEvent?.setDates(newStart, newEnd, { allDay: false });
fcEvent?.remove();
```

### onApiReady / onApiDestroy

Use `onApiReady` to access the API after the first render (useful for loading remote events):

```tsx
const handleApiReady = useCallback((api: CalendarApi) => {
  api.addEvent({ id: "remote-1", title: "Remote Event", start: "2024-06-15" });
}, []);

<Calendar onApiReady={handleApiReady} onApiDestroy={(api) => console.log("destroyed", api)} />;
```

---

## Event Tag Rendering

When `selectionDetailEvents` is provided, the component automatically renders `Tag` badges inside the FullCalendar grid cells using the `useCalendarEventTags` hook. Tags use the `type` field of each `CalendarSelectionEvent` as their variant.

This works by matching FullCalendar's internal events (from `addEvent` / `initialEvents`) with `selectionDetailEvents` by `id` (preferred) or by `title + start time`. Keep event `id` values consistent between both stores to ensure correct badge rendering.

### Custom event rendering

If you provide your own `eventContent`, `eventDidMount`, or `eventClassNames` props, they take precedence over the automatic tag rendering:

```tsx
<Calendar selectionDetailEvents={events} eventContent={(arg) => <span>{arg.event.title}</span>} />
```

---

## Responsive Header

The `CalendarHeader` automatically adapts:

- **Desktop** (wider than tablet breakpoint): `ButtonGroup` with segment control for view switching
- **Tablet and smaller**: `Dropdown` showing the currently active view label

The breakpoint is resolved via `useResponsiveMediaQuery("tablet")` from `@neuron/core`. No configuration is required.

---

## Props Reference

| Prop                    | Type                                     | Default          | Description                                                        |
| ----------------------- | ---------------------------------------- | ---------------- | ------------------------------------------------------------------ |
| `initialView`           | `string`                                 | `"dayGridMonth"` | Starting FullCalendar view                                         |
| `initialEvents`         | `EventInput[]`                           | —                | Initial events loaded into the FullCalendar grid                   |
| `selectionDetailEvents` | `CalendarSelectionEvent[]`               | `[]`             | Neuron events for the selection panel and Tag badges               |
| `onAddEventClick`       | `(selection: CalendarSelection) => void` | —                | Called when "Add event" is clicked in the selection panel          |
| `onApiReady`            | `(api: CalendarApi) => void`             | —                | Called once after the first render with the FullCalendar API       |
| `onApiDestroy`          | `(api: CalendarApi) => void`             | —                | Called on unmount with the last known API instance                 |
| `className`             | `string`                                 | —                | CSS class applied to the wrapper `div`                             |
| `testId`                | `string`                                 | —                | Sets `data-testid` on the wrapper element                          |
| `ref`                   | `RefObject<CalendarRef>`                 | —                | Exposes `getApi()` for programmatic control                        |
| `…FullCalendarOptions`  | any                                      | —                | All standard FullCalendar props (except `headerToolbar` and `ref`) |

### CalendarSelection

```ts
type CalendarSelection = {
  start: Date; // selection start
  end: Date; // selection end
  allDay: boolean;
};
```

### CalendarRef

```ts
interface CalendarRef {
  getApi: () => CalendarApi | null;
}
```

---

## Accessibility

- Navigation buttons use Neuron `Button` with `variant="secondary"` — keyboard accessible.
- View switching with `ButtonGroup` (segment control) supports keyboard and screen readers.
- `SideSheet` panel traps focus when open (`closeOnEscape` enabled by default).
- All user-facing labels in the header use i18n keys (`neuron.ui.calendar.*`).

---

## i18n Keys

The Calendar component uses these translation keys:

| Key                                   | Default value (fallback)                       |
| ------------------------------------- | ---------------------------------------------- |
| `neuron.ui.calendar.btnToday`         | Today                                          |
| `neuron.ui.calendar.emptyTitle`       | Žádné události                                 |
| `neuron.ui.calendar.emptyDescription` | V tomto období není naplánovaná žádná událost. |
| `neuron.ui.calendar.addEvent`         | Přidat událost                                 |

Add these keys to your translation files to provide localized labels.

---

## Common Use Cases

### Dashboard Calendar (read-only with event list)

```tsx
const [events] = useState<CalendarSelectionEvent[]>([
  {
    id: "1",
    title: "Policy Review",
    date: new Date("2024-06-10T09:00"),
    end: new Date("2024-06-10T10:00"),
    type: "info",
  },
  { id: "2", title: "Claim Filing", date: new Date("2024-06-12"), type: "warning" },
]);

<Calendar
  initialView="dayGridMonth"
  initialEvents={[
    {
      id: "1",
      title: "Policy Review",
      start: "2024-06-10T09:00",
      end: "2024-06-10T10:00",
      extendedProps: { type: "info" },
    },
    { id: "2", title: "Claim Filing", start: "2024-06-12", allDay: true, extendedProps: { type: "warning" } },
  ]}
  selectionDetailEvents={events}
/>;
```

### Programmatic navigation (today button in page header)

```tsx
const calendarRef = useRef<CalendarRef>(null);

const goToToday = () => calendarRef.current?.getApi()?.today();

<Button onClick={goToToday}>Jump to Today</Button>
<Calendar ref={calendarRef} initialView="timeGridWeek" />
```

### Loading events from API after mount

```tsx
const handleApiReady = useCallback(async (api: CalendarApi) => {
  const data = await fetchCalendarEvents(); // your RTK Query or fetch call
  data.forEach((event) => api.addEvent(event));
}, []);

<Calendar onApiReady={handleApiReady} initialView="dayGridMonth" />;
```

---

## Best Practices

### DO

- ✅ Always keep `selectionDetailEvents` and FullCalendar grid events in sync — update both on create/edit/delete.
- ✅ Assign stable `id` values to events so that `getEventById` and Tag badge matching work correctly.
- ✅ Use `initialEvents` for static/pre-loaded data; use `onApiReady` + `api.addEvent()` for async data.
- ✅ Wrap `selectionDetailEvents` with `useMemo` when you transform them (e.g., attaching `buttonProps`).
- ✅ Use `ref.getApi()` for all programmatic operations (navigation, add, edit, delete).
- ✅ Use `Form.DatePicker` with `showTime` for timed events in the create/edit form.
- ✅ Use `Form.Switch` for the full-day toggle and disable `dateFrom`/`dateTo` when `fullDay` is true.
- ✅ Provide all i18n keys in your translation files to avoid fallback strings in production.

### DON'T

- ❌ Never pass `headerToolbar` prop — the Neuron custom header is always used.
- ❌ Never pass `ref` as a FullCalendar option — use the `CalendarRef` interface via `forwardRef`.
- ❌ Don't directly mutate `selectionDetailEvents` items — use functional state updates.
- ❌ Don't skip providing `id` on events when you need edit/delete operations.
- ❌ Don't hardcode button labels — use i18n translation keys.
- ❌ Don't render a custom event creation form outside `SideSheet` — use `SideSheet` for consistency with the design system.
- ❌ Don't rely on `eventContent` / `eventDidMount` for basic badge rendering when `selectionDetailEvents` already handles it automatically.

---

## Common Mistakes

### ❌ Forgetting to sync both event stores

```tsx
// ❌ BAD — only adds to FullCalendar grid, detail panel won't show the event
calendarRef.current?.getApi()?.addEvent({ id: "1", title: "Meeting", start });

// ✅ GOOD — update both
calendarRef.current?.getApi()?.addEvent({ id: "1", title: "Meeting", start, extendedProps: { type: "info" } });
setSelectionDetailEvents((prev) => [...prev, { id: "1", title: "Meeting", date: start, type: "info" }]);
```

### ❌ Providing `headerToolbar` prop

```tsx
// ❌ BAD — overrides the Neuron header
<Calendar headerToolbar={{ left: "prev,next today", center: "title", right: "dayGridMonth,timeGridWeek" }} />

// ✅ GOOD — omit headerToolbar entirely
<Calendar initialView="dayGridMonth" />
```

### ❌ Missing `id` on events when editing

```tsx
// ❌ BAD — getEventById returns null, edit silently fails
calendarRef.current?.getApi()?.addEvent({ title: "Meeting", start });

// ✅ GOOD
const id = String(Date.now());
calendarRef.current?.getApi()?.addEvent({ id, title: "Meeting", start });
setSelectionDetailEvents((prev) => [...prev, { id, title: "Meeting", date: start }]);
```

### ❌ Defining eventsWithButtons inline in JSX

```tsx
// ❌ BAD — creates new array on every render, causing unnecessary re-renders
<Calendar
  selectionDetailEvents={selectionDetailEvents.map((e) => ({
    ...e,
    buttonProps: { onClick: () => openForEdit(e) },
  }))}
/>;

// ✅ GOOD — memoize the transform
const eventsWithButtons = useMemo(
  () => selectionDetailEvents.map((e) => ({ ...e, buttonProps: { onClick: () => openForEdit(e) } })),
  [selectionDetailEvents, openForEdit],
);
<Calendar selectionDetailEvents={eventsWithButtons} />;
```

---

## For the AI Assistant

### 🚨 MANDATORY RULES

**ALWAYS:**

- Import `Calendar` from `@neuron/ui` — never from FullCalendar directly.
- Use `CalendarRef`, `CalendarSelection`, `CalendarSelectionEvent` types from `@neuron/ui`.
- Keep FullCalendar `initialEvents` / `addEvent` events **and** `selectionDetailEvents` in sync when implementing CRUD.
- Assign a stable `id` to every event when create/edit/delete is needed.
- Wrap `selectionDetailEvents` transformations (e.g., adding `buttonProps`) in `useMemo`.
- Use `SideSheet` (from `@neuron/ui`) for the event create/edit form — never a custom modal.
- Use `Form.*` components (Input, TextArea, Select, Switch, DatePicker) inside the SideSheet form.
- Use `useForm` from `react-hook-form` for the event form.
- Use `onAddEventClick` to open the create form with the selected time slot context.
- Use `buttonProps.onClick` on `CalendarSelectionEvent` to open the edit form.
- Use `ref.getApi()` for all programmatic calendar operations.
- Use i18n keys for all user-facing labels — never hardcode Czech or English strings.

**NEVER:**

- Pass `headerToolbar` to `Calendar` — the Neuron custom header is always rendered.
- Pass `ref` as a regular FullCalendar option — use `forwardRef` pattern with `CalendarRef`.
- Import from `@fullcalendar/*` directly in application code — use the re-exported types from `@neuron/ui`.
- Mutate `selectionDetailEvents` items directly — always use functional state updates.
- Define `selectionDetailEvents` inline in JSX without `useMemo`.
- Create custom event badge rendering when `selectionDetailEvents` already handles it.
- Skip the dual-update pattern (grid + selectionDetailEvents) on create/edit/delete.

### Implementation Patterns

**Minimal read-only calendar:**

```tsx
<Calendar initialView="dayGridMonth" initialEvents={events} selectionDetailEvents={neuronEvents} />
```

**Calendar with create/edit (standard pattern):**

1. Declare `calendarRef = useRef<CalendarRef>(null)`
2. Declare `selectionDetailEvents` state
3. Pass `eventsWithButtons` (memoized with `buttonProps`) to `selectionDetailEvents`
4. Pass `onAddEventClick={openForNew}` callback
5. Render a separate `SideSheet` with `Form.*` fields for create/edit
6. On submit: update FullCalendar via `ref.getApi()` AND update `selectionDetailEvents` state

**Event type → Tag variant mapping:**

- `"info"` → blue badge (default)
- `"success"` → green badge
- `"warning"` → yellow badge
- `"danger"` → red badge

**TypeScript import pattern:**

```tsx
import {
  Calendar,
  type CalendarRef,
  type CalendarSelection,
  type CalendarSelectionEvent,
  type CalendarApi,
  type EventInput,
} from "@neuron/ui";
```

## Sync Metadata

- **Component Version:** v1.1.1
- **Component Source:** `packages/neuron/ui/src/lib/data/calendar/Calendar.tsx`
- **Guideline Command:** `/neuron-ui-calendar`
- **Related Skill:** `neuron-ui-data`

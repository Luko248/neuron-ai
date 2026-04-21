---
name: neuron-unit-testing
description: Authoritative rules and patterns for writing unit tests in the Neuron frontend monorepo. Use when creating or updating tests.
---

# Neuron Unit Testing Guidelines

This document defines authoritative rules and patterns for writing tests in this repo.

- If it conflicts with generic "best practices", follow THIS document.

## 0) Primary goal

Write tests that prevent regressions with minimal maintenance cost.

Priority order:

1. Correctness (real behavioral guarantees)
2. Stability (deterministic, non-flaky)
3. Readability (future devs can maintain)
4. Coverage % (nice side-effect, not the goal)

---

## 1) Repository structure and what to test first

Repo layout:

- `packages/neuron/ui/` – shared framework UI components
- `apps/[app-name]/src/app/elements/` – app-specific reusable components
- `apps/[app-name]/src/app/features/` – main feature implementation and logic
- `apps/[app-name]/src/app/pages/` – main component for a page
- `apps/[app-name]/src/app/store/` – Redux-related code
- `apps/[app-name]/src/app/routing/` – routing configuration

### Test priority (default)

1. `apps/[app-name]/src/app/store/` (reducers/slices, selectors) and `common` utils
2. `packages/neuron/ui/` (framework UI components - behavior-focused tests, following Storybook scenarios)
3. `apps/[app-name]/src/app/features/` (critical business logic and component behavior)
4. `apps/[app-name]/src/app/elements/` (critical reusable components)
5. Pages and Routing (only if explicitly requested)

---

## 2) Hard rules (must)

1. **No production behavior changes.** Tests must adapt to code, not the other way around.
2. **No dependency upgrades** unless absolutely required to run tests AND explicitly approved.
3. **Minimal diff**: add only what is needed (tests + small test seams if unavoidable).
4. **Deterministic tests only**:
   - No real network
   - No real time without control
   - No randomness
5. **No snapshot spam**:
   - Snapshots only if explicitly justified (rare).
   - Prefer explicit assertions.
6. **Do not assert translated UI strings** by default.
   - Prefer mocking i18n so `t(key) => key`.
7. **Mock at boundaries**, not internals:
   - Mock APIs, RTK Query, storage, window.location, time, etc.
   - Do NOT mock internal helpers of the unit under test.
8. Every test must answer: **what behavior is guaranteed?** If not clear, delete the test.
9. **Mandatory Cleanup:** All mocks (`jest.spyOn`, `vi.spyOn`) must be cleaned in `afterEach` (for example `jest.clearAllMocks()`).
10. **Storybook Alignment:** For UI components, test scenarios must follow the structure of scenarios defined in Storybook for that component.

---

## 3) Test location & naming conventions (mandatory)

### Where tests live

Every component/module folder that is tested must have a `__tests__` subfolder.

Examples:

- `packages/neuron/ui/src/lib/knzsus/card/__tests__/Card.test.tsx`
- `apps/[app-name]/src/app/features/dashboard/__tests__/Dashboard.test.tsx`
- `apps/[app-name]/src/app/store/user/__tests__/userSlice.test.ts`

### File naming

- Use `*.test.ts` for non-React tests.
- Use `*.test.tsx` for React component tests.

No other naming styles (`spec`, `tests.ts`, etc.).

---

## 4) Test pyramid for this repo

### Tier A — Pure logic (highest priority)

Test:

- mappers (API -> UI model)
- reducers / slices
- selectors
- pure utils (formatters, validators, guards)

Rules:

- No React
- No DOM
- Fast, minimal mocking

### Tier B — Hooks & orchestration

Test:

- hooks with business logic (pagination, derived state, error glue)
- RTK Query integration behavior (boundary mocked)

Rules:

- Focused renders only
- Stable mocks

### Tier C — Components (critical flows only)

Test:

- meaningful user interactions and outcomes (loading/error/empty/permission states)
  Avoid:
- DOM structure testing
- className assertions
- testing design system internals
- huge "page snapshots"

---

## 5) Tooling & assertions conventions

- Use the repo's existing test runner (Jest or Vitest). Do NOT introduce a second runner.
- Use React Testing Library for component tests.
- Prefer `@testing-library/user-event` for interactions.
- Query preference order:
  1. `getByRole` / `findByRole`
  2. `getByLabelText`
  3. `getByText` only if stable (and preferably for key-like strings)
  4. `getByTestId` as last resort
- Avoid brittle selectors (CSS classes, deep DOM traversal).

---

## 6) Mocking rules (repo-specific)

### i18n (react-i18next)

Default in tests:

- mock translation so `t(key) => key`
- do not load real translation resources unless a test explicitly validates localization behavior

### Network / API / RTK Query

- Never call real endpoints.
- Mock at the boundary (generated hooks module boundary or baseQuery boundary).
- Do not test RTK Query internals; test how OUR code reacts to:
  - loading
  - success
  - error
  - empty result

### Time

If code depends on `Date.now()` / timers:

- freeze time OR use fake timers intentionally
- always restore after the test

---

## 7) Shared test utilities (mandatory)

All hook/component tests MUST use shared helpers instead of ad-hoc providers.

Required utilities (create once, reuse everywhere):

- `renderWithProviders(ui, options)`:
  - wraps Redux Provider (store factory)
  - optional Router wrapper if needed
  - i18n mock provider
- `makeTestStore(preloadedState?)`
- `factories/*` for test data (see next section)

Ad-hoc local provider setups are not allowed unless explicitly justified.

---

## 8) Factory pattern (mandatory)

### Why

This repo uses typed domain models and/or API models. Writing raw objects inline leads to:

- massive boilerplate
- fragile tests when types evolve
- repeated "just enough fields" mistakes

Factories solve this.

### Rules

1. Every non-trivial type used in tests must have a factory builder.
2. Factories must create **valid default objects**.
3. Tests can override only fields relevant to the scenario.
4. Factories must keep defaults stable and deterministic (no random IDs unless fixed).

### Location

Create factories under a shared location and reuse, following existing patterns in the app (e.g. `src/app/common/test/factories/`).

### Factory API style (preferred)

Use builder functions with overrides:

- `makeUser(overrides?: Partial<User>): User`
- `makePost(overrides?: Partial<PostRest>): PostRest`
- `makePagination(overrides?: Partial<IPagination>): IPagination`

For arrays:

- `makePostList(count = 3, overrides?: Partial<PostRest>): PostRest[]`

For nested:

- Compose factories, do not inline deep object trees.

### Example

```ts
// factories/post.ts
export const makePost = (overrides: Partial<PostRest> = {}): PostRest => ({
  id: "post-1",
  title: "Title",
  state: "PUBLISHED",
  locations: [],
  ...overrides,
});
```

---

## 9) What "good tests" look like (examples)

✅ **Good:**

- mapping function returns correct UI model for representative inputs
- reducer transitions state correctly (happy path + edge cases)
- component disables submit button until form valid
- structure mirrors Storybook variant configurations

❌ **Bad:**

- snapshot of an entire page
- asserting exact translated text
- mocking half the module under test
- "test just to increase coverage"

## Coverage strategy (realistic)

- Target: protect critical behavior first.
- If coverage thresholds exist, meet them without low-value tests.
- Prefer Tier A tests to increase coverage quickly and meaningfully.

## Definition of Done for each test

A module is considered "covered" when:

- happy path is tested
- at least one meaningful failure/edge case is tested
- tests are deterministic and readable
- mocks are minimal and explicit
- for UI components, all meaningful Storybook scenarios are tested

---

## 10) Integrity Protocol (Anti-Bias)

If you are in the role of an AI Agent, proceed as follows:

1. **Analyze code and requirements**: Review both the existing code and the task requirements. If you spot an obvious bug in the code before writing the test, **report it immediately**.
2. **Write the test based on requirements**: Create a test that represents the **correct behavior** (as defined by requirements or Storybook variants), not just the current implementation.
3. **Run the test**:
   - If the test fails due to a bug in the production code: **STOP**. Describe the discrepancy to the user: _"The code returns X, but according to the requirements, it should return Y. Should I fix the production code or adjust the test?"_
4. **No "Pass-at-all-costs"**: Never modify an `expect()` statement just to make a test pass over a bug in the code.

---

## 11) Review checklist (PR gate)

- [ ] No production behavior changes
- [ ] No snapshots unless justified
- [ ] No assertions on translated strings (unless intentional)
- [ ] Mocks only at boundaries
- [ ] Tests read like behavior specs
- [ ] UI component test suite structure matches Storybook variants
- [ ] Tests pass locally and in CI

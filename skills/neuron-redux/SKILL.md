---
name: neuron-redux
description: Implement Redux state management using Redux Toolkit in Neuron applications. Use when setting up store, creating slices, or integrating middleware. Covers store configuration, slice creation with createSlice, RTK Query integration, middleware setup (redux-observable, logger), and component usage patterns. Provides type-safe Redux implementation.
---

# Redux State Management

Implement Redux state management using Redux Toolkit in Neuron applications.

## Process

1. **Configure store** - Set up Redux store with middleware
2. **Create slices** - Define state slices with createSlice
3. **Add middleware** - Configure redux-observable and logger
4. **Integrate RTK Query** - Add API slices to store
5. **Use in components** - Access state with useSelector and dispatch

## Store Configuration

**configureStore.ts:**

```typescript
import { configureStore } from "@reduxjs/toolkit";
import { createEpicMiddleware } from "redux-observable";
import { createReduxSerializationMiddleware } from "@neuron/core";
import logger from "redux-logger";
import { rootReducer } from "./rootReducer";
import { rootEpic } from "./rootEpic";

const epicMiddleware = createEpicMiddleware();
const serializationMiddleware = createReduxSerializationMiddleware();

export const store = configureStore({
  reducer: rootReducer,
  middleware: (getDefaultMiddleware) =>
    getDefaultMiddleware({ serializableCheck: false })
      .concat(epicMiddleware)
      .concat(serializationMiddleware)
      .concat(logger),
});

epicMiddleware.run(rootEpic);

export type RootState = ReturnType<typeof store.getState>;
export type AppDispatch = typeof store.dispatch;
```

**rootReducer.ts:**

```typescript
import { combineReducers } from "@reduxjs/toolkit";
import { insertReducer } from "@neuron/core";
import { myApiApi } from "@api/my-api";
import counterReducer from "./slices/counterSlice";

export const rootReducer = combineReducers({
  counter: counterReducer,
  ...insertReducer(myApiApi),
});
```

## Slice Creation

**counterSlice.ts:**

```typescript
import { createSlice, PayloadAction } from "@reduxjs/toolkit";

interface CounterState {
  value: number;
  status: "idle" | "loading" | "failed";
}

const initialState: CounterState = {
  value: 0,
  status: "idle",
};

export const counterSlice = createSlice({
  name: "counter",
  initialState,
  reducers: {
    increment: (state) => {
      state.value += 1;
    },
    decrement: (state) => {
      state.value -= 1;
    },
    incrementByAmount: (state, action: PayloadAction<number>) => {
      state.value += action.payload;
    },
    setStatus: (state, action: PayloadAction<CounterState["status"]>) => {
      state.status = action.payload;
    },
  },
});

export const { increment, decrement, incrementByAmount, setStatus } = counterSlice.actions;
export default counterSlice.reducer;
```

## Redux Observable (Epics)

**rootEpic.ts:**

```typescript
import { combineEpics } from "redux-observable";
import { counterEpics } from "./epics/counterEpics";

export const rootEpic = combineEpics(counterEpics);
```

**counterEpics.ts:**

```typescript
import { Epic } from "redux-observable";
import { filter, map, delay } from "rxjs/operators";
import { increment, setStatus } from "../slices/counterSlice";

export const incrementEpic: Epic = (action$) =>
  action$.pipe(
    filter(increment.match),
    delay(1000),
    map(() => setStatus("idle")),
  );

export const counterEpics = combineEpics(incrementEpic);
```

## Component Usage

**Using Redux in components:**

```typescript
import { useSelector, useDispatch } from "react-redux";
import { RootState, AppDispatch } from "app/store/configureStore";
import { increment, decrement, incrementByAmount } from "app/store/slices/counterSlice";

const Counter = () => {
  const count = useSelector((state: RootState) => state.counter.value);
  const status = useSelector((state: RootState) => state.counter.status);
  const dispatch = useDispatch<AppDispatch>();

  return (
    <div>
      <p>Count: {count}</p>
      <p>Status: {status}</p>
      <button onClick={() => dispatch(increment())}>Increment</button>
      <button onClick={() => dispatch(decrement())}>Decrement</button>
      <button onClick={() => dispatch(incrementByAmount(5))}>Add 5</button>
    </div>
  );
};
```

## RTK Query Integration

**Add API to store:**

```typescript
import { myApiApi } from "@api/my-api";

export const rootReducer = combineReducers({
  ...insertReducer(myApiApi),
});

export const store = configureStore({
  reducer: rootReducer,
  middleware: (getDefaultMiddleware) => getDefaultMiddleware().concat(myApiApi.middleware),
});
```

## Examples

**Example 1: Simple counter slice**

```typescript
export const counterSlice = createSlice({
  name: "counter",
  initialState: { value: 0 },
  reducers: {
    increment: (state) => {
      state.value += 1;
    },
    decrement: (state) => {
      state.value -= 1;
    },
  },
});
```

**Example 2: Async action with epic**

```typescript
const fetchDataEpic: Epic = (action$) =>
  action$.pipe(
    filter(fetchData.match),
    switchMap(() =>
      from(api.getData()).pipe(
        map((data) => fetchDataSuccess(data)),
        catchError((error) => of(fetchDataFailure(error))),
      ),
    ),
  );
```

**Example 3: Using typed hooks**

```typescript
// hooks.ts
import { TypedUseSelectorHook, useDispatch, useSelector } from "react-redux";
import { RootState, AppDispatch } from "./configureStore";

export const useAppDispatch = () => useDispatch<AppDispatch>();
export const useAppSelector: TypedUseSelectorHook<RootState> = useSelector;

// Usage
const count = useAppSelector((state) => state.counter.value);
const dispatch = useAppDispatch();
```

## Best Practices

- Use Redux Toolkit's createSlice for reducers
- Use insertReducer for RTK Query APIs
- Configure middleware in proper order
- Use redux-observable for async side effects
- Add redux-logger in development only
- Use typed hooks (useAppSelector, useAppDispatch)
- Keep slices focused and small
- Use PayloadAction for typed actions
- Combine epics with combineEpics
- Export actions and reducer from slices

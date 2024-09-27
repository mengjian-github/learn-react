---
title: reducer 和 context 结合使用
sidebar_position: 
---

# reducer 和 context 结合使用

Reducers 让你将组件的状态更新逻辑集中在一起。Context 让你将信息深入传递给其他组件。你可以将 reducers 和 context 结合使用，以管理复杂屏幕的状态。

## 举个例子

```jsx
import React, { createContext, useContext, useReducer } from 'react';

// 1. 创建Context
const CounterContext = createContext();

// 2. 定义reducer
const counterReducer = (state, action) => {
  switch (action.type) {
    case 'INCREMENT':
      return { count: state.count + 1 };
    case 'DECREMENT':
      return { count: state.count - 1 };
    default:
      return state;
  }
};

// 3. 创建Provider组件
const CounterProvider = ({ children }) => {
  const [state, dispatch] = useReducer(counterReducer, { count: 0 });

  return (
    <CounterContext.Provider value={{ state, dispatch }}>
      {children}
    </CounterContext.Provider>
  );
};

// 4. 创建使用Context的子组件
const Counter = () => {
  const { state, dispatch } = useContext(CounterContext);

  return (
    <div className="p-4 bg-gray-100 rounded-lg shadow-md">
      <h2 className="text-2xl font-bold mb-4">计数器: {state.count}</h2>
      <div className="space-x-2">
        <button
          className="px-4 py-2 bg-blue-500 text-white rounded hover:bg-blue-600"
          onClick={() => dispatch({ type: 'INCREMENT' })}
        >
          增加
        </button>
        <button
          className="px-4 py-2 bg-red-500 text-white rounded hover:bg-red-600"
          onClick={() => dispatch({ type: 'DECREMENT' })}
        >
          减少
        </button>
      </div>
    </div>
  );
};

// 主应用组件
const App = () => {
  return (
    <CounterProvider>
      <div className="container mx-auto mt-10">
        <h1 className="text-3xl font-bold mb-6">Reducer 和 Context 示例</h1>
        <Counter />
      </div>
    </CounterProvider>
  );
};

export default App;
```
1. 创建 Context：使用 `createContext` 创建一个 Context 对象。
2. 定义 Reducer：定义一个 reducer 函数来处理状态更新逻辑。
3. 创建 Provider：使用 `useReducer` 创建一个 Provider 组件，将状态和 dispatch 函数传递给子组件。
4. 使用 Context：在子组件中使用 `useContext` 获取 Context 的值。

## 适用场景

- 复杂状态管理：当需要管理的状态较为复杂，且需要在多个组件间共享时，使用 reducer 和 context 结合可以很好地管理状态。
- 状态分离：将状态管理逻辑与组件分离，使代码更加清晰和易于维护。

## 适用状态管理的库

对于复杂的状态管理，可以寻找一些成熟的库，比如：

- [zustand](https://github.com/pmndrs/zustand)
- [jotai](https://github.com/pmndrs/jotai)

以下是适用zustand的例子：

```jsx
import React from 'react';
import create from 'zustand';

// 创建store
const useCounterStore = create((set) => ({
  count: 0,
  increment: () => set((state) => ({ count: state.count + 1 })),
  decrement: () => set((state) => ({ count: state.count - 1 })),
}));

// 计数器组件
const Counter = () => {
  const { count, increment, decrement } = useCounterStore();

  return (
    <div className="p-4 bg-gray-100 rounded-lg shadow-md">
      <h2 className="text-2xl font-bold mb-4">计数器: {count}</h2>
      <div className="space-x-2">
        <button
          className="px-4 py-2 bg-blue-500 text-white rounded hover:bg-blue-600"
          onClick={increment}
        >
          增加
        </button>
        <button
          className="px-4 py-2 bg-red-500 text-white rounded hover:bg-red-600"
          onClick={decrement}
        >
          减少
        </button>
      </div>
    </div>
  );
};

// 主应用组件
const App = () => {
  return (
    <div className="container mx-auto mt-10">
      <h1 className="text-3xl font-bold mb-6">Zustand 示例</h1>
      <Counter />
    </div>
  );
};

export default App;

```

可以看到zustand的一些直观优点：

- 代码更简洁：不需要创建Context、Provider或编写reducer函数。
- 更易使用：直接从store中获取状态和操作，无需使用useContext或dispatch。
- 更少的样板代码：整个状态管理逻辑都在create函数中定义。

## 回顾

- 使用 reducer 和 context 结合使用，可以很好地管理复杂的状态。
- Zustand 提供了一种更简洁、更易用的状态管理方式，减少了样板代码。
- 选择状态管理方案时，应考虑项目规模、团队熟悉度和具体需求。

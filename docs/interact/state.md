---
title: react 状态
sidebar_position: 2
---

# react 状态

组件经常需要根据用户交互来改变屏幕上显示的内容。例如:

- 在表单中输入文字应该更新输入框的内容
- 点击图片轮播的"下一张"按钮应该切换显示的图片
- 点击"购买"按钮应该将商品添加到购物车

这些情况下,组件需要"记住"一些信息:当前的输入值、当前显示的图片、购物车中的商品等。在React中,这种组件特有的"记忆"就叫做状态(state)。

## useState

`useState` 是 React 中用于在函数组件中管理状态的钩子。它接受一个初始值作为参数，并返回一个包含当前状态值和更新状态值的数组。

```jsx
const [state, setState] = useState(initialState);
```

要使用新数据更新组件，需要完成两件事:

1. 在渲染之间保留数据。
2. 触发React重新渲染组件以显示新数据。

useState Hook正好提供了这两个功能:

1. 一个状态变量，用于在渲染之间保留数据。
2. 一个状态设置函数，用于更新变量并触发React重新渲染组件。

:::warning
Hooks（以 use 开头的函数）只能在组件的顶层或自定义 Hook 中调用。不能在条件语句、循环或其他嵌套函数中调用 Hooks。虽然 Hooks 是函数，但可以将它理解为声明函数。你在组件顶部"使用"React 特性，就像在文件顶部"导入"模块一样。
:::

## 使用状态

让我们来看一个使用状态的简单例子：

```jsx
import { useState } from 'react';

const Counter = () => {
  const [count, setCount] = useState(0);

  return (
    <div>
      <p>Count: {count}</p>
      <button onClick={() => setCount(count + 1)}>Increment</button>
    </div>
  );
};

export default Counter;
```

在这个例子中，我们使用 `useState` 创建了一个名为 `count` 的状态变量，并初始化为 0。然后，我们使用 `setCount` 函数来更新 `count` 的值。每当用户点击按钮时，`count` 的值就会增加 1，并且组件会重新渲染以显示新的计数值。

:::info
在多个useState调用中，每个useState调用都会创建一个独立的状态变量。那么React如何知道哪个状态变量对应哪个useState调用呢？

实际上，在React背后是用一个数组来存储状态变量的值，数组的下标就是useState调用的顺序。这就是为什么我们不能在条件语句或循环中使用useState，因为这会破坏这个顺序。

一个简单的useState实现：

```js
let componentHooks = [];
let currentHookIndex = 0;

function useState(initialState) {
  let pair = componentHooks[currentHookIndex];
  if (pair) {
    currentHookIndex++;
    return pair;
  }

  pair = [initialState, setState];

  function setState(nextState) {
    pair[0] = nextState;
    render();
  }

  componentHooks[currentHookIndex] = pair;
  currentHookIndex++;
  return pair;
}
```
:::

## state是独立的

每个组件都有自己的状态，它们之间是独立的。

```jsx
const Counter = () => {
  const [count, setCount] = useState(0);

  return (
    <div>
      <p>Count: {count}</p>
      <button onClick={() => setCount(count + 1)}>Increment</button>
    </div>
  );
};

const App = () => {
  return (
    <div>
      <Counter />
      <Counter />
    </div>
  );
};
```

在这个例子中，我们有两个 `Counter` 组件，它们各自拥有自己的状态。当我们点击其中一个按钮时，不会影响另一个组件的状态。

## 回顾

- useState 是 React 中用于在函数组件中管理状态的钩子。
- 每个组件都有自己的状态，它们之间是独立的。
- useState 返回一个包含当前状态值和更新状态值的数组。
- 使用状态变量来在渲染之间保留数据。
- 使用状态设置函数来更新变量并触发React重新渲染组件。




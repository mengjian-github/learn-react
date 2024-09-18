---
title: 第一个组件
sidebar_position: 1
---

# 第一个组件

React 组件是 React 应用的基本构建块。本节我们将学习如何创建一个组件。

## 创建一个组件

React 组件本质是一个 JavaScript 函数：

```jsx
function MyComponent() {
  return <h1>Hello, World!</h1>;
}
```

:::warning

注意，React 组件的名称必须以大写字母开头，以区分于普通的 HTML 标签。

:::

在React中我们可以直接返回一个JSX，JSX是HTML语法在JavaScript中的扩展，React 组件也可以嵌套在其他组件中。

```jsx
function App() {
  return (
    <div>
      <MyComponent />
    </div>
  );
}
```

像上面这样，我们就将 `MyComponent` 组件嵌套在 `App` 组件中。

:::warning

不要在组件内部定义组件，这会导致严重的性能问题以及Bug。例如：

```jsx
function MyComponent() {
  function AnotherComponent() {
    return <h2>Another Component</h2>;
  }
  return (
    <div>
      <AnotherComponent />
    </div>
  );
}
```

正确的做法是在首层定义组件，然后在内部使用。

```jsx
function AnotherComponent() {
  return <h2>Another Component</h2>;
}

function MyComponent() {
  return (
    <div>
      <AnotherComponent />
    </div>
  );
}
```

:::

## 回顾

1. 组件是一个函数，返回一个 JSX。
2. 组件的名称必须以大写字母开头。
3. 组件可以嵌套在其他组件中。
4. 不要在组件内部定义组件，这会导致严重的性能问题以及Bug。
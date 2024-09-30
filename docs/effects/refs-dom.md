---
title: 使用Refs更新DOM
sidebar_position: 2
---

# 使用Refs更新DOM

React 会自动更新 DOM 以匹配你的渲染输出，因此你的组件通常无需直接操作 DOM。

然而，在某些情况下，你可能需要访问由 React 管理的 DOM 元素。例如，你可能想要聚焦一个特定节点、滚动到某个位置，或者测量元素的尺寸和位置。由于 React 没有内置的方法来执行这些操作，你需要使用 ref 来获取对 DOM 节点的引用。

## 举个例子

假设你有一个输入框，你想在用户点击按钮时聚焦它。你可以使用 ref 来获取对输入框的引用，并在点击按钮时调用 `focus()` 方法。

```jsx
import { useRef } from 'react';

export default function Form() {
  const inputRef = useRef(null);

  function handleClick() {
    inputRef.current.focus();
  }

  return (
    <>
      <input ref={inputRef} />
      <button onClick={handleClick}>Focus Input</button>
    </>
  );
}
```

在这个例子中，我们使用 `useRef` 创建了一个 ref 对象，并将其传递给输入框。当用户点击按钮时，`handleClick` 函数被调用，它会调用 `inputRef.current.focus()` 来聚焦输入框。

## ref callback

ref callback是一个函数，而不是一个对象。这个函数会在组件挂载时被调用，并且在组件卸载时再次被调用（这时传入null）。

```jsx
<div ref={(element) => {
  // 这里是ref callback函数体
  // element是对应的DOM元素
}}>
```

使用ref callback可以做到多个元素的ref引用，而不用事先创建多个ref对象。这在列表渲染中非常有用。比如：

```jsx
<li
  key={cat.id}
  ref={node => {
    const map = getMap();
    if (node) {
      // Add to the Map
      map.set(cat, node);
    } else {
      // Remove from the Map
      map.delete(cat);
    }
  }}
>
```

## 为组件添加ref

默认情况下，React 组件不会接收 ref 引用。比如：

```jsx
function MyComponent() {
  return <div>Hello</div>;
}
```

此时，如果你尝试添加ref引用：

```jsx
<MyComponent ref={ref} />
```

React会给出警告：

```
Warning: Function components cannot be given refs. Attempts to access this ref will fail. Did you mean to use React.forwardRef()?
```

要解决这个问题，你需要使用 `React.forwardRef` 来包装你的组件：

```jsx
const MyComponent = React.forwardRef((props, ref) => {
  return <div ref={ref}>Hello</div>;
});
```

在设计系统中，低级组件（如按钮、输入框等）通常会将其 refs 转发到它们的 DOM 节点。另一方面，高级组件（如表单、列表或页面部分）通常不会暴露其 DOM 节点，以避免意外的 DOM 结构依赖。

## 使用 useImperativeHandle

useImperativeHandle 允许你自定义暴露给父组件的 ref 对象。这样，你可以精确控制父组件能够访问的方法和属性。

例如，如果你只想让父组件能够调用子组件的 focus 方法：

```jsx
const MyInput = React.forwardRef((props, ref) => {
  const inputRef = useRef(null);

  useImperativeHandle(ref, () => ({
    focus() {
      inputRef.current.focus();
    }
  }));

  return <input ref={inputRef} />;
});
```

在父组件中，你可以这样使用：

```jsx
function Form() {
  const inputRef = useRef(null);

  function handleClick() {
    inputRef.current.focus();   
  }

  return (
    <>
      <MyInput ref={inputRef} />
      <button onClick={handleClick}>Focus Input</button>
    </>
  );    
}
```

## React什么时机去挂载ref

通常情况下，我们不建议在渲染过程中访问 refs，包括那些持有 DOM 节点的 refs。这是因为：

1. 首次渲染时，DOM 节点还未创建，ref.current 为 null。
2. 更新渲染时，DOM 节点尚未更新，此时读取它们为时过早。

React 在提交阶段设置 ref.current：

1. 更新 DOM 前，React 将相关的 ref.current 值设为 null。
2. 更新 DOM 后，React 立即将它们设置为对应的 DOM 节点。

最佳实践是在事件处理程序中访问 refs。如果你需要在没有特定事件触发的情况下操作 ref，可以考虑使用 Effect。

:::info

如果想要及时获取到DOM的更新，可以使用flushSync。比如下述代码：

```jsx
setTodos([ ...todos, newTodo]);
listRef.current.lastChild.scrollIntoView();
```

由于React的异步更新，上述代码的DOM更新不会立即生效，所以lastChild并不是我们想要找的DOM节点。

此时，我们可以使用flushSync来强制React同步更新DOM：

```jsx
flushSync(() => {
  setTodos([ ...todos, newTodo]);
});
listRef.current.lastChild.scrollIntoView();
```

这样，我们就可以确保DOM更新后，再执行scrollIntoView。

:::

## 回顾

1. 使用 `useRef` 创建一个 ref 对象，并将其传递给 DOM 元素。
2. 使用 `useImperativeHandle` 自定义暴露给父组件的 ref 对象。
3. 在事件处理程序中访问 refs。
4. 使用 `flushSync` 强制 React 同步更新 DOM。

通过这些方法，你可以灵活地访问和操作 DOM 节点，从而实现更复杂的交互和动画效果。

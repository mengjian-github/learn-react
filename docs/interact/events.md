---
title: react 事件
sidebar_position: 1
---

React 事件是用户与应用程序交互时触发的操作。例如，单击按钮、输入文本或提交表单。React 事件处理程序是响应这些交互的函数。

## 添加事件

React 事件处理程序是响应用户交互的函数。它们通常作为属性添加到组件中，并使用驼峰命名法命名，例如 `onClick` 或 `onChange`。

```jsx
const Button = () => {
    const handleClick = () => {
        alert('Button clicked!');
    };

    return <button onClick={handleClick}>Click me</button>;
};
```

:::warning

传递给事件处理程序的应该是函数本身,而不是函数的执行结果。例如,以下写法是错误的:

```jsx
<button onClick={handleClick()}>Click me</button>
```

这将立即调用函数，而不是将其传递给事件处理程序。并且每次渲染时都会调用该函数。

:::

## 事件冒泡

事件冒泡是指当一个元素触发事件时，该事件会从元素开始，逐级向上传播，直到到达最顶层元素。

举个例子：

```jsx
<div onClick={handleDivClick}>
    <button onClick={handleButtonClick}>Click me</button>
</div>
``` 

如果点击按钮，会先执行 `handleButtonClick` 函数，然后执行 `handleDivClick` 函数。

:::info
所有的事件默认都会冒泡，除了onScroll。
:::

## 阻止事件冒泡

阻止事件冒泡是指当一个元素触发事件时，该事件不会继续向上传播。

```jsx
const handleButtonClick = (event) => {
    event.stopPropagation();
};
```

## 事件捕获

事件捕获是指当一个元素触发事件时，该事件会从最顶层元素开始，逐级向下传播，直到到达目标元素。

一般来说，开发中很少使用事件捕获，必要时，可以通过React的捕获事件来实现：

```jsx
<div onClickCapture={() => { /* 事件捕获 */ }}>
  <button onClick={e => e.stopPropagation()} />
  <button onClick={e => e.stopPropagation()} />
</div>
```

事件捕获会先执行，然后执行事件冒泡。

## 阻止默认行为

阻止默认行为是指当一个元素触发事件时，该事件不会执行默认行为。

```jsx
export default function Signup() {
  return (
    <form onSubmit={e => {
      e.preventDefault();
      alert('Submitting!');
    }}>
      <input />
      <button>Send</button>
    </form>
  );
}
```

- `e.stopPropagation()` 阻止事件冒泡,防止事件传播到父元素。
- `e.preventDefault()` 阻止元素的默认行为,如表单提交或链接跳转。

## 回顾

- 事件冒泡：从内向外传播，直到最外层元素。
- 事件捕获：从外向内传播，直到目标元素。
- 阻止默认行为：阻止元素的默认行为，如表单提交或链接跳转。
- 阻止事件冒泡：阻止事件传播到父元素。

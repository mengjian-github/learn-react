---
title: 条件渲染
sidebar_position: 6
---

# 条件渲染

条件渲染是React中的一种技术，用于根据某些条件决定是否渲染组件的某些部分。这在处理用户交互、数据状态和组件的可见性时非常有用。

条件渲染可以通过多种方式实现，包括：

1. **if语句**：使用JavaScript的if语句来决定是否渲染组件。
2. **三元运算符**：使用三元运算符来决定是否渲染组件。
3. **逻辑与运算符**：使用逻辑与运算符来决定是否渲染组件。

## 使用if语句

```jsx
function MyComponent(props) {
    if (props.isLoggedIn) {
        return <h1>Welcome back!</h1>;
    } 
    return <h1>Please sign up.</h1>;
}
```

当`isLoggedIn`为`true`时，组件将渲染`<h1>Welcome back!</h1>`，否则将渲染`<h1>Please sign up.</h1>`。本质上这是两个不同的JSX，React会根据条件渲染其中一个。

## 使用三元运算符

假设一个组件内部只有一点逻辑有区别，例如：

```jsx
if (isPacked) {
  return <li className="item">{name} ✅</li>;
}
return <li className="item">{name}</li>;
```

遵循 [DRY原则](https://en.wikipedia.org/wiki/Don%27t_repeat_yourself)，我们可以简化为三元运算符：

```jsx
return <li className="item">{name} {isPacked ? '✅' : ''}</li>;
```

:::warning

如果你的组件因为过多的嵌套三元运算符而变得混乱，考虑提取子组件来整理代码。在React中，你可以使用变量和函数等工具来整理复杂的表达式。

:::

## 使用逻辑与运算符

在React组件中，当你希望在条件为真时渲染一些JSX，或者在条件为假时渲染空值时，逻辑与运算符（&&）非常有用。例如：

```jsx
return <li className="item">{name} {isPacked && '✅'}</li>;
```

当`isPacked`为`true`时，组件将渲染`<li className="item">{name} ✅</li>`，否则将渲染`<li className="item">{name}</li>`。

:::warning

不要在&&的左边放数字。

JavaScript会自动将左边转换为布尔值。然而，如果左边是0，那么整个表达式会得到那个值（0），React会渲染0，而不是什么都没有。

例如，一个常见的错误是写这样的代码：`messageCount && <p>New messages</p>`。很容易认为当`messageCount`为0时，它会渲染什么都没有，但实际上它渲染的是0本身！

要修复它，使左边成为布尔值：`messageCount > 0 && <p>New messages</p>`。

:::

## 回顾

- 条件渲染是React中的一种技术，用于根据某些条件决定是否渲染组件的某些部分。
- 可以使用if语句、三元运算符和逻辑与运算符来实现条件渲染。
- 不要在&&的左边放数字，否则会导致渲染问题。
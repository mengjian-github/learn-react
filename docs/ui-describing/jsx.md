---
title: 了解JSX
sidebar_position: 3
---

# 了解JSX

JSX 是 JavaScript 的扩展，它允许你在 JavaScript 中使用类似于 HTML 的语法。JSX 是 React 的核心组成部分，它使得开发者能够以更直观的方式描述 UI 结构。

## JSX的背景

Web 是建立在 HTML、CSS 和 JavaScript 之上的。多年来，Web 开发者将内容放在 HTML 中，设计放在 CSS 中，逻辑放在 JavaScript 中——通常是在不同的文件里！内容被标记在 HTML 中，而页面的逻辑则单独存在于 JavaScript 中。

但随着Web变得越来越复杂，逻辑越来越多地决定内容。JavaScript开始掌控HTML！

这就是为什么在React中，组件既包含JavaScript又包含HTML。

每个 React 组件都是一个 JavaScript 函数，React 组件使用一种称为 JSX 的语法扩展来表示这些标记。JSX 看起来很像 HTML，但它更加严格，并且可以显示动态信息。理解这一点的最好方法是将一些 HTML 转换为 JSX。

## 将 HTML 转换为 JSX


```html
<div class="container">
  <h1>Hello, World!</h1>
</div>
```

```jsx
const container = <div className="container">
  <h1>Hello, World!</h1>
</div>;
```

## JSX 的语法规则

### 1. 返回一个单一的根元素

JSX 中的每个元素都必须有一个根元素。这意味着你不能返回多个根元素。

```jsx
const container = <div className="container">
  <h1>Hello, World!</h1>
</div>;
```

如果需要返回多个根元素，可以使用一个数组，在JSX中，用`<></>`来表示一个根元素数组。

```jsx
const container = <>
  <h1>Hello, World!</h1>
  <p>This is a paragraph.</p>
</>;
```

:::info

JSX 在底层会表示成一个JS对象，在JavaScript中，你不可能在同一个函数中返回多个对象，所以JSX中也不允许返回多个根元素，只能返回一个数组，在JSX中，用`<></>`来表示一个数组。

:::

### 2. 闭合标签

在 HTML 中，一些元素需要闭合标签，例如 `<div>` 和 `<span>`。在 JSX 中，这些元素也必须闭合。

```jsx
const container = <div className="container">
  <h1>Hello, World!</h1>
</div>;
```

### 3. 使用驼峰命名法

在 JSX 中，标签名和属性名使用驼峰命名法。在 HTML 中，标签名和属性名是全小写的，而在 JSX 中，全部会转变成JavaScript的属性名，所以，在 JSX 中，标签名和属性名使用驼峰命名法。

在JavaScript中，class是一个保留关键字，所以，在 JSX 中，class 会变成 className。

```jsx
const container = <div className="container">
  <h1>Hello, World!</h1>
</div>;
```

:::warning

唯有一个例外，aria-和data-开头的属性名在JSX中保持不变。这是由于历史原因，这两个属性名在HTML中有特殊含义，在JSX中仍然保持不变。

:::

## 回顾

JSX 是 JavaScript 的扩展，它允许你在 JavaScript 中使用类似于 HTML 的语法。JSX 是 React 的核心组成部分，它使得开发者能够以更直观的方式描述 UI 结构。

JSX 的语法规则：

1. 返回一个单一的根元素
2. 闭合标签
3. 使用驼峰命名法

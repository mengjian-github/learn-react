---
title: 使用Ref Value
sidebar_position: 1
---

# 使用Ref Value

当你希望组件“记住”某些信息，但又不想让这些信息触发新的渲染时，可以使用ref。

ref 对象是一个通用容器，其 current 属性是可变的，并且可以存储任何值，例如一个 DOM 元素。

例如：

```jsx
const ref = useRef(0);
```

ref的值的数据结构如下：

```js
{
  current: 0
}
```

## 不需要触发重新渲染

当ref的值发生变化时，不会触发组件的重新渲染。这是因为ref的值不会作为props传递给子组件，也不会被React用于渲染逻辑。

我们简单对比一下state和ref：

| 特性 | refs | state |
|------|------|-------|
| 创建方式 | `useRef(initialValue)` 返回 `{ current: initialValue }` | `useState(initialValue)` 返回当前状态值和一个设置函数 `[value, setValue]` |
| 更新触发重渲染 | 更改时不会触发重新渲染 | 更改时会触发重新渲染 |
| 可变性 | 可变 - 可以在渲染过程之外修改和更新 `current` 的值 | 不可变 - 必须使用状态设置函数来修改状态变量，以触发重新渲染 |
| 读写时机 | 不应在渲染期间读取（或写入）当前值 | 可以随时读取状态。但每次渲染都有自己的状态快照，不会改变 |

:::info

实际上，ref的底层实现是使用useState来实现的：

```js
// Inside of React
function useRef(initialValue) {
  const [ref, unused] = useState({ current: initialValue });
  return ref;
}
```

我们可以理解为，ref是state的一种特殊形式，它不触发重新渲染，并且可以在渲染过程之外修改和更新。

:::

## 常用场景

- 存储 timeout IDs
- 存储和操作 DOM 元素
- 存储其他不是必需用于计算 JSX 的对象

如果组件需要存储一些值，但这些值不影响渲染逻辑，请选择 refs。

## ref的原则

- refs在处理外部系统或浏览器API时非常有用。但如果你的应用程序逻辑和数据流过度依赖refs，可能需要重新考虑你的方法。
- 避免在渲染过程中读取或写入ref.current。如果渲染时需要某些信息，请使用state代替。因为React无法知道ref.current何时发生变化，即使在渲染过程中仅仅读取它，也会使组件的行为变得难以预测。（唯一的例外是像if (!ref.current) ref.current = new Thing()这样的代码，它只在首次渲染时设置ref一次。）

## 回顾

- Refs 是一种在组件中存储可变值的方式，不会触发重新渲染。
- 使用 `useRef` 钩子创建 ref，它返回一个包含 `current` 属性的对象。
- Refs 常用于存储 DOM 元素、timeout IDs 和其他不影响渲染输出的值。
- 避免在渲染过程中读取或写入 ref.current，因为这可能导致组件行为不可预测。
- 当需要存储一些值但不影响渲染逻辑时，选择使用 refs 而不是 state。
- Refs 在处理外部系统或浏览器 API 时特别有用，但不应过度依赖它们来管理应用程序的核心逻辑。

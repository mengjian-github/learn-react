---
title: 选择状态的结构
sidebar_position: 2
---

# 选择状态的结构

状态的结构是 React 组件设计的核心。良好的状态结构可以使组件易于修改和调试，而糟糕的状态结构可能导致难以发现的错误。

## 一些原则

1. 组合相关状态：如果你总是同时更新两个或更多的状态变量，考虑将它们合并为一个单一的状态变量。

2. 避免状态矛盾：当状态的结构使得几个状态片段可能相互矛盾或"不一致"时，你就为错误留下了空间。尽量避免这种情况。

3. 避免冗余状态：如果你可以在渲染过程中从组件的props或现有状态变量计算出某些信息，就不应将该信息放入组件的状态中。

4. 避免状态重复：当相同的数据在多个状态变量之间或嵌套对象内部重复时，很难保持它们同步。尽可能减少重复。

5. 避免深层嵌套状态：深层次的层级状态更新起来不太方便。尽可能采用扁平化的状态结构。

## 组合相关的状态

看一个例子：

```jsx
const [x, setX] = useState(0);
const [y, setY] = useState(0);
```

大部分情况下，x和y是相关的，一起发生变更，可以合并为一个状态：

```jsx
const [position, setPosition] = useState({ x: 0, y: 0 });
```

## 避免状态矛盾

举个例子：

```jsx
const [isSending, setIsSending] = useState(false);
const [isSent, setIsSent] = useState(false);
```

isSending和isSent是矛盾的，因为isSending和isSent不可能同时为真。

更好的方式是使用一个状态来表示发送状态：

```jsx
const [sendStatus, setSendStatus] = useState('sending');
```

这样避免了同时变更两个状态变量导致的bug风险。

## 避免冗余状态

一个简单的例子：

```jsx
const [firstName, setFirstName] = useState('');
const [lastName, setLastName] = useState('');
const [fullName, setFullName] = useState('');
```

fullName可以通过firstName和lastName计算得到，所以这里不需要单独设置fullName状态。

:::warning

不要在状态中复制props。比如：
```jsx
function Message({ messageColor }) {
  const [color, setColor] = useState(messageColor);
  // ...
}
```

这里，color状态变量被初始化为messageColor prop。问题在于，如果父组件稍后传递不同的messageColor值（例如，'red'而不是'blue'），color状态变量将不会更新！状态仅在第一次渲染时初始化。

只有一种情况是适用的，那就是props仅作为初始值：

```jsx
function Message({ initialColor }) {
  // `color` 状态变量仅保存 `initialColor` 的初始值。
  // 后续对 `initialColor` prop 的更改将被忽略。
  const [color, setColor] = useState(initialColor);
}
```

:::

## 避免状态重复

举个例子：

```jsx
const [items, setItems] = useState(initialItems);
const [selectedItem, setSelectedItem] = useState(
items[0]
);
```

items和selectedItem是重复的，items包含了selectedItem的所有信息，我们更新items时，selectedItem不会更新，反之亦然。

在这个例子中，我们只需要记录selectedId，就能避免掉这种重复：

```jsx
const [items, setItems] = useState(initialItems);
const [selectedId, setSelectedId] = useState(0);

const selectedItem = items.find(item =>
item.id === selectedId
);
```

## 避免深层嵌套状态

深层次的层级状态更新起来不太方便。尽可能采用扁平化的状态结构。

你可以任意嵌套状态，但使状态扁平化可以解决许多问题。它使状态更容易更新，并有助于确保你不会在嵌套对象的不同部分重复数据。


## 回顾

1. 如果两个状态变量总是一起更新，考虑将它们合并为一个。
2. 避免冗余和重复的状态，以避免保持其同步。
3. 除非你特意想要阻止更新，否则不要将props放入状态。
4. 对于UI模式，如选择，将ID或索引保持在状态中，而不是对象本身。
5. 如果更新嵌套状态很复杂，尝试扁平化它。
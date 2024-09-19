---
title: 状态快照
sidebar_position: 4
---

# 状态快照

状态变量可能看起来像普通的 JavaScript 变量,可以直接读取和写入。然而,状态的行为更像是一个快照。设置状态并不会改变你已有的状态变量,而是触发一次重新渲染。这种机制确保了状态的一致性和可预测性,同时也是 React 高效更新 UI 的关键。

## 渲染过程中生成快照

渲染的过程，实际上是执行组件函数的过程，而组件的本质是返回一个JSX，JSX的本质就是一个对象，这个对象可以理解为反应UI的快照。

组件的Props、EventHandler、variable等都是通过状态变量来计算生成的。

当re-render进行时，实际上做了三件事：

1. 调用并执行组件函数
2. 函数返回了一个新的JSX快照
3. 将新的JSX快照与旧的JSX快照进行对比，找出差异并更新到DOM

作为组件的"记忆"，状态并不像普通变量那样在函数返回后就消失了。状态实际上"存在于"函数之外。

当React调用你的组件时,它会为该特定渲染提供一个状态快照。你的组件返回一个UI快照，其中包含一组新的props和事件处理程序，这些都是使用该渲染的状态值计算得出的。

如何理解快照呢，让我们看一个例子：

```jsx
import { useState } from 'react';

export default function Counter() {
  const [number, setNumber] = useState(0);

  return (
    <>
      <h1>{number}</h1>
      <button onClick={() => {
        setNumber(number + 1);
        setNumber(number + 1);
        setNumber(number + 1);
      }}>+3</button>
    </>
  )
}
```

在这个例子中，一次点击按钮调用了三次`setNumber`，但是实际上只加了一次。

实际上，setState 并不会改变已有的状态变量，而是触发一次重新渲染。也就是在下个渲染周期中，这个状态变量才会取到最新的值。

所以当前拿到的state，实际上可以理解为是上一次渲染的状态快照。

## 状态随时间的变化

让我们来看一个极端的例子：

```jsx
import { useState } from 'react';

export default function Counter() {
  const [number, setNumber] = useState(0);

  return (
    <>
      <h1>{number}</h1>
      <button onClick={() => {
        setNumber(number + 5);
        setTimeout(() => {
          alert(number);
        }, 3000);
      }}>+5</button>
    </>
  )
}
```

在这个例子中，尽管我们延时了3秒，但是弹出来的值仍然是0。

这个例子更加说明了，一次渲染过程中，状态变量是快照，而不是实时值，它不会发生改变，所以千万不要试图在渲染过程中改变它，当我们更新状态时，是计算新的快照并触发下一次渲染。

## 回顾

- 状态是组件的"记忆",用于存储随时间变化的数据
- 使用 useState Hook 来在函数组件中添加状态
- 状态更新是异步的,不会立即生效
- 在一次渲染周期内,状态值是固定的快照
- 多次调用 setState 不会立即更新状态,而是触发重新渲染
- 不要试图在渲染过程中直接修改状态变量
- 更新状态会计算新的状态快照并触发下一次渲染
- 使用函数式更新可以基于之前的状态进行更新
- 理解状态快照的概念有助于避免一些常见的错误

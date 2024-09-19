---
title: 状态更新队列
sidebar_position: 5
---

# 状态更新队列

设置状态变量会触发另一次渲染。但有时你可能希望在触发下一次渲染之前对值执行多个操作。为了实现这一点，了解React如何批量处理状态更新是很有帮助的。

React的状态更新机制是经过精心设计的，它通过批量处理和优化来提高性能。当你连续多次调用状态更新函数时，React会将这些更新合并到一个批次中，以减少不必要的渲染次数。这种机制既提高了应用的性能，又保证了状态更新的一致性。

## 批量更新

还是以上个例子来说明：

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

我们可以简单理解为，在click事件里面相当于执行了下下面的方法：

```js
setNumber(0 + 1);
setNumber(0 + 1);
setNumber(0 + 1);
```

在这里, React会合并三次更新，直到事件函数执行完毕，然后进行一次渲染。

## 拿到最新的状态

如果想拿到最新的状态并更新，可以这样做：

```jsx
export default function Counter() {
  const [number, setNumber] = useState(0);

  return (
    <>
      <h1>{number}</h1>
      <button onClick={() => {
        setNumber(n => n + 1);
        setNumber(n => n + 1);
        setNumber(n => n + 1);
      }}>+3</button>
    </>
  )
}
```

这里setNumber的参数是一个函数，这个函数接收最新的状态值，并返回一个新的状态值。这样，React就可以在每次状态更新时拿到最新的状态值。

在更新的时候，react会将function加入到队列中，在事件函数执行完毕后，下一次render周期内，将队列中取出依次执行。

## 更复杂的例子

```jsx
import { useState } from 'react';

export default function Counter() {
  const [number, setNumber] = useState(0);

  return (
    <>
      <h1>{number}</h1>
      <button onClick={() => {
        setNumber(number + 5);
        setNumber(n => n + 1);
        setNumber(42);
      }}>Increase the number</button>
    </>
  )
}
```

让我们来看一下这个例子，当点击按钮时，会发生什么？

1. 首先，`setNumber(number + 5)` 被调用，队列压入：将 `number` 设置为 `5`。
2. 然后，`setNumber(n => n + 1)` 被调用，队列压入：n => n + 1。
3. 最后，`setNumber(42)` 被调用，队列压入：将 `number` 设置为 `42`。

在下一个render周期内，React会依次执行队列中的方法：

1. 将 `number` 设置为 `5`：returns `5`
2. `n => n + 1`执行：returns `5` + `1` = `6`
3. 将 `number` 设置为 `42`：returns `42`

所以，最终的 `number` 值是 `42`。

## 回顾

1. 状态更新函数会先进入队列，在下一个render周期内，依次执行队列中的方法。
2. 如果状态更新函数中，有依赖最新的状态值，那么需要将状态更新函数设置为箭头函数。




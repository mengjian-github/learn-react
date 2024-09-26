---
title: 组件状态的保持
sidebar_position: 4
---

# 组件状态的保持

组件状态是独立且隔离的。React 通过组件在 UI 树中的位置来追踪每个组件的状态。你可以精确控制何时保留状态,以及何时在重新渲染之间重置状态。

## 状态与位置的关联

当你给一个组件状态时，你可能认为状态“存在于”组件内部。但实际上，状态实际上是存储在 React 中。React 通过组件在渲染树中的位置将每个状态片段与正确的组件关联起来。

举个例子：

```jsx
import { useState } from 'react';

export default function App() {
  return (
    <div>
      <Counter />
      <Counter />
    </div>
  );
}

function Counter() {
  const [score, setScore] = useState(0);
  const [hover, setHover] = useState(false);

  let className = 'counter';
  if (hover) {
    className += ' hover';
  }

  return (
    <div
      className={className}
      onPointerEnter={() => setHover(true)}
      onPointerLeave={() => setHover(false)}
    >
      <h1>{score}</h1>
      <button onClick={() => setScore(score + 1)}>
        Add one
      </button>
    </div>
  );
}

```

在这个例子中，两个 `Counter` 组件各自拥有自己的 `score` 和 `hover` 状态。React 通过它们在渲染树中的位置将每个状态片段与正确的组件关联起来。

不过我们稍微改动下：

```jsx
export default function App() {
  const [showB, setShowB] = useState(true);
  return (
    <div>
      <Counter />
      {showB && <Counter />} 
      <label>
        <input
          type="checkbox"
          checked={showB}
          onChange={e => {
            setShowB(e.target.checked)
          }}
        />
        Render the second counter
      </label>
    </div>
  );
}
```

此时如果showB为false，第二个Counter组件不会渲染，它的state也不会被保留。重新挂载时，状态会重置。

总体来说，React 会保留组件的状态，只要它在 UI 树中的位置被渲染。如果组件被移除，或者在同一位置渲染了不同的组件，React 则会丢弃其状态。

## 相同组件在相同位置保留状态

相同组件在相同位置渲染时，状态会被保留。

举个复杂的例子：

```jsx
export default function App() {
  const [isFancy, setIsFancy] = useState(false);
  if (isFancy) {
    return (
      <div>
        <Counter isFancy={true} />
        <label>
          <input
            type="checkbox"
            checked={isFancy}
            onChange={e => {
              setIsFancy(e.target.checked)
            }}
          />
          Use fancy styling
        </label>
      </div>
    );
  }
  return (
    <div>
      <Counter isFancy={false} />
      <label>
        <input
          type="checkbox"
          checked={isFancy}
          onChange={e => {
            setIsFancy(e.target.checked)
          }}
        />
        Use fancy styling
      </label>
    </div>
  );
}

```

在这个例子中，即使 `isFancy` 状态在两次渲染之间被切换，第二个 `Counter` 组件的状态仍然会被保留。这是因为 React 通过它们在渲染树中的位置将每个状态片段与正确的组件关联起来。

## 不同组件在相同位置不保留状态

不同组件在相同位置渲染时，状态不会被保留。

举个例子：

```jsx
export default function App() {
  const [isPaused, setIsPaused] = useState(false);
  return (
    <div>
      {isPaused ? (
        <p>See you later!</p> 
      ) : (
        <Counter /> 
      )}
      <label>
        <input
          type="checkbox"
          checked={isPaused}
          onChange={e => {
            setIsPaused(e.target.checked)
          }}
        />
        Take a break
      </label>
    </div>
  );
}
```

在这个例子中，`Counter` 组件的状态不会被保留，因为p和Counter是两个不同的组件。

:::info

在前面，我们说组件不要定义在组件内部，比如：

```jsx
export default function MyComponent() {
  const [counter, setCounter] = useState(0);

  function MyTextField() {
    const [text, setText] = useState('');

    return (
      <input
        value={text}
        onChange={e => setText(e.target.value)}
      />
    );
  }

  return (
    <>
      <MyTextField />
      <button onClick={() => {
        setCounter(counter + 1)
      }}>Clicked {counter} times</button>
    </>
  );
}
```

为什么呢？上面的例子很明显，每次Button点击，MyTextField都会重新渲染，且状态会丢失。
从diff的角度，每次的function都是新的，所以虽然是相同位置，但状态不会保留。

:::

## 强制重置状态

大多数情况下，React默认策略是最优的，但在某些场景中，我们不需要状态的保留：

```jsx
export default function Scoreboard() {
  const [isPlayerA, setIsPlayerA] = useState(true);
  return (
    <div>
      {isPlayerA ? (
        <Counter person="Taylor" />
      ) : (
        <Counter person="Sarah" />
      )}
      <button onClick={() => {
        setIsPlayerA(!isPlayerA);
      }}>
        Next player!
      </button>
    </div>
  );
}
```

假设这个例子中，我们希望每次点击按钮时，状态重置，我们可以使用 `key` 属性来强制重置状态：

```jsx
export default function Scoreboard() {
  const [isPlayerA, setIsPlayerA] = useState(true);
  return (
    <div>
      {isPlayerA ? (
        <Counter key="Taylor" person="Taylor" />
      ) : (
        <Counter key="Sarah" person="Sarah" />
      )}
      <button onClick={() => {
        setIsPlayerA(!isPlayerA);
      }}>
        Next player!
      </button> 
    </div>
  );
}
```

## 回顾

- 组件状态是独立且隔离的。React 通过组件在 UI 树中的位置来追踪每个组件的状态。
- 相同组件在相同位置渲染时，状态会被保留。
- 不同组件在相同位置渲染时，状态不会被保留。
- 使用 `key` 属性可以强制重置状态。

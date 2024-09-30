---
title: 也许不需要Effect
sidebar_position: 4
---

# 也许不需要Effect

Effect 是 React 范式中的一个"逃生舱"。它们允许你"跳出" React，将组件与外部系统（如非 React 小部件、网络或浏览器 DOM）同步。如果不涉及外部系统（例如，你只是想在某些 props 或 state 变化时更新组件的状态），那么你可能不需要使用 Effect。移除不必要的 Effect 可以使你的代码更易于理解、运行更快速，并且更不容易出错。

以下是一些常见的不需要使用 Effect 的情况：

1. 根据 props 或 state 计算数据
2. 在用户事件后更新组件状态
3. 在渲染后立即更新 DOM

## 怎样移除不必要的Effect

- 在渲染过程中转换数据时，不需要使用Effect。例如，如果你想在显示列表前进行过滤，直接在组件顶层进行数据转换即可。这样可以避免不必要的渲染循环,提高效率。每当props或state变化时,这部分代码会自动重新执行。
- 处理用户事件也不需要使用Effect。例如,当用户购买产品时发送API请求并显示通知,应该在点击事件处理程序中完成。事件处理程序可以准确知道用户的操作,而Effect无法获知具体的触发原因。因此,用户交互相关的逻辑通常应该放在对应的事件处理程序中。

## 基于props和state的计算数据

假设你有一个包含两个状态变量 firstName 和 lastName 的组件。你想通过将它们连接起来计算 fullName：

```jsx
const [firstName, setFirstName] = useState('Taylor');
const [lastName, setLastName] = useState('Swift');
const [fullName, setFullName] = useState('');

useEffect(() => {
  setFullName(firstName + ' ' + lastName);
}, [firstName, lastName]);
```

这个useEffect是没有必要的，你可以直接在渲染过程中计算 fullName，而不需要使用 Effect：

```jsx
const fullName = firstName + ' ' + lastName;
``` 

这样避免了不必要的渲染循环，提高了效率。每当 props 或 state 变化时，这部分代码会自动重新执行。

当某些东西可以从现有的 props 或 state 中计算出来时，不要把它放在 state 中。相反，在渲染过程中计算它。

## 当prop改变时重置state

比如：

```jsx
export default function ProfilePage({ userId }) {
  const [comment, setComment] = useState('');

  // 🔴 Avoid: Resetting state on prop change in an Effect
  useEffect(() => {
    setComment('');
  }, [userId]);
  // ...
}
```

这种方法效率较低,因为ProfilePage及其子组件会先用旧值渲染一次,然后再重新渲染。

同时,这种方法也比较复杂,因为你需要在ProfilePage内部的每个有状态的组件中都这样处理。例如,如果评论UI是嵌套的,你还需要清除嵌套的评论状态。

相反,你可以通过给每个用户的 profile 一个显式的 key 来告诉 React 这些是概念上不同的 profile。将你的组件拆分成两个,并从外层组件向内层组件传递一个 key 属性:

```jsx
export default function ProfilePage({ userId }) {
  return (
    <Profile
      userId={userId}
      key={userId}
    />
  );
}

function Profile({ userId }) {
  // ✅ This and any other state below will reset on key change automatically
  const [comment, setComment] = useState('');
  // ...
}
```

## 当props改变时调整state

比如：

```jsx
function List({ items }) {
  const [isReverse, setIsReverse] = useState(false);
  const [selection, setSelection] = useState(null);

  useEffect(() => {
    setSelection(null);
  }, [items]);
  // ...
}
```

这种方法同样存在问题。每当items发生变化时,List及其子组件首先会使用过时的selection值进行渲染。然后React会更新DOM并运行Effects。最后,setSelection(null)的调用会导致List及其子组件再次重新渲染,重新启动整个过程。

我们直接去除Effect，在渲染过程中进行处理：

```jsx
function List({ items }) {
  const [isReverse, setIsReverse] = useState(false);
  const [selection, setSelection] = useState(null);

  const [prevItems, setPrevItems] = useState(items);
  if (items !== prevItems) {
    setPrevItems(items);
    setSelection(null);
  }
  // ...
}
```

这种存储前一次渲染信息的方法可能不太容易理解，但它比在 Effect 中更新相同的状态要好。

在上面的例子中，setSelection 是在渲染过程中直接调用的。React 会在 List 组件返回后立即重新渲染它。

此时 React 还没有渲染 List 的子组件或更新 DOM，这样可以让 List 的子组件跳过渲染过时的 selection 值。

当你在渲染过程中更新组件状态时，React 会丢弃已返回的 JSX 并立即重试渲染。

为了避免缓慢的级联重试，React 只允许你在渲染过程中更新同一个组件的状态。如果你尝试在渲染过程中更新其他组件的状态，会看到一个错误。

像 items !== prevItems 这样的条件判断是必要的，以避免无限循环。

你可以这样调整状态，但其他副作用（如修改 DOM 或设置定时器）应该保留在事件处理程序或 Effects 中，以保持组件的纯粹性。

当然，更好的做法应该是只保留selectionId在state中，而不是整个selection对象：

```jsx
function List({ items }) {
  const [isReverse, setIsReverse] = useState(false);
  const [selectedId, setSelectedId] = useState(null);
  const selection = items.find(item => item.id === selectedId) ?? null;
  // ...
}
```

## 共享事件逻辑

举个例子：

```jsx
function ProductPage({ product, addToCart }) {
  useEffect(() => {
    if (product.isInCart) {
      showNotification(`Added ${product.name} to the shopping cart!`);
    }
  }, [product]);

  function handleBuyClick() {
    addToCart(product);
  }

  function handleCheckoutClick() {
    addToCart(product);
    navigateTo('/checkout');
  }
  // ...
}
```

这个例子中，useEffect 被用来在 product 变化时显示通知。然而，这种做法存在一些问题：

1. 不必要的 Effect：通知应该是用户操作的直接结果，而不是组件渲染的副作用。

2. 潜在的错误：如果应用在页面重新加载时保留购物车状态，每次刷新页面都会显示通知，因为 product.isInCart 已经为真。

3. 逻辑重复：两个事件处理程序都调用了 addToCart，导致重复的逻辑。

更好的做法是：

1. 移除 Effect：通知不应该因为组件渲染而触发。

2. 创建共享函数：将添加到购物车和显示通知的逻辑封装到一个函数中。

3. 在事件处理程序中调用：直接响应用户操作，而不是依赖于状态变化。

这样可以确保通知只在用户主动添加商品时显示，避免了不必要的副作用和潜在的错误。让我们重构代码来实现这个优化：

```jsx
function ProductPage({ product, addToCart }) {
  function buyProduct() {
    addToCart(product);
    showNotification(`Added ${product.name} to the shopping cart!`);
  }

  function handleBuyClick() {
    buyProduct();
  }

  function handleCheckoutClick() {
    buyProduct();
    navigateTo('/checkout');
  }
  // ...
}
```

## Effect计算链

举个例子：

```jsx
 useEffect(() => {
    if (card !== null && card.gold) {
      setGoldCardCount(c => c + 1);
    }
  }, [card]);

  useEffect(() => {
    if (goldCardCount > 3) {
      setRound(r => r + 1)
      setGoldCardCount(0);
    }
  }, [goldCardCount]);

  useEffect(() => {
    if (round > 5) {
      setIsGameOver(true);
    }
  }, [round]);

  useEffect(() => {
    alert('Good game!');
  }, [isGameOver]);
```

这段代码存在两个主要问题：

首先，它的效率非常低：在每次 set 调用之间，组件（及其子组件）都需要重新渲染。在上面的例子中，最坏的情况下（setCard → 渲染 → setGoldCardCount → 渲染 → setRound → 渲染 → setIsGameOver → 渲染），会导致组件树下方出现三次不必要的重新渲染。

其次，即使不考虑性能问题，随着代码的演进，你可能会遇到这种"链式"写法无法满足新需求的情况。想象一下，如果你要添加一个功能来回顾游戏历史步骤。你可能会通过将每个状态变量更新为过去的值来实现。然而，将 card 状态设置为过去的值会再次触发 Effect 链，从而改变你正在展示的数据。这种代码通常是僵硬且脆弱的。

更好的做法是，在事件处理程序中直接进行状态更新：

```jsx
function handlePlaceCard(nextCard) {
    if (isGameOver) {
      throw Error('Game already ended.');
    }

    // ✅ Calculate all the next state in the event handler
    setCard(nextCard);
    if (nextCard.gold) {
      if (goldCardCount <= 3) {
        setGoldCardCount(goldCardCount + 1);
      } else {
        setGoldCardCount(0);
        setRound(round + 1);
        if (round === 5) {
          alert('Good game!');
        }
      }
    }
  }
```

## 回顾

在本节中，我们学习了：

1. 避免使用 Effect 来响应用户事件或 props 的变化。这通常会导致不必要的渲染和复杂的代码结构。

2. 尽量在事件处理程序中直接更新状态。这样可以提高性能，并使代码更易于理解和维护。

3. 当需要更新多个相关的状态变量时，最好在一个事件处理程序中一次性完成所有更新，而不是依赖 Effect 的链式反应。

4. 通过在事件处理程序中计算和设置所有相关状态，可以避免不必要的渲染和潜在的状态不一致问题。

通过遵循这些原则，我们可以编写更高效、更可靠、更易于维护的 React 应用程序。




---
title: 状态共享
sidebar_position: 3
---

# 状态共享

有时，你希望两个组件的状态总是一起改变。为此，从两者中删除状态，将其移动到它们最近的共同父级，然后通过props传递它。这被称为提升状态，这是编写React代码时最常见的事情之一。

## 提升状态

让我们通过一个例子来说明如何提升状态：

假设我们有两个组件，`Accordion` 和 `Panel`。每个 `Panel` 组件都有自己的 `isOpen` 状态，用于控制面板是否展开。但是，我们希望在任何时候只有一个面板是打开的。

首先，让我们看看未提升状态的版本：

```jsx
function Accordion() {
    return (
        <div>
            <Panel />
            <Panel />
        </div>
    );
}

function Panel() {  
    const [isOpen, setIsOpen] = useState(false);

    return (
        <div>
            <h3>Panel</h3>
            <button onClick={() => setIsOpen(!isOpen)}>Toggle</button>
            {isOpen && <p>Panel content</p>}
        </div>
    );
}   
```

在这个版本中，每个 `Panel` 组件都有自己的 `isOpen` 状态。这意味着当我们打开一个面板时，其他面板的状态不会改变。

为了解决这个问题，我们可以将 `isOpen` 状态提升到 `Accordion` 组件中：

```jsx
function Accordion() {
    const [openPanelId, setOpenPanelId] = useState(null);

    return (
        <div>
            <Panel id={0} isOpen={openPanelId === 0} onToggle={setOpenPanelId} />
            <Panel id={1} isOpen={openPanelId === 1} onToggle={setOpenPanelId} />
        </div>
    );
}

function Panel({ id, isOpen, onToggle }) {
    return (
        <div>
            <h3>Panel {id}</h3>
            <button onClick={() => onToggle(isOpen ? null : id)}>Toggle</button>
            {isOpen && <p>Panel {id} content</p>}
        </div>
    );
}
```

在这个版本中，`Accordion` 组件有一个 `openPanelId` 状态，用于控制哪个面板是打开的。每个 `Panel` 组件都有一个 `id` 和一个 `onToggle` 函数，用于更新 `openPanelId` 状态。

## 受控组件和非受控组件

React 中的组件可以分为两种类型：受控组件和非受控组件。

通常，我们将具有本地状态的组件称为"非受控组件"。例如，原始的 Panel 组件带有 isActive 状态变量，就是非受控的，因为其父组件无法影响面板是否处于活动状态。

相反，当组件的重要信息由 props 驱动而不是由其自身的本地状态驱动时，我们称之为"受控组件"。这使得父组件可以完全指定其行为。最终版本的 Panel 组件带有 isActive prop，就是由 Accordion 组件控制的。

非受控组件在父组件中使用起来更简单，因为它们需要的配置较少。但当你想要协调多个组件时，它们的灵活性较低。受控组件具有最大的灵活性，但它们需要父组件通过 props 完全配置它们。

在编写组件时，考虑哪些信息应该是受控的（通过 props），哪些信息应该是非受控的（通过 state）。但你随时可以改变主意并进行重构。

## 每个状态的单一数据源

在React应用中，状态分布在不同层级的组件中。某些状态可能位于组件树的底层，如输入框的状态；而其他状态可能位于应用的顶层，例如客户端路由库通常会在React状态中存储当前路由，并通过props向下传递。

对于每个独特的状态片段，你需要选择一个合适的组件来管理它。这就是"单一数据源"原则。这并不意味着所有状态都集中在一处，而是每个状态片段都有一个特定的组件作为其唯一来源。避免在组件间复制共享状态，而应将其提升到最近的共同父组件，然后通过props传递给需要它的子组件。

随着应用的发展，你可能需要调整状态的位置。在确定每个状态片段的最佳位置时，你可能会上下移动状态。这是开发过程中的正常现象，有助于优化应用的状态管理。

## 回顾

当你需要共享两个组件的状态时，可以采取以下步骤：

1. 将它们的状态提升到最近的共同父组件。
2. 通过 props 从共同父组件向下传递信息。
3. 传递事件处理函数，使子组件能够修改父组件的状态。
4. 考虑将组件分类为"受控"（由 props 驱动）或"非受控"（由内部 state 驱动）。

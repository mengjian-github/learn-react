---
title: 将UI理解为一颗树
sidebar_position: 8
---

## 什么是UI树

UI树是UI的层级结构，它是由多个节点组成的树状结构。每个节点代表一个UI组件，节点之间的关系代表了组件之间的层级关系。

## 为什么UI树是树状结构

其实在浏览器中，DOM就是一棵树，CSSOM也是一棵树。React建立了组件树，最终渲染到浏览器中，就是将组件树渲染为DOM树。

![](/img/ui-tree.png)

## React组件树

React组件树是React应用的UI层级结构，它是由多个组件组成的树状结构。每个组件代表一个UI组件，组件之间的关系代表了组件之间的层级关系。

举个例子，一个简单的React组件树如下：

```jsx
const App = () => {
    return (
        <div>
            <h1>Hello, World!</h1>
        </div>
    );
}

export default App;
```

## 回顾

- UI树是UI的层级结构，它是由多个节点组成的树状结构。
- 每个节点代表一个UI组件，节点之间的关系代表了组件之间的层级关系。
- React组件树是React应用的UI层级结构，它是由多个组件组成的树状结构。

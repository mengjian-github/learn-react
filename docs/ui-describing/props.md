---
title: 向组件传递Props
sidebar_position: 5
---

# 向组件传递Props

React组件使用props来相互通信。每个父组件都可以通过给子组件传递props来传递一些信息。Props可能会让你想起HTML属性，但你可以通过它们传递任何JavaScript值，包括对象、数组和函数。

## 向子组件传递Props

```jsx
export default function Profile() {
  return (
    <Avatar
      person={{ name: 'Lin Lanying', imageId: '1bX5QH6' }}
      size={100}
    />
  );
}
```

如上所示，`Profile`组件将`person`和`size`作为props传递给`Avatar`组件。

## 子组件接收Props

```jsx
function Avatar({ person, size }) {
  return (
    <img
      className="avatar"
      src={`https://i.imgur.com/${person.imageId}.jpg`}
      alt={person.name}
      width={size} height={size} />
  );
}
```

`Avatar`组件接收`person`和`size`作为props，并使用它们来渲染图像。

在React组件中，props通过函数参数传递。`{ person, size }`是props对象的解构语法。

## 使用JSX展开语法透传Props

有时候，你可能希望将一个组件的所有props传递给另一个组件。你可以使用JSX展开语法来实现这一点。

```jsx
function Profile(props) {
  return (
    <div className="card">
      <Avatar {...props} />
    </div>
  );
}
```

如上所示，`Profile`组件将所有props传递给`Avatar`组件。

:::warning

应该谨慎使用展开语法。如果你在每个组件中都使用它，那可能意味着出现了问题。通常，这表明你应该拆分你的组件，并通过JSX传递子元素。

:::

## JSX传递Children

在React中，你可以通过在组件之间传递JSX来创建复杂的UI。

```jsx
function Card({ children }) {
  return (
    <div className="card">{children}</div>
  );
}

function Profile() {
  return (
    <Card>
      <h2>小明</h2>
      <p>年龄: 25岁</p>
      <p>职业: 软件工程师</p>
    </Card>
  );
}
```

可以简单理解为，`children`是传递给组件的JSX，组件会将其渲染为子元素。


## props是不可变的

props是不可变的，当一个组件需要改变它的props（例如，响应用户交互或新数据），它必须"请求"其父组件传递不同的props——一个新的对象！旧props随后会被丢弃，最终JavaScript引擎会回收内存。


:::warning

不要尝试"改变props"。当你需要响应用户输入（比如改变选中的颜色）时，你需要"设置状态"，在后面的章节中会介绍。

:::

## 回顾

- 组件使用props相互通信。
- 父组件通过给子组件传递props来传递信息。
- 子组件接收props并使用它们来渲染UI。
- 你可以使用JSX展开语法将一个组件的所有props传递给另一个组件。
- 你可以通过JSX传递children来创建复杂的UI。
- props是不可变的，当一个组件需要改变它的props时，它必须"请求"其父组件传递不同的props。

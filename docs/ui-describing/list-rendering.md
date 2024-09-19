---
title: 列表渲染
sidebar_position: 7
---

# 列表渲染

在React中，可以使用JavaScript数组方法来操作数据。列表的渲染是React中常见的需求，例如，你想显示一个用户列表，或者一个评论列表。

可以使用`filter()`和`map()`方法来过滤和转换数据。

## 过滤数据

假设你有一个用户列表，你想显示那些年龄大于18岁的用户。你可以使用`filter()`方法来过滤数据：

```jsx
const users = [
    { name: 'John', age: 20 },
    { name: 'Jane', age: 25 },
    { name: 'Doe', age: 18 },
];

const filteredUsers = users.filter(user => user.age > 18);

// 渲染过滤后的用户列表
const renderUsers = () => {
    return filteredUsers.map(user => <li key={user.name}>{user.name}</li>);
}

```

## 转换数据

假设你有一个用户列表，你想在每个用户名前面添加一个问候语。你可以使用`map()`方法来转换数据：

```jsx
const users = [
    { name: 'John', age: 20 },
    { name: 'Jane', age: 25 },
    { name: 'Doe', age: 18 },
];

const greetedUsers = users.map(user => ({
    greeting: `Hello, ${user.name}`,
    age: user.age,
}));

// 渲染问候语
const renderGreetings = () => {
    return greetedUsers.map(user => <li key={user.name}>{user.greeting}</li>);
}
```

:::warning

在渲染列表时，必须为每个元素指定一个唯一的`key`。这是React用来识别列表中元素的方式。

我们知道，React是基于Virtual DOM的，当数据发生变化时，React会生成一个新的Virtual DOM，然后与旧的Virtual DOM进行比较，找出变化的部分，然后更新到真实的DOM上。

在比较的过程中，React会使用key来识别列表中的元素，如果key相同，则认为元素相同，否则认为元素不同。

当数组项可能移动（例如由于排序）、插入或删除时，这一点变得尤为重要。选择得当的 key 可以帮助 React 推断出究竟发生了什么，并对 DOM 树进行正确的更新。

:::

## 回顾

1. 使用`filter()`方法来过滤数据。
2. 使用`map()`方法来转换数据。
3. 为每个元素指定一个唯一的`key`。

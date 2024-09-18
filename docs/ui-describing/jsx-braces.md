---
title: JSX中的大括号
sidebar_position: 4
---

# JSX中的大括号

JSX 中的大括号允许你在 JSX 中嵌入 JavaScript 表达式。这意味着你可以在 JSX 中使用大括号来表示变量、函数调用、条件表达式等。

## 在 JSX 中使用大括号

```jsx
const user = {
  name: 'John',
  age: 30
};
const element = <h1>Hello, {user.name}</h1>;
```

## 在属性中使用大括号

```jsx
export default function Avatar() {
  const avatar = 'https://i.imgur.com/7vQD0fPs.jpg';
  const description = 'Gregorio Y. Zara';
  return (
    <img
      className="avatar"
      src={avatar}
      alt={description}
    />
  );
}
```

## 在JSX中使用函数表达式

```jsx
const today = new Date();

function formatDate(date) {
  return new Intl.DateTimeFormat(
    'en-US',
    { weekday: 'long' }
  ).format(date);
}

export default function TodoList() {
  return (
    <h1>To Do List for {formatDate(today)}</h1>
  );
}

```

## 在JSX中使用对象表达式

```jsx
<ul style={
  {
    backgroundColor: 'black',
    color: 'pink'
  }
}>
  <li>first</li>
  <li>second</li>
</ul>
```

## 回顾

- 大括号允许你在 JSX 中嵌入 JavaScript 表达式。
- 可以在属性中使用大括号，也可以在 JSX 中使用函数表达式。
- 可以在 JSX 中使用对象表达式。


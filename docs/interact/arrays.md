---
title: 更新状态数组
sidebar_position: 8
---

# 更新状态数组

在JavaScript中，数组是可变的，但当你将它们存储在状态中时，应该将其视为不可变的。与对象类似，当你想要更新存储在状态中的数组时，需要创建一个新的数组（或复制现有数组），然后将状态设置为使用这个新数组。这种做法有助于保持状态的一致性和可预测性，同时也符合React的状态更新原则。

## 数组方法更新

在React中，你可以使用数组方法来更新状态数组，但要避免直接修改原数组，而是返回一个新的数组。以下是一些常见的数组操作及其推荐的方法：

| 操作 | 避免使用（会改变原数组） | 推荐使用（返回新数组） |
|------|--------------------------|------------------------|
| 添加 | push, unshift | concat, [...arr] 展开语法 |
| 删除 | pop, shift, splice | filter, slice |
| 替换 | splice, arr[i] = ... 赋值 | map |
| 排序 | reverse, sort | 先复制数组 |

## 添加元素

```jsx
const originalArray = [1, 2, 3];
const newArray = originalArray.concat(4); // [1, 2, 3, 4]
// 或者使用展开语法
const newArray = [...originalArray, 4]; // [1, 2, 3, 4]
```

## 删除元素

```jsx
const originalArray = [1, 2, 3, 4];
const newArray = originalArray.filter(num => num !== 3); // [1, 2, 4]
```

## 替换元素

```jsx
const originalArray = [1, 2, 3];
const newArray = originalArray.map(num => num === 2 ? 5 : num); // [1, 5, 3]
```

## 排序

```jsx
const originalArray = [3, 1, 2];
const newArray = [...originalArray].sort((a, b) => a - b); // [1, 2, 3]
```

## 复杂数组对象嵌套修改

与对象一样，复杂的数组对象嵌套修改，建议使用 `immer` 库来更新状态。

```jsx
import { useImmer } from 'use-immer';

const [person, setPerson] = useImmer([
  { name: 'John', age: 30, address: { city: 'New York', country: 'USA' } },
  { name: 'Jane', age: 25, address: { city: 'London', country: 'UK' } },
]);

setPerson(draft => {
  draft[0].age = 31;
  draft[1].address.city = 'Paris';
});
```

在 `immer` 中，`draft` 是一个代理对象，对它的操作不会改变原来的状态，而是会返回一个新的状态。

## 回顾

1. 数组是可变的，但当你将它们存储在状态中时，应该将其视为不可变的。
2. 使用数组方法来更新状态数组，但要避免直接修改原数组，而是返回一个新的数组。
3. 对于复杂的数组对象嵌套修改，建议使用 `immer` 库来更新状态。
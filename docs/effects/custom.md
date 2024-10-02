---
title: 自定义 hooks
sidebar_position: 6
--- 

# 自定义 hooks

React 提供了多个内置的 Hook，如 useState、useContext 和 useEffect。有时，你可能希望有一个用于更具体目的的 Hook：例如，用于获取数据、跟踪用户是否在线，或连接到聊天室。虽然你可能在 React 中找不到这些特定的 Hook，但你可以根据应用程序的需求创建自己的自定义 Hook。这种灵活性使得 React 开发更加强大和可扩展。

## 规则

自定义 Hook 的命名规则：

1. 自定义 Hook 必须以 `use` 开头，后跟大写字母。例如：`useOnlineStatus`、`useFetchData`。

2. React 组件的命名规则：
   - 必须以大写字母开头，如 `StatusBar` 和 `SaveButton`。
   - 需要返回可渲染的内容，通常是 JSX。

3. Hook 与组件的区别：
   - Hook 名称以 `use` 开头，如 `useState`（内置）或 `useCustomHook`（自定义）。
   - Hook 可以返回任意类型的值，不限于 JSX。

遵循这些命名约定有助于区分 Hook 和普通函数，并使代码更易读、易维护。

## 为什么需要自定义 Hook

自定义 Hook 能显著提升代码的复用性和可维护性。通过将组件中重复的逻辑抽象出来，它们实现了多个组件间的逻辑共享。这不仅大大减少了代码冗余，还使得代码更易于测试和调试。

将 Effect 封装在自定义 Hook 中还有以下几个重要优势：

1. 数据流向更加清晰：自定义 Hook 使得数据在 Effect 中的流动变得更加明确和可控。

2. 提升组件的关注点分离：组件可以更专注于其核心功能和意图，而不必过多关注 Effect 的具体实现细节。

3. 增强代码的可维护性：当 React 引入新特性时，你可以轻松地修改或替换这些 Effect，而无需改动使用它们的组件代码。

## 示例

```jsx
import { useState, useEffect } from 'react';

// 自定义 Hook: useLocalStorage
function useLocalStorage(key, initialValue) {
  // 获取初始状态
  const [storedValue, setStoredValue] = useState(() => {
    try {
      // 尝试从 localStorage 获取值
      const item = window.localStorage.getItem(key);
      // 如果存在则解析并返回,否则返回初始值
      return item ? JSON.parse(item) : initialValue;
    } catch (error) {
      // 如果出错则返回初始值
      console.log(error);
      return initialValue;
    }
  });

  // 返回一个包装后的版本的 useState 的 setter 函数
  const setValue = (value) => {
    try {
      // 允许值是一个函数,就像普通的 useState
      const valueToStore = value instanceof Function ? value(storedValue) : value;
      // 保存 state
      setStoredValue(valueToStore);
      // 保存到 localStorage
      window.localStorage.setItem(key, JSON.stringify(valueToStore));
    } catch (error) {
      // 如果出错则记录到控制台
      console.log(error);
    }
  };

  return [storedValue, setValue];
}

// 使用自定义 Hook 的组件
function App() {
  const [name, setName] = useLocalStorage('name', '');
  const [age, setAge] = useLocalStorage('age', 0);

  return (
    <div>
      <input
        type="text"
        placeholder="Enter your name"
        value={name}
        onChange={(e) => setName(e.target.value)}
      />
      <input
        type="number"
        placeholder="Enter your age"
        value={age}
        onChange={(e) => setAge(Number(e.target.value))}
      />
      <p>Hello, {name}! You are {age} years old.</p>
    </div>
  );
}

export default App;
```

这个例子展现了自定义hooks的优势：

1. 封装了 localStorage 的读写逻辑,使用起来就像普通的 state。
2. 提供了错误处理,增强了代码的健壮性。
3. 可以在多个组件中复用,提高了代码的可维护性。

## 回顾

- 自定义 Hook 是 React 中的一种函数，以 `use` 开头，用于封装和重用组件中的逻辑。
- 它们通过将组件中的重复逻辑抽象出来，实现了多个组件间的逻辑共享。
- 自定义 Hook 增强了代码的可维护性和可测试性。

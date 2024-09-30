---
title: Effects
sidebar_position: 3
---

# Effects

某些组件需要与外部系统同步。例如，你可能想根据React状态控制非React组件、建立服务器连接，或在组件出现在屏幕上时发送分析日志。Effects允许你在渲染后运行一些代码，从而使你的组件能够与React之外的系统同步。

Effects为你提供了一种在组件生命周期的特定时刻执行副作用的方式。通过使用Effects，你可以:

- 与浏览器API或第三方库集成
- 订阅数据源或设置事件监听器
- 执行数据获取或更新
- 管理动画或计时器

## Effects的作用

Effects 允许你指定由渲染本身引起的副作用，而不是由特定事件触发。这种机制使得组件能够与外部系统保持同步，无论是什么原因导致组件渲染。

举个例子，在聊天应用中，发送消息通常是由用户点击"发送"按钮这样的特定事件直接触发的。相比之下，建立与聊天服务器的连接则是一个 Effect，因为无论组件是因为什么原因出现在屏幕上，这个连接都应该建立。

Effects 在屏幕更新后、渲染提交阶段结束时运行。这个时机非常适合将 React 组件与外部系统（如网络请求、浏览器 API 或第三方库）进行同步，因为此时 DOM 已经更新，组件状态已经稳定。

## 使用Effect

useEffect 是 React 中用于处理副作用的 Hook。它接收两个参数：一个函数和一个依赖数组。

```jsx
import React, { useState, useEffect } from 'react';

const Timer = () => {
  const [count, setCount] = useState(0);

  useEffect(() => {
    const timer = setInterval(() => {
      setCount((prevCount) => prevCount + 1);
    }, 1000);

    // 清理函数
    return () => {
      clearInterval(timer);
    };
  }, []); // 空依赖数组，表示这个效果只在组件挂载时运行一次

  return (
    <div className="p-4 bg-gray-100 rounded-lg shadow-md">
      <h1 className="text-2xl font-bold mb-2">计时器</h1>
      <p className="text-xl">已经运行了 {count} 秒</p>
    </div>
  );
};

export default Timer;
```

## Effect的触发时机

默认情况下，Effects 会在每次渲染后运行。

你可以通过指定依赖数组来控制它们的运行时机。这种机制使得 Effects 的执行更加灵活和高效，让你能够精确地控制副作用何时发生，从而优化应用性能并避免不必要的操作。

举个例子：

```jsx
const [count, setCount] = useState(0);
  const [name, setName] = useState('');

  // 1. 每次渲染后都会执行
  useEffect(() => {
    console.log('Effect 1: 这个效果在每次渲染后都会执行');
  });

  // 2. 只在组件挂载时执行一次
  useEffect(() => {
    console.log('Effect 2: 这个效果只在组件挂载时执行一次');
  }, []);

  // 3. 当 count 改变时执行
  useEffect(() => {
    console.log(`Effect 3: count 改变了，新值是 ${count}`);
  }, [count]);

  // 4. 当 count 或 name 改变时执行
  useEffect(() => {
    console.log(`Effect 4: count 或 name 改变了。count: ${count}, name: ${name}`);
  }, [count, name]);
```

这个例子展示了 useEffect 的四种常见触发时机：

1. 每次渲染后执行

   - 特点：没有依赖数组
   - 执行时机：每次组件渲染后，包括首次渲染和每次更新
   - 使用场景：需要在每次渲染后执行操作，但要谨慎使用以避免性能问题

2. 仅在组件挂载时执行

   - 特点：依赖数组为空 []
   - 执行时机：仅在组件首次渲染（挂载）后执行一次
   - 使用场景：适用于一次性设置，如订阅数据源或设置事件监听器

3. 特定状态变化时执行

   - 特点：依赖数组包含特定状态（如 [count]）
   - 执行时机：首次渲染后执行，之后仅在指定状态变化时执行
   - 使用场景：响应特定状态变化，更新相关状态或执行副作用

4. 多个状态变化时执行

   - 特点：依赖数组包含多个状态（如 [count, name]）
   - 执行时机：首次渲染后执行，之后任一指定状态变化时执行
   - 使用场景：需要同时考虑多个因素来执行操作

## 回顾

在本节中，我们学习了 useEffect 的基本用法和不同的触发时机：

1. useEffect 是 React 中用于处理副作用的钩子函数。
2. 副作用包括数据获取、订阅、手动 DOM 操作等。
3. useEffect 的触发时机由其依赖数组决定：
   - 无依赖数组：每次渲染后执行
   - 空依赖数组：仅在组件挂载时执行一次
   - 有依赖项：在依赖项变化时执行
4. 合理使用 useEffect 可以优化性能，避免不必要的副作用执行。
5. 清理函数可以在 effect 中返回，用于清理订阅或取消定时器等。

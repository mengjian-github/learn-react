---
title: Effect的生命周期
sidebar_position: 5
---

# Effect的生命周期

Effect的生命周期与组件不同。组件可以挂载、更新或卸载，而Effect只能执行两个操作：开始同步某些内容，然后在稍后停止同步。如果Effect依赖于随时间变化的props和state，这个周期可能会多次发生。React提供了一个lint规则，用于检查你是否正确指定了Effect的依赖项。这确保了你的Effect与最新的props和state保持同步。

## Effect生命周期

Effect 的生命周期可以概括为以下几个阶段：

1. 组件挂载
   - 组件首次渲染完成后，React 执行你定义的 Effect。

2. Effect 执行
   - Effect 函数被调用，执行指定的副作用操作（如数据获取、订阅等）。

3. 组件更新
   - 组件重新渲染时，React 检查 Effect 的依赖项是否变化。

4. 依赖项检查
   - 依赖项未变化：Effect 保持不变。
   - 依赖项变化：React 先清理旧 Effect（如果有清理函数），然后执行新 Effect。

5. 组件卸载
   - 组件从 DOM 中移除前，React 执行 Effect 的清理函数（如果提供）。

这个循环确保了 Effect 始终与组件的最新状态保持同步，同时提供了必要的清理机制。

## 示例

```jsx
import { useEffect } from 'react';

function MyComponent() {
    useEffect(() => {
        // 在组件挂载时执行
        return () => {
            // 在组件卸载时执行
        };
    }, []);
}
```

## 每个Effect独立

每个 Effect 都是独立的，React 会为每个 Effect 创建一个新的清理函数。这意味着你不需要担心多个 Effect 之间的相互影响。

比如：

```jsx
function ChatRoom({ roomId }) {
  useEffect(() => {
    logVisit(roomId);
  }, [roomId]);

  useEffect(() => {
    const connection = createConnection(serverUrl, roomId);
    // ...
  }, [roomId]);
  // ...
}
```

上面两个 Effect 是独立的，尽管它们依赖于相同的 roomId。

## 清理 Effect

清理 Effect 是确保在组件卸载时释放资源的重要机制。React 会在组件卸载前执行清理函数，以避免潜在的内存泄漏。

```jsx
useEffect(() => {
  const subscription = props.source.subscribe();
  // ...
  return () => {
    subscription.unsubscribe();
  };
}, [props.source]);
```

## 回顾

- Effect 在组件挂载时执行。
- Effect 函数可以返回一个清理函数，用于在组件卸载时执行清理操作。
- 每个 Effect 都是独立的，React 会为每个 Effect 创建一个新的清理函数。
- 依赖项变化时，React 会先清理旧的 Effect，然后执行新的 Effect。
- 组件卸载时，React 会执行 Effect 的清理函数。

---
title: 组件导入和导出
sidebar_position: 2
---

# 组件导入和导出

组件的魔力在于它们的可重用性：你可以创建由其他组件组成的组件。但是，随着你嵌套越来越多的组件，将它们拆分到不同的文件中通常是有意义的。这样可以让你的文件更容易浏览，并且可以在更多地方重用组件。



## 默认导出和命名导出

在 React 中，你可以使用 `export default` 来导出一个默认的组件，使用 `export` 来导出多个命名组件。

```jsx
// MyComponent.js
export default function MyComponent() {
  return <h1>Hello, World!</h1>;
}

// AnotherComponent.js
export function AnotherComponent() {
  return <h2>Another Component</h2>;
}
```

:::warning

一个文件只能有一个默认导出。

:::

## 导入组件

你可以使用 `import` 语句来导入组件。

```jsx
import MyComponent from './MyComponent.js';
```

如果是默认导出，你可以使用 `import` 语句来导入默认的组件，并且可以重新命名。

如果是命名导出，那么你必须使用 `{}` 来导入。

```jsx
import { AnotherComponent } from './MyComponent.js';
```

:::info

导入与导出是JavaScript的基础能力，更多细节可以参考[MDN文档](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Guide/Modules)。

:::

## 回顾

- 使用 `export default` 来导出一个默认的组件
- 使用 `export` 来导出多个命名组件
- 使用 `import` 来导入组件
- 使用 `import` 来导入默认的组件，并且可以重新命名
- 使用 `import` 来导入多个命名组件

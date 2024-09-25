---
title: 输入状态
sidebar_position: 1
---

# 输入状态
React 提供了一种声明式的方式来操作用户界面。您无需直接操作界面的各个部分，而是描述组件的不同状态，并根据用户输入在这些状态之间切换。这种方式与设计师思考用户界面的方式相似。

## 声明式UI五步法

### 1. 确定组件的不同视觉状态

```jsx
export default function Form({
  // Try 'submitting', 'error', 'success':
  status = 'empty'
}) {
  if (status === 'success') {
    return <h1>That's right!</h1>
  }
  return (
    <>
      <h2>City quiz</h2>
      <p>
        In which city is there a billboard that turns air into drinkable water?
      </p>
      <form>
        <textarea disabled={
          status === 'submitting'
        } />
        <br />
        <button disabled={
          status === 'empty' ||
          status === 'submitting'
        }>
          Submit
        </button>
        {status === 'error' &&
          <p className="Error">
            Good guess but a wrong answer. Try again!
          </p>
        }
      </form>
      </>
  );
}
```

上面的例子中，`Form` 组件根据 `status` 的不同（`empty`、`submitting`、`success`、`error`），呈现不同的 UI。

### 2. 确定哪些操作会触发状态变化

您可以通过两种输入来触发状态更新：

- 人类输入，如点击按钮、在字段中输入、导航链接。
- 计算机输入，如网络响应到达、超时完成、图像加载。

比如对上面的例子来说：

![input-state](/img/input-state.png)

### 3. 使用 useState 表示状态

```jsx
const [isEmpty, setIsEmpty] = useState(true);
const [isTyping, setIsTyping] = useState(false);
const [isSubmitting, setIsSubmitting] = useState(false);
const [isSuccess, setIsSuccess] = useState(false);
const [isError, setIsError] = useState(false);
```

### 4. 删除任何非必要的 state 变量

我们应该避免状态内容中的重复，只跟踪必要的内容。

可以从以下角度思考：

- 这个状态是否会导致矛盾？例如，isTyping 和 isSubmitting 不可能同时为真。矛盾通常意味着状态的约束不够。两个布尔值有四种可能的组合，但只有三种对应有效状态。为了消除"不可能"的状态，你可以将这些组合成一个必须是三个值之一的 status：'typing'、'submitting' 或 'success'。

- 相同的信息是否已经存在于另一个状态变量中？另一个矛盾：isEmpty 和 isTyping 不能同时为真。将它们作为单独的状态变量，你可能会面临它们不同步并导致错误的风险。幸运的是，你可以移除 isEmpty，而是检查 answer.length === 0。

- 你能否从另一个状态变量的反面获得相同的信息？isError 是不必要的，因为你可以检查 error !== null 来代替。

所以，精简后，我们只需要三个状态变量：

```jsx
const [answer, setAnswer] = useState('');
const [error, setError] = useState(null);
const [status, setStatus] = useState('typing'); // 'typing', 'submitting', or 'success'
```

### 5. 连接事件处理程序

```jsx
function handleSubmit(e) {
  e.preventDefault();
  setStatus('submitting');
  // ...
}
```

## 回顾

在本节中，我们学习了如何有效地管理表单状态：

1. 确定组件的所有可视状态：我们列举了表单可能处于的所有不同状态。

2. 确定状态变化的触发因素：我们考虑了可能导致状态变化的人类输入和计算机输入。

3. 使用 useState 表示状态：我们初步使用多个 useState 钩子来表示不同的状态。

4. 删除非必要的状态变量：我们通过分析状态变量之间的关系，消除了冗余和矛盾的状态，最终得到了更精简的状态表示。

5. 连接事件处理程序：我们展示了如何通过事件处理程序来更新状态。

通过这种方法，我们可以创建一个既易于理解又不容易出错的状态管理系统。这种方法不仅适用于表单，也可以应用于其他复杂的 UI 组件。

记住，好的状态管理是 React 应用性能和可维护性的关键。通过仔细思考和设计你的状态结构，你可以大大简化你的组件逻辑，并减少潜在的 bug。


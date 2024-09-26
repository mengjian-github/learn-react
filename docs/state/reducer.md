---
title: Reducer
sidebar_position: 5
---

# Reducer

当组件中的状态更新逻辑变得复杂，并且分散在多个事件处理程序中时，可能会导致代码难以理解和维护。
为了解决这个问题，React 提供了一种称为 reducer 的模式。reducer 是一个函数，它允许你将所有相关的状态更新逻辑集中到组件外部的一个地方，从而使代码更加清晰和易于管理。


## 一个简单的例子

```jsx
export default function TaskApp() {
  const [tasks, setTasks] = useState(initialTasks);

  function handleAddTask(text) {
    setTasks([
      ...tasks,
      {
        id: nextId++,
        text: text,
        done: false,
      },
    ]);
  }

  function handleChangeTask(task) {
    setTasks(
      tasks.map((t) => {
        if (t.id === task.id) {
          return task;
        } else {
          return t;
        }
      })
    );
  }

  function handleDeleteTask(taskId) {
    setTasks(tasks.filter((t) => t.id !== taskId));
  }

  return (
    <>
      <h1>Prague itinerary</h1>
      <AddTask onAddTask={handleAddTask} />
      <TaskList
        tasks={tasks}
        onChangeTask={handleChangeTask}
        onDeleteTask={handleDeleteTask}
      />
    </>
  );
}
```

在上述代码中，我们使用 `useState` 来管理任务列表的状态。但是会发现tasks的设置逻辑分散在多个事件处理程序中，如果任务列表变得复杂，那么代码会变得难以维护和追踪。

## 使用 reducer 重构

```jsx
export default function TaskApp() {
  const [tasks, dispatch] = useReducer(tasksReducer, initialTasks);

  function handleAddTask(text) {
    dispatch({
      type: 'added',
      id: nextId++,
      text: text,
    });
  }

  function handleChangeTask(task) {
    dispatch({
      type: 'changed',
      task: task,
    });
  }

  function handleDeleteTask(taskId) {
    dispatch({
      type: 'deleted',
      id: taskId,
    });
  }

  return (
    <>
      <h1>Prague itinerary</h1>
      <AddTask onAddTask={handleAddTask} />
      <TaskList
        tasks={tasks}
        onChangeTask={handleChangeTask}
        onDeleteTask={handleDeleteTask}
      />
    </>
  );
}
```

这个时候，组件内部就只有dispatch一个函数，所有的状态更新逻辑都在reducer中进行处理，可以统一追踪回溯：

```js
export default function tasksReducer(tasks, action) {
  switch (action.type) {
    case 'added': {
      return [
        ...tasks,
        {
          id: action.id,
          text: action.text,
          done: false,
        },
      ];
    }
    case 'changed': {
      return tasks.map((t) => {
        if (t.id === action.task.id) {
          return action.task;
        } else {
          return t;
        }
      });
    }
    case 'deleted': {
      return tasks.filter((t) => t.id !== action.id);
    }
    default: {
      throw Error('Unknown action: ' + action.type);
    }
  }
}
```

## 比较 useState 和 useReducer 

- 代码量：通常情况下，使用useState需要编写的初始代码较少。而使用useReducer，你需要编写reducer函数和dispatch动作。然而，当多个事件处理程序以类似方式修改状态时，useReducer可以帮助减少代码量。
- 可读性：当状态更新简单时，useState非常易于阅读。但当状态更新变得复杂时，可能会使组件代码变得臃肿，难以理解。在这种情况下，useReducer可以清晰地将"如何更新"的逻辑与事件处理程序中的"发生了什么"分开。
- 调试：使用useState时，如果出现bug，可能难以确定状态在哪里被错误设置以及原因。而使用useReducer，你可以在reducer中添加console.log来查看每次状态更新及其原因（由哪个action触发）。如果每个action都正确，你就知道问题出在reducer逻辑本身。不过，相比useState，你需要检查更多的代码。
- 测试：reducer是一个不依赖于组件的纯函数。这意味着你可以将其导出并单独进行隔离测试。虽然通常最好在更真实的环境中测试组件，但对于复杂的状态更新逻辑，断言reducer对特定初始状态和action返回特定状态可能会很有用。
- 个人偏好：有些人喜欢reducers，有些人不喜欢。这没关系，这是个人偏好问题。你随时可以在useState和useReducer之间来回转换：它们是等效的！

## 回顾

- useState 和 useReducer 是等效的，你可以根据个人偏好选择使用哪一个。
- 当状态更新逻辑变得复杂时，使用 reducer 可以帮助你将所有相关逻辑集中到组件外部的一个地方，从而使代码更加清晰和易于维护。
- 使用 reducer 时，你需要编写 reducer 函数和 dispatch 动作。
- 虽然 reducer 是一个纯函数，但它不依赖于组件。这意味着你可以将其导出并单独进行隔离测试。
- 你可以随时在 useState 和 useReducer 之间来回转换。

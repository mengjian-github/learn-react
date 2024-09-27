---
title: Context
sidebar_position: 6
---

# Context

在React应用中，通常我们通过props将信息从父组件传递给子组件。然而，当需要跨越多层组件传递props，或者多个组件需要共享相同的数据时，这种方式可能变得繁琐且难以维护。这就是Context发挥作用的地方。Context提供了一种优雅的解决方案，允许父组件向组件树中的任何后代组件直接传递数据，无需通过中间组件逐层传递props。这种机制大大简化了数据的共享和管理，使得代码更加清晰和高效。


## 举一个简单的例子

```jsx
// 创建主题Context
const ThemeContext = createContext({
  theme: 'light',
  toggleTheme: () => {},
});

// 主题提供者组件
const ThemeProvider = ({ children }) => {
  const [theme, setTheme] = useState('light');

  const toggleTheme = () => {
    setTheme(prevTheme => prevTheme === 'light' ? 'dark' : 'light');
  };

  return (
    <ThemeContext.Provider value={{ theme, toggleTheme }}>
      {children}
    </ThemeContext.Provider>
  );
};

// 使用主题的组件
const ThemedComponent = () => {
  const { theme, toggleTheme } = useContext(ThemeContext);

  return (
    <div className={`p-4 ${theme === 'light' ? 'bg-white text-black' : 'bg-gray-800 text-white'}`}>
      <h1 className="text-2xl mb-4">当前主题: {theme}</h1>
      <Button onClick={toggleTheme}>
        {theme === 'light' ? <Moon className="mr-2" /> : <Sun className="mr-2" />}
        切换主题
      </Button>
    </div>
  );
};

// 应用程序根组件
const App = () => {
  return (
    <ThemeProvider>
      <div className="container mx-auto mt-8">
        <ThemedComponent />
      </div>
    </ThemeProvider>
  );
};

export default App;
```

这个例子展示了React Context的几个关键优点：

避免prop drilling：我们不需要通过每一层组件传递主题状态，任何需要访问主题的组件都可以直接使用useContext钩子。

集中状态管理：主题状态被集中在ThemeProvider中管理，使得状态逻辑更加清晰和可维护。

组件复用：ThemedComponent可以在应用的任何地方使用，而不需要担心如何获取主题状态。

动态更新：当主题变化时，所有使用这个Context的组件都会自动重新渲染，无需手动传递更新。

## 使用Context之前

Context 的使用非常诱人！然而，这也意味着很容易过度使用它。仅仅因为需要将一些 props 传递几层深并不意味着应该将该信息放入 context 中。

在使用 context 之前，请考虑以下几种替代方案：

1. 从传递 props 开始。如果你的组件不是很简单，通过十几个组件传递十几个 props 并不罕见。这可能感觉很繁琐，但它能清楚地表明哪些组件使用了哪些数据！维护你代码的人会很感激你通过 props 明确了数据流。

2. 提取组件并将 JSX 作为子元素传递给它们。如果你通过多层中间组件传递一些数据，而这些组件并不使用该数据（只是继续向下传递），这通常意味着你忘记了在途中提取一些组件。例如，你可能将像 posts 这样的数据 props 传递给不直接使用它们的视觉组件，如 `<Layout posts={posts} />`。相反，让 Layout 接受 children 作为 prop，然后渲染 `<Layout><Posts posts={posts} /></Layout>`。这减少了指定数据的组件和需要该数据的组件之间的层数。

3. 如果以上两种方法都不适合你，再考虑使用 context。

## Context的常见使用场景

主题设置：如果您的应用允许用户更改外观（例如暗黑模式），您可以在应用顶层放置一个上下文提供者，然后在需要调整视觉效果的组件中使用该上下文。

当前账户：许多组件可能需要知道当前登录的用户信息。将其放入上下文中可以方便地在组件树的任何位置读取。某些应用还允许同时操作多个账户（例如，以不同用户身份发表评论）。在这些情况下，可以方便地将部分UI包装在具有不同当前账户值的嵌套提供者中。

路由：大多数路由解决方案在内部使用上下文来保存当前路由。这就是每个链接如何"知道"自己是否处于活动状态的原理。如果您构建自己的路由器，您可能也想这样做。

状态管理：随着应用的增长，您可能会在应用的顶层积累大量状态。许多远处的子组件可能想要更改这些状态。通常的做法是将reducer与上下文结合使用，以管理复杂的状态，并将其轻松地传递给远处的组件。

## 回顾

- Context 提供了一种在组件树中传递数据的方式，无需通过中间组件逐层传递 props。
- 使用 Context 之前，应考虑通过 props 传递数据、提取组件或将 JSX 作为子元素传递。
- Context 适用于需要跨越多层组件共享的数据，如主题设置、当前账户、路由和状态管理。
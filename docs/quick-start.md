---
sidebar_position: 1
---

# 快速上手

本节将介绍如何快速上手 React。

## 什么是React

[React](https://reactjs.org/) 是由 Facebook（现为 Meta）的软件工程师 Jordan Walke 于 2011 年创建的。它最初被用于 Facebook 的新闻推送，后来在 2013 年开源。

React 的开发团队主要由 Meta 的工程师组成，但也有来自全球各地的开源社区贡献者。团队致力于不断改进 React，使其更加高效和易用。

截至 2024年，React 的最新稳定版本是 18.3.1。React 18 引入了许多新特性，包括：

1. 自动批处理
2. 并发渲染
3. 新的 Suspense SSR 架构
4. 新的客户端和服务器端渲染 API

目前 React 19 正在开发中，预计将带来更多性能优化和新功能。

React 的发展一直保持着稳定而创新的步伐，不断适应现代 Web 开发的需求，这也是它能够在前端开发领域保持领先地位的原因之一。

## 预备知识

React是一个前端框架，使用它之前，你需要先了解以下知识：

- JavaScript
- HTML
- CSS

本教程假设你已经了解以上知识，并且能够使用它们来构建简单的网页。如果以上知识你还不了解，可以参考以下教程：

- [MDN Web Docs](https://developer.mozilla.org/en-US/docs/Learn)
- [W3Schools](https://www.w3schools.com/)

## 安装React

推荐使用[Next.js](https://nextjs.org/)来快速上手React。

Next.js 是由 Vercel 公司开发的 React 框架，于2016年10月首次发布。它的创建目的是为了解决 React 应用开发中的一些常见问题，如服务器端渲染、路由管理等。

Next.js 的主要特点包括：

1. 零配置：提供开箱即用的开发环境
2. 服务器端渲染（SSR）：提高首屏加载速度和搜索引擎优化
3. 静态网站生成（SSG）：适用于内容不经常变化的网站
4. 自动代码拆分：优化加载性能
5. 内置 CSS 支持：简化样式管理
6. API 路由：轻松创建 API 端点

随着时间的推移，Next.js 不断evolve，引入了许多新特性，如增量静态再生成（ISR）、图像优化等。它已成为 React 生态系统中最受欢迎的框架之一，被许多大公司采用，包括 Netflix、TikTok、Twitch 等。

目前，Next.js 也是 React 官方推荐的框架，特别适合那些需要构建全栈 React 应用的开发者。


```bash
npx create-next-app@latest
```

安装的时候，可以选择默认的模板，也可以选择自定义的模板：

```bash
What is your project named? my-app
Would you like to use TypeScript? No / Yes
Would you like to use ESLint? No / Yes
Would you like to use Tailwind CSS? No / Yes
Would you like your code inside a `src/` directory? No / Yes
Would you like to use App Router? (recommended) No / Yes
Would you like to use Turbopack for `next dev`?  No / Yes
Would you like to customize the import alias (`@/*` by default)? No / Yes
What import alias would you like configured? @/*
```

## 项目结构

Next.js 项目结构如下：

```bash
my-app/
├── node_modules/
├── public/ # 静态资源
├── app/ # 页面路由
```

## 运行项目

```bash
npm run dev
```
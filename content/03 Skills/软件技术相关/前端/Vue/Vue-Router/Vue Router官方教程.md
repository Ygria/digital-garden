---
date: ' 2025-09-02'
tags: 
description: 
title: 
draft: false
keywords: []
---
Vue Route： 建立在 Vue 的组件系统上，

**配置路由以告诉 Vue 路由器每个 URL 路径要显示什么内容。**

# 基础示例

单页应用中，`RouterView` 中的内容随着路由变化，渲染不同的组件内容

```typescript
import { createMemoryHistory, createRouter } from 'vue-router'

import HomeView from './HomeView.vue'
import AboutView from './AboutView.vue'
const routes = [
  { path: '/', component: HomeView },
  { path: '/about', component: AboutView },
]

const router = createRouter({
  history: createMemoryHistory(),
  routes,
})
export default router
```


```vue
<template>
  <h1>Hello App!</h1>
  <p>
    <strong>Current route path:</strong> {{ $route.fullPath }}
  </p>
  <nav>
    <RouterLink to="/">Go to Home</RouterLink>
    <RouterLink to="/about">Go to About</RouterLink>
  </nav>
  <main>
    <RouterView />
  </main>
</template>
```

## RouterLink

代替常规 `<a>` 标签，无需重新加载页面，处理 URL 生成、编码等问题。

## 动态路由匹配

使用形如 `/users/:username/posts/:postId` 的格式来匹配动态路由。**使用场景示例：** 为所有的用户映射到同一个用户页面: 

```typescript
import User from './User.vue'

// these are passed to `createRouter`
const routes = [
  // dynamic segments start with a colon
  { path: '/users/:id', component: User },
]
```

## 可以在同一个路线上加多个参数

```typescript
/users/:username/posts/:postId
```
## 正则匹配路由

可以在 url 中定义 pattern，这样可以获取到路由

## 命名路由

创建路由时，**可选传入路由名字**

```typescript

const routes = [
  {
    path: '/user/:username',
    name: 'profile', 
    component: User
  }
]

```
## 嵌套路线

```
/user/johnny/profile                   /user/johnny/posts 
┌──────────────────┐                  ┌──────────────────┐
│ User             │                  │ User             │
│ ┌──────────────┐ │                  │ ┌──────────────┐ │
│ │ Profile      │ │  ────────────>   │ │ Posts        │ │
│ │              │ │                  │ │              │ │
│ └──────────────┘ │                  │ └──────────────┘ │
└──────────────────┘                  └──────────────────┘
```

## 具有多角色、权限管理功能的后台管理系统，必然会使用到：

1. 路由守卫⚔： 不同角色的用户看到的菜单不同


- **Global Guards:** Applied to all routes in your application 
==全局路由守卫：应用于应用程序中的所有路由==
-  **Per-Route Guards:** Applied to specific routes or route configurations.  
==每路守卫：应用于特定路线或路由配置。==
- 

- 嵌套路由在主路由 `chidren` 属性中定义，可以**递归地定义下去**。


Transition： 渐变

`meta`： 使用 meta 字段定义路由






---







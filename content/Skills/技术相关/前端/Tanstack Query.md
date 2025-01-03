---
date: ' 2025-01-03'
tags: 
description: 
title: 
draft: false
---
# Overview 概述

TanStack Query (FKA Vue Query) is often described as the missing data-fetching library for web applications, but in more technical terms, it makes **fetching, caching, synchronizing and updating server state** in your web applications a breeze.  
TanStack Query (FKA Vue Query) 通常被描述为 Web 应用程序缺少的数据获取库，但用更专业的术语来说，它使 Web 应用程序中的获取、缓存、同步和更新服务器状态变得轻而易举。

## Motivation 动机

](https://tanstack.com/query/latest/docs/framework/vue/overview#motivation)

Most core web frameworks **do not** come with an opinionated way of fetching or updating data in a holistic way. Because of this developers end up building either meta-frameworks which encapsulate strict opinions about data-fetching, or they invent their own ways of fetching data. This usually means cobbling together component-based state and side-effects, or using more general purpose state management libraries to store and provide asynchronous data throughout their apps.  
大多数核心 Web 框架并没有提供一种以整体方式获取或更新数据的固执己见的方式。因此，开发人员最终要么构建封装有关数据获取的严格意见的元框架，要么发明自己的获取数据的方法。这通常意味着将基于组件的状态和副作用拼凑在一起，或者使用更通用的状态管理库在整个应用程序中存储和提供异步数据。

While most traditional state management libraries are great for working with client state, they are **not so great at working with async or server state**. This is because **server state is totally different**. For starters, server state:  
虽然大多数传统的状态管理库非常适合处理客户端状态，但它们不太适合处理异步或服务器状态。这是因为服务器状态完全不同。对于初学者来说，服务器状态：

- Is persisted remotely in a location you may not control or own  
    远程保存在您无法控制或拥有的位置
- Requires asynchronous APIs for fetching and updating  
    需要异步 API 来获取和更新
- Implies shared ownership and can be changed by other people without your knowledge  
    意味着共享所有权，其他人可以在您不知情的情况下进行更改
- Can potentially become "out of date" in your applications if you're not careful  
    如果您不小心，您的应用程序可能会“过时”

Once you grasp the nature of server state in your application, **even more challenges will arise** as you go, for example:  
一旦掌握了应用程序中服务器状态的本质，您就会遇到更多挑战，例如：

- Caching... (possibly the hardest thing to do in programming)  
    缓存...（可能是编程中最难做的事情）
- **Deduping multiple requests for the same data into a single request**  
    **将同一数据的多个请求合并为单个请求**
- Updating "out of date" data in the background  
    在后台更新“过时”数据
- Knowing when data is "out of date"  
    了解数据何时“过时”
- Reflecting updates to data as quickly as possible  
    尽快反映数据更新
- **Performance optimizations like pagination and lazy loading data**  
    **性能优化，例如分页和延迟加载数据**
- **Managing memory and garbage collection of server state**  
    **管理服务器状态的内存和垃圾收集**
- **Memoizing query results with structural sharing**  
    **通过结构共享来记忆查询结果**

If you're not overwhelmed by that list, then that must mean that you've probably solved all of your server state problems already and deserve an award. However, if you are like a vast majority of people, you either have yet to tackle all or most of these challenges and we're only scratching the surface!  
如果您没有被该列表淹没，那么这一定意味着您可能已经解决了所有服务器状态问题并且应该获得奖励。然而，如果您像绝大多数人一样，您要么尚未解决所有或大部分这些挑战，而我们只是触及了表面！

Vue Query is hands down one of the _best_ libraries for managing server state. It works amazingly well **out-of-the-box, with zero-config, and can be customized** to your liking as your application grows.  
Vue Query 无疑是管理服务器状态的最佳库之一。它开箱即用，零配置，工作得非常好，并且可以随着应用程序的增长根据您的喜好进行定制。

Vue Query allows you to defeat and overcome the tricky challenges and hurdles of _server state_ and control your app data before it starts to control you.  
Vue Query 允许您战胜并克服服务器状态的棘手挑战和障碍，并在应用程序数据开始控制您之前控制您的应用程序数据。

On a more technical note, Vue Query will likely:  
从更技术的角度来看，Vue Query 可能会：

- Help you remove **many** lines of complicated and misunderstood code from your application and replace with just a handful of lines of Vue Query logic.  
    帮助您从应用程序中删除许多行复杂且容易被误解的代码，并用几行 Vue 查询逻辑进行替换。
- Make your application more maintainable and easier to build new features without worrying about wiring up new server state data sources  
    使您的应用程序更易于维护并且更容易构建新功能，而无需担心连接新的服务器状态数据源
- Have a direct impact on your end-users by making your application feel faster and more responsive than ever before.  
    让您的应用程序感觉比以往更快、响应更快，从而对最终用户产生直接影响。
- Potentially help you save on bandwidth and increase memory performance  
    可能帮助您节省带宽并提高内存性能

[

## You talked me into it, so what now?  
你说服了我，那现在怎么办？

](https://tanstack.com/query/latest/docs/framework/vue/overview#you-talked-me-into-it-so-what-now)

- Learn Vue Query at your own pace with our amazingly thorough [Walkthrough Guide](https://tanstack.com/query/latest/docs/framework/vue/installation) and [API Reference](https://tanstack.com/query/latest/docs/framework/vue/reference/useQuery)  
    通过我们极其详尽的演练指南和 API 参考，按照您自己的节奏学习 Vue Query
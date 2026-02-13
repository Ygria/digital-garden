---
date: ' 2026-02-04'
tags: 
description: 
title: 
draft: false
keywords: []
---
以前 React 的更新是：

1. 更新开始 - 整棵树递归 diff - 一口气算完

`Fiber` 的一句话核心定义：`Fiber` 是 React 的“可中断虚拟调用栈”

React 的最小调用单元是 Fiber，而不是组件


Fiber 的设计目标：分而治之，将长渲染的流程拆分成小块小块，从而：可中断、可恢复；JS 执行时间过长会卡 UI，高优先级（输入、动画）


**接受停止。**


---
date: ' 2026-04-17'
tags: 
description: 
title: 
draft: false
keywords: []
---
事实上的容错库

水密隔舱：当轮船被撞破一个口，底部舱进水了，因为有钢板存在，只有一个底舱进水，大部分舱位依然安全，船就不会沉。

1. Circuit Breaker  熔断器 （保险丝）
2.  Bulkhead 隔板 （船舱个半）
3. Rate Limiter 限流器
4.  Retry 重试机制
5. Time Limiter 超时控制

该库允许使用注解 + 函数式编程。在某个方法上加上该注解，再进行远程调用，则

在调用第三方服务时，先使用“隔板”进行线程、信号量的隔离。

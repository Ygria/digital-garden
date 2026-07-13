---
date: ' 2026-03-09'
tags: 
description: 
title: 
draft: false
keywords: []
---
## 组件介绍


![image.png](https://images.ygria.site/2026/03/36405d954a299b7a0b2f7cf2aa743fda.png)



Producer： 消息生产者

Connection： Producer 与 Broker 之间建立的 TCP 连接

Channel： 轻量、可复用的“连接”

Broker：整个 RabbitMQ Server 被称为 `Broker`

Virtual host： 出于多租户和安全因素设计；可以划分出多个 Virtual host，每个用户在自己的 virtual host 中建立 Exchange 和 Queue

## Rabbit MQ 的四种核心Exchange 类型

### `fanout`

广播。目标：解耦。Producer 不需要知道有多少消费者，也不需要逐个绑定消费队列。

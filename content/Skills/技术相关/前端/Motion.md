---
date: ' 2024-12-10'
tags: 
description: 
title: 
draft: false
---
`<motion />` 通过强大的手势识别扩展了 React 的事件系统。目前它支持悬停、点击、聚焦和拖动。

<motion.button
  whileHover={{ scale: 1.1 }}
  whileTap={{ scale: 0.95 }}
  onHoverStart={() => console.log('hover started!')}
/>
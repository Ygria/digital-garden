---
date: ' 2025-09-25'
tags: 
description: 
title: 
draft: false
keywords: []
---
声明（定义）宏的关键字：

```rust
macro_rules! 宏名字 {
    (匹配模式) => { 展开内容 };
    (另一个模式) => { 展开内容 };
}
```
宏的匹配： 按照参数匹配也可以写多个分支，按顺序尝试匹配。


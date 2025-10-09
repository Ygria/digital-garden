---
date: ' 2025-09-24'
tags: 
description: 
title: 
draft: false
keywords: []
---
1. 按照指示，安装 rustlings，并使用 VS Code + Rust Analyzer



https://rustlings.rust-lang.org/setup/#installing-rust

## 00_intro - macro


```
println!(r#" Welcome to... "#);
```

Q:  这个 print 语句，为什么有个感叹号？好搞笑

A：Rust 里的 `println!` 确实和你熟悉的 `System.out.println`（Java）或者 `console.log`（TypeScript）不一样——因为它后面多了个 **感叹号 `!`**。

### ✅ 原因：`println!` 是一个 **宏（macro）**

在 Rust 里：
- 没有 `!` 的是 **函数调用**。
- 带 `!` 的是 **宏调用**。


宏有点像 **Lombok 注解（@Getter, @Builder）** 或 **泛型擦除前的模板代码**。

###  为什么 Rust 要宏？

Rust 没有运行时反射（不像 Java），也没有动态元编程（不像 JS/TS）。  
但 Rust 需要“写一份代码 → 适配多种情况”，所以就靠 **宏在编译期生成**。
### Intro 2
将错误的宏名称修改成正确的即可。

## 01_Variables  变量

在默认情况下，使用let 关键字声明的rust 变量是不可变的

```
by default, variables are immutable.
```

可变性：通过 `mut` 关键字声明

- variables 1
补充类型声明的关键字 `let` 即可

- Variables 2
补充赋值语句，并加上 mut 声明

### 可变性

可变性是完全取决于你的。
`Constants` ： 常量，与不可变变量一样

与不可变变量区别：不仅是默认不可变，而且总是不可变的，值的类型**必须**进行注释。常量命名规则：全部大写，且在单词之间使用下划线。

### 遮蔽 Shadowing

遮蔽规则（shadowed）： 第二个变量遮蔽第一个变量（大括号中的变量赋值，在作用域结束后回退。）

```rust
fn main() {
    let x = 5;

    let x = x + 1;

    {
        let x = x * 2;
        println!("The value of x in the inner scope is: {x}");
    }

    println!("The value of x is: {x}");
}
```

遮蔽与将变量标记为 `mut` 不同。1. 遮蔽使得可以重新赋值变量、对变量进行转换，而这些转换完成后，变量依然是不可变的。2. 重新使用 let 变量进行声明，实质上是再次创建了一个新变量，可以更改值的类型。而 mut 不能改类型。





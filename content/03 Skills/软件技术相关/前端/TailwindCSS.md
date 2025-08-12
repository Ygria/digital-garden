---
date: " 2025-03-04"
tags:
  - 前端
  - 计算机
description: 
title: 
draft: false
---
> [!question]
> Ring-offset 是什么？

## 动态样式最佳实现：`cn` 函数

- **在公有方法中定义 `cn` 函数**

```typescript
import { type ClassValue, clsx } from "clsx";
import { twMerge } from "tailwind-merge";

export function cn(...inputs: ClassValue[]) {
  return twMerge(clsx(inputs));
}
```

## 动态踩坑：不支持拼接

`JIT` 编译原理不支持动态拼接类，需使用 `variant` 实现！

##  `bg-muted`


`bg-muted` 是 Tailwind CSS 框架中的一个内置颜色类，通常用于设置背景颜色。

### 具体含义：

- **`bg-`**：表示应用背景颜色（background color）。
- **`muted`**：是 Tailwind 设计系统中的一个语义化颜色名称，通常表示“柔和的”或“低对比度的”颜色，具体颜色值取决于你使用的 Tailwind 主题或 UI 库（如 shadcn/ui）。

### 用途：

`bg-muted` 主要用于：

1. **弱化背景**：用于次要元素或辅助信息，例如卡片的背景色、禁用状态等。
2. **增强可读性**：提供柔和的对比度，避免背景色过于突出。



# 布局

## 固定头部

使用  `sticky` 类固定头部。

## 文字被压缩成竖着一条了，怎么回事？

>  问题描述： 使用 `flex` 布局，存在 `el-select`、标签 `span` 等，发现 `span` 被压缩成一个字、竖直的长长一条了，怎么回事？

解答：`el-select` 等组件会自动扩展。给 `span` 加上 `shrink-0` 防止收缩

# BFC 是什么？


## 高度一致

Tailwind 提供了一个 `items-stretch` 的工具类，可以让子元素在主轴为 `row` 的情况下高度自动拉伸一致（前提是没有设置固定高度）。

## 文本超长了怎么办？

### 1.基础文本溢出处理（不允许换行，超出长度变成省略号）

```HTML
<span class="truncate">
  This is a very long text that will be truncated if it overflows its container.
</span>
```

相当于：

```css
.truncate {
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;
}
```

### 2.  换行处理（宽度不溢出，撑开高度）

要允许在单词内部换行，你需要使用 `break-words` 或 `break-all` 类：

```html
<span class="break-words"> ThisIsAVeryLoooooooooooooooooooooooooooooooooooooooooongWord 
</span> 
```

在任何字符之间都可以断行，即使是在单词内部。

```html
<span class="break-all"> ThisIsAVeryLoooooooooooooooooooooooooooooooooooooooooongWord 
</span> 
```

### 3. 多行换行处理

- **安装插件**

```bash
npm install -D @tailwindcss/line-clamp
```

- **在 `tailwind.config.js` 中启用插件:**

```javascript

 // tailwind.config.js
 module.exports = {
   // ...
   plugins: [
     require('@tailwindcss/line-clamp'),
   ],
 }

```

- **使用 `line-clamp-{n}` 类**

```html
<span class="line-clamp-3 break-words"> Lorem ipsum dolor sit amet, consectetur adipiscing elit. Sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. </span>
```

这将把文本限制在 3 行，并在第三行末尾添加省略号。 `line-clamp-{n}` 中的 `n` 可以是 1 到 6 之间的数字。你也可以在配置文件中自定义行数。

```javascript  // tailwind.config.js 
module.exports = { theme: { extend: { lineClamp: { 7: '7', 8: '8', 9: '9', 10: '10', } } } } 
```
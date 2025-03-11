---
date: " 2025-03-02"
tags:
  - Nextjs
  - 编程
  - 课程
description: 
title: 
draft: false
---

安装 [shadcn/ui](https://ui.shadcn.com/)  组件

```shell
bunx --bun shadcn@latest add badge
```


CSV 上传逻辑：

1. 选择上传后，默认的表头均为 Skip
2. 可以选择某个表头选择
3.  Progress Continue （1/3）

Question: React 在什么时候使用 useRef？用处是什么？





## 1. 保存不需要触发重新渲染的值

当你需要保存一个值，但该值的变化不应该导致组件重新渲染时，使用 `useRef` 非常合适。

```javascript
const count = useRef(0);
// 更新count.current不会触发重新渲染
count.current = count.current + 1;
```

## 2. 访问 DOM 元素

`useRef` 最常见的用途是获取对DOM元素的直接引用。

```javascript
const inputRef = useRef(null);
// 之后可以直接访问DOM元素
const focusInput = () => {
  inputRef.current.focus();
};
return <input ref={inputRef} />;
```

## 3. 保存先前的值

在函数组件的渲染之间保存先前的状态值。

```javascript
function Component({ value }) {
  const prevValueRef = useRef();
  
  useEffect(() => {
    prevValueRef.current = value;
  }, [value]);
  
  const prevValue = prevValueRef.current;
  
  // 现在可以比较prevValue和value
}
```

## 4. 避免闭包陷阱

在异步操作或回调函数中访问最新的状态值。

```javascript
const [count, setCount] = useState(0);
const countRef = useRef(count);

useEffect(() => {
  countRef.current = count;
}, [count]);

const handleClick = () => {
  setTimeout(() => {
    console.log(countRef.current); // 总是获取最新的count值
  }, 3000);
};
```

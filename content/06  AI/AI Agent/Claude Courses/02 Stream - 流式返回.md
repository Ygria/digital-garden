---
date: ' 2025-10-12'
tags: 
description: 
title: 
draft: false
keywords: []
---
## 事件和事件类型

设计目标：尽快看到 LLM 的反馈

| 事件名称                 | 事件目标（Purpose）     | 解释               |
| -------------------- | ----------------- | ---------------- |
| RawMessgeEventStart  | 新消息开始             |                  |
| RawContentBlockStart | 新的消息块开始           | 可能包含文本信息、工具调用等等  |
| RawContentBlockDelta | 当前消息块的分片（chunk）信息 | **包含了实际生成的文本内容** |
| RawContentBlockStop  | 当前消息块停止           |                  |
| RawMessageDeltaEvent | 当前消息停止            |                  |
| RawMessageStop       |                   |                  |

![image.png](https://images.ygria.site/2025/10/00ec78fcd22d537e81141b5ed1869c02.png)

```python
messages = []
add_user_message(messages, "Write a 1 sentence description of a fake database")
model = "claude-sonnet-4-5-20250929"
with client.messages.stream(
model = model,
max_tokens=1000,
messages = messages,
) as stream:
	for text in stream.text_stream:
		print(text, end = "")
```


使用 `stream.get_final_message()` 来获取全部信息。

```python
stream.get_final_message()
```
---
date: ' 2025-10-13'
tags: 
description: 
title: 
draft: false
keywords: []
---
两种控制模型输出的方法：

1. 预填充助理消息，用于控制大模型的输出 - 控制大模型的输出偏好
2. 停止序列：提供一个停止序列，当 Claude 生成了任一字符串，就会立刻强制 Claude停止响应。停止序列本身不包含在最终响应中

> [!example]
>  
例如，如果您要求克劳德“从 1 数到 10”，停止序列为“5”，您将得到
```
1, 2, 3, 4, 
```

实际应用：
1. 一致的格式： 使用预填充，确保响应始终以特定格式开始
2. 受控长度： 使用停止序列，限制自然断点处的响应
3. 控制立场：当需要 LLM 采取特定的立场，而不是中立时
4. 结构化输出：结合来这两种技术，来生成特定模板的响应


`fine-grained control`  - **细颗粒度控制** - 可靠性与一致性


### 结构化输出 Structured Data

应用举例： 需要在输入后，点击“生成”，并期望看到可以立即复制和使用的干净 `JSON`。如果 Claude 返回包含在带有解释性文本的 markdown，就算返回的内容是正确的，用户也需要**选中-复制，** 这就给用户体验带来了摩擦。



```python
messages = [] 
add_user_message(messages, "Generate a very short event bridge rule as json") 

add_assistant_message(messages, "```json") 

text = chat(messages, stop_sequences=["```"])
```

技术工作原理：

1. 用户消息-告诉 Claude 要生成什么
2. 预先填充助手消息：让 Claude 认为自己已经生成了一个 json 代码块
3. Claude 会继续编写 json 内容
4. 当 Claude 尝试使用\`\`\` 关闭时，因为传入了停止序列，所以会立刻终止。

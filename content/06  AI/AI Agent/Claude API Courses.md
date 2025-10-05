---
date: " 2025-10-01"
tags:
description:
title:
draft: false
keywords: []
---
- Basic Python fundamentals （基础的对于 Python 的了解）
- Python Environment - 可以运行 Python Notebook 的环境

Claude Model：不同模型的区别在于优化的目标不一样，对于智能和速度的平衡不一样。（`intelligence & speed` ）

Opus : Intelligence ⬆️，适合复杂科研项目，可以持续运行数小时完成多步复杂任务
Sonnet： 智能、费用、速度的完美平衡，适合用于大多数实践场景

## Python Notebook（通常是 Jupyter Notebook）

**Python Notebook（通常是 Jupyter Notebook）** 是一个交互式的 Python 开发环境，常用于数据分析、机器学习、可视化和教学。你可以一边写代码一边运行，并且在同一页面展示结果（包括文字、表格、图表等）
### 使用 conda

```shell
conda info --envs
conda activate (envname)
```

```bash
conda install jupyterlab
conda install notebook
```

在目录下运行：

```bash
jupyter notebook
```

会自动启动一个 `localhost:8888` 的页面。

![image.png](https://images.ygria.site/2025/10/c92ad2b9e3050649a3dd7c54724b4e58.png)

- 点击右上角 **New --> Python 3 (ipykernel)**

在 `Jupyter` 网页里写代码然后 `Shift + Enter` 执行即可。

1. 安装依赖 Install dependencies

> [!note]
> 前面的 **百分号 `%`** 是 **IPython 的魔法命令（magic command）**，它不是普通的 Python 语法。在 **Jupyter Notebook** 里，如果直接写 `pip install`，它可能会装到系统 Python，而不是当前 Notebook 内核用的 Python，导致 notebook 里还是导入不了包。所以 IPython 提供了一个 **魔法命令** `%pip`（和 `%conda` 类似），保证它会把包装到 **当前内核** 所用的环境。

```
%pip install anthropic python-dotenv
```

> [!tip]
> 可以在 Cell 上右键 -清除 Cell Output，避免过多的日志堆积


![image.png](https://images.ygria.site/2025/10/cbbfce0e202e55270cd5c21fe1b73ab4.png)


2. 将 KEY 存储在 `.env` 文件下，这默认 Git Ignore，也就不太可能被泄漏出去。


**The ‘create’ function**

Generate text 

创建 `Claude Client`  的构造函数

![image.png](https://images.ygria.site/2025/10/5abce9dd0819285c40fb3b32c4b78f4d.png)

- User Message: Content we want to feed into the model
- Assistant Message: Content the model has produced

## 模型本身不存储任何信息

为了进行多轮对话，必须将之前的模型返回的内容携带一起返回，共同构成对话的“上文”。
所以当进行对话时，必须维护一个 `list` 来存储与 LLM交换的所有信息，从而保留完整的上下文。

### 安装依赖

```bash
# Install dependencies、
%pip install anthropic python-dotenv
```

### 定义环境变量

在 Notebook（`.ipynb` ）文件平级的地方新建 `.env` 文件，可以自定义调用的 `baseUrl` 和 `token`

```yaml
ANTHROPIC_BASE_URL="XXXXXXX"
ANTHROPIC_AUTH_TOKEN="XXXXXXX"
```

### 加载环境变量

打印方法仅为了验证环境变量已经成功加载。

```python
# Load env environment
from dotenv import load_dotenv
import os
load_dotenv()
base_url = os.getenv("ANTHROPIC_BASE_URL")
auth = os.getenv("ANTHROPIC_AUTH_TOKEN")
print("ANTHROPIC_BASE_URL=", base_url)
print("ANTHROPIC_AUTH_TOKEN =", auth)
```

### 定义工具类

```python
from anthropic import Anthropic
client = Anthropic()

def add_user_message(messages, text):
	user_message = {"role": "user", "content": text}
	messages.append(user_message)

def add_assistant_message(messages, text):
	assistant_message = {"role": "assistant", "content": text}
	messages.append(assistant_message)

def chat(messages):
# Make a request	
	message = client.messages.create(
	model = "claude-sonnet-4-5-20250929",
	max_tokens = 1000,
	messages = messages
	)
	return message.content[0].text
```

### 实际多轮对话

```python
# Make a starting list of messages
messages = []
# Add in the initial user question of "Define quantum computing in one sententce"
add_user_message(messages, "Define quantum computing in one sententce")
# Pass the list of messages into 'chat' to get an answer
ret_message = chat(messages)
# Take the ansewer and add it as an assistant
add_assistant_message(messages, ret_message)
# add in the user's follow-up question
add_user_message(messages, "Give me another sentence")
# Call chat again with the list of messages to get a final answer
ret_message = chat(messages)
```
## Chat Bot Exercise 聊天机器人小练习

```
Make a chat bot using the three helper funtions we just put together.
1. Prompt the user to enter some input using the built-in "input" function
2. Add it to a list of messages
3. Call the API
4. Add generated text to the list of messages.
5. Print the generated text
6. Repeat from #1
```


> [!note]
>  1. `while True` 一直等待用户的输入
>  2.  使用 `built-in` 的 `input` 方法，读取用户输入




---
date: " 2025-02-15"
tags:
  - obisidian
description: Obsidian插件Copilot可以基于本地知识库进行问答交互，帮助用户更好地利用个人笔记进行深度分析和内容创作。
title: 
draft: false
keywords:
  - Obisidian插件
  - Copilot
  - 知识库
---

当需要根据特定垂类内容进行深度分析时，经常会用到知识库，与单纯使用 AI 相比，使用自己选择的内容和材料，有如下好处：

  1. 使用自己的知识库，可以让内容更符合个人认知，也能使用过程中的反馈帮助梳理自己的内容。
  2. 更贴合个人场景，更加垂直


Obisidian 插件 Copilot ，可以直接基于你本地的内容仓库生成知识库，然后直接基于整个内容仓库（Vault）或者单文件做问答交互。

## 1 . Obisidian 安装 Copilot 插件

设置-第三方插件（需先关闭安全模式）。

![image.png](https://images.ygria.site/2025/02/8eb0c94e5df230cbd22d762918f4ce7f.png)

## 2. 配置模型

在设置-Model 中配置使用的模型（需要两个模型：Chat Models （对话模型）和 Embedding Models


![image.png](https://images.ygria.site/2025/03/a058836d4b1a65c17b77d51d4c3278cf.png)


-  在火山引擎/硅基流动/谷歌 AI Studio……等 AI 开放平台，配置推理节点，或使用免费开放的大模型，获取 API 地址和 Key
-  配置两个模型（==往下滚动配置 Embedding 模型，注意不要配错位置了！==）
- 配置时可以点击 `verify` 验证配置的地址是否有效
Chat Model
![image.png](https://images.ygria.site/2025/03/0c1e7c193b12f8b1c83341d97c61200d.png)

如果使用的模型在默认列表中，直接填上 key 就可以了。否则可以添加使用 OpenAI Format 协议 的任意自定义模型。


Embedding Models：我使用的是 BAAI/bge-m3

![image.png](https://images.ygria.site/2025/03/f38385d448fd19545357a32fc6f77ef0.png)
-  在 Basic-general 中配置使用的模型
![image.png](https://images.ygria.site/2025/03/72cb7cb9b2287b7e0b05ac6e8890c2a6.png)


## 3. 使用

点击左侧的图标开始使用

![image.png](https://images.ygria.site/2025/03/4e143503bc6e9c654c344cf2b91032d0.png)

可以切换到 vault QA ，与全库对话

![image.png](https://images.ygria.site/2025/03/91055245f15e637cb1bf784425fff2d1.png)



![image.png](https://images.ygria.site/2025/03/db51478199c500ed584f519bd4ac59eb.png)

![image.png](https://images.ygria.site/2025/03/ed1835afceb5ccf459e3d52f9c924124.png)

可以看到，根据本库内的文件内容，整合出了一份说明。可以根据这些说明去增补关联链接、合理化文件结构，或者用它来更好地检索材料，完成观点提炼和内容输出。

AI 提供了另一个视角去查看自己写下或收藏的内容，也能帮助个人思考的系统化、结构化。

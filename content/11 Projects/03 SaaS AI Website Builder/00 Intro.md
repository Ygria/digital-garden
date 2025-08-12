---
date: ' 2025-08-10'
tags: 
description: 
title: 
draft: false
keywords: []
---
> [!info]
> 技术栈：NextJS 15 + React 19 + Neon（PostSQL） TRPC + Tanstack Query
> Inngest （Background Jobs & Agent Tooling）  


Bolt 、`v0` 等生成 AI Agent 实时预览页面的网站是怎么做的呢？

提供 E 2 B沙箱，运行一个独立的 app，会暴露一个实时的 live 用于预览，最后，完成的项目保存在 neon 数据库中。

对话框显示：还剩余多少积分，并提示升级。

更灵活、更具有互动性的页面：可以拖动


- E2B 是一个开源基础设施，提供**隔离安全的云端沙箱环境**，用于执行由大型语言模型（LLM）生成的代码，完全防止未经信任代码对主系统造成安全风险 [ADaSci](https://adasci.org/mastering-ai-code-execution-in-secure-sandboxes-with-e2b/?utm_source=chatgpt.com)[GitHub](https://github.com/e2b-dev/E2B?utm_source=chatgpt.com)[AI Tools Explorer](https://aitoolsexplorer.com/ai-tools/e2b-secure-ai-code-execution-runtime/?utm_source=chatgpt.com)。
    
- 它尤其适合用于 AI agent、数据分析、可视化、代码评估等场景 [AI Tools Explorer](https://aitoolsexplorer.com/ai-tools/e2b-secure-ai-code-execution-runtime/?utm_source=chatgpt.com)[aijumble.com](https://aijumble.com/software-listings/e2b/?utm_source=chatgpt.com)[aigregator](https://aigregator.com/tools/e2b?utm_source=chatgpt.com)。

## 核心技术与特性

- **超快启动时间**：沙箱启动速度极快，通常在 150–200 毫秒内完成，无需冷启动等待 [ADaSci](https://adasci.org/mastering-ai-code-execution-in-secure-sandboxes-with-e2b/?utm_source=chatgpt.com)[AI Tools Explorer](https://aitoolsexplorer.com/ai-tools/e2b-secure-ai-code-execution-runtime/?utm_source=chatgpt.com)[aijumble.com](https://aijumble.com/software-listings/e2b/?utm_source=chatgpt.com)。
    
- **基于 Firecracker microVM 的隔离安全机制**：使用轻量级微虚拟机确保代码在安全沙箱中运行 [AI Tools Explorer](https://aitoolsexplorer.com/ai-tools/e2b-secure-ai-code-execution-runtime/?utm_source=chatgpt.com)[E2B](https://e2b.dev/blog/how-manus-uses-e2b-to-provide-agents-with-virtual-computers?utm_source=chatgpt.com)。
    
- **多语言支持**：支持 Python、JavaScript、Ruby、C++ 等多种语言运行环境 [AI Tools Explorer](https://aitoolsexplorer.com/ai-tools/e2b-secure-ai-code-execution-runtime/?utm_source=chatgpt.com)[kdjingpai.com](https://www.kdjingpai.com/en/e2b/?utm_source=chatgpt.com)[aijumble.com](https://aijumble.com/software-listings/e2b/?utm_source=chatgpt.com)。
    
- **SDK 支持**：提供 Python 和 JavaScript/TypeScript SDK，用于创建和管理沙箱 [GitHub](https://github.com/e2b-dev/E2B?utm_source=chatgpt.com)[E2B+1](https://e2b.dev/blog/build-ai-data-analyst-with-sandboxed-code-execution-using-typescript-and-gpt-4o?utm_source=chatgpt.com)。
    
- **丰富沙箱功能**：包括文件系统 I/O、安装依赖、访问终端、运行图形界面（如 Jupyter）、生成交互式图表 [E2B+1](https://e2b.dev/blog/build-ai-data-analyst-with-sandboxed-code-execution-using-typescript-and-gpt-4o?utm_source=chatgpt.com)[aijumble.com](https://aijumble.com/software-listings/e2b/?utm_source=chatgpt.com)。
    
- **长期会话与并发支持**：Pro 版支持沙箱运行长达 24 小时甚至更久，同时可以并发启动多个沙箱应对高并发需求 [AI Tools Explorer](https://aitoolsexplorer.com/ai-tools/e2b-secure-ai-code-execution-runtime/?utm_source=chatgpt.com)[aijumble.com](https://aijumble.com/software-listings/e2b/?utm_source=chatgpt.com)。
    
- **自托管部署**：支持在 AWS、GCP、Azure 或私有 Linux 环境中自行部署 [GitHub](https://github.com/e2b-dev/E2B?utm_source=chatgpt.com)[kdjingpai.com](https://www.kdjingpai.com/en/e2b/?utm_source=chatgpt.com)。
    

---

## 典型应用场景

- **AI Agent “虚拟电脑”**：如 Manus 多 agent 系统，通过 E2B 提供完整云端虚拟电脑环境，执行复杂任务（包括浏览网页、运行终端命令、文件操作等），还能暂停、恢复沙箱会话 [E2B](https://e2b.dev/blog/how-manus-uses-e2b-to-provide-agents-with-virtual-computers?utm_source=chatgpt.com)。
    
- **AI 数据分析与可视化**：动态运行脚本分析 CSV、生成图表，让 AI 主动探索数据与呈现见解 [AI Tools Explorer](https://aitoolsexplorer.com/ai-tools/e2b-secure-ai-code-execution-runtime/?utm_source=chatgpt.com)[E2B](https://e2b.dev/blog/build-ai-data-analyst-with-sandboxed-code-execution-using-typescript-and-gpt-4o?utm_source=chatgpt.com)[aijumble.com](https://aijumble.com/software-listings/e2b/?utm_source=chatgpt.com)。
    
- **智能编码助手与自动化 Agent**：允许 LLM 生成并执行代码，实现自动化任务、调试、持续集成等功能 [Runbear](https://runbear.io/use-cases-software/e2b?utm_source=chatgpt.com) [Applied AI Tools](https://appliedai.tools/product/e2b-best-to-securely-scale-ai-code-execution-in-cloud-sandboxes/?utm_source=chatgpt.com)[aigregator](https://aigregator.com/tools/e2b?utm_source=chatgpt.com)。



Docker： 生成所需要执行的项目模版文件
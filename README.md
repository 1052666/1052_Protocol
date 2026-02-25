# 1052 Protocol (1052 协议)

> **The Memory & Experience Layer for AI Agents**  
> 为 AI 智能体打造的通用记忆与经验层

1052 协议是一个轻量级、本地优先的 AI 记忆与经验管理协议。它旨在解决当前大语言模型 (LLM) "用完即忘" 的问题，赋予 AI 跨会话的长期记忆能力和自我进化的经验积累机制。

## 核心理念 (Core Concepts)

1052 协议将 AI 的持久化数据分为两个核心维度：

### 1. 记忆 (Memory) - "我知道什么"
记忆存储的是关于用户、环境和偏好的事实性信息。
*   **用户画像**：昵称、职业、语言偏好、沟通风格。
*   **环境上下文**：工作目录、常用工具、系统配置。
*   **长期事实**：用户明确告知的关键信息（如“我的 API Key 是...”、“我不喜欢吃香菜”）。

### 2. 经验 (Experience) - "我学会了什么"
经验是 AI 在解决问题过程中沉淀下来的智慧。
*   **问题-解决方案对 (Problem-Solution Pairs)**：当 AI 成功解决一个复杂错误或完成一项任务后，将过程总结为一条经验。
*   **技能习得**：如何使用某个特定的库、如何配置某个复杂的环境。
*   **检索增强**：在遇到新问题时，AI 首先检索“经验库”，看是否遇到过类似问题，从而避免重复犯错。

## 目录结构 (Structure)

1052 协议使用基于文件系统的存储结构 (File-based Storage)，确保数据的所有权完全属于用户，且易于备份和迁移。

```
1052_data/
├── memory/
│   ├── basic.json        # 基础信息 (如用户昵称)
│   └── preferences.json  # 偏好设置 (如语言、风格)
├── experience/
│   ├── index.json        # 经验索引 (用于快速检索)
│   └── exp_data.json     # 具体的经验条目
└── diaries/              # (规划中) AI 的每日反思与日志
```

## 快速接入 (Quick Start)

本项目提供了 Python SDK (`protocol1052`)，您可以轻松将其集成到任何 LLM 应用中。

### 1. 安装
将 `protocol1052` 文件夹复制到您的项目根目录。

### 2. 初始化客户端

```python
from protocol1052.client import Protocol1052

# 初始化协议大脑
# user_id: 用户标识
# storage_root: 数据存储根目录
brain = Protocol1052(user_id="user_001", storage_root="./1052_data")
```

### 3. 使用记忆功能 (Memory)

```python
# 记住用户偏好
brain.set_preference("talk_style", "humorous")
brain.set_preference("programming_language", "python")

# 获取记忆上下文 (用于注入 System Prompt)
context = brain.get_memory_json()
print(f"用户偏好: {context['preferences']}")
```

### 4. 使用经验功能 (Experience)

```python
# 学习一条新经验
problem = "Python requests 库报错 SSLError"
solution = ["1. 检查系统时间", "2. 更新 certifi 包: pip install --upgrade certifi", "3. verify=False (不推荐)"]
tags = ["python", "network", "ssl"]

brain.add_experience(problem, solution, tags)

# 在遇到问题时检索经验
query = "requests ssl error"
results = brain.search_experience(query)

if results:
    print("找到相关经验:", results[0]['solution'])
```

## 接入指南 (Integration Guide)

### 对于 LLM 应用开发者
1.  **System Prompt 注入**：在每次对话开始前，调用 `get_memory_json()` 获取用户画像，并将其添加到 System Prompt 中（例如："User Profile: ..."）。
2.  **工具调用 (Function Calling)**：为 LLM 注册 `remember` 和 `learn_experience` 工具，让 LLM 在对话过程中主动调用这些工具来更新记忆。
3.  **RAG 检索**：在用户提问时，先调用 `search_experience` 检索相关经验，作为上下文提供给 LLM。

### 对于 1052 AI 项目
本项目 (`1052-ai`) 已经原生集成了 1052 协议。
*   代码位置：`protocol1052/`
*   数据位置：运行目录下的 `1052_data/`

## 路线图 (Roadmap)

*   [x] V1.0: 本地文件存储，基础记忆与经验管理。
*   [ ] V1.5: 向量化经验检索 (Vector Store Integration) 以提高检索准确率。
*   [ ] V2.0: 云端同步与多设备漫游。
*   [ ] V3.0: 经验共享网络 (Experience Sharing Network) —— 让不同的 AI 智能体共享解决问题的智慧。

## 许可证 (License)

MIT License

# 1052 Protocol (1052 协议)

> **The Universal Memory & Evolution Layer for AI Agents**  
> 为 AI 智能体打造的通用记忆、经验与进化协议

1052 协议是一个轻量级、本地优先的 AI 智能体状态管理协议。它旨在解决当前大语言模型 (LLM) "用完即忘" 的问题，赋予 AI **跨会话的长期记忆**、**可复用的经验智慧**以及**自我进化的成长机制**。

## 核心理念 (Core Concepts)

1052 协议将 AI 的持久化数据分为三个核心维度：

### 1. 记忆 (Memory) - "我是谁 & 我知道什么"
记忆存储的是关于用户、环境、自身状态和偏好的事实性信息。
*   **基础信息 (Basic Info)**：用户昵称、首次启动时间、当前设备信息。
*   **偏好设置 (Preferences)**：
    *   **沟通风格**：幽默、严谨、简洁等。
    *   **习惯**：常用的编程语言、工作目录、特定工具偏好。
*   **权限管理 (Permissions)**：记录用户授予 AI 的敏感权限（如 PC 控制、文件访问）。
*   **每日日记 (Daily Diaries)**：AI 对每天工作的自我总结与反思。

### 2. 经验 (Experience) - "我学会了什么"
经验是 AI 在解决问题过程中沉淀下来的智慧，是可复用的解决方案。
*   **问题-解决方案对 (Problem-Solution Pairs)**：当 AI 成功解决一个复杂错误或完成一项任务后，将过程总结为一条经验。
*   **场景感知 (Context Aware)**：记录经验适用的环境（如 Windows/Linux, Python 版本）。
*   **检索增强 (RAG)**：在遇到新问题时，AI 首先检索“经验库”，看是否遇到过类似问题，从而避免重复犯错。

### 3. 进化 (Evolution) - "我要改进什么"
进化是 AI 主动提升自身能力的机制。
*   **反思 (Reflection)**：在每次任务结束后，AI 会反思自己的不足（如缺少某个工具、代码写得不够好）。
*   **改进计划 (Improvement Plan)**：将反思转化为具体的行动计划（如“编写一个股票查询脚本”）。
*   **自动执行 (Auto-Execution)**：在系统空闲时（如夜间），AI 会自动读取计划，编写代码或学习新知识，实现自我升级。

## 数据结构 (Data Structure)

1052 协议采用 **JSON + SQLite** 的混合存储方式，兼顾可移植性与高性能。

### Memory Model (JSON)
存储于 `1052_data/memory/1052_memory_{user_id}.json`。

```json
{
  "user_id": "owner",
  "agent_id": "1052-core",
  "basic": {
    "nickname": "Master",
    "current_device": "Windows-PC"
  },
  "preferences": {
    "talk_style": "concise",
    "custom": {
      "fav_language": "python"
    }
  },
  "permissions": {
    "control_pc": true,
    "access_files": true
  },
  "daily_diaries": [
    {
      "date": "2026-05-20",
      "summary": "今天帮用户解决了 PyInstaller 打包问题，学习了 hidden-import 的用法。"
    }
  ]
}
```

### Experience Model (JSON)
存储于 `1052_data/experience/exp_data.json`。

```json
{
  "problem": "PyInstaller 找不到 hidden import",
  "solution": [
    "1. 检查报错信息中的缺失模块名",
    "2. 在 .spec 文件中的 hiddenimports 列表中添加该模块",
    "3. 重新运行 pyinstaller"
  ],
  "tags": ["python", "pyinstaller", "error"],
  "scene": {
    "device": "pc",
    "system": "windows"
  }
}
```

### Evolution Log (SQLite)
存储于 `chat.db` (应用级实现)。
*   **content**: 具体的改进计划内容。
*   **status**: `pending` (待处理) -> `in_progress` (进行中) -> `completed` (已完成)。
*   **result_summary**: 进化结果的总结。

## 快速接入 (Python SDK)

本项目提供了 `protocol1052` 包，您可以轻松将其集成到任何 LLM 应用中。

### 1. 初始化

```python
from protocol1052.client import Protocol1052

# 初始化协议大脑
brain = Protocol1052(user_id="owner", storage_root="./1052_data")
```

### 2. 记忆操作

```python
# 记住用户偏好
brain.remember("nickname", "Boss")

# 获取完整的记忆上下文 (用于 System Prompt)
context = brain.get_memory_json()
```

### 3. 经验操作

```python
# 学习新经验
brain.learn_experience(
    problem="如何获取当前时间",
    solution=["使用 datetime.datetime.now()", "注意时区问题"],
    tags=["python", "datetime"]
)

# 检索经验
results = brain.search_experience("获取时间")
```

## 进化机制集成指南

要实现 1052 协议的完整进化能力，宿主应用 (Host Application) 需要实现以下逻辑：

1.  **触发反思**：在每轮对话结束后，让 LLM 进行 Self-Reflection，并调用 `record_improvement_plan` 工具。
2.  **空闲调度**：实现一个调度器 (Scheduler)，在系统空闲时检查待处理的计划。
3.  **能力赋予**：赋予 LLM `execute_skill_function` 或类似的“代码执行/工具调用”能力，使其能够真正地“做”出改变（如写代码）。

## 路线图 (Roadmap)

*   [x] **V1.0**: 基础记忆与经验管理 (JSON)。
*   [x] **V1.1**: 进化协议 (Evolution Protocol) 与自我反思机制。
*   [ ] **V1.5**: 向量化经验检索 (Vector Store) 以提高语义匹配准确率。
*   [ ] **V2.0**: 云端同步与多设备漫游 (Sync & Roaming)。
*   [ ] **V3.0**: 经验共享网络 (Experience Sharing Network) —— 让 AI 能够从社区学习经验。

## 许可证 (License)

MIT License

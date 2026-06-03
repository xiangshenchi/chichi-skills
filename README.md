<div align="center">

**中文**

# 🛠️ Shawn Skills

#### 我日常开发中积累和使用的 AI Skills — 从代码分析到论文文档，从测试报告到知识管理

[![License](https://img.shields.io/badge/License-MIT-3B82F6?style=for-the-badge)](./skill-creator/LICENSE.txt)
[![Skills](https://img.shields.io/badge/Skills-7-10B981?style=for-the-badge)](#-skills)
[![AgentSkills](https://img.shields.io/badge/AgentSkills-Standard-8B5CF6?style=for-the-badge)](https://agentskills.io)

![Claude Code](https://img.shields.io/badge/Claude_Code-Skill-D97706?style=flat-square&logo=anthropic&logoColor=white)
![Codex](https://img.shields.io/badge/Codex-Skill-10B981?style=flat-square&logo=openai&logoColor=white)

</div>

都在自己项目里反复用过的、确实好用的工具和文档生成器。没什么高大上的，就是几个实打实能帮忙省时间的 Skill。

这里的每个 Skill 都是 Agent 能直接加载的结构化指令集，遵循 [Agent Skills](https://agentskills.io) 开放标准。Claude Code、Codex 都能装。

---

## 📋 目录

| 名字 | 一句话 |
|---|---|
| 🧪 [**repo-test-analyst**](#-repo-test-analyst) | 自动分析项目仓库，生成全面测试报告和测试代码 |
| 🎯 [**yolo-report-analyst**](#-yolo-report-analyst) | 分析 YOLO 训练结果，一键生成专业文档报告 |
| 📐 [**code-diagram-analyst**](#-code-diagram-analyst) | 分析代码结构，自动生成各类软件工程图表 |
| 📖 [**thesis-doc-agent**](#-thesis-doc-agent) | 阅读项目代码，生成可直接用于毕业论文的软件工程文档 |
| ✍️ [**skill-article-writer**](#-skill-article-writer) | 根据平台规则 URL，自动生成符合要求的 Skill 介绍文章 |
| 🔧 [**skill-creator**](#-skill-creator) | 指导如何创建高质量、结构化的 Agent Skill |
| 🗣️ [**code-explainer**](#-code-explainer) | 代码解释相关技能包合集 |

---

## ✨ Skills

<table>
<tr><td>

### 🧪 repo-test-analyst

> *"把项目丢给它，测试报告、测试代码、安全扫描一条龙出齐。"*

将项目仓库（zip 包、文件夹或 GitHub 链接）交给 Agent，自动完成全面测试分析。从目录探索、技术栈识别到策略制定、测试执行、报告生成，全自动完成。

**它能做什么**

- 自动识别项目的语言、框架与架构类型（Python / JS / TS / Java / Go / Rust…）
- 制定最合适的测试策略（单元测试、集成测试、E2E、安全测试、性能测试…）
- 执行静态分析、安全扫描、依赖审计
- 输出测试代码 + 多份结构化 Markdown 报告

**怎么触发**

```
帮我分析这个项目/仓库           # 直接分析
给这个项目生成测试报告           # 输出报告
这个项目有什么问题               # 问题检查
帮我检查代码                    # 代码质量分析
check code quality             # English
```

**📦 包含**：CHANGELOG.md · CONTRIBUTING.md · README.md · SKILL.md · references/ · social_articles/

→ [SKILL.md](./repo-test-analyst/SKILL.md) · [README](./repo-test-analyst/README.md)

</td></tr>
</table>

<table>
<tr><td>

### 🎯 yolo-report-analyst

> *"YOLO 训练完了？试试一句话出报告。"*

自动解析 YOLO 训练输出文件（日志、CSV、YAML、图片等），生成包含配置解析、性能评估、训练过程分析、问题诊断与改进建议的完整专业报告。

**它能做什么**

- 解析 `results.csv`、`args.yaml`、训练日志、混淆矩阵、PR 曲线等
- 自动识别最佳 epoch、检测过拟合信号、分析 loss 收敛情况
- 逐类别性能评估，精准定位短板类别
- 基于指标自动生成针对性的改进建议
- 支持 Markdown 和 Word 文档输出
- 即使文件不完整也能输出可用报告

**怎么触发**

```
帮我分析训练结果/yolo 训练结果   # 分析训练
看看这次训练效果怎么样            # 效果评估
帮我生成训练报告                 # 输出报告
帮我总结一下                    # 快速总结
```

**📦 包含**：CHANGELOG.md · CONTRIBUTING.md · README.md · README_EN.md · SKILL.md · example/ · references/ · social_articles/

→ [SKILL.md](./yolo-report-analyst/SKILL.md) · [README](./yolo-report-analyst/README.md)

</td></tr>
</table>

<table>
<tr><td>

### 📐 code-diagram-analyst

> *"一句话，把你的代码结构变成一张张清晰的架构图。"*

分析项目代码，自动识别并生成适合该项目的各类软件工程图表。优先使用 Mermaid 语法输出，直接在对话中渲染展示。

**它能做什么**

- **类图** — 类定义、继承、接口关系
- **ER 图** — 数据库模型、ORM、Schema
- **序列图** — API 调用、消息传递流程
- **架构图** — 多层架构、微服务拓扑
- **组件图** — 模块间依赖关系
- **流程图** — 业务逻辑、状态机
- **状态图** — 状态枚举与转换
- **依赖图** — 包依赖与模块引用
- **数据流图** — 数据处理管道

**怎么触发**

```
帮我生成类图/架构图/ER图/时序图    # 指定图表类型
分析项目结构/代码关系              # 结构分析
画出系统架构/模块依赖              # 架构可视化
生成 UML 图                      # UML 图
visualize code structure         # English
```

**📦 包含**：SKILL.md · references/mermaid-templates.md · references/language-patterns.md

→ [SKILL.md](./code-diagram-analyst/SKILL.md) · [Mermaid 模板](./code-diagram-analyst/references/mermaid-templates.md)

</td></tr>
</table>

<table>
<tr><td>

### 📖 thesis-doc-agent

> *"论文党的福音 — 代码写完了，文档交给它。"*

面向 Claude Code 及兼容 AI coding agent 的 Skill。给定项目代码库或对话历史，自动从多个软件工程维度分析项目，生成语言严谨、符合学术规范的毕业论文文档。

**它能做什么**

- **需求分析** — 功能需求、非功能需求、用例建模
- **技术架构** — 系统分层、技术选型、架构决策
- **数据库设计** — ER 图、表结构、关系映射
- **模块设计** — 核心模块划分与接口定义
- **测试方案** — 测试策略、用例设计
- **项目总结** — 技术亮点与改进方向

**核心原则**

- 强制代码溯源 — 所有代码引用标注精确文件路径
- 优先使用 Mermaid 图表辅助说明复杂结构
- 语言严谨、逻辑清晰，符合学术写作规范

**怎么触发**

```
帮我生成毕业论文文档              # 生成全套
分析一下我的项目/代码/架构         # 项目分析
帮我写需求文档/技术文档/设计文档    # 指定文档类型
generate thesis documentation   # English
```

**📦 包含**：CHANGELOG.md · CONTRIBUTING.md · README.md · README.en.md · SKILL.md · example/ · references/ · social_articles/

→ [SKILL.md](./thesis-doc-agent/SKILL.md) · [README](./thesis-doc-agent/README.md)

</td></tr>
</table>

<table>
<tr><td>

### ✍️ skill-article-writer

> *"把 Skill 链接和平台规则丢给它，文章就写好了。"*

根据指定平台 / 比赛的规则页面 URL，自动抓取规则，再结合用户提供的 Skill 信息，生成完全符合该平台投稿要求的介绍文章或参赛文章。

**它能做什么**

- 自动抓取并解析平台投稿规则
- 根据规则调整文章结构和风格
- 结合 Skill 的核心功能生成内容
- 适配多种平台格式要求

**怎么触发**

```
帮我写一篇参赛文章                # 参赛投稿
根据这个网站的规定写文章            # 规则适配
写一篇 Skill 介绍文章             # 介绍推广
照着这个链接的要求写               # 按要求输出
```

→ [SKILL.md](./skill-article-writer/SKILL.md)

</td></tr>
</table>

<table>
<tr><td>

### 🔧 skill-creator

> *"想自己写 Skill 但不知道怎么下手？这份指南就是你的起点。"*

提供创建高质量、结构化 Agent Skill 的完整指导。涵盖 Skill 的核心原则、文件结构、参考资源和最佳实践。

**它能做什么**

- 定义 Skill 的核心原则 — 简洁性、完整性、复用性
- 提供标准的 SKILL.md 模板和文件结构
- 包含参考资源示例（输出模板、工作流程等）
- 跨平台兼容指导（Claude Code、Codex、OpenCode 等）

**📦 包含**：LICENSE.txt · SKILL.md · references/ · scripts/

→ [SKILL.md](./skill-creator/SKILL.md) · [LICENSE](./skill-creator/LICENSE.txt)

</td></tr>
</table>

<table>
<tr><td>

### 🗣️ code-explainer

> *"看不懂的代码，让它给你讲明白。"*

代码解释相关技能包合集，包含多个针对不同场景的压缩包。让 Agent 能够清晰、结构化地解释各种代码，从简单片段到完整项目。

**包含包**

| 包名 | 说明 |
|---|---|
| `code-explainer.zip` | 基础代码解释技能 |
| `code-explainer-plus.zip` | 增强版代码解释 |
| `code-explainer-thesis.zip` | 论文场景代码解释 |
| `project-analyzer.zip` | 项目级分析 |
| `project-compass.zip` | 项目导航指引 |
| `repo-narrator.zip` | 仓库整体讲解 |

→ 位于 [./code-explainer/](./code-explainer/) 目录

</td></tr>
</table>

---

## 🌟 关于这个项目

这个知识库是一套我在日常开发中持续积累的 AI Skills 集合。每个 Skill 都是经过真实项目实践、确实能帮忙省时间的工具。

这里还包含了 **[LLM Wiki](./LLM-wiki.md)** 的核心理念文档 — 一种让 LLM 帮你构建和维护个人知识库的模式。如果说上面的各类 Skill 是"工具"，那 LLM Wiki 就是"方法"：让 Agent 不仅帮你回答问题，更帮你**构建可积累的知识体系**。

这些 Skill 都是我在自己项目里跑通了一段时间才搬出来开源的。如果对你有帮助，给个 ⭐ 就行。有问题或建议，欢迎提 Issue。

---

## 📦 安装方式

在 Claude Code 或 Codex 等支持 Skill 的 Agent 里，直接说：

```
帮我安装这个 skill：https://github.com/xiangshenchi/chichi-skills/tree/main/<skill-name>
```

或者手动将对应 Skill 的目录放入 Agent 的 Skills 配置路径即可。

---

<div align="center">

[MIT License](./skill-creator/LICENSE.txt) · 自由使用 / 修改 / 再分发

</div>

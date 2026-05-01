---
title: "Obsidian + Claude Code 第二大脑工作流全解析"
type: synthesis
tags: [Obsidian, Claude Code, 第二大脑, 知识管理, AI工作流, Agent Skills, LLM Wiki]
sources: [https://youtu.be/RZEb6FLZSHE]
last_updated: 2026-05-01
---

# Obsidian + Claude Code 第二大脑工作流全解析

> 基于 Serena 心心加州 的视频教程，系统梳理 Obsidian + Claude Code 搭建个人第二大脑的完整方法论。该视频于 2025 年发布，作者分享了从 Milanote 迁移到 Obsidian + AI 的完整实操经验。

---

## 一、核心理念：为什么是本地文件？

### 问题起点

作者使用了 Milanote 多年，但发现两个致命缺陷：

| 问题 | 后果 |
|------|------|
| 数据在云端 | AI 无法系统性读取 |
| 层层嵌套结构 | 阻碍 LLM 遍历和组织 |

### 关键洞察

> 如果 AI 可以读取我所有的读书笔记和视频脚本，它是不是就能够了解我这个人的思维方式？

解决方案：**Folder as an App** — 一个本地文件夹 + Markdown 文件，就是最好的知识库。不需要做 App，不需要依赖特定平台。

### 为什么是 Obsidian

- Obsidian 的**本质**：本地 Markdown 文件管理器
- 比裸文件夹多了**更好的 UI** 和**插件生态**
- **本地文件 = AI 可以直接操作**（这正是 LLM 最擅长的）

> 这一逻辑与 [[LLM_as_Knowledge_Librarian]] 中的角色分工高度一致：人类策源、LLM 编译整理。

---

## 二、两层导航架构（Token 控制核心）

这是该工作流最值得学习的设计模式，直接对应 Anthropic 设计 Claude Code 的**渐进式披露**原理。

### 第一层：根目录 `CLAUDE.md`（总地图）

```markdown
作用：
  1. 介绍你这个人（身份、目标、风格）
  2. 给出文件夹地图（什么文件夹做什么、何时读哪个）
```

Claude 每次启动强制先读此文件 — **永远不需要重新自我介绍**。

### 第二层：每个文件夹的 `instructions.md`（局部地图）

```markdown
规则：进入任何文件夹之前，必须先读该文件夹的 instructions.md

内容：
  - 文件夹的具体结构
  - 文件命名规则
  - 操作注意事项和约束
```

### Token 模型

```
Claude 视野
├── 第一层: CLAUDE.md         ← 始终加载（全局上下文）
├── 第二层: folder/instructions.md ← 按需加载（任务相关）
└── 第三层: folder/*.md        ← 精准加载（具体文件）

永远不扫描整个知识库
```

> 这与 [[RAG_vs_Wiki_Paradigm]] 中描述的 Wiki 范式优势一致：结构化的导航优于暴力检索。

---

## 三、三条自动输入管道

### 1. Obsidian Web Clipper（网页剪藏）

- Obsidian **官方**浏览器插件
- 一键保存任何网页为 Markdown
- **阅读模式**：视频页面可边看边读字幕 + 点击时间戳跳转
- 一键导出视频全部字幕内容到笔记

### 2. Apple Books Highlights（读书划线）

- 插件自动拉取 Apple Books 中所有划线段落
- 导入后可**用 Claude 对话讨论**书中内容
- 作者因此从纸质书转向 Apple Books

### 3. iPhone Action Button（闪念捕捉）

```
流程：点击按钮 → Siri 聆听 → 语音转文字 → 追加到当天 Daily Notes

适用场景：走路时、开车时、任何灵感一现的瞬间
```

Daily Notes 的 `instructions.md` 告知 AI：带时间戳的条目都是当下的碎碎念，供后续整理和二次创作。

> 这三条管道的核心设计原则：**输入成本趋近于零** → 降低坚持的门槛 → 信息源源不断流入。

---

## 四、Canvas 思维导图：AI 原生操作

作者的一个核心痛点是思维导图，在 Milanote 中这是强项。

### Obsidian Canvas 的优势

- `.canvas` 文件 = **JSON 格式** = AI 可以直接生成和编辑
- 截屏 Milanote 的图 → 发送给 Claude Code → 让它**用 Canvas 复刻**
- 这正是 LLM 最擅长的：操作结构化代码文件

> 原则：**如果数据格式是代码（JSON/YAML/Markdown），就能用 AI 开挂。**

---

## 五、Claude Code 的三种接入方式

| 方式 | 说明 | 适用场景 |
|------|------|----------|
| 直接拖拽 | 将 vault 文件夹拖入 Claude Code | 最原始、最灵活 |
| YOLO 模式 | 跳过权限确认 | 高频操作，信任度已建立 |
| Terminal 插件 | Obsidian 内嵌终端直接调 Claude | 无需切换窗口 |

### 典型工作流示例

```
用户: "帮我把最近的碎碎念整理成一篇文章"
       ↓
Claude: 读取 CLAUDE.md（我是谁、写作风格）
       → 进入 daily/ 文件夹
       → 读取 instructions.md（碎碎念格式说明）
       → 只读最近几天的 Daily Notes
       → 输出文章
```

---

## 六、Token 节省五策

| 策略 | 原理 | 实现 |
|------|------|------|
| 1. 渐进式披露 | AI 不需一次性读取全部 | 两层导航架构 |
| 2. 按需索取 | 只加载当前任务所需上下文 | instructions.md 定向引导 |
| 3. Daily Notes 上下文传递 | 新 session 只需读最近几天日志 | 每次结束让 Claude 写当日总结 |
| 4. Obsidian CLI | 用命令行代替 Claude Token 操作 | 官方 CLI 工具 |
| 5. 本地 Skills | Skills reference 文档随笔记更新 | Skills 文件直接放 vault 中 |

### 策略 3 的关键细节

> 每天打开新 Session → 当天结束让 Claude 写 Daily Notes → 下次新 Session 只读最近几天 → 立即知道之前做了什么、卡在哪里、现在该做什么。

这是用一个极小的文件（Daily Notes）实现了 session 间的**上下文连续性**，本质上是一种穷人版的长期记忆机制。

---

## 七、Skill 系统的本地化管理

作者将 Claude Skills 放在 Obsidian vault 中：

```
优势：
  1. Skills 的 reference 文档与笔记内容共存，统一维护
  2. 随着笔记增长，Skills 可以持续迭代优化
  3. 不依赖外部平台存储工作流逻辑

示例 Skills：
  - 发布博客 skill（写作风格随笔记积累而演进）
  - 视频制作 skill（剪辑流程、素材管理规则）
```

> 这与你当前的 `.claude/skills/` 目录结构是同类实践。Skills 本质上是 **让 AI 稳定输出特定格式的方法论编码**。

---

## 八、与 PhoenixluoWhouse 现有架构的对照

| 维度 | 视频方案 | PhoenixluoWhouse 方案 |
|------|----------|----------------------|
| 全局入口 | 根目录 `CLAUDE.md` | 根目录 [[CLAUDE.md]] ✓ |
| 分层导航 | 每文件夹 `instructions.md` | `CLAUDE.md` 中的路径约定 |
| 原始素材 | 未明确分区 | `raw/` 不可变层（更严格） |
| 知识输出 | 未明确分区 | `wiki/` 四区分类（更结构化） |
| 媒体资产 | 未明确分区 | `assets/` 统一管理 |
| 操作日志 | Daily Notes 方式 | `wiki/log.md` Grep-friendly 格式 |
| 内容审核 | 无 | `/lint` 死链/孤儿/冲突检测 |
| Skills | 放 vault Skills 文件夹 | `.claude/skills/` 统一管理 |

### 可借鉴改进

- **文件夹级 `instructions.md`**：当前你的系统依赖 `CLAUDE.md` 中的路径描述，可以考虑在关键文件夹添加局部的 `instructions.md` 进一步降低 AI 的 Token 消耗
- **Daily Notes 上下文传递**：视频中让 Claude 每天结束时写总结到 Daily Notes 的做法，可以在你的工作流中引入
- **Web Clipper + 自动导入**：当前 `raw/` 的素材是手动放入的，可以考虑建立自动输入管道

---

## 九、长远视角：Karpathy 的 LLM Wiki 理念

视频结尾引用了 [[Andrej Karpathy]] 的概念（这也是你整个知识库的理论基础）：

> 每天接触大量原始信息 → 用 LLM 本地结构化编译 → 用 Obsidian + AI 查看和操作 → 随时间积累越来越强的个人知识系统。

> "这不是某个 AI 公司卖给你的功能，这是一个完全你自己搭建出来的、完全本地化、完全私有的、任何 AI 都可以使用的。"

你的 PhoenixluoWhouse 正是在实践这个方向。

---

## 关联连接

- [[LLM_as_Knowledge_Librarian]] — LLM 作为知识管理员的角色分工
- [[RAG_vs_Wiki_Paradigm]] — RAG 与 Wiki 两种知识范式的对比
- [[Compounding_Knowledge]] — 知识复合增长的核心动力
- [[摘要-karpathy-llm-wiki]] — Karpathy LLM Wiki 方法论原文
- [[CLAUDE.md]] — 当前知识库的全局心智规范

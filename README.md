[![Version](https://img.shields.io/badge/version-v1.8-blue)](https://github.com/RyanYang25/brief-relay-framework)
[![License](https://img.shields.io/badge/license-MIT-green)](https://github.com/RyanYang25/brief-relay-framework/blob/main/LICENSE)
[![Language](https://img.shields.io/badge/language-Markdown-lightgrey)](https://github.com/RyanYang25/brief-relay-framework)
[![Platform](https://img.shields.io/badge/platform-agnostic-orange)](https://github.com/RyanYang25/brief-relay-framework)
[![Docs: 中文](https://img.shields.io/badge/docs-中文-blue)](https://github.com/RyanYang25/brief-relay-framework/blob/main/%E7%AE%80%E6%8A%A5%E6%8E%A5%E5%8A%9B.md)
[![Docs: English](https://img.shields.io/badge/docs-English-blue)](https://github.com/RyanYang25/brief-relay-framework/blob/main/Brief-Relay-Framework.md)

# 简报接力框架 · BRIEF Relay Framework (BRF)

> **English** — A platform-agnostic method for **cross-agent / cross-platform project context handoff**. Keep one living *BRIEF* as the single entry point, paired with a three-piece toolkit (contract header + relay pointer + resume protocol), so any AI agent on any platform can pick up exactly where the last one left off — zero-cost onboarding, zero context loss.
>
> **中文** — 用一份持续演进的「项目简报（BRIEF）」作为唯一入口，配合「契约头 + 接力棒 + 恢复协议」三件套，让任何 AI Agent 平台都能零成本接手、跨平台可迁移、项目上下文不丢失。

*Keywords: context management · agent handoff · cross-platform · LLM workflow · prompt methodology · knowledge management · AI agent collaboration · project brief*

> **本地项目空间说明（Local note，勿推送此说明至公开仓库）**：本目录是 BRF 方法论的自管理项目空间，含内部运维记录与个人 IP 参考资料，**仅限本地、禁止整体推送公开仓库**（红线见 `project-context/context/standards.md` 第三章）。对外公开交付物见 [github.com/RyanYang25/brief-relay-framework](https://github.com/RyanYang25/brief-relay-framework)；本地方法规范在 `project-context/handoffs/`（`简报接力.md` / `Brief-Relay-Framework.md`）。

---

## 是什么（Overview）

BRF 是一套**领域无关、平台无关**的项目上下文接力方法。它解决一个越来越普遍的问题：

> 当同一个项目在多个 AI Agent、多个平台、多轮会话之间流转时，上下文如何不丢失、不重复、不冲突？

它不绑定任何 IDE 或 Agent 平台，只用纯 Markdown + HTML 注释实现，因此可以被任意工具读取、被任意团队套用、被开源社区共同迭代。

**适用场景**：咨询、写作、研发、运营——一切需要多文件协作与状态跟踪的项目。

---

## 为什么（Why BRF）

- **可迁移性第一约束**：纯 Markdown + HTML 注释，不依赖任何平台的特有能力。把整个文件夹拷到 WorkBuddy / Trae / Qoderwork 等任意平台，新 Agent 读 BRIEF 顶部的契约头即可接手。
- **活的演进引擎**：BRIEF 既是接手入口，也是随项目生长的演进引擎——它不是静态的 `AGENTS.md`，而是会随项目一起更新的活系统。
- **中断安全**：靠「会话恢复协议 + 增量落盘」，最坏情况只丢失一个正在执行的步骤，不会整体失忆。

---

## 与同类方案的区别（Differentiation）

Agent 上下文接力 / 记忆是活跃方向，BRF 与三类常见路线刻意保持不同：

| 路线 | 常见形态 | BRF 的取舍 |
|------|---------|-----------|
| IDE / 平台记忆库 | Memory Bank 系、AGENTS.md 事件钩子自动改写 | 不绑定任何 IDE / 平台，纯 Markdown + 轻量脚本，拷走即用（规范 16.3） |
| 向量记忆 / 知识图谱 | embedding 检索、数据库、运行时服务 | 零运行时、零外部依赖，用目录结构 + 契约头 + 指针代替向量检索 |
| 规格驱动 / 多 Agent 框架 | 重型流程模板或代码编排框架 | 是轻量方法论而非框架：不规定开发流程，只解决"上下文如何在 Agent / 平台 / 会话间不丢、且可被信任" |

**BRF 的生态位**：领域无关、纯 Markdown、平台中立、零依赖——一套"文件怎么组织、交接怎么可信、状态怎么可验证、流程怎么按场景弹性"的约定，可单独使用，也可作为上述任何重型方案的底层上下文层。v1.7 新增「交接可信化」（验证声明 / UNKNOWN 账本 / 交接质量五问 / 文档健康门禁）；v1.8 新增「流程弹性」（接手意图分类 / 任务规模分级 / standards 宪法对齐），让大任务不丢深度、小任务不被流程压重。

---

## 核心功能（Core Features）

| # | 功能 | 作用 |
|---|------|------|
| 1 | **@context 契约头** | 接力的"握把"——声明文件角色、上下游依赖、更新时机 |
| 2 | **接力棒（downstream 字段）** | 显式声明"改完这一步，下一步该动谁"，像接力棒一棒交一棒 |
| 3 | **会话恢复协议** | 跨会话 / 跨平台接手的保险，新 Agent 第一步就能对齐状态 |
| 4 | **增量落盘** | 每步即落盘，上下文随项目生长，不依赖"中断自动同步" |
| 5 | **开箱模板** | 中 / 英文 starter，复制即用，含 BRIEF / current / standards 三件套 |
| 6 | **交接可信化（v1.7）** | 验证声明 + UNKNOWN 账本 + 交接质量五问 + 文档健康门禁，让"做完"可验证、假设不被当事实下传 |
| 7 | **流程弹性（v1.8）** | 接手意图分类 + 任务规模分级 + 接手深度路由 + standards 宪法对齐——大任务不丢深度、小任务不被流程压重 |

---

## 快速开始（Quick Start）

```bash
# 方式一：直接复制开箱模板到你的项目根目录
# 从本仓库 template/ 目录取 BRIEF.md / current.md / standards.md 三个 starter 文件
# 放到你的项目根目录与 project-context/ 下即可

# 方式二：克隆后提取模板
git clone https://github.com/RyanYang25/brief-relay-framework.git
# 进入 template/ 目录，复制所需 starter 到你的项目
```

然后按 `BRIEF.md` 顶部的「更新 SOP」填写你的项目背景，开始使用。

---

## 使用示例（Usage）

`BRIEF.md` 顶部的 `@context-root` 契约头是接力的起点：

```html
<!-- @context-root
role: 项目唯一入口 + 演进引擎
produced_by: 更新 SOP（自动/手动触发）
consumed_by: 所有 Agent 与人类接手者，每次会话开头必读
update_trigger: 任何状态/决策/文件结构变更后
upstream: context/current.md（活跃状态）、context/standards.md（约束）
downstream: context/current.md → context/standards.md → references/
-->
```

每个上下文文件（`current.md` / `standards.md`）也都带有 `@context` 契约头，新 Agent 一"摸"就知道自己该看什么、改什么、改完下一步动谁。

---

## 目录结构（Structure）

本仓库为**方法论开源包**，目录如下：

```
brief-relay-framework/
├── README.md                              ← 本文件
├── LICENSE                                ← MIT License
├── CONTRIBUTING.md                        ← 贡献指南
├── template/                              ← 开箱模板（中 / 英文 starter）
│   ├── BRIEF.md
│   ├── context/current.md
│   ├── context/standards.md
│   └── en/                                ← 英文 starter
└── 方法规范（权威交付物，根目录·去版本后缀）
    ├── 简报接力.md                          ← 方法规范（中文，当前版）
    └── Brief-Relay-Framework.md            ← Method Spec (English, current)
> 历史版本（v1.0 / v1.1 / v1.2 / v1.3 / v1.4 / v1.5 / v1.6 / v1.7）已通过 git tag 锚定，不在根目录堆副本；`git checkout v1.7` 可取回。
```

---

## 文档（Documentation）

- **方法规范（中文 · 当前 v1.8）**：[简报接力.md](简报接力.md)
- **Method Spec (English · current v1.8)**：[Brief-Relay-Framework.md](Brief-Relay-Framework.md)
- **历史版本（v1.0 / v1.1 / v1.2 / v1.3 / v1.4 / v1.5 / v1.6 / v1.7）**：通过 git tag 锚定，[`git checkout v1.7`](https://github.com/RyanYang25/brief-relay-framework/releases) 可取回，根目录不堆副本
- **开箱模板**：[template/](template/)
- **贡献指南**：[CONTRIBUTING.md](CONTRIBUTING.md)

---

## 许可证（License）

[MIT License](LICENSE) —— 自由使用、修改、分发，只需保留版权声明。

## 贡献（Contributing）

欢迎提交 Issue 与 Pull Request，共同完善这套跨平台上下文接力标准。详见 [CONTRIBUTING.md](CONTRIBUTING.md)。

---

<p align="center">
<sub>BRIEF Relay Framework · 让项目上下文在任何 Agent 之间稳稳接力 🤝</sub>
</p>

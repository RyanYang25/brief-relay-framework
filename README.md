# 简报接力框架 (BRIEF Relay Framework, BRF) v1.0

一套**跨 Agent、跨平台可迁移**的项目上下文接力方法。核心是一个始终作为「入口 + 演进引擎」的项目简报（BRIEF），配合 `@context` 契约头把「谁产、谁消费、下一步动谁」显式化，让任意一个新 Agent（WorkBuddy / Trae / Qoderwork 等）拿到项目文件夹就能零成本接手。

## 为什么需要它

- 多 Agent 平台切换频繁，平台自带记忆不互通
- 对话中断后 Agent 无法自主动作，上下文易丢
- 项目知识散落在对话里，难以沉淀与复用

## 核心机制

1. **BRIEF 作为唯一入口 + 演进引擎**
2. **`@context` 契约头**：每个上下文文件顶部声明角色 / 维护者 / 读者 / 更新触发 / 上下游接力
3. **会话恢复协议**：任何 Agent 重接手先读 BRIEF + current，比对时间戳后 reconcile
4. **增量落盘纪律**：每步完成立即写入文件，中断最坏只丢一个 in-flight 步骤

## 仓库内容

| 文件 | 说明 |
|------|------|
| `简报接力-v1.0.md` | 完整方法规范（中文，权威版） |
| `template/` | **开箱即用模板（中文）**：复制进你的项目即可套用 |
| `template/使用说明.md` | 模板使用指南（中文） |
| `template/en/` | **英文版模板**：面向国际用户，与中文模板同步维护 |
| `template/en/README.md` | 英文模板使用指南 |
| `README.en.md` | 英文简介 |
| `CONTRIBUTING.md` | 贡献指南 |
| `LICENSE` | MIT 许可证 |

## 一分钟上手

1. 把 `template/` 整个目录复制到你的项目根目录；
2. 打开 `BRIEF.md`，替换 `{项目名称}` 等占位符；
3. 状态更新时按 BRIEF 底部「更新 SOP」联动改文件，**不要只改 BRIEF.md**。

## 设计原则

- **纯 Markdown + HTML 注释**，不依赖任何平台特有能力（无 YAML frontmatter、无平台私有记忆、无"中断回调"依赖）；
- 把整个项目文件夹拷到另一个 Agent 平台，新 Agent 读 BRIEF 顶部契约头即可接手——这是本框架的硬指标。

## License

MIT

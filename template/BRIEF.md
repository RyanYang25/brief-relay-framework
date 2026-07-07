<!-- @context-root
role: 项目唯一入口 + 演进引擎
produced_by: 更新 SOP（自动/手动触发）
consumed_by: 所有 Agent 与人类接手者，每次会话开头必读
update_trigger: 任何状态/决策/文件结构变更后
upstream: context/current.md（活跃状态）、context/standards.md（约束）
downstream: context/current.md → context/standards.md → {会议记录目录}/
-->

# BRIEF.md - 项目简报

> **📌 如果你收到"更新 brief / 更新项目状态 / 同步进度"类指令**：
> 不要只改本文件！先读本文件的「更新 SOP」章节，找到对应场景的操作指引，
> 按 SOP 顺序联动更新 current.md / standards.md 等文件。

<!-- 元信息：最后更新 ｜ 更新人 ｜ 更新原因 -->
> 最后更新：YYYY-MM-DD ｜ 更新人： ｜ 更新原因：初始化

## 一、项目画像（稳定信息，几乎不变）
- **项目名称**：{项目名称}
- **目标**：{一句话说清要达成什么}
- **交付物**：{核心产出是什么}
- **关键约束**：{时间 / 资源 / 合规等限制}
- **时间节点**：{关键里程碑}

| 角色 | 姓名 | 决策权 |
|------|------|--------|
| 项目负责人 | {姓名} | ⭐⭐⭐ 最高 |
| 对接人 | {姓名} | ⭐⭐ 高 |
| 执行/顾问 | {姓名} | ⭐ 中 |

## 二、文件组织（实际目录树）
<!-- 列出你项目的真实目录结构；若已有 A01/B02/C01 之类编码前缀，直接沿用，不要另起方案 -->
```
{项目根目录}/
├── BRIEF.md
├── {会议记录目录}/
└── project-context/
    ├── context/
    │   ├── current.md
    │   └── standards.md
    ├── references/
    ├── handoffs/
    ├── .working/
    ├── changelog.md
    └── PROJECT_GUIDANCE.md（可选）
```

## 三、当前状态（一句话 + 指针）
{一句话概括当前进度，如"第一版方案完成，待反馈"} → 详见 `context/current.md`

## 四、文件索引（路由表）
- `context/current.md`：当前状态、进行中任务、近期决策
- `context/standards.md`：输出规范、协作流程、历史踩坑
- `references/`：原始材料
- `handoffs/`：已完成交付物
- `{会议记录目录}/`：会议 / 沟通记录
- `PROJECT_GUIDANCE.md`（可选）：历史决策完整记录

## 五、读取规则（渐进式披露）
1. 先读本文件（含顶部 @context-root 块）获取全局上下文
2. 再读 `context/current.md`（含 @context 块）了解当前状态
3. 按需读 `context/standards.md` 或 `references/` 下文件
4. 查历史决策按需读 `PROJECT_GUIDANCE.md`
5. 不要一次性读所有文件

## 六、会话恢复协议（跨平台接手保险）
任何 Agent 重新接手本项目（新会话 / 换平台 / 中断恢复），第一步必须：
读 BRIEF.md + context/current.md → 比对更新时间 → 若不一致先 reconcile → 再干活。
本框架不依赖"中断时自动同步"，靠恢复协议 + 增量落盘保证不丢上下文。

## 七、更新 SOP（强制遵循）
<!-- 收到"更新 brief / 状态"指令时，必须先找对应场景，再联动更新，不要只改本文件。
     完整 SOP（7 个场景）见方法规范 简报接力-v1.0.md 第八章。 -->
- 新会议纪要 → 存 {会议记录目录}/ → 更新 current.md → 同步本文件"当前状态"
- 任务状态变更 → 更新 current.md → 影响全局则同步本文件
- 重要决策 → 更新 current.md（含依据/理由）→ 同步本文件 → 记 PROJECT_GUIDANCE.md
- 新增交付物 → 存 handoffs/ → 更新 current.md → 本文件索引
- 踩坑 / 规范 → 只增不改 standards.md

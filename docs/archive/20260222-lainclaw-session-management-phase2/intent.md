# Intent: Personal Assistant Memory V2 (Persistent Memory + Compaction)

> **状态**: DRAFT
> **创建日期**: 2026-02-22
> **对应 Issue**: 开启第二阶段，聚焦记忆能力（不是会话列表管理）

## 1. 现状与背景 (Context & Motivation)

第一阶段已完成会话持久化与上下文重放，但缺少跨轮长期记忆与上下文压缩策略，长对话体验仍然局限在 `session` 转写窗口内。

OpenClaw 的思路是：先保留会话历史、再在上下文压缩时固化长期信息，给模型注入“长期记忆”而不是只靠最近几条对话。

Lainclaw 需要同方向的最小实现：
- 可选开启长期记忆；
- 简单 compaction 触发；
- 记忆结果落盘为长期文件，后续会话可注入上下文。

## 2. 核心目标 (Goals)

1. 增加 `ask --memory` 开关（每会话开关可记住）。
2. 在对话上下文中注入“长期记忆”片段（当开启 memory 时）。
3. 当会话长度增长超过阈值后执行简单 compaction：把旧对话摘要写入 `~/.lainclaw/memory/<sessionKey>.md`，并记录已压缩边界。
4. 保持第一阶段上下文重放能力不变，新增行为作为增强层。

## 3. 非目标 (Non-Goals / Out of Scope)

1. 在本阶段不做向量检索、检索式记忆读取或复杂策略语言。
2. 不引入 openclaw 的多代理树/工具级内存策略。
3. 不做远端 gateway/多端同步。
4. 不在本阶段添加 `sessions` 管理命令（list/history/clear）。

## 4. 验收标准 (Acceptance Criteria)

1. `lainclaw ask --memory <input>` 在会话内开启长期记忆模式；后续同会话可读到同一 `sessionKey` 关联的记忆文件。
2. 当会话未开启 memory 时不触发记忆写入，行为与第一阶段一致。
3. 当 memory 开启且累计消息量达到阈值时，系统写入 `~/.lainclaw/memory/<sessionKey>.md` 的摘要块，并更新会话索引中的 compaction 元数据。
4. `ask` 输出结果包含 memory 相关状态（memory 是否启用、是否已执行 compaction）。
5. 对于同一 `sessionKey`，切换 sessionId（`--new-session`）时，长期记忆文件可复用。

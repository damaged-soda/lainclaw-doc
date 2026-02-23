# Intent

## 背景

- openclaw 的 heartbeat 与 ask 都在构造“模型上下文”时各自实现，导致上下文重复、能力一致性差。
- 现有 `HEARTBEAT.md` 已切换为文件任务源，旧有结构化规则配置已移除（`heartbeat-rules.json` 已删除），可以直接按文件内容和工作区元信息构造提示。

## 目标

- 把“工作区上下文构造”抽成可复用模块（含 `workspaceDir / now / 关键文件角色 / 顶层未知条目`）。
- heartbeat 与普通 ask 在进入模型前共享同一套上下文基础内容。
- 通过 `RequestContext.systemPrompt` 把上下文一次性注入 `codexAdapter`。

## 非目标

- 不改造工具命名规范或 `openai-codex` 外的路由策略。
- 不恢复 `heartbeat-rules.json` 的兼容层。

## 验收标准

- `runAsk(... )` 在 openai-codex 路径下会在请求上下文注入 workspace 上下文（含时间）。
- heartbeat 继续保留规则判定逻辑，但 rule prompt 由共享上下文模块产出。
- 不改 `cli` 入口与现有工具权限策略。

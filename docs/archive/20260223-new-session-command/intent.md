# Intent: `/new` 开启新会话

## 背景
当前 `lainclaw` 已支持 `--new-session` 与 `getOrCreateSession(..., forceNew)`，但网关/客户端消息通道中没有 `/new` 这类会话命令。用户在对话里输入 `/new` 仍会被当作普通文本交给模型处理。

## 目标
- 当 `lainclaw` 收到输入文本为 `/new`（忽略首尾空白）时，立即在当前 `sessionKey` 下创建新 `sessionId`。
- 保持会话 `sessionKey` 不变，仅切换 `sessionId`，并返回新会话信息。
- 该命令应绕过模型推理，直接执行会话重置。

## 非目标
- 不修改 `--new-session` 的现有 CLI 行为。
- 不引入对 `/new` 的工具链/模型路由改造。
- 不改动 SOT 文档（仅提案阶段）。

## 验收标准
1. 对话输入为 `/new` 时，应返回成功结果，并返回新的 `sessionId`。
2. `/new` 输入后，接续该 `sessionKey` 的下一条 `ask` 应使用新的 `sessionId`，不复用上一轮上下文。
3. 非 `/new` 的输入行为保持原有，不受影响。

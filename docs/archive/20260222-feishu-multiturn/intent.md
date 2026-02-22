# 意图（Intent）

## 背景

当前 `feishu` 网关能接收并回传消息，但 `runAsk` 调用只透传了 `sessionKey`，未传 `provider/profile/tool` 等上下文，命中 `pipeline` 默认 `stub` 路径，导致结果表现为回显。

## 目标（Goals）

1. 在 Feishu 网关端实现对模型的真实调用（默认优先复用 openai-codex）。
2. 保持每个飞书用户的多轮对话记忆（沿用现有 `sessionKey` 机制）。
3. 提供可配置的飞书网关调用参数（provider/profile/tools/memory），支持按环境变量、启动参数和持久化配置三层读取。
4. 在调用失败时给飞书用户清晰可操作的错误提示。

## 非目标（Non-Goals）

1. 不在本次先引入群聊路由、提及规则、附件/卡片流式回显等 openclaw 全量能力。
2. 不改变 `cli ask` 的交互主语义（仍由单独命令保留）。
3. 不对认证流程本身做重构（仅复用现有 auth profile 查找与持久化机制）。

## 验收标准（Acceptance Criteria）

1. 已登录 openai-codex 后，`feishu` 消息返回内容不再是 `[stub-echo]`。
2. 同一 `openId` 的连续飞书消息命中同一 `sessionId`，第二条消息可参考第一条上下文。
3. `feishu` 支持通过 `--provider openai-codex`、`--profile <id>` 或环境变量覆盖模型参数。
4. 无可用 profile 时，网关不静默失败，能返回可操作提示并在日志输出错误。

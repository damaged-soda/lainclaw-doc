# Intent

## Why

在当前 `lainclaw` 的基础上，下一步不再先追求多渠道/网关化，而是优先把单点 CLI 变成“能调用工具”的可用助手，以提升真实工作产出效率。

## Goals

1. 在 CLI-only 前提下建立可运行的工具调用能力。
2. 明确工具调用的最小合同（工具发现、可执行工具、失败提示与审计日志字段）。
3. 保留并强化现有会话与记忆闭环，确保工具调用不破坏 ask 流程。
4. 完成“工具能力”的验收可证明材料，后续可再平滑扩展到更多工具。

## Non-Goals

1. 暂不接入 OpenClaw 的多渠道网关（WhatsApp/Telegram/Slack 等）。
2. 暂不做常驻 gateway、daemon 服务。
3. 暂不实现语音、Canvas、浏览器自动化等高级工具。

## Acceptance Criteria

1. CLI 输出包含工具上下文（`toolCalls`/`toolResults`）且可读。
2. 至少有一个可调用的默认工具（例如 `time.now`、`echo`）并可在一次 `ask` 中被触发。
3. 工具调用失败时有明确错误码与恢复建议，不阻塞 `ask` 的基础返回格式。
4. 会话与记忆仍保持可持续：同一个 `sessionKey` 可跨次保留上下文。
5. `lainclaw tool list` 与 `lainclaw tool info <name>` 可用于工具可见性与可操作性调试。

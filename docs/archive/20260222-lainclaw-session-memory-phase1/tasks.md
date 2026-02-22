# Implementation Plan: Session Memory V1 (Persistent Session + Replay)

> **关联 Spec**: `spec_delta.md`  
> **执行状态**: [x] In Progress

## Phase 1: 基础上下文持久化

- [x] [SOT] 建立会话目录与会话清单读取/创建能力（`~/.lainclaw/sessions/sessions.json`）
  - 备注：新增 `src/lainclaw/src/sessions/sessionStore.ts` 并通过 `getOrCreateSession` 管理 `sessionId` 与 `session` 映射。
- [x] [SOT] 实现会话转写追加能力（`~/.lainclaw/sessions/<sessionId>.jsonl`）
  - 备注：新增 `appendSessionMessage`，每次 `ask` 成功后追加 user + assistant 条目。
- [x] [SOT] `ask` 命令新增 `--session` 与 `--new-session` 参数
  - 备注：`src/lainclaw/src/cli/cli.ts` 已新增参数解析并透传。
- [x] [SOT] gateway 加入上下文注入，并将 `sessionKey/sessionId` 透传到结果
  - 备注：`runAsk` 已在 `src/lainclaw/src/gateway/askGateway.ts` 注入历史消息并回写到输出。

## Phase 2: 运行时兼容与输出

- [x] [ARCHIVE-ONLY] 更新 stub/codex adapter 对 messages 的兼容读取
  - 备注：`src/lainclaw/src/adapters/stubAdapter.ts` 和 `codexAdapter.ts` 已读取 `context.messages`。
- [x] [ARCHIVE-ONLY] 更新 `src/lainclaw/README.md` 的 `ask` 使用说明，补充 `--session` 示例
  - 备注：新增会话模式示例，强调 `sessionKey/sessionId` 输出。

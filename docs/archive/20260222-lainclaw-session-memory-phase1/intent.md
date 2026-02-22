# Intent: Personal Assistant Session Memory V1 (Persistent Session + Context Replay)

> **状态**: DRAFT  
> **创建日期**: 2026-02-22  
> **对应 Issue**: 继续 openclaw 能力对齐

## 1. 现状与背景 (Context & Motivation)

当前 `ask` 是单轮交互，跨次调用不会复用上下文，个人助理体验不足。  
OpenClaw 通过会话持久化与上下文重放实现持续对话，当前 Lainclaw 尚未具备同类能力。

## 2. 核心目标 (Goals)

1. 建立可持久化的 `session` 与 `session transcript`，支持同一 `sessionKey` 下继续对话。
2. 在 `runAsk -> gateway -> pipeline` 流程中注入最近上下文，让模型可见历史消息。
3. 记录会话内 user/assistant 消息转储，供后续上下文回放与后续 memory 压缩使用。

## 3. 非目标 (Non-Goals / Out of Scope)

1. 复杂记忆压缩（summary/compaction）与长期 MEMO 文件写入。
2. 多模型并行路由或跨会话搜索索引。
3. UI 侧会话管理界面（仅 CLI）。

## 4. 验收标准 (Acceptance Criteria)

1. `ask --session <name> <input>` 在第二次调用时可复用同一会话上下文。
2. 首次调用会自动创建会话记录并落盘；可选 `--new-session` 触发新会话。
3. 每次成功执行在 `~/.lainclaw/sessions/<sessionId>.jsonl` 追加 user 与 assistant 两条转写记录。
4. `gateway/pipeline` 在运行时仍保持当前默认行为：无 provider 时走 stub；`openai-codex` 路径不回退。
5. `lainclaw ask` 输出返回包含 `sessionKey` 与 `sessionId`，可用于定位会话。

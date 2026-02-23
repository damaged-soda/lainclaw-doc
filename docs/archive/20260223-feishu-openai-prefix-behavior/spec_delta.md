# Spec Delta: 飞书/统一调用链去除默认 openai-codex 前缀

## ADDED Requirements

- [SOT] `ask` 及其网关转发流程返回给用户端的最终文本默认不得包含 `openai-codex` 调试性前缀，除非显式打开调试开关。
- [ARCHIVE-ONLY] 为调试场景增加环境变量开关 `LAINCLAW_CODEX_PREFIX_RESPONSE`，支持 `true/1/yes/on` 打开 `openai-codex` 前缀显示，保持 `false` 或未设置为默认关闭。

## MODIFIED Requirements

- [SOT] `src/lainclaw/src/adapters/codexAdapter.ts` 中 `runCodexAdapter` 的返回 `result` 拼装行为改为“默认无前缀”。
- [SOT] 飞书网关继续透传 `runAsk().result`，前缀显示由适配器侧约束，不在网关侧二次拼接。

## REMOVED Requirements

- [ARCHIVE-ONLY] 去除 `openai-codex` 返回默认前缀的硬编码行为，改为可配置开关控制（默认关闭）。

# Tasks: 飞书/统一调用链默认去除 openai-codex 响应前缀

- [x] 在 `src/lainclaw/src/adapters/codexAdapter.ts` 中修改 `runCodexAdapter` 的 `result` 前缀拼接逻辑，默认返回纯文本（无 `[openai-codex:<profileId>]` 前缀）。
  - 已完成：添加布尔解析与条件前缀分支，默认值返回空前缀。
- [x] 增加调试开关 `LAINCLAW_CODEX_PREFIX_RESPONSE`，仅在显式开启时带回调试前缀。
  - 已完成：支持 `true/1/yes/on` 生效，其他值与未设置默认关闭。
- [x] 补充与 `src/lainclaw` 相关文档（如 README）说明新开关及默认行为。
- [x] 手工或自测一次 `openai-codex` 飞书路径，确认默认不再出现方括号前缀并可复现开启开关时带回前缀。
  - 验收命令：`node --loader /tmp/lainclaw-loader.mjs /tmp/check-codex-prefix.mjs`
  - 结果：`default: 在的！有什么我可以帮你？`、`with-prefix: [openai-codex:5cfda8d7-c861-4ffa-9025-4985400aec5a] 在的！有什么我可以帮你？`

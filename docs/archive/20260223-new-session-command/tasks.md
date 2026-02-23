# Implementation Tasks: `/new` direct session command

> **关联 Spec**: `spec_delta.md`

- [x] [SOT] 在 `src/lainclaw/src/gateway/askGateway.ts` 中识别 `/new` 作为会话命令入口（trim 后精确匹配）。
  - 备注：在 `runAsk` 顶部增加 `/new` 快路径分支。
- [x] [SOT] `/new` 分支直接执行 `getOrCreateSession` 的 `forceNew: true`，构造无需调用 pipeline 的成功回包（`sessionKey/sessionId/route/stage/result`）。
  - 备注：返回 `session` 命令语义下的成功结果与新 `sessionId`。
- [x] [SOT] 直接返回路径不应产生 `toolCalls/toolResults/toolError`，并保持 `sessionContextUpdated` 为 false。
  - 备注：`toolCalls/toolResults/toolError/promptAudit` 均不填充。
- [x] [SOT] 保留对非 `/new` 输入走既有 `manualToolPath / autoToolPath / single_pass` 流程。
  - 备注：`/new` 分支前提前返回，避免修改其余处理链路。
- [x] [ARCHIVE-ONLY] 补充 `/new` 验收手册（`docs/acceptance/ask-new-command-acceptance.md`）与索引（`docs/acceptance/README.md`）。

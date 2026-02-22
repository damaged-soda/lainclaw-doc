# 变更草案（Spec Delta）

## ADDED Requirements

- [SOT] 飞书网关命令必须支持以下可配置项：
  - provider（默认 `openai-codex`）
  - profileId（可空；缺省走 active profile）
  - withTools（默认可配置，避免飞书侧未确认工具可用时默认行为不一致）
  - toolAllow（`--tool-allow <tool1,tool2>`，空值表示默认允许）
  - toolMaxSteps（`--tool-max-steps <N>`，`N>=1`）
  - memory（默认 false，可配置）
  - 新增配置源优先级：CLI args > 环境变量 > `~/.lainclaw/feishu-gateway.json`。
- [SOT] 飞书网关的 `--tool-allow` 与 `--tool-max-steps` 应透传到 `runAsk`（并沿用同 `ask` 命令的合法性约束），与 `withTools` 一起决定工具执行边界与次数上限。
- [SOT] 引入统一入口 `gateway` 命令，并通过 `--channel <name>` 选择实现；当前仅支持 `--channel feishu`。
- [SOT] 保留 `feishu` 命令作为兼容入口，等价于 `gateway --channel feishu`，但新文档和推荐路径使用 `gateway`。
- [SOT] `src/lainclaw/src/channels/feishu/server.ts` 不再承载 provider/工具权限提示/错误提示语义；这些策略下沉到 `src/lainclaw/src/cli/cli.ts`（网关层）统一处理，并通过回调注入。
- [SOT] 在 `feishu` 启动参数中，新增 `--provider <provider>`、`--profile <profileId>`、`--with-tools/--no-with-tools`、`--memory/--no-memory` 等选项，透传到 `runAsk`。
- [SOT] 在 `~/.lainclaw/feishu-gateway.json` 中持久化飞书网关的模型调用配置（如 provider/profile/withTools/memory），并在下次启动自动回读。

## MODIFIED Requirements

- [SOT] `src/lainclaw/src/cli/cli.ts` 新增统一网关入口 `gateway --channel feishu`；`src/lainclaw/src/channels/feishu/server.ts` 继续保持 `sessionKey = feishu:dm:<openId>` 的现有多轮机制。
- [CANDIDATE] 后续按相同模式补齐更多 `--channel` 实现（如 webhook、dingtalk）；当前仍以 `feishu` 首发。
- [SOT] `src/lainclaw/src/channels/feishu/server.ts` 应保持 `sessionKey = feishu:dm:<openId>`，并在日志/错误提示中保留 `sessionKey` 与 `sessionId` 以便排障。
- [SOT] `src/lainclaw/src/channels/feishu/server.ts` 在 `runAsk` 失败（如未登录、provider 不支持、模型调用超时）时，向用户返回可执行建议（例如提示执行 `lainclaw auth login openai-codex` 或管理员补齐配置）。
- [SOT] `src/lainclaw/src/channels/feishu/config.ts` 新增 provider/profile/withTools/memory 的解析函数与持久化字段；CLI 与环境变量优先于文件值。
- [ARCHIVE-ONLY] 统一错误返回为带 `requestId` 的结构化文案（供后续运维脚本抓取）。
- [ARCHIVE-ONLY] 为飞书消息体加上发送者标签前缀（如 `user:<openId>:`）以降低上下文歧义。

- [CANDIDATE] 在未来增量中按 openclaw 的 `root_id` 思路增加 group/topic 会话隔离；当前阶段仅保留 DM 回路。

## REMOVED Requirements

- [ARCHIVE-ONLY] 暂不移除现有 `session`、`memory` 功能。

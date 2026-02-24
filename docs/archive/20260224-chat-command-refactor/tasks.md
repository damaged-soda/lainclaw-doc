# 执行清单（PROPOSAL）

- [x] 评审并确认本 `spec_delta.md` 中 `[SOT]` 与 `[ARCHIVE-ONLY]` 划分是否符合你的迁移策略。
- [x] 在实现前确认 `ask` 全量移除边界：
  - 对外 CLI 不再暴露 `ask`。
  - 仅允许在内部变量、测试快照中保留历史文本（如确有必要）。
- [x] 在代码层按任务粒度拆解执行清单：
  - [x] `src/lainclaw/src/cli/cli.ts`：新增/改造 `agent` 命令分支；移除 `ask` 命令分支；更新 help/usage 示例。
  - [x] `src/lainclaw/src/gateway/askGateway.ts`：重命名为 `gateway.ts`；必要时将主入口从 `runAsk` 切为 `runAgent`，并更新调用关系。
  - [x] `src/lainclaw/src/channels/{feishu,local}/server.ts` 与 `src/lainclaw/src/heartbeat/runner.ts`：更新网关入口调用为 `gateway` 命名链路。
  - [x] `src/lainclaw/README.md`：命令示例主入口切换为 `agent`，并移除 `ask` 对外说明。
- [x] 逐项提交并在 `tasks.md` 打钩：每个文件变更完成后立即记录影响面。

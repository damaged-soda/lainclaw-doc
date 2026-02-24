# 规格变更草案（提案）

## ADDED Requirements

- [SOT] 在 CLI 合同中将交互主命令定义为 `agent`：`lainclaw agent` 作为用户唯一对外入口，参数与原 `ask` 行为等价（`--provider`, `--profile`, `--session`, `--new-session`, `--memory`, `--with-tools`, `--tool-allow`, `--tool-max-steps`）。`agent` 结果输出与退出码与原行为一致。
- [SOT] 从对外命名上移除 `ask`：`ask` 命令不作为兼容入口提供，所有用户文档与帮助信息不再将其列为可见入口。
- [SOT] 在对外文档层（`docs/sot/overview.md`, `docs/sot/architecture.md`）将“当前 MVP 对外入口”从 `ask <input>` 改写为 `agent <...>`，并在实现细节说明中明确当前命名已完成收敛（无兼容要求）。

## MODIFIED Requirements

- [ARCHIVE-ONLY] 在网关层将 `askGateway` 重命名为 `gateway`（含文件与主入口导出）。如需函数级区分，可同步将 `runAsk` 改为 `runAgent`；不要求保留旧符号。
- [ARCHIVE-ONLY] 将 CLI 内部解析路径从 `parseAskArgs` / `ASK...` 命名迁移为 `parseAgentArgs` / `AGENT...` 或会话语义字段，并维持参数行为与错误码兼容。
- [ARCHIVE-ONLY] 为审计与日志文案加入与会话一致的中性术语（如 `agent_session`、`conversation`），并去除对 `ask` 命令入口的记录。
- [CANDIDATE] 在 `docs/acceptance` 中新增/更新 `agent` 命令验收流程，覆盖：命令发现、参数回归、会话状态持久化、网关入口映射与退出码一致性。

## REMOVED Requirements

- [SOT] `ask` 命令作为文档/使用说明中的公开入口。
- [SOT] `ask` 到 `chat` 的兼容分支与迁移提示文案。

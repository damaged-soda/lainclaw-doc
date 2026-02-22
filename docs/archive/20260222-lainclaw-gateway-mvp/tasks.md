# Tasks: Gateway MVP scaffold (proposal stage)

- [x] [SOT-REQ-1] 在 `src/lainclaw/src/cli` 增加参数解析与路由：支持 `--help`, `ask <input>`，并保留兼容入口入口行为。
  - 完成：新增 `src/lainclaw/src/cli/cli.ts` 并接入 `--help`、`ask`、`--version`。
- [x] [SOT-REQ-2] 在 `src/lainclaw/src/gateway` 增加网关编排入口：进行输入校验、构建 `RequestContext`、调用 pipeline。
  - 完成：新增 `src/lainclaw/src/gateway/askGateway.ts`。
- [x] [SOT-REQ-3] 在 `src/lainclaw/src/pipeline`/`adapters` 增加 stub execution provider，输出结构化、可复用的执行结果。
  - 完成：新增 `src/lainclaw/src/pipeline/pipeline.ts` 与 `src/lainclaw/src/adapters/stubAdapter.ts`。
- [x] [ARCHIVE-ONLY] 定义共享上下文类型（输入、输出、阶段日志）并在 modules 间传递。
  - 完成：新增 `src/lainclaw/src/shared/types.ts`。
- [x] [ARCHIVE-ONLY] 更新 `src/lainclaw/src/index.ts` 作为 CLI 主入口，接入新 CLI 路由。
  - 完成：`src/lainclaw/src/index.ts` 改为调用 `runCli()`。
- [x] [ARCHIVE-ONLY] 更新 `src/lainclaw/README.md`：新增 gateway MVP 的启动示例。
  - 完成：补充 `ask` 命令与 `bootstrap` 流程示例。

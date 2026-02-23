# Implementation Tasks for WIP: 20260223-local-channel-acceptance

- [x] 定义 `local` 通道验收目标并确认只用于本地闭环，不改动在线生产行为（intent/spec delta 对齐）。
  - 备注：2026-02-23 已在 WIP 文档与 `docs` 约定路径内完成。
- [x] 在 `gateway` 参数分派中放开 `--channel local`，并保留对 `feishu` 的默认/原有行为。
  - 备注：已在 `src/lainclaw/src/cli/cli.ts:787` 及 `parseGatewayArgs` 完成解析分支。
- [x] 实现 `local` 通道 start/status/stop 的同一生命周期（含 `--daemon`、`--pid-file`、`--log-file`）。
  - 备注：已在 `src/lainclaw/src/cli/cli.ts:1703` 增加 `runLocalGatewayService` 并复用状态文件/子进程模型。
- [x] 实现本地通道消息处理逻辑（离线输入、离线回执、无外网依赖）。
  - 备注：已使用 `src/lainclaw/src/channels/local/server.ts` 的 `runLocalGatewayServer` 处理 inbox/outbox JSONL 并落库 `runAsk` 响应。
- [x] 制定本地隔离验收模板（支持独立 HOME/state/log）。
  - 备注：`gateway` `--channel local` 的 `--pid-file/--log-file` 作为实例级隔离入口，state/log 使用通道名区分，避免打扰线上实例。
- [x] 输出结果观测口径（statePath/logPath/pid/回执路径）。
  - 备注：status 命令返回统一 JSON 状态，local 回执通过 outbox JSONL 直接观测。

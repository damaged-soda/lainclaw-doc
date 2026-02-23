# Specification Delta: Lainclaw Gateway Service Mode (Channel-Agnostic)

## ADDED Requirements

- [SOT] `lainclaw gateway` 改为显式子命令模式（不再保留旧开关语义）：
  - `lainclaw gateway start ...`：启动网关；可选 `--daemon` 持久化到后台并立即返回。
  - `lainclaw gateway status ...`：打印当前服务状态（running/stopped、pid、channel、state/log 路径、启动时间）。
  - `lainclaw gateway stop ...`：停止已在运行的后台网关进程。
- [SOT] `--channel <channel>` 作为可选配置项，默认值为 `feishu`，仅用于服务命名与配置选择，不作为命令入口语义的主导项。
- [SOT] 不再支持 `gateway --status` 与 `gateway --stop` 的旧式开关写法；所有控制动作统一走 `gateway status` 与 `gateway stop` 子命令。
- [SOT] 新增 `gateway config` 配置子命令：
  - `gateway config set ...`：将网关运行参数持久化到 `~/.lainclaw/` 下对应的频道配置文件（默认 `feishu-gateway.json`，`--channel` 用于显式选择频道）作为下次启动默认值。
  - `gateway config show`：打印当前已持久化配置（敏感字段需脱敏显示）。
  - `gateway config clear`：清理网关持久化配置文件。
- [SOT] 启动服务时生成并持久化 service state（`~/.lainclaw/service/feishu-gateway-service.json`）：
  - 默认路径按通道维度区分（如 `~/.lainclaw/service/<channel>-gateway-service.json`，当前实现可继续保持 `feishu` 命名）。
  - 字段至少包含 `channel, pid, startedAt, command, statePath, logPath, argv`。
  - 作为 `--daemon` 启动的幂等保护与状态查询依据。
- [SOT] 后台启动流程需复用现有网关启动入口（当前代码中的 `runFeishuGatewayServer`/`runFeishuGatewayWithHeartbeat`）：
  - 启动后的 child 进程仅作为网关主循环，不应重复执行管理层 spawn 逻辑。
  - child 模式通过内部标记（如 `--service-child`）区分，避免递归重启。
- [SOT] 状态查询与停止逻辑遵循进程检查：
  - `status/stop` 必须判断 PID 是否真实存活；若文件存在但进程不存在，应回收陈旧 state 并按未运行处理。
- [CANDIDATE] 启动前提供凭据兜底：
  - 当 `--app-id/--app-secret` 未提供且缓存配置为空或明显占位值（例如短值、纯数字、常见测试占位词）时，应在启动前给出明确错误与清理建议，而不是进入 websocket 重试循环。

- [SOT] `printUsage()` 需新增上述服务化命令用法与示例。
- [CANDIDATE] 未来版本可增加 `--restart` 与 crash 重启策略（目前只实现手工 start/stop/status）。

## MODIFIED Requirements

- [SOT] `parseGatewayArgs` / `parseFeishuServerArgs` 需识别服务化相关选项并返回到网关运行逻辑。
- [SOT] `runFeishuGatewayWithHeartbeat` 的入口行为改为：
  - 前台/子进程下继续运行完整网关循环；
  - `--daemon` 分支先持久化服务 state 并 `spawn` 后台 child，主进程退出。
- [ARCHIVE-ONLY] 日志文件路径默认值与文件名风格可调整，不作为长期对外契约。

## REMOVED Requirements

- [CANDIDATE] 无。

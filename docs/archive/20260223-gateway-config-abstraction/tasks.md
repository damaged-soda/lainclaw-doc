# Tasks: Gateway Config Namespace Abstraction Refactor

- [x] [ARCHIVE-ONLY] 在 `src/lainclaw/src/channels/feishu/config.ts` 定义通用网关配置模型（`GatewayConfigStore`）与 I/O 能力：`loadGatewayConfig`、`persistGatewayConfig`、`clearGatewayConfig`。
- [x] [ARCHIVE-ONLY] 以 `gateway.json` 为单文件模型实现 `default` 与 `channels` 分层：字段优先级为 `runtime args/env > channel override > default`。
- [x] [ARCHIVE-ONLY] 将 `gateway config set/show/clear` 的 `--channel` 语义改为“分层键选择器”：无 `--channel` 写 `default`，`--channel X` 写 `channels.X`。
- [x] [ARCHIVE-ONLY] 在 `gateway config show --channel X`、`gateway start --channel X` 输出/使用 `merge(default, channels.X)` 的结果，并标注字段来源（default / override）。
- [x] [ARCHIVE-ONLY] 补充配置语义验收点：`provider` 仅允许落在 `default`，`gateway config set --channel <name> --provider` 不应允许；`gateway config show --channel <name>` 仅可在 `default` 源看到 provider。
- [x] [ARCHIVE-ONLY] `gateway start/stop/status` 的服务态持久化改为 `~/.lainclaw/service/gateway-service.json`，以 `channel` 作为状态条目索引。
- [x] [ARCHIVE-ONLY] 保留历史配置的只读迁移路径：引入 `gateway config migrate --dry-run`（或同等命令）输出 `gateway.json` 合并草稿，不做旧文件删除。
- [x] [ARCHIVE-ONLY] 更新 `src/lainclaw/README.md`：文案改为单文件网关配置模型，并补充“确认后手动清理历史文件”步骤。
- [x] [ARCHIVE-ONLY] 更新 `docs/sot/architecture.md` 对 `gateway config` 与服务状态契约的描述，去除 `<channel>-gateway.json` / `feishu-gateway.json` 的契约化表述。
- [x] [ARCHIVE-ONLY] 逐条对应 `spec_delta.md` 做实现复核：如实现中出现偏差，先回写 spec_delta 后再改代码。

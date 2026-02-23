# Spec Delta: Gateway Config Namespace Abstraction

## ADDED Requirements
- 无（本轮不新增对外新增功能）。

## MODIFIED Requirements
- [SOT] `gateway` 持久化配置文件改为单文件 `~/.lainclaw/gateway.json`；支持 `default` 与 `channels` 两层：  
  - `default`: 所有通道共享的网关默认配置；  
  - `channels`: 以通道名为 key 的覆盖配置。  
- [SOT] `gateway config set/show/clear` 的 `--channel` 语义改为“选择分层路径”，而非“选择持久化文件名”：  
  - 无 `--channel`：读写 `default`；  
  - 有 `--channel X`：读写 `channels.X`。  
- [SOT] `provider` 作为网关级稳定参数，仅允许存在于 `default`，不得由 `channels.<name>` 覆盖；`gateway config set --channel <name> --provider ...` 一律禁止。  
- [SOT] `gateway config show --channel X` 与 `gateway start --channel X` 使用 `merge(default, channels.X)` 作为实际运行配置，字段级后者覆盖前者。
- [SOT] `gateway config clear --channel X` 清理 `channels.X`；`gateway config clear` 清理 `default`；仅在 `gateway config clear --all`（如新增）时可清空全量配置（默认不新增强制删除历史行为）。  
- [SOT] 网关服务状态文件改为 `~/.lainclaw/service/gateway-service.json`，以 `channel` 作为状态条目 key 保存实例快照（`pid/statePath/logPath/argv/startedAt` 等），避免 `service/<channel>-gateway-service.json` 的文件名耦合。
- [ARCHIVE-ONLY] 在 `src/lainclaw/src/channels/feishu/config.ts` 实现通用网关配置模型（`loadGatewayConfig`/`persistGatewayConfig`）并在 `cli.ts` 替换引用。
- [ARCHIVE-ONLY] 在内部保留短期兼容读取工具（可选）以支持老 JSON 合并草稿生成；不作为运行时主路径。
- [ARCHIVE-ONLY] 更新 `src/lainclaw/README.md` 与 `gateway` 命令帮助文本：明确 `gateway` 是单文件配置模型，`channel` 是运行时分层键，不再提示 `<channel>-gateway.json` 为契约文件名。
- [ARCHIVE-ONLY] 更新 `docs/sot/architecture.md`：移除“`~/.lainclaw/<channel>-gateway.json`”与 `feishu-gateway.json` 契约叙述，改为 `gateway.json` 与通道分层视图。
- [ARCHIVE-ONLY] 增加/更新迁移说明文档：手动把历史 `~/.lainclaw/<channel>-gateway.json` 与 `~/.lainclaw/gateway.json` 的关系、字段映射和清理时机，确保数据不丢失。

## REMOVED Requirements
- 无（本轮不删除现有对外功能）。

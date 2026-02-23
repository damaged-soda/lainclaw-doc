# Intent: 将 gateway 配置收敛为单文件并解耦 channel 运行时命名

## Why
`gateway` 是统一能力，不应与具体通道共享文件名语义。
当前 `~/.lainclaw/<channel>-gateway.json` 的做法把通道当成了配置模型主键，导致：
- 文件名语义与网关职责混淆（看起来像 `feishu` 业务专属文件）；
- 默认值复用不自然，未来多 channel 扩展会出现重复与不一致；
- 服务态路径继续按通道分片，仍保留了历史耦合点。

## Goals
1. 使用 `~/.lainclaw/gateway.json` 作为网关持久化单源，支持 `default` 与 `channels` 分层。
2. 保持 `channel` 只作为运行时选择器：`gateway start --channel <name>`、`gateway config --channel <name>` 都只选择配置分层，不决定文件名。
3. 运行态状态文件改为 `~/.lainclaw/service/gateway-service.json`，并在文件内按通道记录状态（保留 channel 隔离的实例信息）。
4. 迁移期不自动覆盖或清理历史 json：在用户确认运行正常后再手动清理。

## Non-Goals
1. 不新增新的 gateway 运行特性（鉴权、配对、工具、模型调用保持不变）。
2. 不提供自动删除历史配置文件的行为。
3. 不引入额外兼容读取策略：运行时默认不把旧的 `*-gateway.json` 当作主配置来源。
4. 不修改 auth/session/pairing 等非网关配置域的存储模型。

## Acceptance Criteria
1. `gateway.json` 支持以下模型并可读写：  
  - `default`：共享默认网关配置；  
  - `channels[<channel>]`：按通道覆盖，缺省回退 `default`。  
  - `provider` 为网关级字段，固定归属 `default`。  
2. `gateway config set`、`gateway config show`、`gateway config clear` 与 `gateway start` 对同一 `channel` 使用 `merge(default, channels[channel])`（后者覆盖前者）。
3. `gateway status/stop` 默认持久化状态文件为 `~/.lainclaw/service/gateway-service.json`，内部状态通过 `channel` 字段索引。
4. 提供手动迁移说明：由旧的 `~/.lainclaw/feishu-gateway.json` 等生成 `gateway.json` 的合并草稿；只读、预览、确认后才可清理旧文件。
5. 所有用户可见文案不再暗示 `feishu-gateway.json` 是主配置契约。

# Intent: 统一飞书配对配置到 gateway.json 并清理旧配对文件

## Why
当前本地配置目录存在 `~/.lainclaw/feishu-pairing.json` 与 `~/.lainclaw/feishu-allowFrom.json` 两类旧文件，与 `gateway.json` 单文件配置模型并存时，导致配置归属不一致。

## Goals
1. 将飞书配对状态与 allowlist 统一落到 `~/.lainclaw/gateway.json` 的 `pairing` 分区。
2. 清理旧的 `*-pairing.json` 与 `*-allowFrom.json` 文件，避免重复来源。
3. 保留一次离线备份，便于回退。

## Non-Goals
1. 不改配对策略语义（`pairingPolicy`、TTL、max pending、审批流程）。
2. 不引入新的频道或新的持久化 schema。

## Acceptance Criteria
1. `~/.lainclaw/gateway.json` 存在 `pairing.version` 与 `pairing.channels`。
2. 旧 `feishu-pairing.json`、`feishu-allowFrom.json` 清理后，运行时配对读写不依赖这些旧文件。
3. `gateway config` 与 `gateway start --channel feishu` 的链路验证通过。

# Gateway Multi-channel Service Delta

## ADDED Requirements
- [CANDIDATE] 在 `src/lainclaw/src/cli/cli.ts` 增加运行时解析结果中的 `channels: GatewayChannel[]`，用于记录一次 `gateway start` 的通道集合。
- [CANDIDATE] 在 `gateway start` 的多通道场景增加 `runGatewayServiceForChannels` 调度器，统一处理前台并发启动与 daemon 启动。

## MODIFIED Requirements
- [SOT] `parseGatewayArgs` 的 `gateway start` 分支应支持重复 `--channel`，并将结果作为 `channels` 透传。
- [SOT] 当 `gateway start` 绑定 1 个 channel 时行为与既有单通道路径兼容；当绑定 >1 个 channel 时，参数解析仅允许通道无关参数，禁止通道专有参数。
- [SOT] 多通道 `daemon` 启动应始终以单一子进程运行，状态文件记录 `channels` 集合。
- [SOT] `GatewayServiceState` 增加 `channels` 字段并在状态持久化中写入绑定集合，`gateway status` 使用该集合回显。
- [SOT] `gateway start` 的多通道路径使用 `runGatewayServiceForChannels` 并发启动各通道服务器（service-child 场景）。

## REMOVED Requirements
- 无

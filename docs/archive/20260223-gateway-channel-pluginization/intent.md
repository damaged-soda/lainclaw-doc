# Intent: 将 gateway 拓扑重构为 plugin 化通道模型

## 背景与动机（Why）
当前 `gateway` 的 `--channel` 被当作运行时实例入口直接分发：同一套 `gateway` 命令在 `feishu/local` 之间走不同启动函数，导致通道语义与网关核心（服务化、启动/停止、状态持久化）耦合。长期来看这会限制：
- 一个 `gateway` 无法清晰支持“同一网关能力，多通信通道并存”；
- 通道/路由逻辑在 CLI 与服务运行路径反复重复，扩展成本高；
- `channel` 同时承载“选择配置分层”和“选择运行实例”两种职责。

## 目标（Goals）
1. 把 `channel` 纯化为 `gateway` 的通信插件键，不再承载网关实例控制职责。
2. 将 `gateway` 核心运行路径（start/status/stop/daemon/pid/log）与通道插件（feishu/local）解耦。
3. 引入 `GatewayChannelPlugin` 抽象（如：`parseStartArgs`, `run`），由插件统一承接通道专有参数与行为。
4. 保持对外命令入口行为可回归兼容（`--channel` 依旧可用，默认通道行为不变）。

## 非目标（Non-Goals）
1. 本轮不重构 gateway 配置持久化的单文件模型。
2. 本轮不改动配对/工具/心跳的业务语义。
3. 本轮不新增新的通信通道类型。

## 验收标准（Acceptance Criteria）
1. 从 `gateway start/status/stop` 的参数解析到执行调度，`channel` 仅用于选择插件。
2. 通道专有参数（例如 `feishu` 的 appId/appSecret/pairing 配置、`local` 的本地执行参数）仅在对应插件内被解析与消费。
3. `gateway` 核心运行上下文（`action`, `daemon`, `statePath`, `logPath`, `service-child`）在所有插件共用，不随 channel 变更。
4. 在现有命令兼容范围内，`gateway start/status/stop` 不发生行为回退（除因重构内部结构导致的非功能性差异）。

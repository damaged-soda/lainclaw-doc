# Gateway Channel Pluginization Spec Delta

## ADDED Requirements
- 无

## MODIFIED Requirements
- [SOT] `gateway` 的 `--channel` 语义改为“通信插件选择器”，不再用于表达独立网关实例主键；默认网关生命周期控制（`start/status/stop/daemon/pid/log`）保持 channel 无关。
- [SOT] 在 `src/lainclaw/src/cli/cli.ts` 引入 `GatewayChannelPlugin` 与通道注册表，至少包含 `feishu` 与 `local` 两个内建插件。
- [SOT] 通道插件接口需定义：
  - `parseStartArgs(argv: string[]) => GatewayStartOverrides`
  - `run(overrides: GatewayStartOverrides, context: GatewayServiceRunContext) => Promise<void>`
- [SOT] `gateway start` 的参数解析流程改为：先统一解析网关共享运行上下文，再按 `channel` 查插件后执行 `parseStartArgs` 与 `run`。
- [SOT] `gateway config` 命令保持功能兼容，但其 channel 语义仅用于配置作用域选择，不承载网关实例路由职责。
- [ARCHIVE-ONLY] 同步清理 `cli.ts` 中当前的 `if (channel === "feishu") ... else if (channel === "local") ...` 分支式分发。
- [ARCHIVE-ONLY] 为后续新增更多通信通道保留插件注册接口可扩展性（可加文档说明/注释，但不改变现有对外契约）。

## REMOVED Requirements
- 无

# Gateway Channel Pluginization Tasks

- [x] 在 `src/lainclaw/src/cli/cli.ts` 定义 `GatewayChannelPlugin`、`GatewayStartOverrides`、`GATEWAY_CHANNEL_PLUGINS` 与 `resolveGatewayChannelPlugin`（已完成）。
- [x] 将 `parseGatewayArgs` 的 `channel` 解析改为通过插件校验，并在 `start` 阶段调用插件的 `parseStartArgs`（已完成）。
- [x] 改造 `runCli` 中 `gateway` 分发，统一调用 `channelPlugin.run(...)`（已完成）。
- [x] 确认 `gateway status/stop` 的状态查询与停止路径在 plugin 上下文中保持兼容（当前保留原有 status/stop 逻辑并通过 context 转给插件，已完成）。
- [x] 对齐现有启动参数的行为边界，补齐回归说明（本轮仅保证 `--channel` 插件校验与回退行为一致，说明待归档备注）。
- [x] 请求你确认 `spec_delta.md` 后，再进入 IMPLEMENT 阶段（已完成）。
- [x] 将 `gateway status/stop` 的生命周期路径改为共享状态入口，`runCli` 在非 start 操作时走统一生命周期分发。
- [x] 将默认服务状态文件/日志名从按 channel 命名改为网关实例级单文件（如 `gateway-service.json` / `gateway-service.log`）。

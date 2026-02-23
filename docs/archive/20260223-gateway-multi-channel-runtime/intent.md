# Intent: 支持同一 gateway 实例绑定多 channel

## 背景与动机（Why）
当前 `gateway start` 的实现是“单通道单实例模型”，`--channel` 仍然决定一次启动的实例边界，导致:
- 一个 gateway 实例无法在同一进程内绑定多个通信通道。
- `gateway` 的生命周期管理与通道配置边界仍存在混淆。
- `gateway service` 状态文件虽然已实例化，运行时模型仍未完全表达通道集合。

## 目标（Goals）
1. 让 `gateway start` 支持 `--channel` 重复，`channels` 能作为该次启动的拓扑输入。
2. 保持单 daemon 子进程，管理多 channel 的前台/后台运行。
3. 让状态模型记录绑定的通道集合，供 `gateway status/stop` 观测。
4. 保持现有单通道行为兼容（包括 `feishu` 专有参数的解析与校验）。

## 非目标（Non-Goals）
1. 本轮不新增新的通信通道类型。
2. 本轮不改动 `gateway config` 的配置隔离模型。
3. 本轮不引入跨进程通道路由管理协议。

## 验收标准（Acceptance Criteria）
1. `gateway start --channel feishu --channel local ...` 可启动包含两通道的实例。
2. `gateway start` 多通道场景不允许混用不支持的通道专有参数（如 `--app-id` 在多通道时直接报错）。
3. `gateway status` 显示 `channels` 字段且与实际运行实例一致。
4. `gateway stop` 可正确停止单实例 daemon，并清理状态。

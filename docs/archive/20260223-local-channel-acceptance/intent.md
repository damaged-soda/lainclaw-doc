# Intent: 本地验证通道（Local Channel）

## 背景
你要的是“本地验收用的 channel”，而不是 openclaw 式外部测试框架。

`LainClaw` 当前只有 `feishu` 网关可运行通道，且 `--channel` 只作命名空间参数。为了让 `agent` 本地验收不依赖外网服务，我们在提案里增加一个仅用于本地验证的 `local` channel。

## 目标
- 新增一个本地专用的 `local` channel，用于本地闭环验收，不依赖飞书、无外网回调。
- 本地验收通过 `lainclaw gateway start --channel local ...` 走统一命令链路。
- 新增通道与现有生产行为隔离：默认保持 `feishu` 行为不变，`local` 只影响本地测试路径。
- 验收时支持独立 `HOME`、独立 `--pid-file/--log-file`，避免和线上实例打架。

## 非目标
- 不替换现有 `gateway` 与 `feishu` 生产行为。
- 不做多通道平台化改造（不是一次性通道系统重构）。
- 不引入第三方服务或外部 webhook 做验收依赖。

## 验收标准
- 在本地可见到 `--channel local` 的启动路径。
- 能用同一个 `gateway` 语义做启动、状态、停止和日志观察。
- 本地命令可在独立 `HOME` 下运行，产物（state/log）可回收。
- 验收证据至少包含：
  - local 服务状态快照（含 `pid/statePath/logPath/channel`）
  - 发送一条本地消息并收到本地回执（文件/日志）
  - 完整停止与清理动作。

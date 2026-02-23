# Intent: Lainclaw Gateway 常驻服务化（可插拔 Channel）

## 背景（Why）
当前网关运行时可长期阻塞进程，但没有内建服务化控制能力（后台启动、状态查询、进程关闭）。用户在终端会话关闭或意外异常退出后需要手工处理，不利于“最低可用个人 AI 助手”长期可用。当前代码中的默认接入是 `feishu`，但它应被视作一种可替换的 channel，而非网关核心。

## 目标（Goals）
- 提供网关的通用服务化能力，使当前任意可运行的 `--channel` 都能以可长期托管方式运行（本阶段以现有默认实现为起点）。
- 提供最小服务生命周期操作：启动到后台、查询状态、停止。
- 进程状态可持久化，便于启动前互斥检查和故障恢复。

## 非目标（Non-Goals）
- 不在本次改造中新增/扩展新的 channel 集成；本次只落地现有已支持的 channel 行为与接入方式。
- 不在本次改造中提供系统级 launchd/systemd 自动安装脚本（仅做可运行时内建控制）。
- 不改变模型路由与 tool-calls 的调用行为。

## 验收标准（Acceptance Criteria）
1. 运行 `lainclaw gateway start --daemon ...` 后命令退出（非阻塞）且网关进入后台运行。
2. 运行 `lainclaw gateway status [--channel <channel>]` 时返回服务状态与 `pid`。
3. `lainclaw gateway stop [--channel <channel>]` 可将后台网关进程关闭，关闭后状态显示为未运行。
4. 不带 `--daemon` 的原有 `gateway start ...` 行为不变，仍为前台阻塞运行。
5. `gateway start --daemon` 写入服务运行快照（默认路径），并在启动前防止重复占用已存活 PID。

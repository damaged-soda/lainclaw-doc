# Spec Delta: Add a local channel for acceptance

## ADDED Requirements
- [SOT] 在 `LainClaw` 中新增 `local` channel 的可用入口：`lainclaw gateway start|status|stop --channel local`。
- [SOT] `local` channel 的实现目标为离线验收：不依赖 Feishu WS，不发起外部网络调用。
- [SOT] `gateway` 的通道分派逻辑必须允许 `--channel local` 通过，且与 `--channel feishu` 兼容共存。
- [SOT] 本地验收文档与命令路径必须支持实例隔离参数：`HOME`、`--pid-file`、`--log-file`。
- [SOT] 本地 `local` channel 的消息回执应落地到可观测位置（建议：本地文件或本地日志），作为验收闭环证据。
- [SOT] 在 `local` 与 `feishu` 共存时，需明确约束：`local` 为验证用途，`feishu` 仍是既有默认通道。
- [ARCHIVE-ONLY] 本地通道可选实现为最小命令驱动触发器（例如通过特定本地入站脚本/文件触发）而非真实 IM 协议。
- [ARCHIVE-ONLY] 增补一组故障排查命令（`status` 空读、log 里无心跳、state 仍在）和恢复建议。

## MODIFIED Requirements
- [ARCHIVE-ONLY] 重写验收路径时，去掉 openclaw 式 `agent`/`reply-channel` 与插件加载假设。
- [ARCHIVE-ONLY] 把“状态隔离闭环”从 `local` 主目标中解耦为共享前置约束（适配未来更多 channel）。

## REMOVED Requirements
- [ARCHIVE-ONLY] 移除“默认支持多通道且当前就能跑”的表述。

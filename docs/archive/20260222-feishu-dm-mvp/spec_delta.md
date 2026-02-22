# Spec Delta: 飞书私聊 MVP

## ADDED Requirements

- [SOT] 系统新增 `feishu` 渠道配置入口（例如 `channels.feishu`），包含至少：`appId`、`appSecret`（或 token）与 callback 校验所需凭据，用于启动飞书入站/出站能力。
- [SOT] 系统新增飞书入站 HTTP 回调入口，支持 URL verification/challenge，并对事件请求进行基础签名或令牌校验。
- [SOT] 系统支持将飞书私聊文本消息事件映射为 Lainclaw 的 ask 输入，调用现有问答执行链路。
- [SOT] 系统支持将 Lainclaw 的文本回复回发给飞书私聊会话，实现最小闭环。
- [SOT] 私聊会话 ID 使用统一格式（如 `feishu:dm:<open_id>`）接入会话存储，实现上下文连续性。
- [ARCHIVE-ONLY] 系统对不支持的飞书群聊/非文本事件返回空操作（200）并记录 `unsupported_event` 审计日志。
- [ARCHIVE-ONLY] 系统在入站事件中增加简易幂等缓存（基于 message/event id），避免短窗口重复处理。
- [ARCHIVE-ONLY] 系统在回包失败时记录 `feishu_send_error` 指标字段（message/chat id、错误码、重试次数）。

## MODIFIED Requirements

- [SOT] Lainclaw 执行链路需扩展为可被“外部事件”触发（非仅 CLI 入口）。
- [ARCHIVE-ONLY] 启动文档新增飞书私聊 MVP 的部署步骤、环境变量与本地验证脚本。

## REMOVED Requirements

- 无

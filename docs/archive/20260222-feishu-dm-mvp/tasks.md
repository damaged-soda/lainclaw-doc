# Tasks: 飞书私聊 MVP

## Proposal gate
- [x] 用户确认本 `spec_delta.md`，并批准进入 IMPLEMENT 阶段。

## Implement tasks
- [x] 新增配置解析：为飞书私聊 MVP 增加 `channels.feishu` 配置字段定义与默认值（仅保留 MVP 所需字段）。  
  - 影响：`src/lainclaw/src/channels/feishu/config.ts`
  - 影响：配置模型与启动参数。
- [x] 新增飞书入站服务：实现 HTTP 回调入口（含 challenge 与签名/Token 验证）。  
  - 影响：`src/lainclaw/src/channels/feishu/server.ts`
  - 影响：新增/改造通道层入口代码。
- [x] 实现飞书私聊事件解析：仅允许私聊文本事件，提取 open_id/chat_id/message_id/message content。  
  - 影响：`src/lainclaw/src/channels/feishu/server.ts`
  - 影响：事件模型与 ask 入参适配。
- [x] 实现 `feishu:dm:<open_id>` 会话键映射并对接现有 sessionStore。  
  - 影响：`src/lainclaw/src/channels/feishu/server.ts`（通过 `runAsk` 会话键）
  - 影响：会话管理层。
- [x] 将私聊文本事件转给现有 ask pipeline（最小适配），返回标准文本结果。  
  - 影响：`src/lainclaw/src/channels/feishu/server.ts`
  - 影响：ask 调度与错误传播。
- [x] 实现私聊文本出站：将文本回复通过飞书 API 发回原会话。  
  - 影响：`src/lainclaw/src/channels/feishu/outbound.ts`、`src/lainclaw/src/channels/feishu/server.ts`
  - 影响：出站适配器与 HTTP 客户端。
- [x] 群聊/非文本事件处理：返回成功码（不处理）并写入 `unsupported_event` 结构化日志。  
  - 影响：`src/lainclaw/src/channels/feishu/server.ts`
  - 影响：入站事件分支逻辑。
- [x] 增加最小可观测与错误日志：记录 requestId、eventId、chatId、userId、错误码。  
  - 影响：`src/lainclaw/src/channels/feishu/server.ts`
  - 影响：日志与监控输出。
- [x] 用户传入 appId/appSecret/verifyToken/host/port/path 等参数自动写入 `.lainclaw` 缓存；下次启动可复用。  
  - 影响：`src/channels/feishu/config.ts`、`src/channels/feishu/server.ts`、`src/cli/cli.ts`
  - 说明：先缓存再解析有效配置，优先级 `CLI参数 > 缓存 > 环境变量 > 默认值`。
- [ ] 提供 MVP 验收步骤：含签名测试、私聊问答回路验证、未支持事件验证。  
  - 影响：待补充验证命令/示例文档（未开始）。
  - 影响：文档。

## Checklist notes policy
- 每完成一项实现任务，立即在本文件对应项打钩，并补一句“改了什么/为什么/影响面”。

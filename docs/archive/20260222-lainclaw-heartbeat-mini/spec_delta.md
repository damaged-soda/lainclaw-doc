# Specification Delta: Lainclaw Heartbeat MVP（自然语言提醒）

> **目标 SOT 文件**: 待定
> **基于意图**: `intent.md`

## 1. 变更摘要 (Synopsis)

为 `lainclaw` 增加最小可用心跳提醒闭环：CLI 管理规则 + 模型驱动周期评估 + 在 Feishu 网关内触发通知。

## 2. 需求变更 (Requirements Delta)

### 🟢 ADDED Requirements (新增需求)

#### [CANDIDATE] Requirement: 心跳规则数据存储与管理

系统应提供本地 JSON 规则存储与读写接口，支持按 `id` 管理规则的增删改查、开关、执行元信息记录。

#### [CANDIDATE] Requirement: 心跳规则文本执行器

系统应在每次心跳周期中，对启用规则调用 `runAsk`，由模型返回 `TRIGGER` / `SKIP` 结论，并附带触发文案。

#### [CANDIDATE] Requirement: 心跳命令入口

系统应新增 `lainclaw heartbeat` 命令，支持：

* `add <ruleText>`
* `list`
* `remove <ruleId>`
* `enable|disable <ruleId>`
* `run`

#### [CANDIDATE] Requirement: Feishu 网关定时执行

系统应在 Feishu 网关运行时，可选启动心跳轮询；若规则触发，发送通知消息到配置接收者。

### 🟡 MODIFIED Requirements (修改需求)

#### [ARCHIVE-ONLY] Requirement: Feishu 网关配置项

- OLD: Feishu 网关配置只服务于消息收发。
- NEW: 增加与 heartbeat 相关的可选字段（如是否启动、轮询间隔、发送目标），用于在网关内保持长期运行。

#### [ARCHIVE-ONLY] Requirement: CLI 使用说明

- OLD: README 只描述 ask 与网关的启动方式。
- NEW: 追加心跳规则与心跳执行说明。

## 3. 数据结构/API 变更 (Schema/API Changes)

* 新增 `Heartbeats` 结构文件（JSON）用于规则持久化。
* 扩展 `FeishuGatewayConfig`（候选实现）以承接 heartbeat 开关、轮询间隔、发送目标与默认会话信息。
* 新增 CLI 子命令与退出码行为。

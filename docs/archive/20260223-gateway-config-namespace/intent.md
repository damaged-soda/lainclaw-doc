# Intent: Clarify Gateway Namespace vs Config Fields

## 背景与动机
`~/.lainclaw/feishu-gateway.json` 目前承载飞书网关运行时参数，使用中出现 “provider 与 channel 的层级关系” 解释歧义（`provider` 是否是 channel 子字段 vs channel 命名空间字段）。  
实际意图是让 `channel` 只承担配置分片/实例名角色，`provider` 等仍属于网关配置对象。

## 目标（Goals）
- 仅澄清层级关系的语义，不改变网关行为。
- 在配置读取、展示与文档提示中显式表达“channel 是缓存文件分片”。
- 减少用户对 `feishu-gateway.json` 用途的误解。

## 非目标（Non-Goals）
- 不改模型路由。
- 不引入新的配置 schema/多渠道运行框架。
- 不调整启动参数优先级、鉴权策略或 heartbeat/pairing 规则。

## 验收标准（Acceptance Criteria）
- CLI 错误提示与配置展示中能明确“channel 是命名空间，`provider` 仍是网关配置字段”。
- README 对 `gateway config` 的文件路径与字段语义有清晰说明。
- 现有功能保持不变：清理、读写、启动流程行为未变。

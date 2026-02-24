# 规格变更草案（提案）

## ADDED Requirements

- [SOT] 在 `src/gateway/askGateway.ts` 的工具结果注入路径上，新增统一的「工具结果大小治理」规则：当单条工具返回 `JSON.stringify` 后超过上限时，必须先做结构化摘要并裁剪为固定上限后再注入模型上下文。
- [SOT] 被裁剪的工具结果必须带上可解析标记字段，例如 `[tool_result_truncated=true]`、`[tool_result_bytes_before]`、`[tool_result_bytes_after]`，并附带“建议用户聚焦查询条件继续”的短提示，避免模型把缺失片段误判为完整答案依据。
- [SOT] 该裁剪策略不得改变工具原始返回对象；仅影响模型上下文注入内容。下游业务若需完整结果，仍可依赖内部日志/持久化内容（若有）或二次查询工具。
- [ARCHIVE-ONLY] 引入配置项 `toolResult`（如 `maxCharacters`, `maxItems`, `tailLines`），用于后续可调优；初始阈值由实现阶段定义并记录在实现文档里。
- [CANDIDATE] 在 ask 入口附近输出工具结果长度与超时原因的观测数据（原始长度、裁剪后长度、是否裁剪、超时类型），用于后续回归验证。
- [SOT] 在 `gateway start` 命令新增 `--debug` 开关。开启时，`writeAskAuditRecord` 输出（成功/失败）应在当前进程标准输出中同步打印，便于调试追踪当前会话。
- [SOT] `gateway start --debug` 不落库到配置文件，不影响非该实例的运行。
- [SOT] 放弃 `LAINCLAW_PROMPT_AUDIT_PATH` 运行时路径覆盖开关；`writeAskAuditRecord` 仅保留默认路径与 fallback 策略，审计输出策略统一由 `gateway start --debug` 表达。

## MODIFIED Requirements

- [SOT] 当 `runAsk` 发生超时/异常导致 `writeAskAuditRecord` 走 error 分支时，日志必须保留完整诊断字段：`inputLength`、`inputChecksum`、`errorKind`、`timeoutMs`（如有）与 `auditStage`，避免“失败记录只剩下 `success:false + error`”。
- [SOT] 在 `writeAskAuditRecord` 输入字段超出可读性上限时，应保留 `input` 完整长度与安全截断预览：`inputPreview`（如长度上限）与 `inputTruncated` 标记。最终审计记录需能支持人工复现“是否输入爆炸”。
- [ARCHIVE-ONLY] 调整 `src/channels/feishu/server.ts` 与网关上下文拼接逻辑，保持超时行为不依赖工具上下文体积变化；超时阈值短期内保持 10000ms，不在本提案中变更默认值。

## REMOVED Requirements

无

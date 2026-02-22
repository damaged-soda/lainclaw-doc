# spec_delta

## ADDED Requirements
- [SOT] 增加 openai-codex 自动工具调用闭环（`runAsk` + `runCodexAdapter` 联动）：
  - 当 provider 为 `openai-codex` 且 `withTools=true` 时，模型返回的 tool_calls 进入 `ToolExecutor` 执行。
  - 支持执行结果追加至上下文并再次调用模型（至少 1 轮复投喂）。
- [SOT] 为 Codex 工具调用定义统一回包输入：
  - 在模型输入中注入可见工具列表（按 `--tool-allow` 与默认规则过滤）。
  - 输出中返回 `toolCalls`（模型原始 tool call）与 `toolResults`（执行归档），并维持 `toolError/sessionContextUpdated`。
- [SOT] 将工具调用循环加上上限参数与默认值：
  - 新增 `runAsk`/CLI 参数 `--tool-max-steps`（可选，默认值 3）。
  - 超过上限时停止继续循环，并将错误写入 `toolError`。
- [SOT] 保持手工 `tool:` 触发路径可用：
  - 在未返回 model tool_calls 或 provider 非 codex 时，沿用现有 `tool:` 前缀解析行为。
- [CANDIDATE] Codex tool_call 适配字段（`arguments`、`id`、`type`）的标准化 schema 与兼容性映射策略，需要在实现验证后锁定版本。
- [ARCHIVE-ONLY] 错误文案文风、日志字段排序、tool 失败重试策略（若引入）作为实现细节在本次不锁入 SOT。

## MODIFIED Requirements
- [SOT] `printUsage` 与 `ask` 参数解析新增 `--tool-max-steps` 及其约束校验。
- [SOT] `GatewayResult` 与/或 `ask` 输出保持向下兼容，并新增/保留以下字段：
  - `toolCalls`、`toolResults`、`toolError`、`sessionContextUpdated`（沿用现有结构）。
- [ARCHIVE-ONLY] CLI 提示文案中“成功案例”示例由手工调用向自动调用迁移时，可先保持并行展示（手工与自动并行说明）。
- [CANDIDATE] 自动调用失败时的回退策略（继续回答、重放错误给模型、或直接返回错误）在本期仅先做最小实现并记录实验日志。

## REMOVED Requirements
- [ARCHIVE-ONLY] 移除现有 `tool:` 手工触发机制（保留以避免双路径回归）。

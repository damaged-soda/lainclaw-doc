# Intent

## 背景
- 当前 CLI 侧已具备工具能力（`tools.*` 命令、`tool:` 前缀手工触发、`ask` 回传 `toolCalls/toolResults`）。
- `openai-codex` 路径下的模型仍未自动发起 tool-calls，`ask` 仍是“手动触发 + 单轮复投喂”。
- 下一步要把工具能力提升为“模型可直接调用”阶段：让 Codex 的回复触发工具执行，再复投喂结果到下一轮模型。

## 目标
- 实现 `openai-codex` 提供商下的自动工具调用闭环。
- 保持现有手工 `tool:` 模式兼容，不打断当前 MVP 交付行为。
- 在异常路径保持 `GatewayResult` 骨架与可观测字段完整返回（不因 tool 执行失败导致 ask 全局失败）。

## 非目标
- 不引入跨工具超时重试、并发执行、权限沙箱或安全隔离策略。
- 不变更现有工具集合与参数约束（`time.now` / `tools.echo` / `shell.pwd` / `fs.list_dir` / `fs.read_file`）。
- 不接入除 `openai-codex` 以外的新模型供应商。

## 验收标准
- `lainclaw ask --provider openai-codex ...` 能在模型返回 tool_calls 时自动执行对应工具并复投喂上下文。
- 模型返回 `tool_calls` 与工具执行不一致时，`toolError` 有明确错误并返回完整 `ask` 结构。
- 自动调用失败时，仍输出 `success=true` 的问答骨架（除非会话/路由初始化失败）。
- 手工 `tool:` 入口仍可用。

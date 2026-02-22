# spec_delta

## ADDED Requirements

- [SOT] 增加最小工具域模块 `src/lainclaw/src/tools/*`，统一三类模型：
  - `ToolSpec`：工具名、描述、输入 schema、执行 handler。
  - `ToolCall`：`id / name / args / source`。
  - `ToolResult`：`ok / data / error / meta`。
- [SOT] 增加 `tools` 注册与发现能力（`toolsRegistry`）：
  - 支持 `list` 全量列表、`get(name)` 获取定义。
  - 支持 `--tool-allow` 的白名单过滤（若未命中则不进入可调用集合）。
- [SOT] 内置 MVP 工具集为 5 个：
  - `time.now`：返回 `timestamp` 与 `iso` 时间。
  - `tools.echo`：回显原样内容，用于链路探针。
  - `shell.pwd`：返回当前工作目录字符串。
  - `fs.list_dir`：列目录，参数含 `path`、`recursive=false`、`maxDepth=4`。
  - `fs.read_file`：读文件，参数含 `path`、`maxBytes=204800`，超限返回错误。
- [SOT] 新增 `ToolExecutor`（`src/lainclaw/src/tools/executor.ts`）：
  - 校验工具是否存在、参数是否符合 schema、执行 `handler` 并返回标准化 `ToolResult`。
  - 约定固定错误码：`tool_not_found`、`invalid_args`、`execution_error`。
- [SOT] 新增工具网关 `toolGateway` 与 CLI 可见性入口：
  - 命令 `lainclaw tools list`，按 `name/description/inputSchema` 输出可见工具清单。
  - 命令 `lainclaw tools info <name>`，输出单工具 schema 与运行时元信息。
  - 命令 `lainclaw tools invoke <name> --args '{...}'`，支持手工工具调用验证。
- [SOT] 扩展 `ask` 流程为最小工具调用闭环：
  - 默认启用工具能力（或显式 `--with-tools`）。
  - 当模型返回 `toolCalls` 时，按顺序执行工具，构建 `toolResults` 附加到会话上下文。
  - 将工具执行结果再次提交给模型，产出最终 assistant 响应。
- [SOT] 扩展 `ask` 输出结构，固定包含：
  - `toolCalls`（本轮计划调用）
  - `toolResults`（每次调用结果）
  - `toolError`（存在失败调用时返回，含 `tool`、`code`、`message`）
  - `sessionContextUpdated`（是否将工具结果写入上下文）
- [ARCHIVE-ONLY] `tools` 命令输出的文案样式、排序和分页属于展示层实现细节，可后续调整。
- [ARCHIVE-ONLY] 工具内建参数校验策略（比如细粒度 regex、长度上限）作为实现细节迭代，不上 SOT。
- [CANDIDATE] 工具执行超时/重试策略初始默认值（如超时 8s）可后续再固定并转入正式 SOT。
- [CANDIDATE] 工具参数和响应的 JSON schema 可再演进为严格 JSON Schema 标准对象，当前先以最小运行时校验。

## MODIFIED Requirements

- [SOT] `ask` 命令参数新增：
  - `--with-tools=<true|false>`：是否开启模型工具调用；默认 true。
  - `--tool-allow=<csv>`：如 `"time.now,shell.pwd"` 指定当前会话可见工具。
- [SOT] `ask` 默认行为修改为：
  - 默认注入 `tools/list` 可见集合为模型上下文；
  - 在同一会话内可复用缓存的工具调用结果，避免重复解析无必要命令。
- [ARCHIVE-ONLY] CLI 的参数短写与别名（如 `-t`）不进入 SOT，按实现迭代。

## REMOVED Requirements

- [ARCHIVE-ONLY] 未移除现有 `ask`/`auth` 命令（仍保持原有行为），在其上补充工具能力层，不做破坏性替换。

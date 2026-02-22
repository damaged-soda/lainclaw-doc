# Tasks

- [x] 在 `src/lainclaw/src/tools/types.ts` 定义 `ToolSpec / ToolCall / ToolResult / ToolError` 结构与注释。（完成）
- [x] 在 `src/lainclaw/src/tools/registry.ts` 实现工具注册、`list`、`get`、白名单过滤逻辑。（完成）
- [x] 在 `src/lainclaw/src/tools/executor.ts` 实现统一执行入口。（完成）
- [x] 参数校验（必填字段、类型）与错误码转换（`tool_not_found / invalid_args / execution_error`）。（完成）
- [x] 在 `src/lainclaw/src/tools/builtin/` 新增 5 个内置工具实现：
  - `time.now`
  - `tools.echo`
  - `shell.pwd`
  - `fs.list_dir`
  - `fs.read_file`（含字节上限保护）
- [x] 在 `src/lainclaw/src/tools/gateway.ts`（或 `toolGateway.ts`）实现 `tools list / info / invoke` 处理器。（完成）
- [x] 在 `src/lainclaw/src/cli/cli.ts` 注册 tools 子命令，支持：
  - [x] `tools list`（完成）
  - [x] `tools info <name>`（完成）
  - [x] `tools invoke <name> --args ...`（完成）
- [x] 在 `src/lainclaw/src/cli/cli.ts` 为 `ask` 增加参数解析：
  - [x] `--with-tools`（完成）
  - [x] `--tool-allow`（完成）
- [x] 修改 `src/lainclaw/src/gateway/askGateway.ts` 或 `pipeline.ts`：接收工具调用意图并触发 `ToolExecutor`。（完成）
- [x] 在 `ask` 流程新增“工具调用-聚合-复投喂”循环：执行 `toolCalls` 后追加 `toolResults` 再次调用模型。（完成）
- [x] 扩展 `ask` 响应结构（`toolCalls / toolResults / toolError / sessionContextUpdated`）并写入输出。（完成）
- [x] 增加基础错误冒泡测试点：tool 不存在、参数错误、执行异常仍返回完整响应骨架。（手工验证通过）
- [x] 在 `docs/wip/20260222-cli-only-mvp/spec_delta.md` 中把实现细化与任务执行偏差逐条回填备注（无代码变更）。（完成）

#### 错误冒泡手工验证计划（已执行）

- [x] 用例 1：`tools invoke nonexistent_tool`（退出码 `1`）返回 `ok=false`，`result.error.code=tool_not_found`。
- [x] 用例 2：`tools invoke tools.echo --args '{}'`（退出码 `1`）返回 `ok=false`，`result.error.code=invalid_args`。
- [x] 用例 3：`tools invoke fs.read_file --args "{\"path\":\"/this/does/not/exist\"}"`（退出码 `1`）返回 `ok=false`，`result.error.code=execution_error`。
- [x] 用例 4：`ask "tool:invalid_tool {}"`（退出码 `0`）返回 `success=true`，且 `toolError.code=tool_not_found`，`toolResults` 与 `toolCalls` 均存在。

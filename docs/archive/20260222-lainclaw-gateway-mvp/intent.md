# Intent: Bootstrap OpenClaw MVP scaffold with Gateway-first execution flow

## 1) Background
- `src/lainclaw` 目前只有一个单文件 `src/index.ts` 示例；缺少可扩展模块结构。
- 当前代码只能做 `hello`，没有“入口 -> 网关 -> 流程编排”闭环。
- 近期目标是先把“跑得通的骨架”搭起来，后续再逐步接入模型能力。

## 2) Goals
- 建立最小可用模块化脚手架（含 `gateway`），让流程从 CLI 命令一路串起来。
- 先用 stub 实现模型能力，不接外部模型/云服务。
- 形成可复用的上下文结构，后续替换实现时不改 CLI 行为。
- 保持与现有仓库约束一致：`src/lainclaw` 可通过 `npm link` 执行 `lainclaw`。

## 3) Non-Goals
- 不接入真实模型 API（OpenAI/本地推理端口）。
- 不做持久化、鉴权、日志平台、监控告警。
- 不做分布式网关、网格路由、多租户/并发隔离。
- 不调整 SOT 文件本体（本阶段仅提案）。

## 4) Acceptance Criteria
- `lainclaw` 命令可以运行，支持：
  1. `lainclaw --help`
  2. `lainclaw ask <文本>`：执行一次完整“网关流程”并返回可读输出
  3. `lainclaw --version`（可选）显示版本
- 模块结构至少包含：`cli`、`gateway`、`pipeline`（或等价的处理编排路径）、`adapters`。
- 关键行为默认可预测：遇到空输入时给出明确错误提示。
- 现有 hello world 友好行为可兼容为新流程的一种命令/输出模式。

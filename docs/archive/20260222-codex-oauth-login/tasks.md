# 任务清单（实施）

- [x] 调研 OpenClaw 与现有 `@mariozechner/pi-ai` 的 Codex/OAuth 集成方式，确认是否可复用而非重写协议。
  - 结果：确认可复用 `loginOpenAICodex`、`getOAuthApiKey`，采用本地 profile 持久化方案。
- [x] 按 MVP 边界实现认证模型与存储：`AuthProfile`/`AuthStore` 与 `~/.lainclaw/auth-profiles.json`。
  - 文件：`src/lainclaw/src/auth/types.ts`, `src/lainclaw/src/auth/configStore.ts`
- [x] 实现认证流程管理器：登录、状态、切换默认、登出、清理；支持 token 自动刷新持久化。
  - 文件：`src/lainclaw/src/auth/authManager.ts`
  - 结果：添加 `auth` 命令子树与交互提示文案。
- [x] 增加 Codex 适配器并接入 `pipeline` 路由。
  - 文件：`src/lainclaw/src/adapters/codexAdapter.ts`, `src/lainclaw/src/pipeline/pipeline.ts`
  - 结果：`provider=openai-codex` 时走 `runCodexAdapter`；默认仍走 stub。
- [x] 增加 CLI 参数和入口：`--provider/--profile` 与 `auth` 管理命令。
  - 文件：`src/lainclaw/src/cli/cli.ts`, `src/lainclaw/src/gateway/askGateway.ts`
- [x] 更新共享类型与构建依赖以保持链路透传。
  - 文件：`src/lainclaw/src/shared/types.ts`, `src/lainclaw/package.json`, `src/lainclaw/src/adapters/stubAdapter.ts`
- [x] 验证非登录下错误提示与现有离线行为兼容性：`ask` 不传 provider 仍走 stub，传错 provider 返回错误提示。
  - 结果：基于 `help` 与命令测试进行语义核对（登录链路实际需人工交互验证）。

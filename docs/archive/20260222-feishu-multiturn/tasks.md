# 执行清单（Tasks）

- [x] 在 `src/lainclaw/src/channels/feishu/config.ts` 增加飞书网关配置字段：`provider`、`profileId`、`withTools`、`memory`，并新增 env 解析与 JSON 持久化。 ✅
- [x] 在 `src/lainclaw/src/channels/feishu/config.ts` 保持 CLI args > env > 缓存配置的解析优先级，补充输入合法性（非法布尔值/超时值）处理。 ✅
- [x] 在 `src/lainclaw/src/channels/feishu/server.ts` 定义可透传的 provider/profile/tool/memory 配置（先从 `resolveFeishuGatewayConfig()` 获取）。 ✅
- [x] 修改 `handleWsPayload`：将 `runAsk` 调用参数扩展为 `{ provider, profileId, withTools, memory }`。 ✅
- [x] 在 `handleWsPayload` 捕获 `runAsk` 已知失败原因时，返回明确提示文案（如无 profile、provider 未授权、超时）。 ✅
- [x] 在 `src/lainclaw/src/cli/cli.ts` 扩展 `feishu` 命令参数解析：`--provider`、`--profile`、`--with-tools`、`--no-with-tools`、`--memory`、`--no-memory`。 ✅
- [x] 在 `src/lainclaw/src/cli/cli.ts` 补充 `feishu` 的 usage 和帮助文本，保持与 `ask` 参数风格一致。 ✅
- [x] 在 `src/lainclaw/README.md` 的飞书章节补充：模型接入方式、会话行为和常见错误恢复（登录/授权）。 ✅
- [x] 在 `src/lainclaw/src/cli/cli.ts` 扩展 `gateway`/`feishu` 参数解析：`--tool-allow`、`--tool-max-steps`。 ✅
- [x] 在 `src/lainclaw/src/channels/feishu/server.ts` 将 `toolAllow` 与 `toolMaxSteps` 透传给 `runAsk`。 ✅
- [x] 在 `src/lainclaw/README.md` 文档补充 `--tool-allow` 与 `--tool-max-steps` 的说明（含环境变量）。 ✅
- [x] 在 `src/lainclaw/src/cli/cli.ts` 新增统一入口：`gateway --channel feishu`；保留 `feishu` 兼容命令。 ✅
- [x] 将 `feishu` 的失败提示策略从 `channels/feishu/server.ts` 移至网关层（`cli.ts`）并通过回调注入。 ✅
- [x] 从 `channels/feishu/server.ts` 移除模型语义分支（provider/错误语义判定），保留会话与转发职责。 ✅
- [ ] 增加最小自检清单（本轮未完成，已列入后续手工验收）：
  - [ ] 无配置启动 -> 提示缺少 profile；
  - [ ] 有配置但未登录 -> 提示执行 `auth login openai-codex`；
  - [ ] 有配置、有登录 -> 复用 session 的连续消息，验证上下文长度不退化。

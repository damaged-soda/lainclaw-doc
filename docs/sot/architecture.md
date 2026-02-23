# 架构说明（SOT）

Last Updated: 2026-02-23

## 模块边界（按 repo 或关键模块描述）

- `src/lainclaw`：轻量版运行时与交互层（首次迭代可从单一模块起步）
- `docs`：单一事实源（SOT）与 WIP 过程信息分离
- `src/lainclaw` 的执行路径采用 `cli -> gateway -> pipeline -> adapter` 三层职责分离，便于后续替换模型后端。

## 关键约束 / 不变量
- 文档事实必须先于代码行为定义：任何对外行为优先通过 `spec_delta` 阐明
- WIP 与 SOT 分离：计划过程不进入 SOT，SOT 只承载长期稳定契约
- 在同一功能方向内保持最小可验证实现，避免一次性堆叠过多模块依赖
- 文档驱动仓库映射必须可达：`docmap.yaml` 中 `repos` 的每个 `path` 应可被构建/链接流程访问。
- 对外 CLI 合同以 `--help`、`ask <input>` 为 MVP 最小可用入口；空输入 `ask` 需要返回明确错误与用法提示。
- `gateway` 需要建立可复用上下文（`requestId`、`createdAt`、原始输入），并通过 pipeline 统一产生结构化输出，便于后续替换模型供应商。
- 飞书网关入口为 `gateway start`（默认频道 `feishu`，可用 `--channel <channel>` 显式覆盖，当前实现仅支持 `feishu`）。启动参数必须透传以下配置到 `runAsk`：
- `--provider <provider>`：默认 `openai-codex`；当前阶段仅支持 `openai-codex`。
- `--profile <profileId>`：绑定 openai-codex profile，缺省走 active profile。
- `--with-tools|--no-with-tools`：控制模型 tool-call 能力。
- `--tool-allow <tool1,tool2>`：工具白名单（空值表示按默认策略）。
- `--tool-max-steps <N>`：工具自动循环上限，`N>=1`。
- `--memory|--no-memory`：会话记忆摘要注入开关。
- `--pairing-policy <open|allowlist|pairing|disabled>`：飞书 DM 鉴权策略，`open` 放行所有、`allowlist` 只允许 allow list、`pairing` 触发配对流程、`disabled` 拒绝全部。
- `--pairing-allow-from <id1,id2>`：配置允许名单（支持按账号或全局文件）。
- `--pairing-pending-ttl-ms <ms>`：配对 pending 超时窗口（默认 `3600000`）。
- `--pairing-pending-max <n>`：待审批 pending 上限（默认 `3`）。
- `gateway` 在进入 `runAsk` 前需按 `pairingPolicy` 做鉴权：
  - `open`：所有飞书 DM 允许执行。
  - `allowlist`：仅 `pairingAllowFrom` 命中允许。
  - `pairing`：未命中时返回 pairing code 并创建/更新 pending；通过后放行。
  - `disabled`：全部飞书 DM 拒绝。
- 飞书配对持久化约定：
  - pending 文件默认落盘 `~/.lainclaw/<channel>-pairing.json`，字段至少包含 `id`、`code`、`createdAt`、`lastSeenAt`、`meta`（可选）。
  - allow-from 文件默认落盘 `~/.lainclaw/<channel>-allowFrom.json` 与 `~/.lainclaw/<channel>-<account>-allowFrom.json`，采用账号隔离读取（优先当前账号，再回退 legacy 全局）。
  - pending 在读取/列出/审批流程中执行 TTL 清理与上限修剪。
- `pairing` 命令对外行为：
  - `pairing list [--channel <channel>] [--account <accountId>] [--json]`：查看待审批项。
  - `pairing approve [--channel <channel>] [--account <accountId>] <code>`：按 code 审批。
  - `pairing revoke [--channel <channel>] [--account <accountId>] <entry>`：移除 allow-from。
  - 当前仅支持 `feishu` 通道。

- `gateway` 服务化入口支持：
  - `gateway status [--channel <channel>]`：读取服务状态（`running`/`stopped`、`pid`、`startedAt`、`statePath`、`logPath`）。
  - `gateway stop [--channel <channel>]`：终止网关进程并清理 service state。
- `gateway start --daemon`：后台化运行并持久化状态文件（默认 `~/.lainclaw/service/gateway-service.json`）。状态结构记录 `channel` 与 `channels`，用于区分多实例运行状态。

- `gateway config` 作为持久化配置入口：
- `gateway config set`：将网关参数持久化到 `~/.lainclaw/gateway.json`（`default` 为通用分层，`channels` 下按频道覆盖）。
  - `gateway config show [--channel <channel>]`：展示已持久化参数（脱敏）。
- `gateway config clear [--channel <channel>]`：清理配置分层（无 `--channel` 清理 `default`，有 `--channel` 清理 `channels[channel]`）。
- 结构化执行结果（含 `route`、`stage`、`result`）属于可观测输出约定，MVP 期间可用 stub 响应替代真实模型。
- 用户可见文本默认不得携带模型源调试前缀；`adapter` 返回的正文应保留纯文本语义，除非显式开启调试开关用于排障观察。
  - 默认行为：不在 `result` 前缀注入 `openai-codex:<profileId>` 等来源标识。
  - 调试行为：在显式开关下可按需注入来源信息，便于链路追踪与问题定位。
- 问答结构化输出在 MVP 阶段继续扩展为记忆可观测字段：`memoryEnabled`、`memoryUpdated`（可选 `memoryFile`）。
- `gateway -> pipeline -> adapter` 的职责分层保持稳定：gateway 组装 `RequestContext`，pipeline 负责路由（stub/openai-codex），adapter 负责模型调用与鉴权上下文消费。
- 认证 profile 持久化落在本地 `~/.lainclaw/auth-profiles.json`，其中 `auth login`、`auth status`、`auth use`、`auth logout` 是 CLI 可见接口。
- 工具能力与 ask 工具闭环契约（MVP）：
  - 系统 SHALL 提供 `tools list`、`tools info`、`tools invoke` 三类可见命令；工具域包含 `time.now`、`tools.echo`、`shell.pwd`、`fs.list_dir`、`fs.read_file` 的内置集合与最小字段约束。
  - ask 工具控制参数统一为 `--with-tools` 与 `--tool-allow`，默认允许工具能力；仅将通过白名单的工具注入并执行。
  - 工具执行统一返回 `toolCalls / toolResults / toolError / sessionContextUpdated`；错误码限定为 `tool_not_found`、`invalid_args`、`execution_error`，并在异常路径下保持 ask 输出骨架完整。
  - openai-codex provider 在 `--provider openai-codex && --with-tools` 下启用模型 tool-call 自动闭环：路由返回 `tool_calls` 时执行后续回写上下文并可进入下一轮，默认上限为 `--tool-max-steps=3`（取值为整数且 `>=1`）。
  - 达到 `tool-max-steps` 上限时停止继续调用，并在 `toolError` 中写入 `execution_error`；该上限对模型自动 tool-call 循环生效。
  - 手工 `tool:` 触发逻辑仍保留：在未返回 model `tool_calls` 或 `provider` 非 openai-codex 时，仍按前缀解析执行并返回可观测的 `toolCalls/toolResults/toolError`。
- 默认 `ask` 仍走离线 stub；仅当 `--provider openai-codex` 时才尝试使用认证 token 发起线上调用；未登录或 profile 缺失时应返回可操作提示。
- `openai-codex` 路径下的系统提示词/调用参数可迭代优化，不作为长期对外契约，默认以可运行与体验优先。
- 会话持久化（第一阶段）：
  - 系统 SHALL 提供基于 `sessionKey` 的 `ask` 持久化会话：首次请求创建 `sessionId`，默认复用；`--new-session` 强制切换新 `sessionId`。
  - `gateway` 在构建上下文时必须携带 `sessionKey`、`sessionId` 与最近历史 `messages`，并将其透传至 pipeline；同会话内新请求需基于历史上下文决策和响应生成策略。
  - 每次成功的 `ask` 必须持久化转写：在 `~/.lainclaw/sessions/<sessionId>.jsonl` 追加 `user` 与 `assistant` 消息，并在 `sessions.json` 中维护 `sessionKey -> sessionId` 索引。
- 会话记忆增强（第二阶段）：
  - 系统 SHALL 通过 `--memory` / `--no-memory` / `--memory=<on|off>` 管理 per-session 记忆开关，并在 `sessions` catalog 中持久化状态。
  - 记忆开启时，`gateway` 先读取并注入 `~/.lainclaw/memory/<sessionKey>.md` 的尾部摘要作为 system 消息到上下文。
  - compaction 在累计消息数超过阈值（MVP：24）触发；保留最近窗口（MVP：12）用于即时上下文，老内容以摘要写入 `memory` 文件并记录 `compactedMessageCount` 防重复摘要。

## 跨 repo 交互（如适用）

- 当前仅单仓库起步：`Lainclaw` 无跨仓库依赖
- 架构演进遵循文档驱动的渐进扩展
- CLI 入口约定：`src/lainclaw/dist/index.js` 为可执行入口，`npm link` 后应提供 `lainclaw` 全局命令。

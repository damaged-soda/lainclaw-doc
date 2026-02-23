# AI Agent 验收指南：`local` channel 闭环（LainClaw）

## 目标
确认 `gateway --channel local` 可走完整闭环，并且不打扰 `feishu` 正式实例。

## 前置条件
- `docs/wip/20260223-local-channel-acceptance` 对应的代码实现已就绪。
- 已在 `src/lainclaw` 执行过 `npm run build`。
- 运行命令时有可用 `openai-codex` profile（从用户本地配置自动读取）。
- 若使用隔离 `HOME`，请提前同步认证文件：
  - `mkdir -p <HOME>/.lainclaw && cp "$HOME_ORIGIN/.lainclaw/auth-profiles.json" <HOME>/.lainclaw/`
  - 例如：`HOME_ORIGIN=/home/<你当前用户>`，`HOME=/tmp/lainclaw-local`.

## 推荐约束
- 建议使用独立环境变量 `HOME`，避免直接污染默认 `~/.lainclaw`：
  - `HOME=/tmp/verify-lainclaw-local`
- 使用固定状态与日志路径避免互相干扰：
  - `LOCAL_STATE=/tmp/lainclaw-local-gateway.pid`
  - `LOCAL_LOG=/tmp/lainclaw-local-gateway.log`

## 验收步骤（AI 执行序列）

1. 编译检查
   - 在 `src/lainclaw` 目录执行 `npm run build`。
   - 通过条件：命令返回码 0，产物在 `dist/`。

2. 验证 auth 可用
   - 执行 `node dist/index.js auth status`。
   - 通过条件：输出包含 `openai-codex/...` 的 profile 及 `expires`。

3. 启动 local 网关（daemon）
   - 执行（带上 `HOME`）:
   - `HOME=<指定隔离目录> npm start -- gateway start --channel local --daemon --provider openai-codex --pid-file $LOCAL_STATE --log-file $LOCAL_LOG`
   - 通过条件：日志包含 `gateway service started as daemon` 且有 pid。

4. 状态校验
   - 执行：`HOME=<同上> npm start -- gateway status --channel local --pid-file $LOCAL_STATE`
   - 通过条件：输出 JSON 的 `status` 为 `running`，`channel` 为 `local`，`pid` 非空。

5. 写入本地消息（inbox）
   - 目标路径：`<HOME>/.lainclaw/local-gateway/local-gateway-inbox.jsonl`
   - 追加一行 JSON，例如：
     ```json
     {"requestId":"local-verify-001","input":"请用3个字说明今天的天气","sessionKey":"local-acceptance"}
     ```
   - 通过条件：命令返回成功，文件追加成功。

6. 观察回执（outbox）
   - 目标路径：`<HOME>/.lainclaw/local-gateway/local-gateway-outbox.jsonl`
   - 等待 1-3 秒后读取新增行。
   - 通过条件：出现至少一条 JSONL，满足：
     - `channel == "local"`
     - `requestSource == "local-verify-001"`
     - `sessionKey == "local-acceptance"`
     - `output.success == true`
     - 有 `output.result` 内容。

7. 停止服务
   - 执行：`HOME=<同上> npm start -- gateway stop --channel local --pid-file $LOCAL_STATE`
   - 通过条件：输出 `gateway service stopped (pid=...)`。

8. 二次状态确认
   - 执行：`HOME=<同上> npm start -- gateway status --channel local --pid-file $LOCAL_STATE`
   - 通过条件：输出 JSON 的 `running == false` 且 `pid == null`。

9. 结果回填
   - 输出 `session`、`statePath`、`logPath`、`pid` 与回执 `requestId`。
   - 记录异常日志：如超时则抓取 `LOG` 与 `outbox` 内容。

## 异常排查
- 如果状态显示 `stopped`，检查 daemon 子进程是否退出（`ps -p <pid>`）。
- 如果回执未生成：
  - 检查 inbox 文件是否存在并成功追加。
  - 检查 outbox 路径是否是 `<HOME>/local-gateway/local-gateway-outbox.jsonl`。
  - 检查日志中是否有请求解析或 `No openai-codex profile` 的错误。

## 通过定义
- `gateway start/status/stop --channel local` 全链路可执行。
- 一次真实输入可产出 outbox 回执且成功。
- 停止后服务变为 stopped，state 不再留存。

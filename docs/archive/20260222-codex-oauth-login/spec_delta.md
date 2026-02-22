# Spec Delta: Codex OAuth Login Integration

## ADDED Requirements

- [SOT] `lainclaw` 登录状态与认证 profile 存储根目录为 `~/.lainclaw/`，至少包含 `auth-profiles.json` 用于持久化 Codex profile 与 token。
- [SOT] CLI 增加认证命令子树：
  - `lainclaw auth login openai-codex`
  - `lainclaw auth status`
  - `lainclaw auth logout [--all | <profile>]`
  - `lainclaw auth use <profile>`
  以上命令为用户可见交互合同；不登录时 `ask` 调用 Codex 的请求应返回可操作错误提示。
- [SOT] `ask` 继续保留当前离线 stub 行为；新增 `--provider openai-codex --profile <name>`（可选）时尝试走认证 token；未登录/失效 token 时提示先执行 `auth login`。
- [ARCHIVE-ONLY] 默认 profile 存储与切换逻辑应支持内存缓存，避免重复读磁盘。
- [ARCHIVE-ONLY] 认证持久化文件应避免明文泄露，优先使用 `600` 权限写入（如环境不支持则降级提示）。
- [CANDIDATE] 在 WIP 验证通过后再引入 `LAINCLAW_HOME` 环境变量覆盖根目录。

## MODIFIED Requirements

- [SOT] `gateway -> pipeline -> adapter` 职责不变：
  - `gateway` 仅建立请求上下文。
  - `pipeline` 决定路由（stub/codex）。
  - `adapter` 负责实际模型调用与鉴权上下文消费。
- [ARCHIVE-ONLY] `ask` 输出结构保留现有 `route/stage/result` 形态，扩展 `provider/profile` 元数据（若适配器返回）。
- [ARCHIVE-ONLY] OAuth 登录流程优先复用既有 OAuth-capable 库；如不可用则回退为手工 code 录入体验。

## REMOVED Requirements

- [ARCHIVE-ONLY] 暂不移除任何现有命令；`--help`、`--version` 与 `ask` 基础语义保持兼容。

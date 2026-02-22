# Intent: Integrate Codex OAuth login for MVP

## 背景
当前 `src/lainclaw` 是最小 CLI + stub pipeline，缺少真实认证与远端模型接入能力。业务方希望接入 Codex 登录，且需要体验稳定、架构整洁。

## 目标
1. 在 CLI 中提供可用的 Codex 登录入口（不阻塞现有 `ask` 离线 stub）。
2. 使用 `~/.lainclaw/` 作为本地认证与配置的统一根目录。
3. 建立最小 Profile 能力：登录、查看状态、切换默认 profile、登出。
4. 在 `ask` 流程中支持带认证调用（可选）与缺失认证时的清晰提示。

## 非目标
1. 不在本阶段重写 OAuth 协议实现（采用现成库能力）。
2. 不进行跨平台系统密钥链深度集成（先落盘文件实现）。
3. 不在本期支持多供应商统一认证抽象（仅围绕 Codex 登录与调用闭环）。

## 验收标准
1. 用户执行 `lainclaw auth login openai-codex` 后可完成 OAuth 并在本地生成 `~/.lainclaw/auth-profiles.json`。
2. `ask` 可在未登录时返回明确提示；登录后可基于 profile 发起调用（先兼容 stub）。
3. 支持 `status` 查看当前 profile 与 token 有效性。
4. 新增命令不影响现有 `ask` 默认离线行为。

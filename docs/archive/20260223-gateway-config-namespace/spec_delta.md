# Specification Delta: Gateway Config Namespace Clarity (No Behavior Change)

## ADDED Requirements

- [ARCHIVE-ONLY] 在 `src/lainclaw/src/channels/feishu/config.ts` 明确注释与命名，使 `channel` 与 `provider` 的关系可读：`channel` 只用于缓存文件分片（`<channel>-gateway.json`），不表示配置项嵌套关系。
- [ARCHIVE-ONLY] 向 `src/lainclaw/src/channels/feishu/config.ts` 暴露 `resolveFeishuGatewayConfigPath(channel)`，用于让 CLI/工具在提示里展示实际落盘路径。
- [ARCHIVE-ONLY] 在 `src/lainclaw/src/cli/cli.ts` 中，`gateway config show` 输出中新增 `channel` 与 `configPath`，提高排障可见性。

## MODIFIED Requirements

- [ARCHIVE-ONLY] 调整 `validateFeishuGatewayCredentials` 与 `gateway config` 相关提示文案为“当前频道网关缓存文件”，避免将 `feishu-gateway.json` 误解为 provider 或 channel 子结构。
- [ARCHIVE-ONLY] 更新 `src/lainclaw/README.md` 中 `gateway config` 说明：默认文件按 channel 分片，`provider` 与其他参数属于同级网关配置。

## REMOVED Requirements

- [CANDIDATE] 无。

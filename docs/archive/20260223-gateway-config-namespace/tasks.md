# Execution Checklist

- [x] 在 `src/lainclaw/src/channels/feishu/config.ts` 添加语义注释，并暴露 `resolveFeishuGatewayConfigPath(channel)`。
- [x] 在 `src/lainclaw/src/cli/cli.ts`：
  - [x] 将错误提示改为“频道网关缓存文件”并使用实际路径。
  - [x] 在 `gateway config show` 中输出 `channel` 与 `configPath`。
- [x] 在 `src/lainclaw/README.md` 中修正文案，明确 channel 分片与 provider 平级的配置语义。
- [x] 复核改动未改动行为路径（仅文案/展示/帮助层面）。

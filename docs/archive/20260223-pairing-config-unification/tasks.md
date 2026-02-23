# Execution Checklist

- [x] 在备份目录备份 `~/.lainclaw` 并确认包含 `feishu-pairing.json`、`feishu-allowFrom.json` 与当前 `gateway.json`。
- [x] 将旧 `~/.lainclaw/feishu-pairing.json` 数据合并到 `~/.lainclaw/gateway.json.pairing.channels.feishu.requests`。
- [x] 将旧 `~/.lainclaw/feishu-allowFrom.json` 数据并入 `~/.lainclaw/gateway.json.pairing.channels.feishu.allowFrom`。
- [x] 清理 `~/.lainclaw/feishu-pairing.json` 与 `~/.lainclaw/feishu-allowFrom.json`。
- [x] 运行 `gateway config set --channel feishu --pairing-allow-from ...` 验证字段可覆盖并落盘。
- [x] 运行 `gateway start/status/stop --channel feishu` 进行端到端守护链路复核。

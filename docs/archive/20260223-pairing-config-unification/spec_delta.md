# Specification Delta: Pairing Persistence Unification

## ADDED Requirements
- [CANDIDATE] 备份目录约定：默认将本次迁移快照输出到 `~/.lainclaw-backup-<iso>`。

## MODIFIED Requirements
- [SOT] 飞书配对持久化模型统一为 `~/.lainclaw/gateway.json` 的顶层 `pairing`：
  - `pairing.version`
  - `pairing.channels[<channel>]`
  - 每个 channel 下可包含 `requests` 与 `allowFrom`。
- [SOT] 配对运行时在无兼容分支时以 `gateway.json` 为唯一持久化源。
- [ARCHIVE-ONLY] 清理旧文件时可移除 `~/.lainclaw/*-pairing.json` 与 `~/.lainclaw/*-allowFrom.json`（不作为对外契约）。

## REMOVED Requirements
- 无

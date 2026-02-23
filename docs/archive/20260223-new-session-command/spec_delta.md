# Specification Delta: `/new` direct session command

> **目标 SOT 文件**: `./docs/sot/architecture.md`（本阶段不直接修改 SOT，仅用于批准后实现）

## ADDED Requirements

### [SOT] `/new` slash command
The system **SHALL** interpret trimmed input exactly equal to `/new` as a session reset command.

### [SOT] New session behavior
When input is `/new`, the system **SHALL**:
1. call `getOrCreateSession` with the current `sessionKey` and `forceNew: true`;
2. create a new `sessionId` for that `sessionKey` while keeping the `sessionKey` unchanged;
3. return `success=true` with the new `sessionId`.

### [SOT] Command handling path
For `/new`, the system **SHALL** bypass normal pipeline/tool handling and return a session-reset acknowledgement result directly.

### [ARCHIVE-ONLY] `/new` help text
`README` / command help **MAY** add a `/new` usage note for channel/gateway输入，但不改变现有 CLI flag 文案。

## MODIFIED Requirements

### [ARCHIVE-ONLY] Gateway command parity
`runAsk` 在网关通道入口层会通过统一命令分支处理 `/new`，以保持 CLI 与网关行为一致。

## REMOVED Requirements

### [CANDIDATE] 无

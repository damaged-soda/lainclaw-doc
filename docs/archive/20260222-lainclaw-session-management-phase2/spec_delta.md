# Specification Delta: Memory V2 (ask --memory + Compaction + Memory File)

> **目标 SOT 文件**: `./docs/sot/architecture.md`（本阶段不直接修改 SOT，仅用于批准后实现）
> **基于意图**: `intent.md`

## ADDED Requirements

### [SOT] 会话级 memory 开关
The system SHALL expose an `ask` option `--memory` to enable memory mode for the target session.

1. `ask` SHALL accept `--memory`, `--no-memory`, and `--memory=<on|off>` forms.
2. When `--memory` is set to `on`, the session SHALL be marked as memory-enabled and memory metadata SHALL persist in the session catalog.
3. When `--memory` is set to `off`, memory mode SHALL be disabled for that session.

### [SOT] 长期记忆落盘
The system SHALL persist compacted memory as plain markdown:

1. Memory file path: `~/.lainclaw/memory/<sessionKey>.md`.
2. Each compaction writes a readable summary block into this file.
3. `ask` SHALL read the memory file (tail-limited) and inject it into the request context as system messages when memory mode is enabled.

### [SOT] 简化 compaction 策略
The system SHALL run a deterministic memory compaction pass in local state:

1. compaction runs when total transcript turns exceed a stable threshold (MVP: 24).
2. keep the latest N turns (MVP: 12) in immediate context for replay, and summarize older turns into memory file.
3. each successful compaction SHALL record progress in session catalog metadata so summaries are not duplicated.
4. compaction does not remove transcript lines.

### [SOT] 结构化输出
`GatewayResult` SHALL include memory metadata:

1. whether memory is enabled;
2. whether memory compaction was executed for this turn;
3. optional memory file path (for debugging).

### [ARCHIVE-ONLY] memory 摘要内容风格
The concrete summary wording is [ARCHIVE-ONLY] and may evolve with UX iteration.

## MODIFIED Requirements

### [SOT] Context payload extension
`RequestContext.messages` SHALL include memory injection messages when memory is enabled, while preserving historical message re-use semantics from phase-1.

## REMOVED Requirements

### [CANDIDATE] None

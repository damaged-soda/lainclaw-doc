# Specification Delta: Session Memory V1 (Persistent Session IDs + Replayable Context)

> **目标 SOT 文件**: `./docs/sot/architecture.md`（本阶段不直接修改 SOT，仅用于批准后实现）  
> **基于意图**: `intent.md`

## ADDED Requirements

### [SOT] Session-aware ask contract
The system **SHALL** support session-scoped interaction:

1. `lainclaw ask` **SHALL** accept `--session <name>` to route input into a persistent session.
2. On first use of a `session` key, the system **SHALL** create a persistent `sessionId`.
3. On reuse, the system **SHALL** load recent session transcript as context and pass it to `pipeline`.

### [SOT] Session transcript persistence
The system **SHALL** persist every successful `ask` turn into `~/.lainclaw/sessions/<sessionId>.jsonl` with at least:

1. one user message entry for current input;  
2. one assistant message entry for model/stub output.

### [SOT] Session identifier output
The system **SHALL** return `sessionKey` and `sessionId` in `ask` structured output so users can locate and reuse a session.

## MODIFIED Requirements

### [SOT] Context-aware context construction
The system **SHALL** adapt gateway context construction so `RequestContext` includes historical messages for context replay in same session, while keeping existing prompt + route semantics.

### [ARCHIVE-ONLY] CLI help hint
`--help` text **MAY** display session-related flags (`--session`, `--new-session`) and continue to keep current `ask` default usage.

## REMOVED Requirements

### [CANDIDATE] None

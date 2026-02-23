## ADDED Requirements

- [SOT] LainClaw MUST introduce a DM policy switch for Feishu channels with values: `open`, `allowlist`, `pairing`, `disabled`.
- [SOT] In `pairing` mode, inbound DM requests MUST be intercepted before `runAsk` and denied when sender is not paired.
- [SOT] In `pairing` mode, paired senders MUST be granted a deterministic allow path to continue execution.
- [SOT] LainClaw MUST persist pairing state with account-scoped isolation and sender allowlist reuse semantics.
- [SOT] Pairing persistence MUST support at least: creation time, expiration time, status, approver, and sender/account relation.
- [SOT] LainClaw MUST provide CLI commands to inspect pending pairing requests and to approve/revoke pairings.
- [SOT] LainClaw MUST pass automated tests for: unpaired blocking, approved pass-through, expired pairing rejection, and cross-account isolation.
- [ARCHIVE-ONLY] Pairing response texts SHOULD reuse OpenClaw wording patterns where non-technical copy is not part of contract.
- [ARCHIVE-ONLY] Storage may be file-based in MVP (no DB dependency) if atomic-write and cleanup behavior are ensured.
- [CANDIDATE] Store adapter shape MAY align with OpenClaw pairing abstractions for later cross-channel reuse.

## MODIFIED Requirements

- [SOT] `src/lainclaw/src/channels/feishu/server.ts` MUST call a pairing gate before calling `runAsk`.
- [SOT] `src/lainclaw/src/channels/feishu/config.ts` MUST expose configuration for pairing policy, pairing TTL, and storage location/retention.
- [SOT] `src/lainclaw/src/cli/cli.ts` MUST add a dedicated pairing command group and wiring for list/approve/revoke.
- [ARCHIVE-ONLY] README and operational docs SHOULD be updated with strategy defaults and CLI usage examples.

## REMOVED Requirements

- [ARCHIVE-ONLY] Existing behavior where Feishu DM always enters execution path without sender authorization under `pairing` policy (this behavior is replaced by explicit pair gate).
- [ARCHIVE-ONLY] Any previous implicit assumptions that pending pairing state can be shared across accounts (all storage must be account-scoped).

## Acceptance Criteria (for VERIFY)

- [SOT] 功能验收：
  - 在 `pairing` 策略下，未配对 sender 不得进入 `runAsk`。
  - 已批准 sender 在 TTL 有效期内可正常进入 `runAsk`。
  - `revoke` 后该 sender 立即失效。
- [SOT] 安全验收：
  - 不同 account 的 pairing 与 allowFrom 完全隔离。
  - `pairing` 关闭/切换后，非配对模式下行为应回归当前默认路径。
- [SOT] 边界验收：
  - 过期 pending 拒绝审批且可自动清理或被动清理后恢复可请求状态。
  - 并发/重复请求不出现重复批准风险；审批码不冲突。
- [SOT] 工程验收：
  - 新增测试覆盖关键路径。
  - 文档说明可复现配对流程与 CLI 命令。
  - 提供命令与日志证据，支持逐项对应 `intent.md` 验收标准。

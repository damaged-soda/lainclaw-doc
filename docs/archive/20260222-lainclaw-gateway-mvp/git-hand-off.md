# Git 提交与推送记录（预案）

## 1) 记录位置
- 该文档位于：`docs/archive/20260222-lainclaw-gateway-mvp/git-hand-off.md`
- 用于保存两仓库的 commit message 预览与复核指令，避免与源代码内容混在一起。

## 2) 计划中的根仓库提交（文档与归档）
- 预估变更范围：
  - `docs/sot/architecture.md`
  - `docs/sot/overview.md`
  - `docs/archive/20260222-lainclaw-gateway-mvp/intent.md`
  - `docs/archive/20260222-lainclaw-gateway-mvp/spec_delta.md`
  - `docs/archive/20260222-lainclaw-gateway-mvp/tasks.md`
  - `docs/archive/20260222-lainclaw-gateway-mvp/git-hand-off.md`

- 备选提交信息（预览）：
  - `chore(docs): archive gateway MVP and update SOT with CLI/ask contract`

## 3) 计划中的子仓库提交（src/lainclaw 实现）
- 预估变更范围：
  - `src/lainclaw/src/index.ts`
  - `src/lainclaw/src/cli/cli.ts`
  - `src/lainclaw/src/gateway/askGateway.ts`
  - `src/lainclaw/src/pipeline/pipeline.ts`
  - `src/lainclaw/src/adapters/stubAdapter.ts`
  - `src/lainclaw/src/shared/types.ts`
  - `src/lainclaw/package.json`
  - `src/lainclaw/tsconfig.json`
  - `src/lainclaw/README.md`

- 备选提交信息（预览）：
  - `feat(cli): scaffold MVP ask flow with in-process gateway and stub adapter`

## 4) 操作命令模板（请在确认后按顺序执行）
- 根仓库：
  - `git add <上面列出的根仓库文件>
  - `git commit -m "chore(docs): archive gateway MVP and update SOT with CLI/ask contract"`
  - `git push`

- 子仓库：
  - `cd src/lainclaw`
  - `git add <上面列出的子仓库文件>
  - `git commit -m "feat(cli): scaffold MVP ask flow with in-process gateway and stub adapter"`
  - `git push`


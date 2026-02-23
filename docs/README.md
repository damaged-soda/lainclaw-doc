# 文档目录（Doc Root）

本目录是 `Lainclaw` 项目的定义空间（文档驱动开发），用于沉淀稳定事实、执行中的提案与完成归档。

- 当前事实（SOT，唯一权威）：`./sot/`
- 进行中需求（WIP）：`./wip/YYYYMMDD-topic/`
- 已完成归档（Archive）：`./archive/YYYYMMDD-topic/`

建议流程：
- Proposal 立项：在 `./docs/wip/` 建立 `YYYYMMDD-<topic>/` 并产出 `intent.md / spec_delta.md / tasks.md`
- 用户确认：审核 `spec_delta.md` 后进入实现
- 实现：仅修改实现前已批准范围内的文件
- 验收/归档：完成自验后进入归档
- 归档：`WIP` 目录移至 `./docs/archive/`

持续复用文档：
- 验收与本地闭环手册：`acceptance/agent-local-channel-acceptance.md`

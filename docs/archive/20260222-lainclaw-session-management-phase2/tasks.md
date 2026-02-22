# Tasks: Memory V2 (ask --memory + compaction)

> **关联**: `spec_delta.md`
> **执行状态**: [x] 已完成（实施 + 自验完成）

## 原子化任务

- [x] [SOT] 扩展会话 catalog 元数据，支持 memory 开关与 compaction 进度
  - [完成] `sessionStore` 支持 memory 元数据、memory 文件路径、读取摘要与追加摘要、metadata 更新。
  - 扩展 session 记录：`memoryEnabled`, `compactedMessageCount`。
  - 提供 memory 文件路径解析、读取（tail 限制）、追加摘要与 metadata 更新。
- [x] [SOT] 扩展 `ask` CLI 选项
  - [完成] CLI 已支持 `--memory`, `--no-memory`, `--memory=on|off`，并透传到 `runAsk`。
  - 支持 `--memory`, `--no-memory`, `--memory=<on|off>`。
  - `ask` 输出返回 `memoryEnabled` 与 `memoryUpdated`。
- [x] [SOT] `runAsk` 增加 memory 注入与 compaction
  - [完成] `askGateway` 在上下文前注入 memory，满足阈值后写入 `~/.lainclaw/memory/<sessionKey>.md` 并更新 compaction 进度。
  - 在上下文构造前注入 memory 内容为 `system` 消息。
  - 成功应答后，根据阈值写入 `~/.lainclaw/memory/<sessionKey>.md` 摘要。
- [x] [SOT] 文档更新（本地文档）
  - [完成] `src/lainclaw/README.md` 增补二阶段长期记忆与 compaction 使用示例。
  - 更新 `src/lainclaw/README.md`，补充二阶段 memory 用法和 `--memory`。
- [x] [ARCHIVE-ONLY] 可选美化（未执行）
  - [未执行] 记录短语与摘要样式在本阶段先不调整，保留为后续迭代优化。

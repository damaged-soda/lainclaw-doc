# AI Agent 验收指南：`/new` 会话命令（Gateway Ask）

## 目标
确认 `ask` 输入 `/new` 能直接重置会话上下文。

## 前置条件
- 已在 `src/lainclaw` 执行过构建（`npm run build`）并可运行 `node dist/index.js ask`。
- 使用独立 `HOME`，避免污染默认会话目录。

## 推荐约束
- 使用隔离目录：`HOME=/tmp/verify-lainclaw-session`。
- 使用固定会话键：`--session acceptance-new`。

## 验收步骤（AI 执行序列）

1. 准备环境
   - `export HOME=/tmp/verify-lainclaw-session`
   - `rm -rf "$HOME/.lainclaw" && mkdir -p "$HOME/.lainclaw"`

2. 第一次发言
   - 执行：`node dist/index.js ask --session acceptance-new 这是第一条消息`
   - 通过条件：返回 JSON，记录 `sessionId` 为 `sessionId-1`。

3. 发送上下文后续消息
   - 执行：`node dist/index.js ask --session acceptance-new 这是第二条消息`
   - 通过条件：返回 JSON，`route`/`stage` 正常，且返回内容可见上下文被读取（若使用 stub，可以观察到返回上下文条数不再为 0）。

4. 发起 `/new`
   - 执行：`node dist/index.js ask --session acceptance-new /new`
   - 通过条件：返回 `success=true` 且包含新的 `sessionId`（`sessionId-2`，与上一步不同）。

5. 验证上下文已重置
   - 执行：`node dist/index.js ask --session acceptance-new 重置后第一条消息`
   - 通过条件：返回内容显示上下文计数回到初始（stub 下通常可见 context 计数变化），`sessionId` 应为 `sessionId-2`。

6. 结果回填
   - 记录三次 `sessionId` 与对应 `requestId`，并说明第一、二条在同一 `sessionId`，`/new` 后会切换到新 `sessionId`。

## 通过定义
- `/new` 输入返回成功并产生新 `sessionId`。
- `/new` 后续消息继续沿用同一 `sessionKey`，但不复用旧 `sessionId` 的上下文。

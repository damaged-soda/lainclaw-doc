# Verify Record: Memory V2

> 归档时间: 2026-02-22

## 验收结论
- 阶段三关键路径验收通过。
- 已验证 `--memory` 参数三态解析、开关持久化、compaction 与 memory 文件落盘。

## 验证命令
- `cd src/lainclaw && npm run build`
- `node ./dist/index.js ask --session testmem --memory '...'`
- `node ./dist/index.js ask --session testmem --no-memory '...'`
- `node ./dist/index.js ask --session testmem --memory=on/off
- `node ./dist/index.js ask --session compact-test --memory "compact 测试消息 X"`（循环 26 次）
- `node ./dist/index.js ask --session testmem --memory=maybe 'should fail'`

## 结果要点
- `memoryEnabled` 开关在输出中与用户命令一致
- `--memory=maybe` 返回错误并退出码 `1`
- `~/.lainclaw/memory/compact-test.md` 创建并追加多次 `Memory Summary`
- `sessions.json` 中对应会话 `memoryEnabled` 与 `compactedMessageCount` 有持久化更新

# Spec Delta

## ADDED Requirements

- 无新增外部接口。

## MODIFIED Requirements

- [ARCHIVE-ONLY] `askGateway` 必须在构建 `RequestContext` 前读取/构造统一的 workspace 上下文（包含 `workspaceDir`、`当前时间`、关键文件角色与顶层文件清单），并将该上下文组装为请求系统提示，写入 `RequestContext.systemPrompt`。
- [ARCHIVE-ONLY] `heartbeat/runner.ts` 不再自行拼装 workspace 上下文；改用共享上下文构造器产出的 workspace 摘要。
- [ARCHIVE-ONLY] `RequestContext` 增加可选 `systemPrompt` 字段，`codexAdapter` 使用该字段替代固定默认提示；若为空则回退到既有默认提示。

## REMOVED Requirements

- 无新增/移除的用户可见命令。

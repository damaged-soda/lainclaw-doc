# Tasks

- [x] 新建共享上下文构建模块（workspaceDir 解析、关键文件探测、未知顶层条目扫描、时间注入、summary 生成）。  
- [x] 新增 `RequestContext.systemPrompt?: string`，并让 `codexAdapter` 使用该字段。  
- [x] 在 `askGateway` 中复用共享上下文构建器，注入 `systemPrompt`（默认 prompt + context）。  
- [x] 将 heartbeat 的上下文拼装替换为共享构建器输出。  

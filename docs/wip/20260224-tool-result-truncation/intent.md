# 减少工具输出导致的 ask 超时（文档化方案）

## 背景
- 最近一次 Feishu ask 审计日志中，最后一条请求出现 `ask timeout after 10000ms`，且 `route/stage` 为空。
- 排查显示网关在执行 ask 时会对工具返回内容直接做 `JSON.stringify` 后注入模型上下文，缺少体积与结构裁剪。
- 当某次对话触发大结果（例如 `listDir` 返回深层目录树）时，超时很可能由上下文体积突增导致。

## 目标
- 约束工具返回注入模型上下文的大小，避免大结果触发 ask 超时。
- 在不丢弃关键语义信息的前提下，保留问题可解释的摘要与可恢复性。
- 将超时来源与工具结果体积的关系做成可持续策略，而非单次人工降级。
- 提供 `gateway start --debug` 启动选项，在当前 stdout 输出 `gateway-audit` 审计记录，便于在线排障。
- 简化审计开关：不再依赖 `LAINCLAW_PROMPT_AUDIT_PATH` 类环境变量，仅以启动参数和内建回退策略驱动调试输出与落盘。

## 非目标
- 不改变 ask 现有鉴权、路由、调用链主流程。
- 不调整业务策略（如是否展示完整 `listDir` 清单）为用户侧功能。
- 不改动 openclaw 侧代码。

## 验收标准
- 工具返回内容进入 prompt 前有统一裁剪/摘要策略，避免单次超大 payload 直接注入。
- 发生裁剪时上下文中可明确看到提示，并保留可重试/继续查询线索。
- 相同规模数据下，ask 响应仍按 `REPLY_TIMEOUT_MS=10000ms` 限制，并能观测到该策略是否生效（例如记录原始/裁剪状态）。
- 异常/超时日志必须保留可追踪输入规模与错误类型字段，即使缺少 `route/stage`，也能判断是否为输入爆炸类问题。
- `gateway start --debug` 能在终端实时看到完整 `ask` 审计记录（至少含 `success/requestId/route/stage/result/error/toolCalls/toolResults` 核心段和输入元信息），不依赖文件打开。

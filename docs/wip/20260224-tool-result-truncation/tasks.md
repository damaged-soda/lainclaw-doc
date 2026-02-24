# 执行清单（PROPOSAL）

- [ ] 与你确认这份 `spec_delta.md`：默认裁剪上限、裁剪后摘要字段、是否引入 `[CANDIDATE]` 的可观测指标项。
- [ ] 确认异常日志字段命名与输出策略（`inputLength`/`inputChecksum`/`inputPreview`/`errorKind`/`auditStage`），确保不会再次出现“失败行缺关键上下文”。
- [ ] 制定工具结果注入的统一裁剪契约（字段命名、长度上限、保留项优先级）。
- [ ] 评估 `listDir` 等高风险工具的默认返回上限与返回格式（只在说明书内，不改实现代码）。
- [ ] 设计 fallback 行为：当工具结果超限时，模型上下文附带何种“继续查询”提示最小可用。
- [ ] 设计 `gateway start --debug` 的输出模式：只打印完整 `askAudit.record` 到 stdout，不写入敏感日志通道、不影响 daemon 子进程默认行为。
- [ ] 确定 `LAINCLAW_PROMPT_AUDIT_PATH` 的去除策略：对应实现代码移除该 env 分支并补充默认/备用路径行为不变说明。
- [ ] 落地实现任务分解并逐项执行（仅在 `spec_delta` 确认后进行）。

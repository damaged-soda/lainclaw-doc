# 架构说明（SOT）

Last Updated: 2026-02-22

## 模块边界（按 repo 或关键模块描述）

- `src/lainclaw`：轻量版运行时与交互层（首次迭代可从单一模块起步）
- `docs`：单一事实源（SOT）与 WIP 过程信息分离
- `src/lainclaw` 的执行路径采用 `cli -> gateway -> pipeline -> adapter` 三层职责分离，便于后续替换模型后端。

## 关键约束 / 不变量
- 文档事实必须先于代码行为定义：任何对外行为优先通过 `spec_delta` 阐明
- WIP 与 SOT 分离：计划过程不进入 SOT，SOT 只承载长期稳定契约
- 在同一功能方向内保持最小可验证实现，避免一次性堆叠过多模块依赖
- 文档驱动仓库映射必须可达：`docmap.yaml` 中 `repos` 的每个 `path` 应可被构建/链接流程访问。
- 对外 CLI 合同以 `--help`、`ask <input>` 为 MVP 最小可用入口；空输入 `ask` 需要返回明确错误与用法提示。
- `gateway` 需要建立可复用上下文（`requestId`、`createdAt`、原始输入），并通过 pipeline 统一产生结构化输出，便于后续替换模型供应商。
- 结构化执行结果（含 `route`、`stage`、`result`）属于可观测输出约定，MVP 期间可用 stub 响应替代真实模型。

## 跨 repo 交互（如适用）

- 当前仅单仓库起步：`Lainclaw` 无跨仓库依赖
- 架构演进遵循文档驱动的渐进扩展
- CLI 入口约定：`src/lainclaw/dist/index.js` 为可执行入口，`npm link` 后应提供 `lainclaw` 全局命令。

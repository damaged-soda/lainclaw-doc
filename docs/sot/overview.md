# 项目概览（SOT）

Last Updated: 2026-02-22

## 项目是什么

Lainclaw 是一个面向轻量级个人 AI 助理场景的项目立项方向，目标是先完成基础可用的文档化基线，再逐步演进能力边界。

## Repo 列表与职责（与 docmap 对齐）
- lainclaw：核心应用与交互层入口（`src/lainclaw/...`）
- docmap 映射：`docmap.yaml` 的 `repos` 路径必须始终与可达的本地仓库实际路径一致。
- `src/lainclaw` 的 CLI 目标是提供最小可验证入口：通过 `npm link` 可直接使用 `lainclaw` 命令。
- `src/lainclaw` 当前 MVP 接口采用 `lainclaw --help` 与 `lainclaw ask <input>`，支持离线 stub 流程与结构化输出回显。
- `src/lainclaw` 已支持 `openai-codex` 认证入口与调用闭环，认证文件固定在 `~/.lainclaw/auth-profiles.json`，并提供 profile 切换能力。
- `src/lainclaw` MVP 工具能力入口已对外稳定化：`tools` 命令与 ask 工具调用闭环作为最小工具链的一部分。

## 本地开发最小路径（只到开发自测）

- `src/lainclaw`：放置轻量版 Lainclaw 的启动代码和最小功能实现。
- `docs/`：变更提案与事实文档；`wip/` 中的计划仅在归档后保留。

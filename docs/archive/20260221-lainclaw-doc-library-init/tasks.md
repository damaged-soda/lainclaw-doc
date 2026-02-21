# Implementation Plan: 启动 Lainclaw 文档库（轻量版 + TypeScript HelloWorld 初始化）

> **关联 Spec**: `spec_delta.md`
> **执行状态**: [x] Verification

## Phase 1: 说明文件与映射修正

* [x] **Context Check**: 确认 `docs/README.md`、`docs/sot/index.md` 与 `docmap.yaml` 当前内容可被覆盖为 Lainclaw 定位说明。  
  备注：按启动要求已清点并确认可替换范围，未引入代码行为变更。
* [x] **Mapping Fix**: 将 `docmap.yaml` 的仓库映射条目与 `./src/lainclaw`（当前可达路径）对齐。  
  备注：已改为 `./src/lainclaw`，并创建 `src/lainclaw` 目录占位。
* [x] **Scope Tagging**: 本次变更覆盖文档初始化与 `src/lainclaw` 轻量代码初始化，未触及 openclaw 业务逻辑。
  备注：除 `src/openclaw` 既有代码外，仅新增 `src/lainclaw` 示例文件。

## Phase 2: SOT 初始化文本

* [x] **SOT 填充**: 更新 `docs/sot/overview.md`（项目简介、repo职责、最小开发路径）。  
  备注：已完成基础定位文本与最小路径说明。
* [x] **SOT 填充**: 更新 `docs/sot/architecture.md`（模块边界、关键约束、跨模块交互）。  
  备注：已完成模块边界与关键约束草案。
* [x] **SOT 索引**: 同步 `docs/sot/index.md` 的更新说明，保持 `Last Updated` 的可落地位置。  
  备注：已将更新时间与指向文件更新为当前版本。

## Phase 3: 模板与占位符清理

* [x] **Template polish**: 在 `docs/_templates/intent.md` 中去除或标准化 OpenClaw 风格占位语句。  
  备注：已移除过度占位，改为与 Lainclaw 对齐表述。
* [x] **Template polish**: 在 `docs/_templates/spec_delta.md` 中明确 `[SOT]/[ARCHIVE-ONLY]/[CANDIDATE]` 与章节结构。  
  备注：已补齐标签化 Requirement 模版。
* [x] **Template polish**: 在 `docs/_templates/tasks.md` 中清理示例任务占位。  
  备注：已改为阶段化执行清单。

## Phase 4: 自检与验收材料

* [x] **Sanity Check**: 重新扫描 `docs/**/*.md` 中显式 `TODO` 与明显占位符是否仍存在。  
  备注：SOT 与主说明文档已去除 OpenClaw/旧占位；模板文件与 WIP 中保留了可复用占位符供立项时填写。
* [x] **Conformity Check**: 与 `intent.md` 逐条对齐，确认验收标准满足。  
  备注：目标已覆盖到 docmap 与文档初始化文本。
* [x] **Report**: 准备提交给用户的执行结果摘要与下一步建议。  
  备注：完成，见本次回复。

## Phase 5: 代码初始化（TypeScript HelloWorld）

* [x] **Bootstrap**: 在 `src/lainclaw/src/index.ts` 增加 `helloWorld` 示例导出与 CLI 运行入口。
* [x] **Project Manifest**: 增加 `src/lainclaw/package.json` 作为独立最小可运行包的清单。

## Phase 6: 命令行入口

* [x] **CLI Script**: 使用 `src/lainclaw/dist/index.js` 作为直接命令行入口，支持直接执行并返回基础问候。
* [x] **Entrypoint Mapping**: 在 `src/lainclaw/package.json` 中声明 `bin.lainclaw`，用于后续环境下直接链接。
* [x] **Runner Alignment**: 在 `src/lainclaw/dist/index.js` 放置运行时入口，作为 `bin` 直接链接目标（openclaw 风格等价变体）。

## Phase 7: 使用方式补充

* [x] **Usage Note**: 在 `src/lainclaw/README.md` 中补充“直接命令行调用”示例。  
  备注：已新增最小 CLI 进入说明。
* [x] **Install Guide**: 在 `src/lainclaw/README.md` 中补充 `npm link` 全局安装与卸载步骤。
  备注：已补齐 `npm link` / `npm unlink -g` 的操作说明。

## Phase 8: 仓库边界对齐

* [x] **Scope Conformance**: 确认当前库仍仅作为文档库管理 `docs` 与过程文档，不追踪 `src` 下的内容（遵循现有 ignore 策略）。
  备注：未放开 `.gitignore`；`src/lainclaw` 保持为外部工作区内容，不纳入文档库 Git 管理。

## Phase 9: TypeScript 工具链修复

* [x] **Type Definitions**: 在 `src/lainclaw` 增加 `tsconfig.json`，将 `types` 设置为 `node`，并安装 `@types/node` 以修复 `Cannot find name 'process'` 报错。

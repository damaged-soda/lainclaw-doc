# Specification Delta: 启动 Lainclaw 文档库（轻量级版本）

> **目标 SOT 文件**: `./docs/sot/overview.md`, `./docs/sot/architecture.md`, `./docs/sot/index.md`, `./docmap.yaml`, `docs/_templates/*.md`
> **基于意图**: `intent.md`

## 1. 变更摘要 (Synopsis)

将文档库从旧占位符状态迁移为 Lainclaw 可用的初始化状态：
1) 统一项目命名和文档锚点，2) 修正仓库映射到可达路径，3) 将模板与 SOT 基本文件中的占位内容替换为可直接使用的初始文本。

## 2. 需求变更 (Requirements Delta)

### 🟢 ADDED Requirements

#### [SOT] Requirement: 项目定义层面在文档空间建立明确的 Lainclaw 定位

The system **SHALL** 在 `docs` 空间中明确项目名为 `Lainclaw`，并将 `docs` 说明改为面向该项目的定义空间。

##### Scenario: WIP 初始化后的文档定位可识别

* **GIVEN** 文档浏览者打开 `docs/README.md`
* **WHEN** 读取“文档库定义”与“变更路径”说明
* **THEN** 看到的是当前项目名与流程，不再使用泛化占位符。

### 🟡 MODIFIED Requirements

#### [SOT] Requirement: `docmap.yaml` 中仓库映射仅指向真实可达路径

The system **SHALL** 将 `docmap.yaml` 的仓库映射与当前仓库中的实际代码路径保持一致。

##### Impact Analysis (影响分析)

* 受影响的代码模块: docs only
* 是否需要数据迁移: No

#### [ARCHIVE-ONLY] Requirement: 模板文件可直接用于 Lainclaw 任务启动

The system **SHALL** 将模板中的显式 OpenClaw/占位符文本替换为可复用的中性表述，便于首次立项直接填写。

##### Impact Analysis (影响分析)

* 受影响的代码模块: docs only
* 是否需要数据迁移: No

#### [ARCHIVE-ONLY] Requirement: 清理 `docs/sot/*.md` 中的 `TODO` 与临时占位行

The system **SHALL** 用简要的项目说明填充 `overview.md` 与 `architecture.md`，使其不再包含 `TODO` 响应位。

##### Impact Analysis (影响分析)

* 受影响的代码模块: docs only
* 是否需要数据迁移: No

#### [ARCHIVE-ONLY] Requirement: `src/lainclaw` 启动 TypeScript HelloWorld

The system **SHALL** 在 `src/lainclaw` 下提供一个可读的 TypeScript HelloWorld 示例文件，作为轻量产品初始化的最小可交付开发样例。

##### Scenario: 轻量示例可复用

* **GIVEN** 开发者打开 `src/lainclaw`
* **WHEN** 查看 `src/lainclaw/src/index.ts`
* **THEN** 能直接看到并复用 `helloWorld` 示例函数。

##### Impact Analysis (影响分析)

* 受影响的代码模块: docs only
* 是否需要数据迁移: No

### 🟢 ADDED Requirements

#### [SOT] Requirement: Lainclaw 命令行入口可直接启动

The system **SHALL** 在 `src/lainclaw/dist/index.js` 提供可执行命令，支持从终端直接运行并输出欢迎文案。

##### Scenario: 直接命令行启动

* **GIVEN** 用户处于仓库可访问路径内
* **WHEN** 执行 `./src/lainclaw/dist/index.js`
* **THEN** 命令直接输出 hello 消息并返回 0。

##### Operational Notes (操作说明)

* **npm link 流程**: 开发者可在 `src/lainclaw` 执行 `npm link`，执行 `npm run build` 后可在任意目录直接调用 `lainclaw`。

### 🟡 MODIFIED Requirements

#### [ARCHIVE-ONLY] Requirement: 提供文档化的 npm link 使用指引

The system **SHALL** 在 `src/lainclaw/README.md` 记录 `npm link`/`npm unlink` 的完整操作步骤，以便快速启动。

##### Impact Analysis (影响分析)

* 受影响的代码模块: `src/lainclaw`（脚本层）
* 是否需要数据迁移: No

## 3. 数据结构/API 变更 (Schema/API Changes)

None。

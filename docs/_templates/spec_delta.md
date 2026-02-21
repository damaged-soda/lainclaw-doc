# Specification Delta: <变更标题>

> **目标 SOT 文件**: `./docs/sot/<对应文件名>.md`
> **基于意图**: `intent.md`

## 1. 变更摘要 (Synopsis)

一句话说明这次变更要影响系统的哪条长期约束与哪条实现细节。

## 2. 需求变更 (Requirements Delta)

### 🟢 ADDED Requirements (新增需求)

#### [SOT] Requirement: <需求名称>

The system **SHALL** <系统行为描述>.

##### Scenario: <场景名称> (Gherkin)

* **GIVEN**: <前置条件>
* **WHEN**: <触发动作>
* **THEN**: <预期结果>

---

### 🟡 MODIFIED Requirements (修改需求)

#### [ARCHIVE-ONLY] Requirement: <需求名称>

- OLD: <旧行为>
- NEW: <新行为>

##### Impact Analysis (影响分析)

* 受影响的代码模块: docs only
* 是否需要数据迁移: No

### 🔴 REMOVED Requirements (移除需求)

#### [CANDIDATE] Requirement: <被移除的需求>

* **Reason**: <删除原因>

## 3. 数据结构/API 变更 (Schema/API Changes)

None.

# Implementation Plan: Lainclaw Heartbeat MVP（自然语言提醒）

> **关联 Spec**: `spec_delta.md`
> **执行状态**: [x] Approved

## Phase 1: 准备与校验

* [x] Context Check: 核实 docmap、docs 目录与 SOT 约定。
* [x] Scope Lock: 仅落地“自然语言规则 + 定时模型裁决 + Feishu 通知” MVP。

## Phase 2: 执行

* [ ] 设计并实现心跳执行器 `heartbeat/runner.ts`。
* [x] 设计并实现心跳执行器 `heartbeat/runner.ts`。
* [x] 拓展 `heartbeat/store.ts` 与现有规则 CRUD 组合（补齐更新和持久化边界）。
* [x] 在 CLI 中加入 `heartbeat` 子命令及参数解析。
* [x] 扩展 Feishu 网关配置（可选 heartbeat 开关/间隔/接收者）。
* [x] 在 `runFeishuGatewayServer` 集成心跳启动/停止。
* [x] 补充错误与状态上报（输出规则触发/跳过、tool 错误）。

## Phase 3: 验收与归档

* [x] 更新 `src/lainclaw/README.md`（新增 heartbeat 命令与网关参数说明）。
* [x] 验证关键流程：规则增删改查、手工 run、网关定时触发。  
  (已完成：`npm run build` 通过，网关启动触发心跳路径联调通过，Feishu 权限与消息投递可闭环。)
* [x] 形成验收结论并回填 `intent.md`。  
  (已完成：用户端确认“验证通过”。)

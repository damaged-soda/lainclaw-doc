# Spec Delta: Gateway-first MVP scaffold (no model wiring)

## ADDED Requirements

### [SOT] CLI command contract for bootstrap flow
- 提供 `lainclaw` 全局可执行入口，保留现有命令入口规则：`src/lainclaw/dist/index.js`。
- CLI 必须支持以下子命令/参数：
  - `--help`：打印可用命令与参数。
  - `ask <input>`：触发网关流程，输出标准化结果。
- `ask` 命令若未提供 `<input>`，返回错误并包含可理解用法提示。

### [SOT] Gateway orchestration contract
- 引入 `gateway` 模块作为命令到执行路径的统一入口。
- `gateway` 必须负责：
  1. 输入校验（非空、类型与长度边界）。
  2. 组装最小上下文 `RequestContext`（请求 ID、时间戳、原始输入）。
  3. 调用一条可替换的“执行链”（例如 pipeline/router/handler）。
  4. 将执行结果标准化为 CLI 输出。
- `gateway` 的输入输出对象应可在后续替换模型供应商时复用。

### [SOT] Stub execution path
- `pipeline`（或等价编排层）默认使用“本地伪模型响应”实现，不依赖网络。
- 对于有效输入，返回结构化字符串（或 JSON 行）说明处理已到位：例如包含 `route`, `stage`, `result`。
- 该行为在后续接入真实模型后可替换，但 CLI 与网关约定不应改变。

### [ARCHIVE-ONLY] Module layout and naming
- 倾向采用 `src/lainclaw/src/cli`, `src/lainclaw/src/gateway`, `src/lainclaw/src/pipeline`, `src/lainclaw/src/adapters` 四层目录。
- 采用函数式模块边界，优先小文件和命名清晰。

### [ARCHIVE-ONLY] Development script polish
- 保持现有 `npm run build` 与 `node dist/index.js` 体验。
- 在 `README.md` 增加“gateway + ask 命令”快速验证示例（不改造构建链。

## MODIFIED Requirements

### [CANDIDATE] 错误提示风格
- 采用统一错误对象与友好文案（例如 `ValidationError` + `errorCode`），后续再决定是否升级为标准化 Error Schema。

## REMOVED Requirements

- 暂无移除项。

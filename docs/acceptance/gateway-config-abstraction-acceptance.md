# AI Agent 验收指南：`gateway config` 分层语义（default / channels）

## 目标
验证网关配置模型里 `default` 与 `channels` 的边界无歧义：
- `provider` 只能在 `default`；
- `appId/appSecret` 这类通道通信参数只能在 `channels.<name>`；
- 运行时按 `merge(default, channels[channel])` 生效。

## 推荐约束
- 建议使用隔离目录避免污染生产配置：
  - `HOME=/tmp/verify-lainclaw-gateway-config`
- 以隔离目录存在的 `gateway.json` 为准进行验证。

## 验收步骤（AI 执行序列）

1. 准备环境
   - 备份并清理网关配置文件：
     - `mkdir -p "$HOME/.lainclaw"`
     - `rm -f "$HOME/.lainclaw/gateway.json"`
   - 进入源码目录并编译：
     - `cd /home/leavan/work/lainclaw-doc/src/lainclaw`
     - `npm run build`

2. 写入默认配置（带 provider）
   - 执行：
     - `HOME=/tmp/verify-lainclaw-gateway-config npm start -- gateway config set --provider openai-codex --request-timeout-ms 12000`
   - 通过条件：输出 `gateway config updated`。

3. 写入通道配置（feishu）
   - 执行：
     - `HOME=/tmp/verify-lainclaw-gateway-config npm start -- gateway config set --channel feishu --app-id test-app --app-secret test-secret --heartbeat-enabled --heartbeat-target-open-id test-open-id`
   - 通过条件：输出 `gateway config updated`。

4. 验证禁止向通道层写 `provider`
   - 执行（应报错）：
     - `HOME=/tmp/verify-lainclaw-gateway-config npm start -- gateway config set --channel feishu --provider openai-codex`
   - 通过条件：命令返回错误，提示与 `provider` 相关（与 `gateway-level` 相关语义一致）。

5. 查看 `default` 配置
   - 执行：
     - `HOME=/tmp/verify-lainclaw-gateway-config npm start -- gateway config show`
   - 通过条件：
     - 输出包含 `provider` 且来源标注为 `default`；
     - `appId` / `appSecret` 不在 `default` 结果里。

6. 查看 `feishu` 通道配置
   - 执行：
     - `HOME=/tmp/verify-lainclaw-gateway-config npm start -- gateway config show --channel feishu`
   - 通过条件：
     - `appId` / `appSecret` 在结果中可见；
     - `provider` 来源为 `default` 或不存在于 override 中（`override` 不应出现）。

7. 检查持久化结果
   - 执行：
     - `cat "$HOME/.lainclaw/gateway.json"`
   - 通过条件：
     - 存在 `default.provider`；
     - `channels.feishu` 存在且包含 `appId/appSecret`；
     - `channels.feishu` 不包含 `provider`。

8. 清理通道覆盖
   - 执行：
     - `HOME=/tmp/verify-lainclaw-gateway-config npm start -- gateway config clear --channel feishu`
   - 通过条件：`gateway config cleared`，并确认 `channels.feishu` 被清空。

9. 清理（可选）
   - `rm -f /tmp/verify-lainclaw-gateway-config/.lainclaw/gateway.json`

## 通过定义
- `provider` 永久落在 `default`；
- `--channel` 仅影响通道覆盖层；
- `gateway config show` 和持久化文件与期望结构一致；
- 错误行为对违规 `--provider`/通道写入有明确拒绝。

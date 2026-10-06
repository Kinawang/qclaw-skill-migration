# QClaw 迁移遗留问题清单

> 适用版本：QClaw 0.3.0 → WorkBuddy（迁移日期 2026-10-06）
> 编写者：QClaw 迁移助手

本清单列出 QClaw → WorkBuddy 迁移过程中**未能自动完成**或**需要人工介入**的事项。请 WorkBuddy 团队或用户在合适时机处理。

---

## 一、未能迁移的会话（1 条）

| 项目 | 值 |
|---|---|
| 会话 ID | `e2f38e4b-6a4f-4b6f-9f45-ff56570bc682` |
| 所属专家 | Qclaw（main） |
| 源文件 | `/Users/wangshuxian/.qclaw/agents/main/sessions/e2f38e4b-6a4f-4b6f-9f45-ff56570bc682.jsonl` |
| 文件大小 | 7254 字节，6 行 |
| 失败原因 | 脚本 bug：`'utf-8' codec can't encode character '\ud83d' surrogates not allowed`。会话内容含未配对的 UTF-16 代理项字符（高概率是 emoji）。 |

**为什么不动它**：修复脚本会污染 WorkBuddy DB 已有数据，与用户"不删旧数据"约束冲突。

**怎么处理**：
- 在 QClaw 端手动 `iconv -f UTF-8 -t UTF-8//IGNORE` 清洗后重新触发会话迁移
- 或把这条 jsonl 整段原文粘贴到 WorkBuddy 主会话中，让 WorkBuddy 自行吸收

---

## 二、未迁移的定时任务（6 条 + 1 条新建）

### 1. Memory Dreaming Promotion ✅ 已计划迁移

| 项目 | 值 |
|---|---|
| 调度 | cron `24 1 * * *` (Asia/Shanghai) |
| 工作目录 | `/Users/wangshuxian/.workbuddy` |
| 模型 | 默认 |
| 投递 | 默认 |
| 提示词 | `__openclaw_memory_core_short_term_promotion_dream__` |

**状态**：migration-plan.json 中 `action=create_via_workbuddy_automation, state=planned`，等用户在确认页确认后由 WorkBuddy Automation 自动创建。

### 2. GBrain Dream Cycle ⏸ 需用户确认投递与工作目录

| 项目 | 值 |
|---|---|
| 调度 | cron `41 6 * * *` (Asia/Shanghai) |
| 所属专家 | main |
| 工作目录（建议） | `/Users/wangshuxian/WorkBuddy/main` |
| 投递 | 默认（无 announce） |
| 提示词 | "执行 GBrain Dream Cycle（夜间 brain 维护）。运行 `/Users/wangshuxian/.bun/bin/bun /Users/wangshuxian/gbrain/src/cli.ts dream --json`..." |
| 错误处理 | embed 阶段缺 `OPENAI_API_KEY` 可忽略；status `ok/clean/partial` 视为成功 |

**遗留原因**：脚本检测到两个未确认的转换（`drop_timeout_seconds`、`map_execution_environment`）。需要用户人工复核。

**怎么处理**：
1. 在 QClaw → WorkBuddy 迁移页打开"任务"分页
2. 找到 GBrain Dream Cycle
3. 把工作目录设为 `/Users/wangshuxian/WorkBuddy/main`
4. 确认无 announce 投递（GBrain 不需要外发通知）
5. 提交重建

### 3. 今日任务提醒（每小时） ⏸ 投递未映射

| 项目 | 值 |
|---|---|
| 调度 | every 3600s |
| 所属专家 | main |
| 已启用 | false |
| 提示词 | "用简洁温暖的语气提醒她今日待办：填报涉外律师申请表、提醒张律师看六干合同..." |
| 投递 | QClaw 模式 `announce`，WorkBuddy 需显式映射 |

**遗留原因**：投递模式 `announce` 在 WorkBuddy Automation 中无直接对应，需用户选择 WeChat/企业微信/邮件等渠道。

**怎么处理**：到 WorkBuddy 自动化页面把任务重建并选择通知渠道。

### 4. 六干案件进度跟进 ❌ 已过期

| 项目 | 值 |
|---|---|
| 调度 | at `2026-04-14T01:50:00.000Z`（单次，已过） |
| 所属专家 | main |

**遗留原因**：单次任务且执行时间已过去。脚本自动跳过。

**怎么处理**：无需迁移。如确需历史性重建，参考 migration-plan.json 中 `targetProjection` 字段。

### 5. 招标信息每日扫描 ⏸ 专家映射未找到

| 项目 | 值 |
|---|---|
| 调度 | cron `0 16 * * *` (Asia/Shanghai) |
| 所属专家 | agent-8e05976c（小虾） |
| 已启用 | true |

**遗留原因**：脚本在迁移时尝试找小虾专家的 WorkBuddy 映射，但当时小虾的 expert 文件尚未完成（断链问题未解决）。

**状态变更**：小虾 expert 文件已于 16:49 完成迁移（plugin.json 已落地）。但本次任务重建未自动重跑。

**怎么处理**：
1. 等 WorkBuddy 重启加载新专家
2. 重新打开任务确认页，对该任务点"重试"
3. 任务会重新生成 `targetProjection`，专家映射到 `/Users/wangshuxian/WorkBuddy/小虾`

### 6. 每日招标检查 ⏸ 投递未映射

| 项目 | 值 |
|---|---|
| 调度 | cron `0 10 * * *` (Asia/Shanghai) |
| 所属专家 | main |
| 已启用 | false |
| 提示词 | 招标类扫描任务 |
| 投递 | QClaw 模式 `announce` |

**遗留原因**：投递模式 announce 需用户显式映射。

**怎么处理**：到 WorkBuddy 自动化页面手动重建并选通知渠道。

### 7. a2a-poll-codex ❌ 不兼容

| 项目 | 值 |
|---|---|
| 调度 | every 20s |
| 所属专家 | uafru5gofdt644lm（游戏设计师） |
| 已启用 | false |
| 提示词 | "轮询A2A聊天室检查codex的新消息并回复..." |

**遗留原因**：脚本只支持"具有有效调度和 agentTurn message"的任务。该任务的 message 字段为空。

**怎么处理**：无需迁移。游戏设计师的会话已迁，如后续启用需手动用 WorkBuddy Automation 重建并填入完整提示词。

---

## 三、未迁移的连接器（6 条全部暂不支持）

| QClaw ID | QClaw 名称 | 状态 | 原因 |
|---|---|---|---|
| aippt | aippt | connected → unsupported | WorkBuddy 注册表无对应连接器 |
| ima | ima | connected → unsupported | WorkBuddy 注册表无对应连接器（WorkBuddy 侧有同类技能腾讯 ima） |
| lark-cli | lark-cli | connected → unsupported | WorkBuddy 注册表无对应连接器（WorkBuddy 侧有同类技能飞书套件 v2.3.0） |
| meituan-travel | meituan-travel | connected → unsupported | WorkBuddy 注册表无对应连接器 |
| wecom-cli | wecom-cli | connected → unsupported | WorkBuddy 注册表无对应连接器 |
| yuandian | yuandian | connected → unsupported | WorkBuddy 注册表无对应连接器（WorkBuddy 侧有同类技能 yuandian-law-search） |

**WorkBuddy 侧同名/同类技能**（已自动启用，可直接用）：
- ima → 腾讯 ima
- yuandian → yuandian-law-search
- lark-cli → lark-unified v2.3.0

**怎么处理**：等 WorkBuddy 连接器市场上架这 6 个，再让用户点"连接"重新授权。

---

## 四、QClaw 端凭据重新填写清单（仅位置，不含值）

WorkBuddy 侧重新填入以下凭据。所有值已留在 QClaw 端，迁移过程中**未带过来**（安全红线）。

| 凭据键 | 用途 | QClaw 中位置 |
|---|---|---|
| `models.providers.deepseek.apiKey` | DeepSeek 模型调用 | `~/.qclaw/openclaw.json` |
| `channels.wechat-access.token` | 微信通道 | `~/.qclaw/openclaw.json` |
| `gateway.auth.token` | 网关鉴权 | `~/.qclaw/openclaw.json` |
| `mcp.servers.mineru.env.MINERU_API_TOKEN` | mineru MCP 服务 | `~/.qclaw/openclaw.json` |
| `env.vars.CHINESELAW_API_KEY` | 元典法律检索 API | `~/.qclaw/openclaw.json` |

**怎么处理**：
1. 用户在 WorkBuddy 连接器管理页或模型配置页重新填入
2. 或在 WorkBuddy 主会话中说"填一下这些凭据：…" 并附上从 QClaw 端备份的密钥

---

## 五、其他待人工处理

### 1. 重启 WorkBuddy 加载新专家
小虾、Qclaw 等 6 个专家的 plugin.json 已落盘，但需要重启 WorkBuddy 客户端才会显示在专家列表中。

### 2. 重新调度任务
重启后第一次调度轮询（建议等 5-10 分钟）才会激活新的 Memory Dreaming Promotion；其他待用户确认的任务仍需人工处理。

### 3. GBrain 路径同步
QClaw 中 GBrain 路径是 `/Users/wangshuxian/gbrain`，bun 运行时 `/Users/wangshuxian/.bun/bin/bun`。WorkBuddy 侧尚未确认有相同的 gbrain 安装。如 GBrain Dream Cycle 需在 WorkBuddy 跑，需先确认 gbrain 是否还在。

### 4. skill 安装提醒（无丢失）
100 个 QClaw 侧技能**均未**通过 globals 段迁移。WorkBuddy 侧已有的技能市场已包含 QClaw 主要会用到的技能（pdf、docx、xlsx、impeccable 等）。如发现某个 QClaw 专用技能在 WorkBuddy 没有，可手动：
- 把 QClaw 的 `~/.qclaw/skills/<skill-name>/SKILL.md` 复制到 WorkBuddy 的 `~/.workbuddy/skills/<skill-name>/SKILL.md`
- 重启 WorkBuddy

### 5. session 没有绑定到 expert_id
sessions 表中 435 条会话的 `expert_id` 字段暂时为空。WorkBuddy 端专家重启后会自动按工作空间路径反查绑定。

---

## 六、迁移成功项（留作正向反馈）

| 资产 | 数量 | 位置 |
|---|---|---|
| 6 个专家 plugin.json | 6 | `~/.workbuddy/plugins/marketplaces/my-experts/plugins/<expert>/.codebuddy-plugin/plugin.json` |
| 6 个专家的工作空间 markdown | 6 个目录 | `~/WorkBuddy/<专家名>/` |
| 专家级 skills | 12+15+16+... 个 | `~/.workbuddy/plugins/marketplaces/my-experts/plugins/<expert>/skills/` |
| 专家会话 | 435 条 | workbuddy.db sessions 表 |
| 定时任务迁移计划 | 1 条新建 + 6 条待人工 | migration-plan.json |
| 连接器迁移指引 | 6 条 unsupported | migration-plan.json connectorGuide |

---

*报告路径：`/Users/wangshuxian/.workbuddy/.qclaw-migration/migration-plan.json`*
*会话级错误摘要：`/Users/wangshuxian/.workbuddy/.qclaw-migration/agent/agent-8e05976c/expert-report.json`*

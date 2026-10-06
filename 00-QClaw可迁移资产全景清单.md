# QClaw → WorkBuddy 可迁移资产全景清单

> 生成时间：2026-10-06
> 源：QClaw v0.3.0，数据目录 `~/.qclaw`
> 目标：WorkBuddy，数据目录 `~/.workbuddy`
>
> 本文档覆盖除官方连接器（aippt/ima/lark-cli/meituan-travel/wecom-cli/yuandian）外的所有可迁移资产。
> 连接器需等 WorkBuddy 上线对应项后手动连接，不在本文档范围。

---

## 一、资产总览

| 资产类别 | 数量 | 迁移方式 | 状态 |
|----------|------|----------|------|
| 专家（Agent） | 6 个 | 脚本自动迁移 | ✅ dry-run 完成 |
| 会话历史 | 549 条 | 脚本自动迁移 | ✅ dry-run 完成 |
| 工作空间文件 | ~12000+ 个 | 脚本自动迁移 | ✅ dry-run 完成 |
| 技能（全局） | 92 个 | 归纳为 MD 文档 | ✅ 已生成 |
| 技能（专家空间独有） | 12 个 | 归纳为 MD 文档 | ⏳ 待生成 |
| 定时任务 | 7 个 | 归纳为 MD 文档 | ⏳ 待生成 |
| MCP Server 配置 | 4 个 | 归纳为 MD 文档 | ⏳ 待生成 |
| 自定义模型配置 | 2 个 provider | 归纳为 MD 文档 | ⏳ 待生成 |
| GBrain 知识库 | 293MB / 380+ md | 归纳为 MD 文档 | ⏳ 待生成 |
| DREAMS 纠错记录 | 6607 行 | 归纳为 MD 文档 | ⏳ 待生成 |
| 专家人设文件 | 6 套 | 归纳为 MD 文档 | ⏳ 待生成 |
| 招标扫描脚本 | 6 个 | 归纳为 MD 文档 | ⏳ 待生成 |
| 频道绑定配置 | 1 条 | 归纳为 MD 文档 | ⏳ 待生成 |
| 环境变量 | 1 个 | 归纳为 MD 文档 | ⏳ 待生成 |

---

## 二、已通过脚本自动迁移的资产（dry-run 已完成）

### 2.1 专家与会话

| 专家 | 显示名 | 会话数 | 工作空间文件数 | 目标路径 |
|------|--------|--------|---------------|----------|
| main | 自由小虾 | 247 | 6837 | `/Users/wangshuxian/WorkBuddy/main` |
| agent-8e05976c | 小虾 | 167 | ~5000 | `/Users/wangshuxian/WorkBuddy/小虾` |
| op-eb2bda3c | 全屋定制报价顾问 | 1 | 292 | `/Users/wangshuxian/WorkBuddy/全屋定制报价顾问` |
| s3g2kaeuqwmx0n4s | 选股技术分析专家 | 8 | 389 | `/Users/wangshuxian/WorkBuddy/选股技术分析专家` |
| uafru5gofdt644lm | 游戏设计师 | 125 | 4573 | `/Users/wangshuxian/WorkBuddy/游戏设计师` |
| xfndlk74sib06h88 | WorkBuddy一键迁移助手 | 1 | 60 | `/Users/wangshuxian/WorkBuddy/WorkBuddy一键迁移助手` |

**合计**：6 个专家、549 条会话。

### 2.2 工作空间 Markdown 文件

以下文件会随专家工作空间一起迁移：

| 专家 | .md 文件数 | 含记忆/人设/纠错等 |
|------|-----------|-------------------|
| main | 84 | MEMORY.md, SOUL.md, DREAMS.md 等 |
| 小虾 | 250+ | 含大量法律案件分析、招标扫描报告 |
| 全屋定制报价顾问 | ~10 | 家装国标对照表等 |
| 选股技术分析专家 | ~20 | 含操作方案、复盘记录 |
| 游戏设计师 | 120+ | 含律途游戏开发文档、SQE题目 |
| WorkBuddy一键迁移助手 | 7 | 迁移助手自身配置 |

---

## 三、需要归纳为 MD 文档迁移的资产

### 3.1 定时任务（7 个）

脚本自动迁移仅能新建 1 个（Memory Dreaming Promotion），其余 6 个因兼容性问题无法自动迁移。以下是全部 7 个任务的完整信息，可在 WorkBuddy 自动化页面手动重建。

#### ① GBrain Dream Cycle（每日夜间 brain 维护）
- **状态**：启用
- **调度**：每天 06:41（Asia/Shanghai）
- **原始专家**：main
- **执行内容**：
  ```
  export PATH="/Users/wangshuxian/.bun/bin:$PATH"
  cd "/Users/wangshuxian/gbrain" && /Users/wangshuxian/.bun/bin/bun /Users/wangshuxian/gbrain/src/cli.ts dream --json
  ```
- **错误处理**：embed 阶段缺少 OPENAI_API_KEY 可忽略；status 为 ok/clean/partial 均视为成功
- **在 WorkBuddy 重建**：新建自动化任务 → 调度 cron `41 6 * * *` → 工作目录 `/Users/wangshuxian/gbrain`

#### ② Memory Dreaming Promotion（记忆晋升）
- **状态**：启用
- **调度**：每天 01:24
- **原始专家**：无绑定
- **执行内容**：系统内部指令 `__openclaw_memory_core_short_term_promotion_dream__`
- **迁移状态**：✅ 脚本可自动迁移

#### ③ 今日任务提醒（每小时）
- **状态**：已停用
- **调度**：每 1 小时
- **原始专家**：main
- **提示词**：
  > 你是王舒娴律师的贴心助手。请用简洁温暖的语气提醒她今日待办：
  > 📋 **今日待办（下午5点前完成）：**
  > 1. 填报涉外律师申请表
  > 2. 提醒张律师看六干合同
  > 要求：(1) 不要回复 HEARTBEAT_OK (2) 不要调用 message 工具 (3) 直接输出提醒文字 (4) 控制在 2-3 句话以内
- **在 WorkBuddy 重建**：新建自动化任务 → 调度 every 1h → 按需修改提示词中的待办项

#### ④ 六干案件进度跟进（单次任务）
- **状态**：已停用
- **调度**：2026-04-14 01:50 UTC（已过期）
- **提示词**：提醒跟进省军区清退小组会议结果，并更新腾讯文档和本地记录
- **迁移状态**：单次任务已过期，无需迁移

#### ⑤ 招标信息每日扫描
- **状态**：启用
- **调度**：每天 16:00（Asia/Shanghai）
- **原始专家**：小虾 (agent-8e05976c)
- **执行内容**：
  ```bash
  node /Users/wangshuxian/.qclaw/workspace-agent-8e05976c/scripts/bidding-scan-all.js
  ```
  脚本覆盖 7 个平台：湖州绿色采购、中国水利水电第十二局、嘉兴禾采联、浙建采、浙资运营、浙江政府采购网、浙江马云采
- **在 WorkBuddy 重建**：需先复制脚本到 WorkBuddy 专家空间，再新建自动化任务 → cron `0 16 * * *`

#### ⑥ 每日招标检查
- **状态**：已停用
- **调度**：每天 10:00
- **原始专家**：main
- **执行内容**：
  ```bash
  PYTHONPATH=/Users/wangshuxian/.local/lib/python3.9/site-packages python3 /Users/wangshuxian/.qclaw/workspace/skills/lawyer-contracts/scripts/check_bids.py
  ```
- **在 WorkBuddy 重建**：需复制脚本并更新路径

#### ⑦ a2a-poll-codex（A2A 聊天轮询）
- **状态**：已停用
- **调度**：每 20 秒
- **提示词**：轮询A2A聊天室检查codex的新消息并回复。房间码：2eukrjavg2
- **迁移状态**：无需迁移（实验性任务，已停用）

---

### 3.2 MCP Server 配置（4 个）

QClaw 的 `openclaw.json` 中配置了 4 个 MCP Server，迁移到 WorkBuddy 需在 `~/.workbuddy/mcp.json` 中手动添加。

#### ① mineru（PDF 解析）
```json
{
  "command": "uvx",
  "args": ["--from", "mineru-open-mcp"],
  "env": {
    "MINERU_API_TOKEN": "<需在 WorkBuddy 重新填入>"
  }
}
```
- **用途**：PDF 文档解析与提取
- **注意**：凭据不复制，需在 WorkBuddy 侧重新配置

#### ② pdf-reader
```json
{
  "command": "npx",
  "args": ["-y", "@sylphx/pdf-reader-mcp"]
}
```
- **用途**：PDF 读取

#### ③ mcp-pandoc
```json
{
  "command": "uvx",
  "args": ["--with", "mcp>=1.2,<2", "mcp-pandoc"]
}
```
- **用途**：格式转换（Markdown/HTML/PDF/DOCX 等）

#### ④ excel
```json
{
  "command": "uvx",
  "args": ["excel-mcp-server", "stdio"]
}
```
- **用途**：Excel 读写操作

**WorkBuddy 当前状态**：`~/.workbuddy/mcp.json` 已有 mineru，但 env 格式不同。需手动补充其余 3 个。

---

### 3.3 自定义模型配置（2 个 provider）

#### ① DeepSeek
- **Provider ID**：deepseek
- **Base URL**：`https://api.deepseek.com/`
- **API**：openai-completions
- **模型**：deepseek-reasoner（推理模型，131072 上下文窗口，65536 最大输出）
- **超时**：72000 秒

#### ② Ollama（本地模型）
- **Provider ID**：ollama
- **Base URL**：`http://localhost:11434`
- **API**：ollama
- **模型**：
  - Qwen3.5-2B（32000 上下文，不支持工具调用）
  - Qwen3.5-4B（32000 上下文，不支持工具调用）
  - Qwen2.5-7B（32000 上下文，不支持工具调用）
- **超时**：72000 秒

**在 WorkBuddy 重建**：在 WorkBuddy 设置 → 模型管理中手动添加。

---

### 3.4 频道绑定配置

```json
{
  "agentId": "main",
  "match": {
    "channel": "openclaw-weixin",
    "accountId": "*"
  }
}
```
- **用途**：微信频道消息绑定到 main 专家
- **在 WorkBuddy 重建**：在 WorkBuddy 设置 → 频道绑定中配置

---

### 3.5 环境变量

| 变量名 | 用途 |
|--------|------|
| `CHINESELAW_API_KEY` | 元典法律检索 API 密钥 |

**注意**：凭据不自动复制，需在 WorkBuddy 侧手动设置。

---

### 3.6 GBrain 知识库（293MB）

GBrain 是用户的个人知识管理系统，位于 `/Users/wangshuxian/gbrain/`，包含：

| 内容 | 数量/大小 | 说明 |
|------|----------|------|
| Markdown 文件 | 380+ 个 | 包含知识页面、会议记录、信号捕获等 |
| 自定义技能 | 50 个 | brain-ops, signal-detector, meeting-ingestion 等 |
| Dream cycles | 4 个 | 知识库维护日志 |
| 源代码 | src/ 目录 | TypeScript，bun 运行 |
| 配置 | gbrain.yml | 全局配置 |
- **迁移方式**：GBrain 是独立项目，不需要"迁移"到 WorkBuddy。只需在 WorkBuddy 的定时任务中重新配置 `bun run dream --json` 命令即可继续使用。
- **依赖**：bun 运行时（`/Users/wangshuxian/.bun/bin/bun`）、OPENAI_API_KEY（embed 阶段，可缺省）

---

### 3.7 DREAMS 纠错记录（6607 行）

各专家的 DREAMS.md 是记忆纠错和经验沉淀日志：

| 专家 | DREAMS.md 行数 | 说明 |
|------|---------------|------|
| main | 2372 行 | 自由小虾的纠错记录 |
| 小虾 | 2707 行 | 法律工作纠错记录 |
| 全屋定制报价顾问 | 231 行 | 报价系统纠错记录 |
| 选股技术分析专家 | 509 行 | 选股策略纠错记录 |
| 游戏设计师 | 788 行 | 游戏开发纠错记录 |
| WorkBuddy一键迁移助手 | 0 行 | 无 |

**迁移方式**：这些文件会随专家工作空间一起自动迁移，无需单独处理。

---

### 3.8 专家人设文件（6 套）

每位专家有完整的人设配置文件：

| 文件 | 用途 |
|------|------|
| SOUL.md | 人格/语气/哲学 |
| IDENTITY.md | 名称/头像/标签 |
| AGENTS.md | 工作规则/行为约束 |
| USER.md | 用户信息/偏好 |
| TOOLS.md | 工具使用备注 |
| MEMORY.md | 长期记忆 |
| HEARTBEAT.md | 心跳配置 |
| DREAMS.md | 纠错记录 |

**迁移方式**：这些文件会随专家工作空间一起自动迁移。

---

### 3.9 招标扫描脚本（6 个）

位于 `~/.qclaw/workspace-agent-8e05976c/scripts/`，是小虾专家用于招标信息每日扫描的自定义脚本：

| 脚本 | 大小 | 用途 |
|------|------|------|
| bidding-scan-all.js | 25KB | 增强版统一扫描（7 平台一次完成） |
| gen_pdf.py | 20KB | PDF 生成 |
| heli-scan.js | 7KB | 合力科技扫描 |
| zfcg-scan.js | 5KB | 政府采购扫描 |
| zjjc-scan.js | 2KB | 浙江建设扫描 |
| zjzsco-scan.js | 6KB | 浙资运营扫描 |

**迁移方式**：
1. 执行正式迁移后，这些脚本会随小虾专家工作空间一起复制到 `/Users/wangshuxian/WorkBuddy/小虾/scripts/`
2. 手动重建定时任务时，将脚本路径更新为 WorkBuddy 侧路径

---

### 3.10 专家空间独有技能（12 个）

这些技能不在全局 `~/.qclaw/skills/` 目录中，而是各专家自己工作空间内的 `skills/` 子目录：

| 专家 | 技能名 | 说明 |
|------|--------|------|
| 小虾 | video-to-evidence-layout | 录屏转证据截图排版（WorkBuddy 已有 v2.2.1） |
| 小虾 | yuandian-law-search | 元典法律检索（WorkBuddy 已有） |
| 全屋定制报价顾问 | brand-guidelines | 品牌规范 |
| 全屋定制报价顾问 | custom-cabinet-quote-expert | 定制柜报价专家 |
| 全屋定制报价顾问 | fullstack-dev | 全栈开发 |
| 全屋定制报价顾问 | quote-estimation | 报价估算 |
| 全屋定制报价顾问 | skyline | 天际线 |
| 全屋定制报价顾问 | tdesign-miniprogram | TDesign 小程序组件 |
| 全屋定制报价顾问 | wxa-skills-eval | 小程序技能评估 |
| 全屋定制报价顾问 | wxa-skills-generate | 小程序技能生成 |
| 全屋定制报价顾问 | wxa-skills-validate | 小程序技能验证 |
| WorkBuddy一键迁移助手 | qclaw-to-workbuddy | 迁移技能本身 |

**迁移方式**：
- video-to-evidence-layout 和 yuandian-law-search 在 WorkBuddy 已有更新版本，无需迁移
- 全屋定制报价顾问的 8 个技能是该专家专用，随工作空间一起自动迁移
- qclaw-to-workbuddy 随工作空间自动迁移

---

### 3.11 技能使用统计

`~/.qclaw/skill-usage.json` 记录了技能使用历史，可供参考哪些技能最常用：

| 技能 | 最近使用 |
|------|---------|
| chineselaw-openapi | 2026-03 |
| brain-ops | 2026-03 |
| pdf-text-extractor | 2026-03 |
| ima | 2026-03 |
| canvas-design | 2026-03 |
| xlsx | 2026-03 |
| video-to-evidence-layout | 2026-03 |
| signal-detector | 2026-03 |
| openai-whisper | 2026-03 |

**迁移方式**：统计信息不迁移，仅作参考。

---

## 四、WorkBuddy 已有资产（无需迁移）

以下技能在 WorkBuddy 侧已有：

| 技能 | 说明 |
|------|------|
| 腾讯ima | IMA 知识库技能（WorkBuddy 自带） |
| 飞书套件 | 飞书 CLI 全能套件 v2.3.0 |
| 企业微信套件 | 企微 CLI 套件 v1.0.5 |
| video-to-evidence-layout | 录屏转证据 v2.2.1（比 QClaw 的 v2.0.20 更新） |
| yuandian-law-search | 元典法律检索 |
| Apple备忘录 | 苹果备忘录技能 |
| pua | PUA 技能 |
| wechat-article-extractor | 微信文章提取 |

---

## 五、迁移后操作清单

执行正式迁移（`--confirm`）后，以下事项需在 WorkBuddy 中手动完成：

| 序号 | 事项 | 操作位置 | 优先级 |
|------|------|---------|--------|
| 1 | 重启 WorkBuddy | 桌面端 | 🔴 高 |
| 2 | 添加 MCP Server（pdf-reader, mcp-pandoc, excel） | 设置 → MCP | 🔴 高 |
| 3 | 添加自定义模型（DeepSeek, Ollama） | 设置 → 模型管理 | 🟡 中 |
| 4 | 设置环境变量 CHINESELAW_API_KEY | 设置 → 环境变量 | 🟡 中 |
| 5 | 重建定时任务：GBrain Dream Cycle | 自动化页面 | 🟡 中 |
| 6 | 重建定时任务：招标信息每日扫描 | 自动化页面 | 🟡 中 |
| 7 | 重建定时任务：今日任务提醒 | 自动化页面 | 🟢 低 |
| 8 | 配置频道绑定（微信→main 专家） | 设置 → 频道 | 🟡 中 |
| 9 | 在技能广场搜索安装常用技能 | 技能广场 | 🟢 低 |
| 10 | 连接器上线后手动连接 | 设置 → 连接器 | 🟢 低 |

---

*本文档由 QClaw → WorkBuddy 迁移助手自动生成*
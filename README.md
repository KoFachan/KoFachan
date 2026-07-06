# Hello World!

**This is my first repository!**


- 👋 Hi, I’m **@KoFachan**
- 👀 I’m interested in Data Science
- 🌱 I’m currently learning *Data Science｜Machine learning | Mathematics | Statistics ｜*
- 💞️ I’m looking to collaborate on ...
- 📫 How to reach me ***kefazhan@gamil.com***

My favourite website is [***Google***](https://www.google.com) 😹🉑
<!---
KoFachan/KoFachan is a ✨ special ✨ repository because its `README.md` (this file) appears on your GitHub profile.
You can click the Preview link to take a look at your changes.
--->

# Codex 本地架构配置说明

本文档说明当前这台 Windows 机器上的 Codex 本地化配置方式，以及以后新增规则、知识、Skill、Hook、Agent、Plugin、MCP、项目知识库时应该放在哪里。

它是给人看的说明书，不是 Codex 每次都必须加载的运行规则。真正的自动路由规则已经放在全局 `AGENTS.md`；完整架构判断逻辑放在全局 Skill `codex-architecture-governance`。

## 当前结论

本机已经建立了一个更适合长期使用的 Codex 架构治理方式：

1. 架构知识主体放入全局 Skill：`codex-architecture-governance`
2. 详细五层架构、本机路径映射、放置决策规则放入该 Skill 的 `references/`
3. 全局 `AGENTS.md` 只加入一条短路由规则
4. 没有把完整架构知识写进全局 Memories
5. 当前机器盘点快照保留在当前项目的 `outputs/` 和项目知识库中

这样做的核心原因是：全局 `AGENTS.md` 会影响每个新对话，不适合承载大段方法论；Skill 可以按需触发，更省上下文窗口，也更容易维护。

## 关键路径

### 全局架构治理 Skill

```text
C:\Users\admin\.codex\skills\codex-architecture-governance
```

结构：

```text
codex-architecture-governance/
├── SKILL.md
├── agents/
│   └── openai.yaml
└── references/
    ├── layer-model.md
    ├── local-path-map.md
    └── placement-rules.md
```

各文件作用：

| 文件 | 作用 |
|---|---|
| `SKILL.md` | 触发条件、使用流程、输出格式、边界规则 |
| `references/layer-model.md` | 本地 Codex 的 5+2 层架构解释 |
| `references/placement-rules.md` | 判断内容应该放 AGENTS、Skill、reference、project knowledge、hook、agent 还是 plugin |
| `references/local-path-map.md` | 当前机器已验证的 Codex 路径、数量、审计命令 |
| `agents/openai.yaml` | Skill 在 Codex UI/Skill 列表里的展示元数据 |

### 全局 AGENTS.md

```text
C:\Users\admin\.codex\AGENTS.md
```

已加入的短路由规则：

```text
Use codex-architecture-governance for Codex architecture audits and for deciding where AGENTS.md rules, skills, references, hooks, agents, plugins, MCP servers, connectors, memories, or project knowledge should live.
```

注意：这里只放一条短规则，不放完整架构说明。

### 当前项目架构快照

```text
C:\Users\admin\Documents\Codex配置\outputs\codex-local-architecture-20260706.md
```

这是 2026-07-06 对本机 Codex 架构的盘点快照。它记录了当时的 Skill 数量、项目知识库、Hooks、Agents、Plugins、MCP 等状态。

### 本次实现记录

```text
C:\Users\admin\Documents\Codex配置\outputs\codex-architecture-governance-implementation-20260706.md
```

记录了 `codex-architecture-governance` 的创建过程、修改内容和验证结果。

### 当前项目持久状态

```text
C:\Users\admin\.codex\knowledge\projects\Codex配置-a3bbcc3fe7\current-state.md
```

用于让新对话恢复当前项目状态。它记录了这次 Skill 创建和架构决策，但不是完整说明书。

## 本地 Codex 的 5+2 层架构

附件中的五层架构可以作为心智模型，但本机实际更适合理解成 5+2 层。

| 层 | 本机对应 | 作用 |
|---|---|---|
| Layer 1 规则/记忆层 | `AGENTS.md`、`config.toml`、`knowledge/`、`memories/` | 稳定规则、上下文恢复、全局约束 |
| Layer 2 Skills 知识层 | `.codex\skills`、`.agents\skills`、系统 Skill | 可复用工作流、领域知识、SOP、脚本 |
| Layer 3 Hooks 护栏层 | `hooks.json`、hook scripts | 会话启动、压缩前后、停止时的自动动作 |
| Layer 4 委派层 | `agents/*.toml`、subagents、ECC prompts | 探索、审查、文档核验、并行任务 |
| Layer 5 插件/分发层 | plugins cache、connectors、MCP、CLI | 外部工具、浏览器、GitHub、飞书、TiDB 等能力 |
| 补充层 A 项目知识库 | `.codex\knowledge\projects` | 每个项目的长期状态、决策、wiki |
| 补充层 B 外部能力层 | TiDB MCP、node_repl、lark-cli、gh 等 | 数据库、CLI、API、外部系统访问 |

## 如何使用架构治理 Skill

重启 Codex 或新开对话后，可以直接说：

```text
Use $codex-architecture-governance，判断这段业务背景应该放 AGENTS.md、Skill reference、项目知识库还是 memory。
```

也可以自然语言触发：

```text
这个规则应该写进全局 AGENTS，还是做成 Skill？
```

```text
帮我审计一下当前项目的 Codex 架构，看看哪些配置应该全局化，哪些应该项目化。
```

```text
我想把机器学习模型选择流程沉淀下来，应该放在哪一层？
```

如果自动触发不稳定，显式写 `Use $codex-architecture-governance`。

## 放置决策原则

### 放进全局 AGENTS.md

适合：

- 每个项目都应该遵守的稳定规则
- 安全边界
- 上下文管理原则
- Skill 路由规则
- 很短的公司级战略锚点

不适合：

- 大段业务背景
- 完整方法论
- 机器学习教材
- 当前项目状态
- 易变权限、token、数据库连接信息
- 大量路径清单

### 放进项目 AGENTS.md

适合：

- 只在某个 repo 或项目内生效的工程规则
- 项目独有的测试、提交、目录规范
- 项目内协作约定

如果规则只对一个项目有效，不要写进全局 AGENTS。

### 放进 Skill

适合：

- 会反复使用的工作流
- 领域 SOP
- 业务分析方法
- 架构治理方法
- ML 模型选择流程
- VOC 分析流程
- 飞书/Lark 工作流
- 可复用脚本和模板

Skill 的 `SKILL.md` 应该短，只放触发条件和流程。详细内容放 `references/`。

### 放进 Skill references

适合：

- 详细知识库
- 业务背景
- 决策表
- 示例
- 数据口径解释
- 算法选择条件
- 本机路径映射

references 是“按需加载”的知识层，比 AGENTS 更省上下文。

### 放进项目知识库

适合：

- 当前目标
- 已验证发现
- 决策记录
- 输出路径
- 开放问题
- 当前项目状态
- 长线程压缩前后的恢复点

项目知识库不是源数据本身，而是对源数据的维护性总结。

### 放进 Hooks

适合：

- 会话启动时加载项目知识
- `/compact` 前写 checkpoint
- `/compact` 后恢复
- Stop 时记录轻量状态

不适合：

- 自动做市场研究
- 自动执行高风险命令
- 自动发布或修改第三方系统
- 自动选择复杂业务方案

### 放进 Agent/Subagent

适合：

- 独立探索
- 代码审查
- 文档核验
- 日志分析
- 并行研究

主线程仍负责最终判断和汇总。

### 放进 Plugin/MCP/CLI

适合：

- 接外部系统
- 浏览器、Chrome、GitHub、Gmail、Google Drive
- TiDB、node_repl
- 飞书 CLI、GitHub CLI
- 团队分发和版本控制

Plugin/MCP/CLI 是工具能力，不是知识存放处。

## 对 Memories 的判断

本次没有把完整架构知识写入全局 Memories。

原因：

1. Memories 更适合保存跨会话偏好、历史结论、项目经验索引。
2. 架构治理是一套可复用流程，更适合 Skill。
3. 大段架构知识写入 Memories 会增加不可控触发和上下文噪音。

可以写入 Memories 的内容应当是短结论，例如：

```text
用户偏好：Codex 架构、Skill/AGENTS/Hook/Plugin 放置问题，优先使用 codex-architecture-governance。
```

但完整架构说明不应放入 Memories。

## 对 ML/数学知识的判断

其它 AI 给出的建议里提到把微积分、概率论、机器学习模型解释放进 `skills/memory.md`，再用 AGENTS.md 触发。

当前建议不采用这个做法。

原因：

1. `skills/memory.md` 不是一个清晰的 Codex Skill 结构。
2. 公司背景、数学知识、ML 模型选择、业务规则混在一个文件里，会变成难维护的大杂烩。
3. AGENTS.md 不应该承载完整 ML 工作流，否则会干扰非 ML 任务。

更好的未来结构是单独创建：

```text
ml-model-selection/
├── SKILL.md
├── references/
│   ├── data-diagnosis.md
│   ├── supervised-models.md
│   ├── unsupervised-models.md
│   ├── evaluation-metrics.md
│   └── petlibro-business-use-cases.md
└── scripts/
    └── optional data profiling helpers
```

全局 AGENTS 只需要一条短规则：

```text
For ML, prediction, classification, clustering, causal modeling, or model selection tasks, use the focused ML skill and start by defining metric, numerator, denominator, time window, labels, sample size, leakage risk, and business objective.
```

这样既能全局触发，又不会污染普通对话。

## 当前本机已知状态

来自 2026-07-06 盘点：

| 项目 | 状态 |
|---|---|
| `.codex\skills` | 创建前 6 个，创建后增加 `codex-architecture-governance` |
| `.codex\skills\.system` | 5 个系统 Skill |
| `.agents\skills` | 64 个全局 Skill |
| `.codex\knowledge\projects` | 12 个项目知识库 |
| `.codex\agents` | 3 个 agent 配置 |
| `.codex\prompts` | 94 个 ECC prompt 模板 |
| `hooks.json` | 已接入 `knowledge-context-manager` |
| plugins cache | openai-bundled、openai-curated、openai-curated-remote、openai-primary-runtime |

注意：这些数字会变。以后要以实时审计为准。

## 安全规则

不要把以下内容写入 AGENTS、Skill、references、Memories、项目知识库或 README：

- API key
- access token
- refresh token
- password
- private key
- OAuth device code
- 临时授权 URL
- 数据库密码
- 任何可直接复用的凭证

需要记录时，只记录“需要某类权限/某个 scope”，不要记录密钥本身。

## 常用审计命令

以下命令只用于查看结构，不应输出密钥值。

```powershell
Get-ChildItem -LiteralPath "$env:USERPROFILE\.codex" -Force
Get-ChildItem -LiteralPath "$env:USERPROFILE\.codex\skills" -Directory
Get-ChildItem -LiteralPath "$env:USERPROFILE\.agents\skills" -Directory
Get-Content -LiteralPath "$env:USERPROFILE\.codex\hooks.json"
```

查看 `config.toml` 时必须脱敏：

```powershell
Select-String -LiteralPath "$env:USERPROFILE\.codex\config.toml" `
  -Pattern '^\s*\[.*\]|^\s*[A-Za-z0-9_.-]+\s*=' |
  ForEach-Object { $_.Line -replace '=.*$','= <redacted>' }
```

## 后续维护建议

### 新增业务背景

先问：

1. 是所有项目都需要，还是某个项目需要？
2. 是稳定背景，还是当前阶段信息？
3. 是规则，还是知识？
4. 是否很长？

推荐：

- 很短、稳定、全局影响：AGENTS.md
- 大段业务背景：项目知识库或业务上下文 Skill reference
- 当前项目状态：current-state.md
- 会复用的业务分析方法：Skill

### 新增工作流

如果一个流程会重复使用三次以上，优先做 Skill。

### 新增自动化

如果它是确定性的、低风险的生命周期动作，可以考虑 Hook。

如果它需要判断、调研、写作、发布或调用第三方系统，不要放 Hook。

### 新增外部工具

如果是工具能力，优先考虑：

- MCP
- connector
- CLI
- plugin

如果是“怎么用这个工具”的经验，放 Skill。

## 使用检查清单

以后遇到“这个东西该放哪儿”时，按这个顺序判断：

1. 是源数据吗？保留在源系统或项目文件里。
2. 是当前项目状态吗？放项目知识库。
3. 是全局稳定规则吗？放 AGENTS.md，但要短。
4. 是可复用流程吗？做 Skill。
5. 是详细背景知识吗？放 Skill references 或项目 wiki。
6. 是确定性自动动作吗？考虑 Hook。
7. 是独立角色任务吗？考虑 Agent/Subagent。
8. 是外部工具能力吗？用 Plugin/MCP/CLI。
9. 是团队分发需求吗？再考虑打包成 plugin 或安装包。

## 当前使用入口

显式调用：

```text
Use $codex-architecture-governance，帮我判断这段内容应该放到哪里。
```

自然语言调用：

```text
这段公司背景应该写进 AGENTS.md，还是做成 Skill？
```

```text
帮我审计当前项目的 Codex 架构。
```

```text
我想沉淀一个机器学习模型选择流程，应该怎么放？
```

如果新 Skill 没出现在当前对话里，重启 Codex 或新开对话。


# Obsiwiki

[English](#obsiwiki) | [中文](#中文说明)

Obsiwiki helps you turn scattered sources, notes, and conversations in Obsidian into structured, navigable knowledge assets that compound over time.

It gives your agents a shared way to:

- turn incoming sources and conversations into reusable knowledge
- keep original materials traceable while building cleaner wiki pages
- connect pages through maps, indexes, and overview notes
- preserve your judgment about what is worth keeping
- answer questions from the vault instead of starting from scratch
- review recent changes and surface what still needs attention
- keep the knowledge base healthy as it grows

## Contents

- [Usage Quick Reference](#usage-quick-reference)
- [Install Skill](#install-skill)
- [Update Skill](#update-skill)
- [Core Workflows](#core-workflows)
- [Vault Folder Structure](#vault-folder-structure)

## Acknowledgements

Obsiwiki is inspired by Andrej Karpathy's [LLM Wiki](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f) note. Thanks to Karpathy for articulating the pattern of using LLM agents to incrementally maintain a persistent, interlinked wiki over raw source material.

## Repository Layout

```text
.
├── SKILL.md
├── adapters/
│   ├── codex.md
│   ├── claude-code.md
│   ├── hermes.md
│   └── openclaw.md
├── references/
│   ├── lint-checklist.md
│   ├── page-types.md
│   └── schema.md
└── starter-vault/
    ├── System/
    │   ├── Agents/
    │   └── Schema/
    │       └── purpose.md
    ├── assets/
    ├── raw/
    └── wiki/
```

## Usage Quick Reference

| Agent | Typical usage |
| --- | --- |
| Codex | `$obsiwiki ingest https://example.com/article` |
| Claude Code / Hermes / OpenClaw | `/obsiwiki ingest https://example.com/article` |

Common workflow verbs:

- `ingest`: save a source into `raw/`, create a source summary in `wiki/sources/`, and update maps/index/log.
- `capture`: extract reusable conclusions from a conversation and update the best target page.
- `query`: answer from `wiki/index.md`, maps, and formal wiki pages.
- `lint`: check for orphan pages, missing sources, duplicates, stale pages, asset placement issues, and graph health issues.
- `review`: summarize recent additions, notable updates, topic clusters, open review items, overview drift, and next actions.

Canonical workflow prompts:

```text
/obsiwiki ingest this source: https://example.com/article
/obsiwiki capture reusable conclusions from this conversation.
/obsiwiki answer this from the vault: <question>
/obsiwiki lint this vault.
/obsiwiki review recent additions to this vault.
/obsiwiki generate this week's knowledge base report.
/obsiwiki set up scheduled maintenance for this vault.
```

## Install Skill

Send this prompt to the agent to install Obsiwiki skill:

```text
Install Obsiwiki for the agent I am currently using. Use this repository: https://github.com/TengShao/Obsiwiki

Inspect the repository and decide which Obsiwiki files are needed for this agent. Read the relevant adapter note under adapters/ from the repository for guidance, but do not install adapters/ locally.

Decide where this agent keeps reusable skills, project instructions, commands, plugins, or context. Install the necessary Obsiwiki files there.

Ask me for the target Obsidian vault path. If I do not know it, help me find likely vault folders and ask me to confirm one before making changes.

If the confirmed vault is new or lightly structured, use starter-vault/ as the initialization reference. If it already has notes, do not bulk move them; propose incremental changes instead.

If the confirmed vault contains System/Schema/, treat it as the vault-local source of truth and do not overwrite local customizations.

Report which files you installed, where they were installed, and how to reload the agent.
```

After installation, call Obsiwiki in the way your agent supports, for example:

```text
/obsiwiki ingest https://example.com/article
```

## Update Skill

Send this prompt to the agent to update Obsiwiki skill:

```text
Update the existing Obsiwiki installation for the agent I am currently using. Use this repository: https://github.com/TengShao/Obsiwiki

Inspect the latest repository and decide which installed Obsiwiki files need refreshing. Read the relevant adapter note under adapters/ from the repository for guidance, but do not install adapters/ locally.

Find the current Obsiwiki skill installation and refresh it from the latest repository version. If any vault-local System/Schema/ files are involved, preserve local customizations.

After refreshing the skill, compare the latest default schema, value guidance, and support pages with the target vault if it has System/Schema/. Do not overwrite the vault. Report schema/workflow changes separately from value-guidance additions such as System/Schema/purpose.md and support-page additions such as wiki/overview.md and wiki/review.md. If System/Schema/purpose.md is missing, ask whether to initialize one from the starter template or draft one for this vault. If wiki/overview.md or wiki/review.md is missing, scan wiki/index.md, wiki/maps/, wiki/log.md, and recent formal page updates, then ask whether to initialize those support pages. Draft wiki/overview.md as a current-state summary and wiki/review.md as a backlog with issues found during the scan. Ask me whether to merge any suggested vault-local migration.

Report what changed and how to reload the agent.
```

## Core Workflows

### Ingest

Use `ingest` to turn a URL, article, PDF, screenshot pack, transcript, meeting note, or unprocessed note into connected vault knowledge.

Expected behavior:

1. Run source analysis first without writing files.
2. Consult `System/Schema/purpose.md` when present and state a short value assessment.
3. Identify the source thesis, reusable claims, important entities, concepts, related pages, possible duplicates, conflicts, and synthesis candidates.
4. Preserve source-relevant images, figures, diagrams, screenshots, PDFs, and attachments under `assets/raw/<source-slug>/`, embed or link local copies from the raw/source note, and record skipped or unavailable assets.
5. Propose wiki changes second: `raw/`, `assets/raw/<source-slug>/`, `wiki/sources/`, concept/entity/synthesis updates, maps, index, and log.
6. Prefer updating existing `wiki/concepts/`, `wiki/entities/`, or `wiki/syntheses/` over creating duplicates.
7. Use `wiki/review.md` when an issue needs human judgment instead of forced automatic cleanup.
8. Write changes after confirmation unless you asked for automatic execution.

Example:

```text
/obsiwiki ingest this source: https://example.com/article
```

### Capture

Use when a conversation produced a reusable conclusion, decision, definition, tradeoff, or workflow.

Expected behavior:

1. Extract only reusable knowledge.
2. Avoid saving the full chat transcript by default.
3. Consult `System/Schema/purpose.md` when present and state why the conclusion is worth preserving.
4. Suggest the best target page.
5. Update the target page after confirmation.
6. Create a `wiki/syntheses/` page only when the answer spans multiple topics or sources.
7. Use `wiki/review.md` when a valuable conclusion still needs human judgment.

Example:

```text
/obsiwiki capture reusable conclusions from this conversation.
```

### Query

Use when you want the agent to answer from the vault.

Expected behavior:

1. Start from `wiki/index.md`.
2. Follow relevant `wiki/maps/`.
3. Read only the necessary `concepts`, `entities`, `sources`, and `syntheses`.
4. Suggest a new or updated `synthesis` only when the answer is broadly reusable.

Example:

```text
/obsiwiki answer this from the vault: What does my knowledge base say about the relationship between prompt engineering and UXD?
```

### Lint

Use when you want a vault health check.

Expected behavior:

- flag wiki pages not covered by maps or index
- flag source pages without formal links
- flag duplicate or near-duplicate topics
- flag missing `last_updated`, missing sources, or missing `Related Links`
- flag raw-source attachment placement problems
- flag wiki pages that should promote assets that still live only under `raw/` into `assets/wiki/`
- flag stale or missing value guidance or support pages such as `System/Schema/purpose.md`, `wiki/overview.md`, or `wiki/review.md` when expected by the active schema
- flag graph health issues: clusters without maps, source clusters without concepts, concepts without sources, stale syntheses, overloaded maps, duplicate clusters, and bridge candidates
- add or propose `wiki/review.md` items when graph health issues require interpretation
- exclude `System/` from ordinary content lint; do not require schema or agent files to be covered by maps, sources, `last_updated`, or `Related Links`
- run only lightweight configuration checks for `System/`: schema files are readable, agent adapters follow `System/Schema/`, and vault-local schema customizations are preserved

Example:

```text
/obsiwiki lint this vault.
```

### Review

Use when you want a read-only review of recent additions, recent updates, or this week's knowledge base changes.

Default range: the last 7 days unless you specify a range.

Expected behavior:

1. Resolve ranges such as `yesterday`, `this week`, `this month`, `last 14 days`, `since 2026-04-01`, or `2026-04-01 to 2026-04-15`.
2. Read `wiki/log.md` first, page frontmatter `last_updated` second, and file modification time only as a fallback.
3. Summarize the time range, new sources and pages, notable updates, topic clusters, open organization questions, open review items, overview drift, and suggested next `ingest`, `capture`, or `lint` actions.
4. Keep the review read-only by default.
5. Propose `wiki/overview.md` updates when the review changes the compressed picture of the knowledge base.
6. Propose `wiki/review.md` items for unresolved duplicate, source, stale synthesis, unclear value, or graph health questions.
7. If you want to save a weekly report or durable summary, ask the agent to switch to `capture` or propose a `wiki/syntheses/` update and confirm before writing.

Examples:

```text
/obsiwiki review recent additions to this vault.
/obsiwiki review recent additions from yesterday.
/obsiwiki review recent additions from this week.
/obsiwiki review recent additions from this month.
/obsiwiki review recent additions since 2026-04-01.
/obsiwiki generate this week's knowledge base report.
```

### Scheduled Maintenance

Use when you want the agent to help create a cron task or another recurring automation for periodic `review` and `lint`.

Expected behavior:

1. Ask whether the user wants to create scheduled maintenance for recent-update review and periodic lint.
2. Let the user choose the cadence and time. Default to every Monday at 09:00 in the user's locale.
3. Confirm whether review and lint should run as one combined job or separate jobs.
4. Confirm the target vault path, output destination, and whether the automation is allowed to write follow-up changes.
5. Keep scheduled review and scheduled lint read-only by default. If the job finds durable updates, it should propose `capture`, `synthesis`, `overview`, or `review.md` changes rather than silently writing them.
6. Use the host agent's native scheduler when available; otherwise guide the user through cron or the environment's preferred automation mechanism.
7. Show the final schedule and maintenance prompt before creating or modifying the scheduled task.

Suggested recurring task prompt:

```text
Review recent additions to this vault and lint this vault. Keep the run read-only by default. Summarize recent additions, notable updates, open review items, overview drift, lint issues, graph health issues, and suggested next actions. Propose any durable writes for user confirmation.
```

Example:

```text
/obsiwiki set up scheduled maintenance for this vault.
```


## Vault Folder Structure

The starter vault uses four main layers plus vault-local guidance under `System/Schema/`:

```text
raw/
├── articles/        web articles, excerpts, tutorials, and practical guides
├── papers/          papers, PDFs, reports, and research material
├── transcripts/     transcripts and long-form conversations
└── meeting-notes/   meetings, interviews, and field notes

assets/
├── raw/             files belonging to one original source, grouped by source slug
├── wiki/            reusable long-lived knowledge assets
├── projects/        project-specific assets
└── shared/          assets reused across topics

wiki/
├── concepts/        durable concepts, protocols, frameworks, and methods
├── entities/        tools, products, companies, people, and named systems
├── sources/         one-to-one source summaries
├── syntheses/       integrated analysis across sources or conversations
├── maps/            topic maps / MOCs that keep pages connected
├── index.md         top-level navigation entry
├── log.md           append-only maintenance log
├── overview.md      compressed state of the current knowledge base
└── review.md        backlog for human judgment and later agent follow-up

System/
└── Schema/          vault-local authority for rules, workflows, and value guidance
    └── purpose.md   value judgment guidance for agents
```

Folder intent:

- `raw/` keeps source material close to its original form.
- `assets/` keeps attachments and media files out of note folders.
- `System/Schema/purpose.md` tells agents how to judge what is worth preserving.
- `wiki/` is the durable knowledge layer agents should query and update.
- `wiki/maps/` is the main anti-orphan mechanism.
- `wiki/overview.md` is a compressed state view for agents and humans.
- `wiki/review.md` records uncertain issues that need human judgment or later follow-up.
- `System/Schema/`, when present, is the vault-local authority; agent-specific files should follow it instead of defining separate rules.
- `System/Agents/` is optional; use it only when an agent needs vault-local adapter notes.

---

# 中文说明

Obsiwiki 帮你把 Obsidian 里的零散资料、笔记和对话整理成结构化、可导航、可长期沉淀的知识资产。

它让不同 Agent 能用同一套方式帮你：

- 把新增资料和对话转化为可复用知识
- 保留原始材料的来源，同时沉淀更清晰的 wiki 页面
- 通过 maps、index 和 overview 串起知识脉络
- 延续你对“什么值得保留”的判断
- 基于知识库回答问题，而不是每次从零开始
- 回顾近期变化，发现还需要整理的内容
- 在知识库持续增长时保持结构健康

## 目录

- [使用速查](#使用速查)
- [安装 Skill](#安装-skill)
- [更新 Skill](#更新-skill)
- [核心工作流](#核心工作流)
- [知识库目录结构](#知识库目录结构)

## 致谢

Obsiwiki 受到 Andrej Karpathy 的 [LLM Wiki](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f) 这篇笔记启发，也就是让大语言模型持续维护一套持久、互联的 wiki。感谢 Karpathy 清晰描述了这个模式。

## 仓库结构

```text
.
├── SKILL.md
├── adapters/
│   ├── codex.md
│   ├── claude-code.md
│   ├── hermes.md
│   └── openclaw.md
├── references/
│   ├── lint-checklist.md
│   ├── page-types.md
│   └── schema.md
└── starter-vault/
    ├── System/
    │   ├── Agents/
    │   └── Schema/
    │       └── purpose.md
    ├── assets/
    ├── raw/
    └── wiki/
```

## 使用速查

| Agent | 典型用法 |
| --- | --- |
| Codex | `$obsiwiki ingest https://example.com/article` |
| Claude Code / Hermes / OpenClaw | `/obsiwiki ingest https://example.com/article` |

常用工作流：

- `ingest`：将来源保存到 `raw/`，在 `wiki/sources/` 创建来源摘要，并更新 maps/index/log。
- `capture`：从对话中提取可复用结论，并更新最合适的目标页面。
- `query`：从 `wiki/index.md`、maps 和正式 wiki 页面中回答问题。
- `lint`：检查孤立页面、缺失来源、重复主题、过期页面、附件放置问题和知识图谱健康问题。
- `review`：梳理近期新增、重要变化、主题聚类、待处理事项，以及 overview 是否需要更新。

常用提示：

```text
/obsiwiki ingest this source: https://example.com/article
/obsiwiki capture reusable conclusions from this conversation.
/obsiwiki answer this from the vault: <question>
/obsiwiki lint this vault.
/obsiwiki review recent additions to this vault.
/obsiwiki generate this week's knowledge base report.
/obsiwiki set up scheduled maintenance for this vault.
```

## 安装 Skill

把下面这段文字发给 Agent 进行安装：

```text
Install Obsiwiki for the agent I am currently using. Use this repository: https://github.com/TengShao/Obsiwiki

Inspect the repository and decide which Obsiwiki files are needed for this agent. Read the relevant adapter note under adapters/ from the repository for guidance, but do not install adapters/ locally.

Decide where this agent keeps reusable skills, project instructions, commands, plugins, or context. Install the necessary Obsiwiki files there.

Ask me for the target Obsidian vault path. If I do not know it, help me find likely vault folders and ask me to confirm one before making changes.

If the confirmed vault is new or lightly structured, use starter-vault/ as the initialization reference. If it already has notes, do not bulk move them; propose incremental changes instead.

If the confirmed vault contains System/Schema/, treat it as the vault-local source of truth and do not overwrite local customizations.

Report which files you installed, where they were installed, and how to reload the agent.
```

安装后，用你的 Agent 支持的方式调用 Obsiwiki，比如：

```text
/obsiwiki ingest https://example.com/article
```

## 更新 Skill

把下面这段文字发给 Agent 进行更新：

```text
Update the existing Obsiwiki installation for the agent I am currently using. Use this repository: https://github.com/TengShao/Obsiwiki

Inspect the latest repository and decide which installed Obsiwiki files need refreshing. Read the relevant adapter note under adapters/ from the repository for guidance, but do not install adapters/ locally.

Find the current Obsiwiki skill installation and refresh it from the latest repository version. If any vault-local System/Schema/ files are involved, preserve local customizations.

After refreshing the skill, compare the latest default schema, value guidance, and support pages with the target vault if it has System/Schema/. Do not overwrite the vault. Report schema/workflow changes separately from value-guidance additions such as System/Schema/purpose.md and support-page additions such as wiki/overview.md and wiki/review.md. If System/Schema/purpose.md is missing, ask whether to initialize one from the starter template or draft one for this vault. If wiki/overview.md or wiki/review.md is missing, scan wiki/index.md, wiki/maps/, wiki/log.md, and recent formal page updates, then ask whether to initialize those support pages. Draft wiki/overview.md as a current-state summary and wiki/review.md as a backlog with issues found during the scan. Ask me whether to merge any suggested vault-local migration.

Report what changed and how to reload the agent.
```

## 核心工作流

### Ingest

用 `ingest` 把 URL、文章、PDF、截图包、转录稿、会议记录或原始笔记整理成知识库里的关联知识。

预期行为：

1. 先做 source analysis，不写文件。
2. 如果存在 `System/Schema/purpose.md`，先读取并给出简短 value assessment。
3. 识别来源的核心 thesis、可复用 claim、重要实体、概念、相关页面、可能重复、冲突和 synthesis 候选。
4. 默认保留与来源理解有关的图片、图表、截图、PDF 和附件，保存到 `assets/raw/<source-slug>/`，在 raw/source 文档中嵌入或链接本地副本，并记录跳过或无法获取的资产。
5. 再提出 wiki changes：`raw/`、`assets/raw/<source-slug>/`、`wiki/sources/`、concept/entity/synthesis 更新、maps、index 和 log。
6. 优先更新已有的 `wiki/concepts/`、`wiki/entities/` 或 `wiki/syntheses/`，而不是创建重复页面。
7. 当问题需要人类判断时，使用 `wiki/review.md`，不要强行自动清理。
8. 除非用户要求自动执行，否则确认后再写入。

示例：

```text
/obsiwiki ingest this source: https://example.com/article
```

### Capture

当一次对话产生了可复用结论、决策、定义、权衡或工作流时使用。

预期行为：

1. 只提取可复用知识。
2. 默认不保存完整聊天记录。
3. 如果存在 `System/Schema/purpose.md`，说明这个结论为什么值得沉淀。
4. 建议最合适的目标页面。
5. 确认后更新目标页面。
6. 仅当答案跨多个主题或来源时，才创建 `wiki/syntheses/` 页面。
7. 当有价值的结论仍需要人类判断时，使用 `wiki/review.md`。

示例：

```text
/obsiwiki capture reusable conclusions from this conversation.
```

### Query

当你希望 Agent 从知识库中回答问题时使用。

预期行为：

1. 从 `wiki/index.md` 开始。
2. 跟随相关 `wiki/maps/`。
3. 只读取必要的 `concepts`、`entities`、`sources` 和 `syntheses`。
4. 只有当答案具有广泛复用价值时，才建议新增或更新 `synthesis`。

示例：

```text
/obsiwiki answer this from the vault: What does my knowledge base say about the relationship between prompt engineering and UXD?
```

### Lint

当你想做知识库健康检查时使用。

预期行为：

- 标记未被 maps 或 index 覆盖的 wiki 页面
- 标记没有正式链接的来源页面
- 标记重复或近似重复主题
- 标记缺失 `last_updated`、缺失 sources 或缺失 `Related Links`
- 标记原始来源附件的放置问题
- 标记只保存在 raw 层、但应提升到 `assets/wiki/` 的附件
- 当当前 schema 需要时，标记缺失或过期的价值判断指南或支持页面，例如 `System/Schema/purpose.md`、`wiki/overview.md` 或 `wiki/review.md`
- 标记知识图谱健康问题：没有 map 覆盖的页面聚类、多个 source 指向同一主题但没有 concept、没有来源支撑的 concept、过期 synthesis、只堆链接的 map、疑似重复页面聚类、两个 map 之间可能需要 synthesis 或交叉链接
- 当 graph health 问题需要解释判断时，新增或建议 `wiki/review.md` item，而不是把它当成确定性失败
- 将 `System/` 排除在普通内容 lint 之外；不要要求 schema 或 agent 文件被 maps、sources、`last_updated` 或 `Related Links` 覆盖
- 对 `System/` 只做轻量配置检查：schema 文件可读、agent 适配遵循 `System/Schema/`、知识库本地 schema 自定义不被静默覆盖

示例：

```text
/obsiwiki lint this vault.
```

### Review

当你希望 Agent 只读回顾近期新增内容、显著更新或本周知识库变化时使用。

默认范围：如果没有指定范围，则使用最近 7 天。

预期行为：

1. 解析 `昨天`、`本周`、`本月`、`last 14 days`、`since 2026-04-01` 或 `2026-04-01 to 2026-04-15` 等范围。
2. 优先读取 `wiki/log.md`，其次使用页面 frontmatter 的 `last_updated`，最后才用文件修改时间兜底。
3. 总结时间范围、新增来源与页面、显著更新、主题聚类、待处理 review item、overview 漂移、值得继续整理的问题，以及下次 `ingest`、`capture` 或 `lint` 建议。
4. 默认保持只读，不写入知识库。
5. 当回顾改变了知识库整体状态判断时，建议更新 `wiki/overview.md`。
6. 对未解决的重复主题、来源缺失、过期 synthesis、价值不明或 graph health 问题，建议写入 `wiki/review.md`。
7. 如果你希望保存周报或长期摘要，请让 Agent 转入 `capture`，或建议更新 `wiki/syntheses/`，并在写入前确认。

示例：

```text
/obsiwiki review recent additions to this vault.
/obsiwiki review recent additions from yesterday.
/obsiwiki review recent additions from this week.
/obsiwiki review recent additions from this month.
/obsiwiki review recent additions since 2026-04-01.
/obsiwiki generate this week's knowledge base report.
```

### Scheduled Maintenance

当你希望 Agent 帮你创建 cron 任务或其它周期性自动化，用来定期执行 `review` 和 `lint` 时使用。

预期行为：

1. 询问用户是否需要为近期更新 review 和定期 lint 创建 scheduled maintenance。
2. 让用户选择周期和时间。默认使用用户本地时区的每周一 09:00。
3. 确认 review 和 lint 是作为一个组合任务运行，还是拆成两个任务运行。
4. 确认目标知识库路径、输出位置，以及该自动化是否允许写入后续修改。
5. 定期 review 和定期 lint 默认只读。如果任务发现值得长期保存的更新，应建议 `capture`、`synthesis`、`overview` 或 `review.md` 修改，而不是静默写入。
6. 优先使用当前 Agent 或宿主环境的原生调度能力；否则引导用户使用 cron 或该环境推荐的自动化机制。
7. 在创建或修改定时任务前，展示最终 schedule 和 maintenance prompt。

建议的定时任务内容：

```text
Review recent additions to this vault and lint this vault. Keep the run read-only by default. Summarize recent additions, notable updates, open review items, overview drift, lint issues, graph health issues, and suggested next actions. Propose any durable writes for user confirmation.
```

示例：

```text
/obsiwiki set up scheduled maintenance for this vault.
```


## 知识库目录结构

starter 知识库使用四个主要层次，并在 `System/Schema/` 下保存知识库本地价值判断指南：

```text
raw/
├── articles/        网页文章、摘录、教程和实践指南
├── papers/          论文、PDF、报告和研究材料
├── transcripts/     转录稿和长篇对话
└── meeting-notes/   会议、访谈和现场笔记

assets/
├── raw/             属于某个原始来源的文件，按来源 slug 分组
├── wiki/            长期复用的知识资产
├── projects/        项目相关资产
└── shared/          跨主题复用的资产

wiki/
├── concepts/        稳定概念、协议、框架和方法
├── entities/        工具、产品、公司、人物和命名系统
├── sources/         原始来源的一对一摘要
├── syntheses/       跨来源或跨对话的综合分析
├── maps/            主题地图 / MOC，用来保持页面连接
├── index.md         顶层导航入口
├── log.md           追加式维护日志
├── overview.md      当前知识库整体状态的压缩视图
└── review.md        需要人类判断或后续 Agent 跟进的待处理事项

System/
└── Schema/          知识库本地规则、工作流和价值判断指南
    └── purpose.md   面向 Agent 的价值判断指南
```

目录意图：

- `raw/` 尽量保留来源材料的原始形态。
- `assets/` 避免图片、PDF、截图等附件散落在笔记目录里。
- `System/Schema/purpose.md` 告诉 Agent 如何判断哪些内容值得沉淀。
- `wiki/` 是 Agent 应该查询和更新的稳定知识层。
- `wiki/maps/` 是主要的防孤立页面机制。
- `wiki/overview.md` 是给 Agent 和人看的知识库状态压缩视图。
- `wiki/review.md` 记录需要人类判断或后续跟进的不确定事项。
- `System/Schema/` 存在时，是知识库本地的权威规则来源；各 Agent 的适配文件应该遵循它，而不是另立一套规则。
- `System/Agents/` 是可选目录；只有当某个 Agent 需要读取知识库本地适配说明时才需要。

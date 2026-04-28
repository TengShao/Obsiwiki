# Obsiwiki

[English](#obsiwiki) | [中文](#中文说明)

`Obsiwiki` is an agent-agnostic workflow and skill package for maintaining an Obsidian vault as an LLM Wiki.

It gives agents a shared operating model for:

- ingesting links, articles, papers, transcripts, meeting notes, and unprocessed notes
- preserving source material in `raw/`
- compiling durable knowledge pages in `wiki/`
- capturing reusable conclusions from conversations
- answering questions from existing wiki pages
- reviewing recent additions and weekly knowledge base changes
- linting the vault for orphan pages, missing sources, duplicate topics, stale pages, and asset placement issues

## Contents

- [Agent Usage Quick Reference](#agent-usage-quick-reference)
- [Install For Codex](#install-for-codex)
- [Install For Claude Code](#install-for-claude-code)
- [Install For Hermes](#install-for-hermes)
- [Install For OpenClaw](#install-for-openclaw)
- [Add The Starter Vault Skeleton](#add-the-starter-vault-skeleton)
- [Vault Folder Structure](#vault-folder-structure)
- [Schema Precedence](#schema-precedence)
- [Core Workflows](#core-workflows)
- [Page Types](#page-types)
- [Minimal Frontmatter](#minimal-frontmatter)
- [Using Other Agents](#using-other-agents)
- [Update Skill](#update-skill)

## Acknowledgements

Obsiwiki is inspired by Andrej Karpathy's [LLM Wiki](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f) note. Thanks to Karpathy for articulating the pattern of using LLM agents to incrementally maintain a persistent, interlinked wiki over raw source material.

## Repository Layout

```text
.
├── SKILL.md
├── adapters/
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
    ├── assets/
    ├── raw/
    └── wiki/
```

## Agent Usage Quick Reference

| Agent | How to load Obsiwiki | Typical usage |
| --- | --- | --- |
| Codex | Ask Codex to read this README, install the needed files where it keeps skills, and restart Codex. | `Use Obsiwiki to ingest this source: https://example.com/article` |
| Claude Code | Ask Claude Code to read this README, install the needed files where it keeps project instructions or skills, and use `adapters/claude-code.md`. | `Use Obsiwiki to ingest this source: https://example.com/article` |
| Hermes | Ask Hermes to read this README, install the needed files where it keeps skills/plugins/context, and use `adapters/hermes.md`. | `Use Obsiwiki to ingest this source: https://example.com/article` |
| OpenClaw | Ask OpenClaw to read this README, install the needed files where it keeps commands/plugins/skills, and expose `/obsiwiki` commands. | `/obsiwiki ingest https://example.com/article` |

Common workflow verbs:

- `ingest`: save a source into `raw/`, create a source summary in `wiki/sources/`, and update maps/index/log.
- `capture`: extract reusable conclusions from a conversation and update the best target page.
- `query`: answer from `wiki/index.md`, maps, and formal wiki pages.
- `lint`: check for orphan pages, missing sources, duplicates, stale pages, and asset placement issues.
- `review`: summarize recent additions, notable updates, topic clusters, and next actions without writing to the vault.

Canonical workflow prompts:

```text
Use Obsiwiki to ingest this source: https://example.com/article
Use Obsiwiki to capture reusable conclusions from this conversation.
Use Obsiwiki to answer this from the vault: <question>
Use Obsiwiki to lint this vault.
Use Obsiwiki to review recent additions to this vault.
Use Obsiwiki to generate this week's knowledge base report.
```

## Install For Codex

Agent-friendly install prompt:

```text
Read https://github.com/TengShao/Obsiwiki and install Obsiwiki for Codex.

Decide where Codex should keep reusable skills in this environment, copy or clone the necessary Obsiwiki files there, and make sure the installed skill is named obsiwiki.

After installation, tell me which files you installed, where you installed them, and how to restart or reload Codex so the skill is picked up.
```

Use the same canonical prompts as other agents. In Codex, `$obsiwiki` can also be used when you want to explicitly invoke the skill.

```text
Use Obsiwiki to ingest this source: https://example.com/article
Use Obsiwiki to capture reusable conclusions from this conversation.
Use Obsiwiki to answer this from the vault: What is the relationship between A2A and MCP?
Use Obsiwiki to lint this vault.
Use Obsiwiki to review recent additions to this vault.
Use Obsiwiki to generate this week's knowledge base report.
```

## Install For Claude Code

Agent-friendly install prompt:

```text
Read https://github.com/TengShao/Obsiwiki and install Obsiwiki for Claude Code.

Decide where Claude Code should keep reusable project instructions or skills in this environment, copy the necessary Obsiwiki files there, and set up the target vault so Claude Code reads adapters/claude-code.md and follows Obsiwiki schema precedence.

After installation, tell me which files you copied and where.
```

Claude Code should use the adapter:

```text
adapters/claude-code.md
```

Recommended vault-local setup: copy the starter vault files, then create a `CLAUDE.md` file at the vault root that points Claude Code to the vault-local schema:

```text
System/Schema/vault-schema.md
System/Schema/workflows.md
System/Schema/page-contracts.md
System/Agents/claude-code.md
```

Typical Claude Code prompts:

```text
Use Obsiwiki to ingest this source: https://example.com/article
Use Obsiwiki to capture reusable conclusions from this conversation.
Use Obsiwiki to answer this from the vault: ...
Use Obsiwiki to lint this vault.
Use Obsiwiki to review recent additions to this vault.
Use Obsiwiki to generate this week's knowledge base report.
```

## Install For Hermes

Agent-friendly install prompt:

```text
Read https://github.com/TengShao/Obsiwiki and install Obsiwiki for Hermes.

Decide where Hermes should keep reusable skills, plugins, or context in this environment, copy the necessary Obsiwiki files there, and configure Hermes to load adapters/hermes.md together with the Obsiwiki references and schema.

After installation, tell me which files you copied and where.
```

Hermes should use the adapter:

```text
adapters/hermes.md
```

Typical Hermes-style prompts:

```text
Use Obsiwiki to ingest this source: https://example.com/article
Use Obsiwiki to capture reusable conclusions from this conversation.
Use Obsiwiki to answer this from the vault: ...
Use Obsiwiki to lint this vault.
Use Obsiwiki to review recent additions to this vault.
Use Obsiwiki to generate this week's knowledge base report.
```

## Install For OpenClaw

Agent-friendly install prompt:

```text
Read https://github.com/TengShao/Obsiwiki and install Obsiwiki for OpenClaw.

Decide where OpenClaw should keep reusable slash commands, plugins, or skills in this environment, copy the necessary Obsiwiki files there, and expose the /obsiwiki command workflow using adapters/openclaw.md.

After installation, tell me which files you copied and where.
```

OpenClaw should use the adapter:

```text
adapters/openclaw.md
```

Recommended slash-command shape:

```text
/obsiwiki ingest <url-or-source>
/obsiwiki capture
/obsiwiki capture <target-page>
/obsiwiki capture synthesis
/obsiwiki query <question>
/obsiwiki lint
/obsiwiki review [range]
/obsiwiki weekly
/obsiwiki help
```

Each command should route into the same shared `ingest / capture / query / lint / review` workflows defined by this skill and `System/Schema/`.

## Add The Starter Vault Skeleton

If you are starting from a new or lightly structured Obsidian vault, copy the starter files into the vault root:

```bash
cp -R starter-vault/* /path/to/your/obsidian-vault/
```

The starter vault creates this knowledge architecture:

```text
raw/        original source material
assets/     attachments, screenshots, PDFs, reusable visuals
wiki/       durable knowledge pages for agent query and synthesis
System/     schema, workflow, page contracts, and agent adapters
```

If your vault already has notes, do not bulk move them first. Let an agent use the lint workflow to suggest incremental changes.

## Vault Folder Structure

The starter vault uses four main layers:

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
└── log.md           append-only maintenance log

System/
├── Schema/          optional vault-local authority for rules and workflows
└── Agents/          adapter notes for Codex, Claude Code, Hermes, OpenClaw, or other agents
```

Folder intent:

- `raw/` keeps source material close to its original form.
- `assets/` keeps attachments and media files out of note folders.
- `wiki/` is the durable knowledge layer agents should query and update.
- `wiki/maps/` is the main anti-orphan mechanism.
- `System/Schema/`, when present, is the vault-local authority; agent-specific files should follow it instead of defining separate rules.

## Schema Precedence

Use the installed Obsiwiki `references/` and `starter-vault/System/Schema/` as the default schema.

If the target vault contains `System/Schema/`, treat that vault-local schema as authoritative. Use the installed schema only for comparison, update suggestions, and migrations. Do not silently overwrite vault-local schema customizations.

Agent adapters only explain how each agent should load and run Obsiwiki. Directory layout, page requirements, and workflow rules come from the active schema.

## Core Workflows

### Ingest

Use when you give the agent a URL, article, PDF, screenshot pack, transcript, meeting note, or unprocessed note.

Expected behavior:

1. Save or summarize the source material under `raw/`.
2. Save attachments under `assets/raw/<source-slug>/`.
3. Create or update a one-to-one source summary under `wiki/sources/`.
4. Update existing `wiki/concepts/`, `wiki/entities/`, or `wiki/syntheses/` when useful.
5. Attach the new or updated knowledge to at least one `wiki/maps/` page.
6. Update `wiki/index.md` and append to `wiki/log.md` after confirmed changes.

Example:

```text
Use Obsiwiki to ingest this source: https://example.com/article
```

### Capture

Use when a conversation produced a reusable conclusion, decision, definition, tradeoff, or workflow.

Expected behavior:

1. Extract only reusable knowledge.
2. Avoid saving the full chat transcript by default.
3. Suggest the best target page.
4. Update the target page after confirmation.
5. Create a `wiki/syntheses/` page only when the answer spans multiple topics or sources.

Example:

```text
Use Obsiwiki to capture reusable conclusions from this conversation.
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
Use Obsiwiki to answer this from the vault: What does my knowledge base say about the relationship between prompt engineering and UXD?
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

Example:

```text
Use Obsiwiki to lint this vault.
```

### Review

Use when you want a read-only review of recent additions, recent updates, or this week's knowledge base changes.

Default range: the last 7 days unless you specify a range.

Expected behavior:

1. Resolve ranges such as `yesterday`, `this week`, `this month`, `last 14 days`, `since 2026-04-01`, or `2026-04-01 to 2026-04-15`.
2. Read `wiki/log.md` first, page frontmatter `last_updated` second, and file modification time only as a fallback.
3. Summarize the time range, new sources and pages, notable updates, topic clusters, open organization questions, and suggested next `ingest`, `capture`, or `lint` actions.
4. Keep the review read-only by default.
5. If you want to save a weekly report or durable summary, ask the agent to switch to `capture` or propose a `wiki/syntheses/` update and confirm before writing.

Examples:

```text
Use Obsiwiki to review recent additions to this vault.
Use Obsiwiki to review recent additions from yesterday.
Use Obsiwiki to review recent additions from this week.
Use Obsiwiki to review recent additions from this month.
Use Obsiwiki to review recent additions since 2026-04-01.
Use Obsiwiki to generate this week's knowledge base report.
```

Weekly reports are prompt-triggered, not a built-in scheduler. If you want an automatic weekly report, ask the agent to help set up cron or another system automation that regularly sends:

```text
Use Obsiwiki to generate this week's knowledge base report.
```

## Page Types

Formal wiki pages use these `type` values:

- `concept`: durable concept, protocol, framework, or method
- `entity`: tool, product, company, person, or named system
- `source`: one-to-one summary of a raw source
- `synthesis`: cross-source or cross-discussion integrated page
- `map`: topic map / MOC

## Minimal Frontmatter

Formal wiki pages should include:

```yaml
---
title:
type:
tags: []
sources: []
last_updated: YYYY-MM-DD
aliases: []
---
```

Tags should describe the topic, not the folder or workflow state. Prefer lowercase English `kebab-case` tags.

## Using Other Agents

For agents other than Codex, Claude Code, OpenClaw, or Hermes, point them at:

- `SKILL.md`
- `adapters/claude-code.md`, `adapters/hermes.md`, or `adapters/openclaw.md` if relevant
- `references/schema.md`
- `references/page-types.md`
- `references/lint-checklist.md`
- `starter-vault/System/Schema/`

Then use the same canonical prompts:

```text
Use Obsiwiki to ingest this source: https://example.com/article
Use Obsiwiki to capture reusable conclusions from this conversation.
Use Obsiwiki to answer this from the vault: <question>
Use Obsiwiki to lint this vault.
Use Obsiwiki to review recent additions to this vault.
Use Obsiwiki to generate this week's knowledge base report.
```

## Update Skill

Ask the target agent to read this README, find its existing Obsiwiki install location, and refresh the source files from the latest repository version.

For an agent-friendly update prompt, use:

```text
Read https://github.com/TengShao/Obsiwiki and update the existing Obsiwiki installation for this agent.

Find where Obsiwiki was installed in this environment, refresh the copied or cloned source files from the latest repository version, preserve any vault-local System/Schema/ customizations, make sure review and weekly report prompts are included in the skill, schema, adapters, and README, and tell me what changed.
```

Make sure the agent reloads the refreshed files:

- `SKILL.md`
- `references/schema.md`
- `references/page-types.md`
- `references/lint-checklist.md`
- `starter-vault/System/Schema/` or the vault-local `System/Schema/`
- the relevant adapter under `adapters/`
- `README.md`, including the canonical `review` and weekly report prompts

If the target vault already has a customized `System/Schema/`, compare it with the latest schema and manually merge the useful changes. Do not overwrite user-edited files directly.

---

# 中文说明

`Obsiwiki` 是一套不绑定具体 Agent 的 Obsidian 知识库维护方案，用来把知识库维护成适合大语言模型读取和持续维护的结构。

它为不同 Agent 提供一套共享的操作模型，用于：

- 收录链接、文章、论文、转录稿、会议记录和原始笔记
- 将原始材料保存在 `raw/`
- 将稳定知识编译到 `wiki/`
- 将对话中形成的可复用结论沉淀到知识库
- 基于已有 wiki 页面回答问题
- 回顾近期新增内容和本周知识库变化
- 对知识库做健康检查，发现孤立页面、缺失来源、重复主题、过期页面和附件放置问题

## 目录

- [Agent 使用速查](#agent-使用速查)
- [为 Codex 安装](#为-codex-安装)
- [为 Claude Code 安装](#为-claude-code-安装)
- [为 Hermes 安装](#为-hermes-安装)
- [为 OpenClaw 安装](#为-openclaw-安装)
- [添加 Starter 知识库骨架](#添加-starter-知识库骨架)
- [知识库目录结构](#知识库目录结构)
- [Schema 优先级](#schema-优先级)
- [核心工作流](#核心工作流)
- [页面类型](#页面类型)
- [最小 Frontmatter](#最小-frontmatter)
- [其他Agent使用方法](#其他agent使用方法)
- [更新 Skill](#更新-skill)

## 致谢

Obsiwiki 受到 Andrej Karpathy 的 [LLM Wiki](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f) 这篇笔记启发，也就是让大语言模型持续维护一套持久、互联的 wiki。感谢 Karpathy 清晰描述了这个模式。

## 仓库结构

```text
.
├── SKILL.md
├── adapters/
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
    ├── assets/
    ├── raw/
    └── wiki/
```

## Agent 使用速查

| Agent | 如何加载 Obsiwiki | 典型用法 |
| --- | --- | --- |
| Codex | 让 Codex 读取本 README，把必要文件安装到它保存技能的目录，然后重启 Codex。 | `Use Obsiwiki to ingest this source: https://example.com/article` |
| Claude Code | 让 Claude Code 读取本 README，把必要文件安装到它保存项目说明或技能的目录，并使用 `adapters/claude-code.md`。 | `Use Obsiwiki to ingest this source: https://example.com/article` |
| Hermes | 让 Hermes 读取本 README，把必要文件安装到它保存技能、插件或长期上下文的目录，并使用 `adapters/hermes.md`。 | `Use Obsiwiki to ingest this source: https://example.com/article` |
| OpenClaw | 让 OpenClaw 读取本 README，把必要文件安装到它保存命令、插件或技能的目录，并暴露 `/obsiwiki` 命令。 | `/obsiwiki ingest https://example.com/article` |

常用工作流：

- `ingest`：将来源保存到 `raw/`，在 `wiki/sources/` 创建来源摘要，并更新 maps/index/log。
- `capture`：从对话中提取可复用结论，并更新最合适的目标页面。
- `query`：从 `wiki/index.md`、maps 和正式 wiki 页面中回答问题。
- `lint`：检查孤立页面、缺失来源、重复主题、过期页面和附件放置问题。
- `review`：只读总结近期新增内容、显著更新、主题聚类和后续行动建议。

常用提示：

```text
Use Obsiwiki to ingest this source: https://example.com/article
Use Obsiwiki to capture reusable conclusions from this conversation.
Use Obsiwiki to answer this from the vault: <question>
Use Obsiwiki to lint this vault.
Use Obsiwiki to review recent additions to this vault.
Use Obsiwiki to generate this week's knowledge base report.
```

## 为 Codex 安装

可直接发给 Agent 的安装提示：

```text
Read https://github.com/TengShao/Obsiwiki and install Obsiwiki for Codex.

Decide where Codex should keep reusable skills in this environment, copy or clone the necessary Obsiwiki files there, and make sure the installed skill is named obsiwiki.

After installation, tell me which files you installed, where you installed them, and how to restart or reload Codex so the skill is picked up.
```

使用方式与其它 Agent 相同。在 Codex 中，如果希望显式调用，也可以使用 `$obsiwiki`。

```text
Use Obsiwiki to ingest this source: https://example.com/article
Use Obsiwiki to capture reusable conclusions from this conversation.
Use Obsiwiki to answer this from the vault: What is the relationship between A2A and MCP?
Use Obsiwiki to lint this vault.
Use Obsiwiki to review recent additions to this vault.
Use Obsiwiki to generate this week's knowledge base report.
```

## 为 Claude Code 安装

可直接发给 Agent 的安装提示：

```text
Read https://github.com/TengShao/Obsiwiki and install Obsiwiki for Claude Code.

Decide where Claude Code should keep reusable project instructions or skills in this environment, copy the necessary Obsiwiki files there, and set up the target vault so Claude Code reads adapters/claude-code.md and follows Obsiwiki schema precedence.

After installation, tell me which files you copied and where.
```

Claude Code 应使用这个适配文件：

```text
adapters/claude-code.md
```

推荐的知识库本地设置：复制 starter 知识库文件，然后在知识库根目录创建 `CLAUDE.md`，让 Claude Code 指向知识库本地 schema：

```text
System/Schema/vault-schema.md
System/Schema/workflows.md
System/Schema/page-contracts.md
System/Agents/claude-code.md
```

典型 Claude Code 提示：

```text
Use Obsiwiki to ingest this source: https://example.com/article
Use Obsiwiki to capture reusable conclusions from this conversation.
Use Obsiwiki to answer this from the vault: ...
Use Obsiwiki to lint this vault.
Use Obsiwiki to review recent additions to this vault.
Use Obsiwiki to generate this week's knowledge base report.
```

## 为 Hermes 安装

可直接发给 Agent 的安装提示：

```text
Read https://github.com/TengShao/Obsiwiki and install Obsiwiki for Hermes.

Decide where Hermes should keep reusable skills, plugins, or context in this environment, copy the necessary Obsiwiki files there, and configure Hermes to load adapters/hermes.md together with the Obsiwiki references and schema.

After installation, tell me which files you copied and where.
```

Hermes 应使用这个适配文件：

```text
adapters/hermes.md
```

典型 Hermes 提示：

```text
Use Obsiwiki to ingest this source: https://example.com/article
Use Obsiwiki to capture reusable conclusions from this conversation.
Use Obsiwiki to answer this from the vault: ...
Use Obsiwiki to lint this vault.
Use Obsiwiki to review recent additions to this vault.
Use Obsiwiki to generate this week's knowledge base report.
```

## 为 OpenClaw 安装

可直接发给 Agent 的安装提示：

```text
Read https://github.com/TengShao/Obsiwiki and install Obsiwiki for OpenClaw.

Decide where OpenClaw should keep reusable slash commands, plugins, or skills in this environment, copy the necessary Obsiwiki files there, and expose the /obsiwiki command workflow using adapters/openclaw.md.

After installation, tell me which files you copied and where.
```

OpenClaw 应使用这个适配文件：

```text
adapters/openclaw.md
```

推荐 slash-command 形态：

```text
/obsiwiki ingest <url-or-source>
/obsiwiki capture
/obsiwiki capture <target-page>
/obsiwiki capture synthesis
/obsiwiki query <question>
/obsiwiki lint
/obsiwiki review [range]
/obsiwiki weekly
/obsiwiki help
```

每个命令都应该进入同一套由 Obsiwiki 和当前生效 schema 定义的 `ingest / capture / query / lint / review` 工作流。

## 添加 Starter 知识库骨架

如果你正在从一个新的或结构较轻的 Obsidian 知识库开始，可以把 starter 文件复制到知识库根目录：

```bash
cp -R starter-vault/* /path/to/your/obsidian-vault/
```

starter 知识库会创建这样的知识架构：

```text
raw/        原始来源材料
assets/     图片、PDF、截图和其它附件
wiki/       可供 Agent 查询和综合的稳定知识页面
System/     schema、工作流、页面规范和 Agent 适配说明
```

如果你的知识库已经有笔记，不要先批量移动它们。让 Agent 使用 lint 工作流逐步提出增量调整建议。

## 知识库目录结构

starter 知识库使用四个主要层次：

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
└── log.md           追加式维护日志

System/
├── Schema/          可选的知识库本地规则和工作流权威来源
└── Agents/          Codex、Claude Code、Hermes、OpenClaw 或其它 Agent 的适配说明
```

目录意图：

- `raw/` 尽量保留来源材料的原始形态。
- `assets/` 避免图片、PDF、截图等附件散落在笔记目录里。
- `wiki/` 是 Agent 应该查询和更新的稳定知识层。
- `wiki/maps/` 是主要的防孤立页面机制。
- `System/Schema/` 存在时，是知识库本地的权威规则来源；各 Agent 的适配文件应该遵循它，而不是另立一套规则。

## Schema 优先级

默认使用已安装的 Obsiwiki `references/` 和 `starter-vault/System/Schema/` 作为 schema。

如果目标知识库包含 `System/Schema/`，则将该知识库本地 schema 视为权威规则来源。已安装 schema 只用于对照、更新建议和迁移。不要静默覆盖知识库本地 schema 自定义内容。

Agent 适配文件只负责说明不同 Agent 应该如何加载和执行 Obsiwiki。目录结构、页面要求和工作流规则都以当前生效的 schema 为准。

## 核心工作流

### Ingest

当你给 Agent 一个 URL、文章、PDF、截图包、转录稿、会议记录或原始笔记时使用。

预期行为：

1. 将原始来源保存或摘要到 `raw/`。
2. 将附件保存到 `assets/raw/<source-slug>/`。
3. 在 `wiki/sources/` 下创建或更新一对一来源摘要。
4. 在有用时更新已有的 `wiki/concepts/`、`wiki/entities/` 或 `wiki/syntheses/`。
5. 将新增或更新的知识挂到至少一个 `wiki/maps/` 页面。
6. 在确认修改后更新 `wiki/index.md`，并追加 `wiki/log.md`。

示例：

```text
Use Obsiwiki to ingest this source: https://example.com/article
```

### Capture

当一次对话产生了可复用结论、决策、定义、权衡或工作流时使用。

预期行为：

1. 只提取可复用知识。
2. 默认不保存完整聊天记录。
3. 建议最合适的目标页面。
4. 确认后更新目标页面。
5. 仅当答案跨多个主题或来源时，才创建 `wiki/syntheses/` 页面。

示例：

```text
Use Obsiwiki to capture reusable conclusions from this conversation.
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
Use Obsiwiki to answer this from the vault: What does my knowledge base say about the relationship between prompt engineering and UXD?
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

示例：

```text
Use Obsiwiki to lint this vault.
```

### Review

当你希望 Agent 只读回顾近期新增内容、显著更新或本周知识库变化时使用。

默认范围：如果没有指定范围，则使用最近 7 天。

预期行为：

1. 解析 `昨天`、`本周`、`本月`、`last 14 days`、`since 2026-04-01` 或 `2026-04-01 to 2026-04-15` 等范围。
2. 优先读取 `wiki/log.md`，其次使用页面 frontmatter 的 `last_updated`，最后才用文件修改时间兜底。
3. 总结时间范围、新增来源与页面、显著更新、主题聚类、值得继续整理的问题，以及下次 `ingest`、`capture` 或 `lint` 建议。
4. 默认保持只读，不写入知识库。
5. 如果你希望保存周报或长期摘要，请让 Agent 转入 `capture`，或建议更新 `wiki/syntheses/`，并在写入前确认。

示例：

```text
Use Obsiwiki to review recent additions to this vault.
Use Obsiwiki to review recent additions from yesterday.
Use Obsiwiki to review recent additions from this week.
Use Obsiwiki to review recent additions from this month.
Use Obsiwiki to review recent additions since 2026-04-01.
Use Obsiwiki to generate this week's knowledge base report.
```

周报由提示词触发，不是内置调度器。如果你想自动生成周报，可以让 Agent 帮你建立 cron 或其它系统自动化，定期发送：

```text
Use Obsiwiki to generate this week's knowledge base report.
```

## 页面类型

正式 wiki 页面使用这些 `type` 值：

- `concept`：稳定概念、协议、框架或方法
- `entity`：工具、产品、公司、人物或命名系统
- `source`：原始来源的一对一摘要
- `synthesis`：跨来源或跨讨论的综合页面
- `map`：主题地图 / MOC

## 最小 Frontmatter

正式 wiki 页面应包含：

```yaml
---
title:
type:
tags: []
sources: []
last_updated: YYYY-MM-DD
aliases: []
---
```

Tags 应描述主题，而不是文件夹或工作流状态。优先使用 lowercase English `kebab-case` tags。

## 其他Agent使用方法

对于非 Codex / Claude Code / OpenClaw / Hermes 的 Agent，请让它们读取：

- `SKILL.md`
- 相关适配文件：`adapters/claude-code.md`、`adapters/hermes.md` 或 `adapters/openclaw.md`
- `references/schema.md`
- `references/page-types.md`
- `references/lint-checklist.md`
- `starter-vault/System/Schema/`

然后使用同样的规范提示：

```text
Use Obsiwiki to ingest this source: https://example.com/article
Use Obsiwiki to capture reusable conclusions from this conversation.
Use Obsiwiki to answer this from the vault: <question>
Use Obsiwiki to lint this vault.
Use Obsiwiki to review recent additions to this vault.
Use Obsiwiki to generate this week's knowledge base report.
```

## 更新 Skill

让目标 Agent 读取本 README，找到已有 Obsiwiki 安装位置，并从最新仓库版本刷新源文件。

可直接发给 Agent 的更新提示：

```text
Read https://github.com/TengShao/Obsiwiki and update the existing Obsiwiki installation for this agent.

Find where Obsiwiki was installed in this environment, refresh the copied or cloned source files from the latest repository version, preserve any vault-local System/Schema/ customizations, make sure review and weekly report prompts are included in the skill, schema, adapters, and README, and tell me what changed.
```

确保 Agent 重新加载刷新后的文件：

- `SKILL.md`
- `references/schema.md`
- `references/page-types.md`
- `references/lint-checklist.md`
- `starter-vault/System/Schema/` 或知识库本地 `System/Schema/`
- 位于 `adapters/` 的相关适配文件
- `README.md`，包括规范的 `review` 和周报提示词

如果目标知识库已经有自定义的 `System/Schema/`，更新时请先对比差异，再手动合并需要的变化；不要直接覆盖用户编辑过的文件。

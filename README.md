# Obsiwiki

[English](#obsiwiki) | [中文](#中文说明)

`Obsiwiki` is an agent-agnostic workflow and skill package for maintaining an Obsidian vault as an LLM Wiki.

It gives agents a shared operating model for:

- ingesting links, articles, papers, transcripts, meeting notes, and raw notes
- preserving source material in `raw/`
- compiling durable knowledge pages in `wiki/`
- capturing reusable conclusions from conversations
- answering questions from existing wiki pages
- linting the vault for orphan pages, missing sources, duplicate topics, stale pages, and asset placement issues

The package includes a reusable `SKILL.md`, reference rules, and a starter vault skeleton. It does not include personal notes or private knowledge content.

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
- [Notes For Other Agents](#notes-for-other-agents)
- [Privacy](#privacy)
- [Update Skill](#update-skill)

## Acknowledgements

Obsiwiki is inspired by Andrej Karpathy's [LLM Wiki](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f) idea file. Thanks to Karpathy for articulating the pattern of using LLM agents to incrementally maintain a persistent, interlinked wiki over raw source material.

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

- `ingest`: save a source into `raw/`, create a digest in `wiki/sources/`, and update maps/index/log.
- `capture`: extract reusable conclusions from a conversation and update the best target page.
- `query`: answer from `wiki/index.md`, maps, and formal wiki pages.
- `lint`: check for orphan pages, missing sources, duplicates, stale pages, and asset placement issues.

Canonical workflow prompts:

```text
Use Obsiwiki to ingest this source: https://example.com/article
Use Obsiwiki to capture reusable conclusions from this conversation.
Use Obsiwiki to answer this from the vault: <question>
Use Obsiwiki to lint this vault.
```

## Install For Codex

Ask Codex to install Obsiwiki from this repository. It should read this README, decide where Codex stores skills in the current environment, copy or clone the necessary files there, and tell you how to reload the skill.

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
```

## Install For Claude Code

Ask Claude Code to install Obsiwiki from this repository. It should read this README, choose the correct Claude-accessible instruction, skill, or project context location, copy the necessary source files there, and use the schema precedence rules below.

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
```

## Install For Hermes

Ask Hermes to install Obsiwiki from this repository. It should read this README, decide where Hermes stores reusable skills, plugins, or long-lived context in the current environment, and copy the necessary source files there.

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

Give Hermes these files as operating context:

- `SKILL.md`
- `references/schema.md`
- `references/page-types.md`
- `references/lint-checklist.md`
- `starter-vault/System/Schema/`

Typical Hermes-style prompts:

```text
Use Obsiwiki to ingest this source: https://example.com/article
Use Obsiwiki to capture reusable conclusions from this conversation.
Use Obsiwiki to answer this from the vault: ...
Use Obsiwiki to lint this vault.
```

## Install For OpenClaw

Ask OpenClaw to install Obsiwiki from this repository. It should read this README, decide where OpenClaw stores slash commands, plugins, or skills in the current environment, copy the necessary source files there, and expose the Obsiwiki command workflow.

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
/obsiwiki help
```

Each command should route into the same shared `ingest / capture / query / lint` workflows defined by this skill and `System/Schema/`.

## Add The Starter Vault Skeleton

If you are starting from a new or lightly structured Obsidian vault, copy the starter files into the vault root:

```bash
cp -R starter-vault/* /path/to/your/obsidian-vault/
```

The starter vault creates this knowledge architecture:

```text
raw/        original source material
assets/     binary files, screenshots, PDFs, reusable visuals
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
├── raw/             files belonging to one raw source, grouped by source slug
├── wiki/            reusable long-lived knowledge assets
├── projects/        project-specific assets
└── shared/          assets reused across topics

wiki/
├── concepts/        durable concepts, protocols, frameworks, and methods
├── entities/        tools, products, companies, people, and named systems
├── sources/         one-to-one digests of raw sources
├── syntheses/       integrated analysis across sources or conversations
├── maps/            topic maps / MOCs that keep pages connected
├── index.md         top-level navigation entry
└── log.md           append-only maintenance log

System/
├── Schema/          optional vault-local source of truth for rules and workflows
└── Agents/          adapter notes for Codex, Claude Code, Hermes, OpenClaw, or other agents
```

Folder intent:

- `raw/` keeps source material close to its original form.
- `assets/` keeps binary files out of note folders.
- `wiki/` is the durable knowledge layer agents should query and update.
- `wiki/maps/` is the main anti-orphan mechanism.
- `System/Schema/`, when present, is the vault-local source of truth; agent-specific files should adapt it, not fork it.

## Schema Precedence

Use the installed Obsiwiki `references/` and `starter-vault/System/Schema/` as the default schema.

If the target vault contains `System/Schema/`, treat that vault-local schema as the source of truth. Use the installed schema only for comparison, update suggestions, and migrations. Do not silently overwrite vault-local schema customizations.

Agent adapters translate the same schema for each agent. They must not fork directory semantics, page contracts, or workflow behavior.

## Core Workflows

### Ingest

Use when you give the agent a URL, article, PDF, screenshot pack, transcript, meeting note, or raw note.

Expected behavior:

1. Save or summarize the raw source under `raw/`.
2. Save attachments under `assets/raw/<source-slug>/`.
3. Create or update a one-to-one digest under `wiki/sources/`.
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
- flag raw attachment placement problems
- flag wiki pages that should promote raw-only assets into `assets/wiki/`

Example:

```text
Use Obsiwiki to lint this vault.
```

## Page Types

Formal wiki pages use these `type` values:

- `concept`: durable concept, protocol, framework, or method
- `entity`: tool, product, company, person, or named system
- `source`: one-to-one digest of a raw source
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

## Notes For Other Agents

This skill is agent-agnostic. For agents that do not support Codex skills directly, point them at:

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
```

## Privacy

This repository intentionally contains only reusable rules and empty starter structure. Do not publish your personal `raw/`, `wiki/`, `Projects/`, `Work/`, `Opinions/`, or `Journal/` content unless you have reviewed it for private or sensitive information.

## Update Skill

Ask the target agent to read this README, find its existing Obsiwiki install location, and refresh the source files from the latest repository version.

For an agent-friendly update prompt, use:

```text
Read https://github.com/TengShao/Obsiwiki and update the existing Obsiwiki installation for this agent.

Find where Obsiwiki was installed in this environment, refresh the copied or cloned source files from the latest repository version, preserve any vault-local System/Schema/ customizations, and tell me what changed.
```

Make sure the agent reloads the refreshed files:

- `SKILL.md`
- `references/schema.md`
- `references/page-types.md`
- `references/lint-checklist.md`
- `starter-vault/System/Schema/` or the vault-local `System/Schema/`
- the relevant adapter under `adapters/`

If the target vault already has a customized `System/Schema/`, reconcile it with the latest schema instead of blindly replacing user-edited files.

---

# 中文说明

`Obsiwiki` 是一个与 agent 无关的 workflow / skill 包，用来把 Obsidian vault 维护成一个 LLM Wiki。

它为不同 agent 提供一套共享的操作模型，用于：

- ingest 链接、文章、论文、转录稿、会议记录和原始笔记
- 将原始材料保存在 `raw/`
- 将稳定知识编译到 `wiki/`
- 从对话中捕获可复用结论
- 基于已有 wiki 页面回答问题
- lint vault，检查孤立页面、缺失来源、重复主题、过期页面和附件放置问题

这个包包含可复用的 `SKILL.md`、参考规则和 starter vault 骨架。它不包含个人笔记或私有知识内容。

## 目录

- [Agent 使用速查](#agent-使用速查)
- [为 Codex 安装](#为-codex-安装)
- [为 Claude Code 安装](#为-claude-code-安装)
- [为 Hermes 安装](#为-hermes-安装)
- [为 OpenClaw 安装](#为-openclaw-安装)
- [添加 Starter Vault 骨架](#添加-starter-vault-骨架)
- [Vault 目录结构](#vault-目录结构)
- [Schema 优先级](#schema-优先级)
- [核心工作流](#核心工作流)
- [页面类型](#页面类型)
- [最小 Frontmatter](#最小-frontmatter)
- [给其它 Agent 的说明](#给其它-agent-的说明)
- [隐私](#隐私)
- [更新 Skill](#更新-skill)

## 致谢

Obsiwiki 受到 Andrej Karpathy 的 [LLM Wiki](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f) idea file 启发。感谢 Karpathy 清晰描述了使用 LLM agent 递增维护持久、互联 wiki 的模式。

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
| Codex | 让 Codex 读取本 README，把必要文件安装到它保存 skills 的位置，然后重启 Codex。 | `Use Obsiwiki to ingest this source: https://example.com/article` |
| Claude Code | 让 Claude Code 读取本 README，把必要文件安装到它保存项目说明或 skills 的位置，并使用 `adapters/claude-code.md`。 | `Use Obsiwiki to ingest this source: https://example.com/article` |
| Hermes | 让 Hermes 读取本 README，把必要文件安装到它保存 skills/plugins/context 的位置，并使用 `adapters/hermes.md`。 | `Use Obsiwiki to ingest this source: https://example.com/article` |
| OpenClaw | 让 OpenClaw 读取本 README，把必要文件安装到它保存 commands/plugins/skills 的位置，并暴露 `/obsiwiki` 命令。 | `/obsiwiki ingest https://example.com/article` |

常用工作流动词：

- `ingest`：将来源保存到 `raw/`，在 `wiki/sources/` 创建 digest，并更新 maps/index/log。
- `capture`：从对话中提取可复用结论，并更新最合适的目标页面。
- `query`：从 `wiki/index.md`、maps 和正式 wiki 页面中回答问题。
- `lint`：检查孤立页面、缺失来源、重复主题、过期页面和附件放置问题。

规范工作流提示：

```text
Use Obsiwiki to ingest this source: https://example.com/article
Use Obsiwiki to capture reusable conclusions from this conversation.
Use Obsiwiki to answer this from the vault: <question>
Use Obsiwiki to lint this vault.
```

## 为 Codex 安装

让 Codex 从本仓库安装 Obsiwiki。它应该读取本 README，判断当前环境中 Codex 保存 skills 的位置，把必要文件复制或 clone 到那里，并告诉你如何重新加载 skill。

Agent-friendly 安装提示：

```text
Read https://github.com/TengShao/Obsiwiki and install Obsiwiki for Codex.

Decide where Codex should keep reusable skills in this environment, copy or clone the necessary Obsiwiki files there, and make sure the installed skill is named obsiwiki.

After installation, tell me which files you installed, where you installed them, and how to restart or reload Codex so the skill is picked up.
```

使用方式与其它 agent 相同。在 Codex 中，如果希望显式调用 skill，也可以使用 `$obsiwiki`。

```text
Use Obsiwiki to ingest this source: https://example.com/article
Use Obsiwiki to capture reusable conclusions from this conversation.
Use Obsiwiki to answer this from the vault: What is the relationship between A2A and MCP?
Use Obsiwiki to lint this vault.
```

## 为 Claude Code 安装

让 Claude Code 从本仓库安装 Obsiwiki。它应该读取本 README，选择 Claude 可访问的 instruction、skill 或 project context 位置，复制必要源文件，并遵循下面的 schema 优先级规则。

Agent-friendly 安装提示：

```text
Read https://github.com/TengShao/Obsiwiki and install Obsiwiki for Claude Code.

Decide where Claude Code should keep reusable project instructions or skills in this environment, copy the necessary Obsiwiki files there, and set up the target vault so Claude Code reads adapters/claude-code.md and follows Obsiwiki schema precedence.

After installation, tell me which files you copied and where.
```

Claude Code 应使用这个 adapter：

```text
adapters/claude-code.md
```

推荐的 vault-local 设置：复制 starter vault 文件，然后在 vault 根目录创建 `CLAUDE.md`，让 Claude Code 指向 vault-local schema：

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
```

## 为 Hermes 安装

让 Hermes 从本仓库安装 Obsiwiki。它应该读取本 README，判断 Hermes 在当前环境中保存 reusable skills、plugins 或 long-lived context 的位置，并复制必要源文件。

Agent-friendly 安装提示：

```text
Read https://github.com/TengShao/Obsiwiki and install Obsiwiki for Hermes.

Decide where Hermes should keep reusable skills, plugins, or context in this environment, copy the necessary Obsiwiki files there, and configure Hermes to load adapters/hermes.md together with the Obsiwiki references and schema.

After installation, tell me which files you copied and where.
```

Hermes 应使用这个 adapter：

```text
adapters/hermes.md
```

给 Hermes 加载这些文件作为上下文：

- `SKILL.md`
- `references/schema.md`
- `references/page-types.md`
- `references/lint-checklist.md`
- `starter-vault/System/Schema/`

典型 Hermes 提示：

```text
Use Obsiwiki to ingest this source: https://example.com/article
Use Obsiwiki to capture reusable conclusions from this conversation.
Use Obsiwiki to answer this from the vault: ...
Use Obsiwiki to lint this vault.
```

## 为 OpenClaw 安装

让 OpenClaw 从本仓库安装 Obsiwiki。它应该读取本 README，判断 OpenClaw 在当前环境中保存 slash commands、plugins 或 skills 的位置，复制必要源文件，并暴露 Obsiwiki command workflow。

Agent-friendly 安装提示：

```text
Read https://github.com/TengShao/Obsiwiki and install Obsiwiki for OpenClaw.

Decide where OpenClaw should keep reusable slash commands, plugins, or skills in this environment, copy the necessary Obsiwiki files there, and expose the /obsiwiki command workflow using adapters/openclaw.md.

After installation, tell me which files you copied and where.
```

OpenClaw 应使用这个 adapter：

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
/obsiwiki help
```

每个命令都应该进入同一套由本 skill 和 active schema 定义的 `ingest / capture / query / lint` 工作流。

## 添加 Starter Vault 骨架

如果你正在从一个新的或结构较轻的 Obsidian vault 开始，可以把 starter 文件复制到 vault 根目录：

```bash
cp -R starter-vault/* /path/to/your/obsidian-vault/
```

starter vault 会创建这样的知识架构：

```text
raw/        原始来源材料
assets/     二进制文件、截图、PDF 和可复用视觉资产
wiki/       可供 agent 查询和综合的稳定知识页面
System/     schema、workflow、page contracts 和 agent adapters
```

如果你的 vault 已经有笔记，不要先批量移动它们。让 agent 使用 lint workflow 逐步提出增量调整建议。

## Vault 目录结构

starter vault 使用四个主要层次：

```text
raw/
├── articles/        网页文章、摘录、教程和实践指南
├── papers/          论文、PDF、报告和研究材料
├── transcripts/     转录稿和长篇对话
└── meeting-notes/   会议、访谈和现场笔记

assets/
├── raw/             属于某个 raw source 的文件，按 source slug 分组
├── wiki/            长期复用的知识资产
├── projects/        项目相关资产
└── shared/          跨主题复用的资产

wiki/
├── concepts/        稳定概念、协议、框架和方法
├── entities/        工具、产品、公司、人物和命名系统
├── sources/         raw source 的一对一 digest
├── syntheses/       跨来源或跨对话的综合分析
├── maps/            主题地图 / MOC，用来保持页面连接
├── index.md         顶层导航入口
└── log.md           追加式维护日志

System/
├── Schema/          可选的 vault-local 规则和 workflow source of truth
└── Agents/          Codex、Claude Code、Hermes、OpenClaw 或其它 agent 的 adapter notes
```

目录意图：

- `raw/` 尽量保留来源材料的原始形态。
- `assets/` 避免二进制文件散落在笔记目录里。
- `wiki/` 是 agent 应该查询和更新的稳定知识层。
- `wiki/maps/` 是主要的 anti-orphan 机制。
- `System/Schema/` 存在时，是 vault-local source of truth；agent-specific 文件应该适配它，而不是 fork 它。

## Schema 优先级

默认使用已安装的 Obsiwiki `references/` 和 `starter-vault/System/Schema/` 作为 schema。

如果目标 vault 包含 `System/Schema/`，则将该 vault-local schema 视为 source of truth。已安装 schema 只用于对照、更新建议和迁移。不要静默覆盖 vault-local schema 自定义内容。

Agent adapters 只负责把同一套 schema 翻译给不同 agent 使用。它们不能 fork 目录语义、页面契约或 workflow 行为。

## 核心工作流

### Ingest

当你给 agent 一个 URL、文章、PDF、截图包、转录稿、会议记录或 raw note 时使用。

预期行为：

1. 将 raw source 保存或摘要到 `raw/`。
2. 将附件保存到 `assets/raw/<source-slug>/`。
3. 在 `wiki/sources/` 下创建或更新一对一 digest。
4. 在有用时更新已有的 `wiki/concepts/`、`wiki/entities/` 或 `wiki/syntheses/`。
5. 将新增或更新的知识挂到至少一个 `wiki/maps/` 页面。
6. 在确认修改后更新 `wiki/index.md`，并追加 `wiki/log.md`。

示例：

```text
Use Obsiwiki to ingest this source: https://example.com/article
```

### Capture

当一次对话产生了可复用结论、决策、定义、权衡或 workflow 时使用。

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

当你希望 agent 从 vault 中回答问题时使用。

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

当你想做 vault 健康检查时使用。

预期行为：

- 标记未被 maps 或 index 覆盖的 wiki 页面
- 标记没有正式链接的 source 页面
- 标记重复或近似重复主题
- 标记缺失 `last_updated`、缺失 sources 或缺失 `Related Links`
- 标记 raw attachment 放置问题
- 标记仍依赖 raw-only assets、应提升到 `assets/wiki/` 的 wiki 页面

示例：

```text
Use Obsiwiki to lint this vault.
```

## 页面类型

正式 wiki 页面使用这些 `type` 值：

- `concept`：稳定概念、协议、框架或方法
- `entity`：工具、产品、公司、人物或命名系统
- `source`：raw source 的一对一 digest
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

Tags 应描述主题，而不是文件夹或 workflow 状态。优先使用 lowercase English `kebab-case` tags。

## 给其它 Agent 的说明

这个 skill 与 agent 无关。对于不能直接加载 Codex skills 的 agent，请让它们读取：

- `SKILL.md`
- 相关 adapter：`adapters/claude-code.md`、`adapters/hermes.md` 或 `adapters/openclaw.md`
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
```

## 隐私

本仓库有意只包含可复用规则和空的 starter structure。不要发布你的个人 `raw/`、`wiki/`、`Projects/`、`Work/`、`Opinions/` 或 `Journal/` 内容，除非你已经审查过其中的私密或敏感信息。

## 更新 Skill

让目标 agent 读取本 README，找到已有 Obsiwiki 安装位置，并从最新仓库版本刷新源文件。

Agent-friendly 更新提示：

```text
Read https://github.com/TengShao/Obsiwiki and update the existing Obsiwiki installation for this agent.

Find where Obsiwiki was installed in this environment, refresh the copied or cloned source files from the latest repository version, preserve any vault-local System/Schema/ customizations, and tell me what changed.
```

确保 agent 重新加载刷新后的文件：

- `SKILL.md`
- `references/schema.md`
- `references/page-types.md`
- `references/lint-checklist.md`
- `starter-vault/System/Schema/` 或 vault-local `System/Schema/`
- 位于 `adapters/` 的相关 adapter

如果目标 vault 已经有自定义的 `System/Schema/`，请将其与最新 schema 协调，而不是静默替换用户编辑过的文件。

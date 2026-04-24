# Obsiwiki

`Obsiwiki` is a Codex skill for maintaining an Obsidian vault as an agent-friendly LLM Wiki.

It gives agents a shared operating model for:

- ingesting links, articles, papers, transcripts, meeting notes, and raw notes
- preserving source material in `raw/`
- compiling durable knowledge pages in `wiki/`
- capturing reusable conclusions from conversations
- answering questions from existing wiki pages
- linting the vault for orphan pages, missing sources, duplicate topics, stale pages, and asset placement issues

The package includes a reusable `SKILL.md`, reference rules, and a starter vault skeleton. It does not include personal notes or private knowledge content.

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

## Install For Codex

Clone or copy this repository into your Codex skills directory:

```bash
mkdir -p ~/.codex/skills
git clone <repo-url> ~/.codex/skills/obsiwiki
```

Restart Codex after installing the skill.

Use it explicitly in a prompt:

```text
用 $obsiwiki 处理这个链接：https://example.com/article
用 $obsiwiki 把刚才这段讨论沉淀进知识库
用 $obsiwiki 查询 A2A 和 MCP 的关系
用 $obsiwiki lint 我的 vault
```

## Use With Claude Code

For Claude Code, use the adapter:

```text
adapters/claude-code.md
```

Recommended setup: copy the starter vault files, then create a `CLAUDE.md` file at the vault root that points Claude Code to the local schema:

```text
System/Schema/vault-schema.md
System/Schema/workflows.md
System/Schema/page-contracts.md
System/Agents/claude-code.md
```

Typical Claude Code prompts:

```text
Use Obsiwiki to ingest this article into the vault: https://example.com/article
Use Obsiwiki to capture the reusable conclusions from this conversation.
Use Obsiwiki to answer this from the vault: ...
Use Obsiwiki to lint this vault.
```

## Use With Hermes

For Hermes or another agent that does not load Codex skills directly, use the adapter:

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
Use the Obsiwiki workflow and ingest this article: https://example.com/article
Use the Obsiwiki workflow and capture the reusable conclusions from this conversation.
Use the Obsiwiki workflow and answer this from the vault: ...
Use the Obsiwiki workflow and lint this vault.
```

## Use With OpenClaw

For OpenClaw, use the adapter:

```text
adapters/openclaw.md
```

Recommended slash-command shape:

```text
/wiki ingest <url-or-source>
/wiki capture
/wiki capture [[target page]]
/wiki capture synthesis
/wiki query <question>
/wiki lint
/wiki help
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
├── Schema/          source of truth for vault rules and workflows
└── Agents/          adapter notes for Codex, Claude Code, Hermes, OpenClaw, or other agents
```

Folder intent:

- `raw/` keeps source material close to its original form.
- `assets/` keeps binary files out of note folders.
- `wiki/` is the durable knowledge layer agents should query and update.
- `wiki/maps/` is the main anti-orphan mechanism.
- `System/Schema/` is the source of truth; agent-specific files should adapt it, not fork it.

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
用 $obsiwiki ingest 这篇文章：https://example.com/article
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
用 $obsiwiki capture 刚才关于团队 AI 工作流的结论
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
用 $obsiwiki query：我的知识库里对 prompt engineering 和 UXD 的关系有什么判断？
```

### Lint

Use when you want a vault health check.

Expected behavior:

- flag wiki pages not covered by maps or index
- flag source pages without formal links
- flag duplicate or near-duplicate topics
- flag missing `last_updated`, missing sources, or missing `关联连接`
- flag raw attachment placement problems
- flag wiki pages that should promote raw-only assets into `assets/wiki/`

Example:

```text
用 $obsiwiki lint 我的 vault
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

Then use commands such as:

```text
/wiki ingest <url>
/wiki capture
/wiki query <question>
/wiki lint
```

## Privacy

This repository intentionally contains only reusable rules and empty starter structure. Do not publish your personal `raw/`, `wiki/`, `Projects/`, `Work/`, `Opinions/`, or `Journal/` content unless you have reviewed it for private or sensitive information.

# Obsiwiki

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

Ask Claude Code to install Obsiwiki from this repository. It should read this README, choose the correct Claude-accessible instruction, skill, or project context location, copy the necessary source files there, and point the target vault at the local schema.

Agent-friendly install prompt:

```text
Read https://github.com/TengShao/Obsiwiki and install Obsiwiki for Claude Code.

Decide where Claude Code should keep reusable project instructions or skills in this environment, copy the necessary Obsiwiki files there, and set up the target vault so Claude Code reads the local schema and adapters/claude-code.md.

After installation, tell me which files you copied and where.
```

Claude Code should use the adapter:

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

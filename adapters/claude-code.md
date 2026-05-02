# Claude Code Adapter

Use this adapter when Claude Code should maintain an Obsidian vault with Obsiwiki.

Claude Code works best when the vault contains a local `CLAUDE.md` or an equivalent instruction file that points it to the Obsiwiki rules and schema precedence.

## Context To Load

Point Claude Code at these files:

- `SKILL.md`
- `references/schema.md`
- `references/page-types.md`
- `references/lint-checklist.md`
- `starter-vault/System/Schema/vault-schema.md`
- `starter-vault/System/Schema/workflows.md`
- `starter-vault/System/Schema/page-contracts.md`
- `starter-vault/System/Schema/purpose.md`
- `starter-vault/wiki/overview.md`
- `starter-vault/wiki/review.md`

Use the installed Obsiwiki schema as the default. If the target vault contains `System/Schema/`, prefer that vault-local schema over the installed starter files.

Do not install `adapters/` into the local Claude Code instruction directory. Read adapters from the repository as reference notes.

## Update Behavior

When updating the installed Obsiwiki skill, refresh the skill files first. Then, if the target vault has `System/Schema/`, compare the latest default schema, value guidance, and support pages with the vault-local files.

- Do not overwrite vault-local schema during a skill update.
- Report schema/workflow differences separately from value-guidance additions such as `System/Schema/purpose.md` and support-page additions such as `wiki/overview.md` and `wiki/review.md`.
- If `System/Schema/purpose.md` is missing, ask whether to initialize one from the starter template or draft one for this vault. Do not create it silently.
- If `wiki/overview.md` or `wiki/review.md` is missing, scan `wiki/index.md`, `wiki/maps/`, `wiki/log.md`, and recent formal page updates, then ask whether to initialize those support pages before depending on them. Draft `wiki/overview.md` as a current-state summary and `wiki/review.md` as a backlog with any duplicate-topic, missing-source, stale-synthesis, unclear-value, or graph-health issues found during the scan.
- If new workflow behavior exists only in the installed defaults, such as two-stage `ingest`, scheduled maintenance, or graph-health lint, ask the user whether to merge it into the vault-local schema before using it as active vault behavior.

## Recommended Vault Instruction

Create a `CLAUDE.md` file at the vault root with this content:

```markdown
# Claude Code Instructions

Use Obsiwiki to maintain this Obsidian vault as an agent-friendly LLM Wiki.

Read these files before modifying the vault:

- System/Schema/vault-schema.md
- System/Schema/workflows.md
- System/Schema/page-contracts.md
- System/Schema/purpose.md

If this vault has System/Agents/claude-code.md, read it as optional vault-local adapter notes.

Core rules:

- Keep original source material in raw/.
- Keep binary files and media in assets/.
- Keep durable knowledge pages in wiki/.
- Use `System/Schema/purpose.md` for agent value judgment when present.
- Treat System/Schema/ as the vault-local source of truth when present.
- Use wiki/maps/ as the main anti-orphan mechanism.
- Use wiki/overview.md as the compressed knowledge base state.
- Use wiki/review.md for uncertain issues that need human judgment or later follow-up.
- Prefer updating existing wiki pages over creating duplicates.
- Do not save full chat transcripts by default.

Supported workflows:

- ingest: analyze source first, then propose raw/source/page/map/index/log changes
- capture: extract reusable conclusions, ask before writing, update target pages
- query: answer from wiki/index.md, maps, concepts, entities, sources, syntheses
- lint: find orphan pages, missing sources, duplicates, stale pages, asset issues, and graph health issues
- review: summarize recent additions, weekly changes, topic clusters, open review items, overview drift, and next actions without writing
- scheduled maintenance: guide creation of a recurring review/lint job; default to Monday 09:00 unless the user chooses another schedule
```

## Recommended Prompts

```text
/obsiwiki ingest this source: https://example.com/article
/obsiwiki capture reusable conclusions from this conversation.
/obsiwiki answer this from the vault: ...
/obsiwiki lint this vault.
/obsiwiki review recent additions to this vault.
/obsiwiki generate this week's knowledge base report.
/obsiwiki set up scheduled maintenance for this vault.
```

## Operating Rules

- Use two-stage draft-first behavior for `ingest`: source analysis first, proposed wiki changes second.
- Use suggest-and-confirm behavior for `capture`.
- Start `query` from `wiki/index.md` and relevant `wiki/maps/`.
- Keep `review` and weekly reports read-only by default; use the last 7 days unless the user specifies a range, and use `this week` for weekly reports.
- Consult `System/Schema/purpose.md` before promoting material into durable wiki content when present.
- Use `wiki/review.md` for uncertain value, duplicate, source, stale synthesis, or graph health decisions.
- For scheduled maintenance, ask whether the user wants recurring review and lint, let them choose the cadence, default to Monday 09:00 in their locale, and keep the job read-only unless they confirm writes.
- Use `wiki/maps/` as the main anti-orphan mechanism.
- Update `wiki/log.md` for meaningful ingest, capture, lint, and synthesis changes.
- Keep `System/Schema/` as the vault-local source of truth when present.
- Do not create Claude-specific rules that conflict with the active Obsiwiki schema.

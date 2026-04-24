# Claude Code Adapter

Use this adapter when Claude Code should maintain an Obsidian vault with Obsiwiki.

Claude Code works best when the vault contains a local `CLAUDE.md` or an equivalent instruction file that points it to the Obsiwiki rules and schema.

## Context To Load

Point Claude Code at these files:

- `SKILL.md`
- `references/schema.md`
- `references/page-types.md`
- `references/lint-checklist.md`
- `starter-vault/System/Schema/vault-schema.md`
- `starter-vault/System/Schema/workflows.md`
- `starter-vault/System/Schema/page-contracts.md`

If the target vault already contains `System/Schema/`, prefer the vault-local schema over the starter files.

## Recommended Vault Instruction

Create a `CLAUDE.md` file at the vault root with this content:

```markdown
# Claude Code Instructions

Use Obsiwiki to maintain this Obsidian vault as an agent-friendly LLM Wiki.

Read these files before modifying the vault:

- System/Schema/vault-schema.md
- System/Schema/workflows.md
- System/Schema/page-contracts.md
- System/Agents/claude-code.md

Core rules:

- Keep original source material in raw/.
- Keep binary files and media in assets/.
- Keep durable knowledge pages in wiki/.
- Treat System/Schema/ as the source of truth.
- Use wiki/maps/ as the main anti-orphan mechanism.
- Prefer updating existing wiki pages over creating duplicates.
- Do not save full chat transcripts by default.

Supported workflows:

- ingest: save source material, create source digests, update maps/index/log
- capture: extract reusable conclusions, ask before writing, update target pages
- query: answer from wiki/index.md, maps, concepts, entities, sources, syntheses
- lint: find orphan pages, missing sources, duplicates, stale pages, and asset issues
```

## Recommended Prompts

```text
Use Obsiwiki to ingest this source: https://example.com/article
Use Obsiwiki to capture reusable conclusions from this conversation.
Use Obsiwiki to answer this from the vault: ...
Use Obsiwiki to lint this vault.
```

## Operating Rules

- Use draft-first behavior for `ingest`.
- Use suggest-and-confirm behavior for `capture`.
- Start `query` from `wiki/index.md` and relevant `wiki/maps/`.
- Use `wiki/maps/` as the main anti-orphan mechanism.
- Update `wiki/log.md` for meaningful ingest, capture, lint, and synthesis changes.
- Keep `System/Schema/` as the source of truth.
- Do not create Claude-specific rules that conflict with `System/Schema/`.

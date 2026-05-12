---
title: Vault Schema
type: schema
tags: []
last_updated: 2026-05-07
aliases: []
---

# Vault Schema

This vault is maintained in five main areas, with value guidance stored inside the vault-local schema:

1. `raw/`: original source material.
2. `assets/`: binary files, screenshots, PDFs, and reusable visuals.
3. `wiki/`: durable knowledge pages for agent query, update, and synthesis.
4. `exports/`: disposable generated review/report artifacts.
5. `System/`: vault marker, rules, workflows, page contracts, and vault-level value guidance.

## Vault Identity

- `System/obsiwiki.toml` marks the root of an Obsiwiki-managed vault.
- Agents can identify the vault by walking upward from their current working directory until they find `System/obsiwiki.toml`.
- User-level registries such as `~/.config/obsiwiki/vaults.toml` may store absolute paths to confirmed vaults.
- `System/obsiwiki.toml` should stay portable and use relative paths only.

## Directory Semantics

- `raw/articles/`: web articles, excerpts, tutorials, and practical guides.
- `raw/papers/`: papers, PDFs, reports, and research material.
- `raw/transcripts/`: transcripts and long-form conversations.
- `raw/meeting-notes/`: meetings, interviews, and field notes.
- `raw/archive/`: ingested raw material that should stay available but no longer changes.
- `assets/raw/<source-slug>/`: files belonging to one raw source.
- `assets/wiki/<topic-slug>/`: reusable long-lived knowledge assets.
- `assets/projects/<project-name>/`: project-specific assets.
- `assets/shared/`: assets reused across topics.
- `exports/reviews/`: generated HTML dashboards, weekly reports, and other disposable review artifacts.
- `System/obsiwiki.toml`: vault identity marker and relative path map.
- `System/Schema/purpose.md`: value criteria and active themes that guide whether material should be promoted into durable wiki content.
- `wiki/overview.md`: compressed state of the current knowledge base.
- `wiki/review.md`: structured backlog for uncertain issues, human decisions, and later agent follow-up.
- `wiki/concepts/`: concepts, protocols, frameworks, and methods.
- `wiki/entities/`: tools, products, companies, people, and named systems.
- `wiki/sources/`: one-to-one digests of raw sources.
- `wiki/syntheses/`: integrated analysis across sources or conversations.
- `wiki/maps/`: topic maps / MOCs that keep pages connected.

## Read And Write Boundaries

- `raw/` may stay lightweight and source-like.
- `raw/` should not store binary attachments.
- `assets/` is the shared home for files and media.
- During ingest, source-relevant images, figures, diagrams, screenshots, PDFs, and other attachments should be saved under `assets/raw/<source-slug>/` and embedded or linked from the raw/source note as local vault assets.
- Captions, alt text, and source URLs should be preserved when available; skipped or unavailable assets should be recorded with a short reason.
- `wiki/` is the default durable knowledge layer.
- `exports/` is not a source of truth. Agents may write generated HTML dashboards there when the user asks for an export/dashboard/report file or chooses HTML output before suitable review or lint-report work.
- `System/obsiwiki.toml` identifies the vault root and relative path layout.
- `System/Schema/purpose.md` should guide value assessment during `ingest`, `capture`, and `review`.
- `System/Schema/` is the source of truth for workflow rules.
- `System/Agents/` adapts those rules for specific agents and must not fork the schema.

## Formal Wiki Frontmatter

Formal wiki pages should include:

```yaml
---
title:
type:
tags: []
sources: []
last_updated:
aliases: []
---
```

Allowed `type` values:

- `concept`
- `entity`
- `source`
- `synthesis`
- `map`

Special support pages may use:

- `purpose`
- `overview`
- `review`
- `log`

Tags should describe the content topic. Prefer lowercase English `kebab-case` tags and avoid using tags for workflow state.

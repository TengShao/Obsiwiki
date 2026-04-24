# OpenClaw Adapter

Use this adapter when OpenClaw should expose the Obsidian LLM Wiki as a slash-command workflow.

## Entry Point

Recommended top-level command:

```text
/wiki
```

Suggested subcommands:

```text
/wiki ingest <url-or-source>
/wiki capture
/wiki capture [[target page]]
/wiki capture synthesis
/wiki query <question>
/wiki lint
/wiki help
```

## Command Semantics

### `/wiki ingest <url-or-source>`

Use when the user gives a URL, article, paper, transcript, meeting note, or raw note.

Expected behavior:

- preserve source material in `raw/`
- store attachments in `assets/raw/<source-slug>/`
- create or update `wiki/sources/<source-slug>.md`
- update related `concept`, `entity`, or `synthesis` pages where useful
- attach new or updated pages to at least one `wiki/maps/` page
- update `wiki/index.md` and `wiki/log.md`

### `/wiki capture`

Use when a conversation produced reusable knowledge.

Expected behavior:

- extract conclusions, decisions, definitions, tradeoffs, and workflows
- avoid saving full chat transcripts by default
- suggest the best target page
- write only after confirmation unless the user requested automatic execution

### `/wiki query <question>`

Use when the user asks a question that should be answered from the vault.

Expected behavior:

- start with `wiki/index.md`
- follow relevant `wiki/maps/`
- read only the necessary formal pages
- suggest a `synthesis` update if the answer is broadly reusable

### `/wiki lint`

Use for vault health checks.

Expected behavior:

- flag orphan pages
- flag formal pages not covered by maps or index
- flag source pages without formal links
- flag duplicate or near-duplicate topics
- flag missing `last_updated`, missing sources, or missing `关联连接`
- flag attachment placement issues

## Rules

- Commands route into the shared `ingest / capture / query / lint` workflows.
- Directory semantics and page contracts come from `System/Schema/`.
- Do not maintain private OpenClaw rules that conflict with the schema.
- Prefer updating existing pages over creating duplicates.

# Schema

Use these write targets:

- raw text sources: `raw/`
- binary attachments: `assets/`
- durable knowledge: `wiki/`
- value judgment guidance: `System/Schema/purpose.md`
- current knowledge state: `wiki/overview.md`
- uncertain issues and human decisions: `wiki/review.md`
- rules and contracts: `System/Schema/`

Key rules:

- Do not write binary files into `raw/`.
- During ingest, source-relevant images, figures, diagrams, screenshots, PDFs, and other attachments should be stored under `assets/raw/<source-slug>/` and embedded or linked from the raw/source note as local vault assets.
- Preserve captions, alt text, and source URLs for ingested assets when available; record skipped or unavailable assets with a short reason.
- Do not treat `System/Agents/` as the source of truth.
- Consult `System/Schema/purpose.md` before promoting material into durable wiki content.
- Use `wiki/maps/` to absorb pages into the graph before over-optimizing body links.
- Use `wiki/review.md` when an issue needs human judgment instead of forced automatic cleanup.

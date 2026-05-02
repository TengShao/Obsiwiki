# Schema

Use these write targets:

- raw text sources: `raw/`
- binary attachments: `assets/`
- durable knowledge: `wiki/`
- value judgment guidance: `purpose.md`
- current knowledge state: `wiki/overview.md`
- uncertain issues and human decisions: `wiki/review.md`
- rules and contracts: `System/Schema/`

Key rules:

- Do not write binary files into `raw/`.
- Do not treat `System/Agents/` as the source of truth.
- Consult `purpose.md` before promoting material into durable wiki content.
- Use `wiki/maps/` to absorb pages into the graph before over-optimizing body links.
- Use `wiki/review.md` when an issue needs human judgment instead of forced automatic cleanup.

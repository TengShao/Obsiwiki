# Lint Checklist

Check these first:

- Is the file under `System/`? If yes, exclude it from ordinary content lint.
- For `System/`, only check configuration health: key schema files are readable, agent adapters follow `System/Schema/`, and vault-local schema customizations are not silently overwritten.
- Is the page completely isolated?
- Is it at least attached to one map or the index?
- Does it have a clear type and `last_updated`?
- Does it cite or link its upstream source?
- Does a `source` page point to at least one concept, entity, or map?
- Is a `synthesis` page linked from a map and multiple formal pages?
- Are raw attachments stored in `assets/raw/`?
- Should any raw-only asset be promoted to `assets/wiki/`?
- Does `System/Schema/purpose.md` exist when the active schema expects value judgment guidance?
- Is `wiki/overview.md` stale relative to recent review or major knowledge base changes?
- Are unresolved duplicate, missing-source, stale-synthesis, or unclear-value questions captured in `wiki/review.md`?

Graph health checks:

- Cluster without map: several related pages exist but no coherent map covers them.
- Source cluster without concept: multiple source pages point to the same durable idea but no concept page exists.
- Concept without sources: a concept contains durable claims but lacks an upstream source or explicit user judgment.
- Stale synthesis: a synthesis depends on recently updated pages but has not been reviewed.
- Overloaded map: a map is mostly an undifferentiated link dump without grouping, descriptions, or organizing judgment.
- Duplicate cluster: titles, aliases, sources, or related links suggest overlapping pages.
- Bridge candidate: two maps share enough pages or themes that a synthesis or cross-link may be useful.

When a graph health issue requires interpretation, record it as a `wiki/review.md` item instead of treating it as a deterministic lint failure.

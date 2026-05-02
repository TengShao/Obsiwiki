---
title: Purpose
type: purpose
tags: []
last_updated:
aliases: []
---

# Purpose

This file tells agents how to judge what is worth preserving in this vault.

## Vault Mission

Describe why this vault exists and what long-lived knowledge it should preserve.

Example:

```text
This vault preserves reusable concepts, sources, decisions, patterns, and syntheses that can support future thinking, writing, research, design, and agent-assisted work.
```

## Active Themes

List the major themes currently worth recognizing. A personal vault can be multi-topic; do not force all material into one theme.

- Theme 1
- Theme 2
- Theme 3

## Value Criteria

Agents should prefer preserving content that satisfies one or more of these criteria:

- Reusable: likely to support future query, writing, planning, or decisions.
- Connected: can link to an existing concept, entity, source, synthesis, map, project, or opinion.
- Judgment-bearing: contains definitions, tradeoffs, patterns, claims, decisions, examples, or counterexamples.
- Sourceable: important claims can be traced to a source, conversation context, or explicit user judgment.
- Durable: likely to remain useful beyond the current task or moment.

## Low-Value Content

Agents should not promote these into formal wiki pages by default:

- full chat transcripts
- one-off task chatter
- unprocessed fragments with no reusable conclusion
- temporary status updates
- unsupported claims that cannot yet be tied to a source or user judgment

## Agent Decision Rule

Before writing durable wiki content, state why the material is worth preserving.

Use this short form in ingest, capture, and review drafts:

```text
Value assessment: <why this is worth preserving, how it connects, and what future use it supports>
```

If the value is unclear, create or suggest a review item instead of silently promoting the material into `wiki/`.

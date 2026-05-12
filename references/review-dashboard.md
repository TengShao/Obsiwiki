# Review Dashboard Webpage Template

`templates/review-dashboard.html` is an agent-side webpage generation template.

Use it when a user asks for an HTML dashboard, interactive report, visual review, exported report file, or confirms a proposed dashboard artifact during `review`, weekly report, or `lint` follow-up work.

## Output Path

Write generated webpages under:

```text
exports/reviews/
```

Recommended filenames:

- `YYYY-MM-DD-review-dashboard.html`
- `YYYY-MM-DD-weekly-review.html`
- `YYYY-MM-DD-lint-review.html`

Do not write generated HTML into `wiki/`. Markdown remains the durable source of truth.

## Generation Contract

1. Copy `templates/review-dashboard.html`.
2. Replace only the JSON inside:

```html
<script id="review-data" type="application/json">
```

3. Keep the generated webpage self-contained.
4. Report the generated file path to the user.

## Payload Shape

Fields may be plain strings or localized objects such as `{ "en": "...", "zh": "..." }`.

`reviewItems` render in the top-level Action Required module. Use them for items that need attention or follow-up, including items that require a user decision.

Long `pageChanges.new` and `pageChanges.updated` lists are progressively disclosed by the template: each group shows 3 items first, then reveals 3 more items per click.

```json
{
  "title": { "en": "Review Dashboard Sample", "zh": "Review Dashboard 示例" },
  "summary": { "en": "Brief summary", "zh": "简短摘要" },
  "range": { "en": "sample review window", "zh": "示例回顾周期" },
  "generatedAt": "2026-01-01 09:00",
  "vaultName": "Demo Vault",
  "metrics": {
    "newPages": 0,
    "updatedPages": 0
  },
  "pageChanges": {
    "new": [
      {
        "title": { "en": "New page title", "zh": "新增页面标题" },
        "detail": { "en": "Why this page was created", "zh": "创建这个页面的原因" },
        "path": "wiki/concepts/example-topic.md"
      }
    ],
    "updated": [
      {
        "title": { "en": "Updated page title", "zh": "更新页面标题" },
        "detail": { "en": "What changed in this page", "zh": "这个页面发生了什么变化" },
        "path": "wiki/overview.md"
      }
    ]
  },
  "clusters": [
    { "name": { "en": "Topic", "zh": "主题" }, "count": 1 }
  ],
  "overviewDrift": [
    {
      "title": { "en": "Drift title", "zh": "漂移标题" },
      "detail": { "en": "What changed", "zh": "变化说明" },
      "links": ["wiki/overview.md"]
    }
  ],
  "reviewItems": [
    {
      "id": "review-001",
      "title": { "en": "Issue title", "zh": "问题标题" },
      "type": "synthesis-candidate",
      "status": "open",
      "severity": "medium",
      "requiresUserDecision": true,
      "relatedPages": ["wiki/review.md"],
      "evidence": [
        { "en": "Evidence", "zh": "证据" }
      ],
      "suggestedAction": {
        "en": "Suggested action",
        "zh": "建议动作"
      }
    }
  ],
  "notableUpdates": [
    {
      "title": { "en": "Update title", "zh": "更新标题" },
      "detail": { "en": "Update detail", "zh": "更新说明" },
      "links": ["wiki/log.md"]
    }
  ],
  "nextActions": [
    {
      "title": { "en": "Next action", "zh": "下一步" },
      "detail": { "en": "Action detail", "zh": "动作说明" }
    }
  ]
}
```

Allowed `reviewItems.type` values should follow `wiki/review.md` when possible:

- `duplicate-topic`
- `missing-source`
- `synthesis-candidate`
- `stale-synthesis`
- `unclear-value`
- `graph-health`
- `other`

Allowed `severity` values:

- `high`
- `medium`
- `low`
- `normal`

# Security and row-level security

**Principle: security lives in the data layer, never in the prompt.**

```yaml
# metrics view (excerpt)
security:
  access: true
  row_filter: "restaurant_id IN ({{ .user.restaurant_ids }})"
```

| Layer | Responsibility |
|---|---|
| Identity | each user signs in to the MCP server with their own account (OAuth) |
| Semantic layer | applies `row_filter` to **every** query, in every tool |
| MCP server | exposes only metrics views (aggregates), never raw tables or SQL |
| Skill | reminds the agent of scope; never decides access |
| AI assistant | sees only what the query returned |

## What an AI cannot do here
- Read another client's rows: the filter is applied server-side, before results exist.
- Run arbitrary SQL: the MCP tools accept measures, dimensions and filters only.
- Leak raw personal data: views expose aggregates, not customer records.

## Operational rules
- Least privilege: brand owners see their brand; branch managers see their branch.
- Offboarding: disable the user and the connector stops returning data immediately.
- Audit: keep query logs (view, measures, filters, user), not result data.

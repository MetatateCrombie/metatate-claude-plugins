---
description: Find governed Metatate assets by database, schema, domain, sensitivity, PII status, or compliance tag.
---

# Discover Governed Context

Use the `metatate` MCP tool `discover-context`.

Map the user's request to the scalar managed MCP arguments:

- `database_name`
- `schema_name`
- `min_sensitivity`
- `compliance_framework`
- `has_pii`
- `domain`

Return candidate assets, sensitivity, owner/domain, PII status, policy count,
and the best next Metatate inspection step.

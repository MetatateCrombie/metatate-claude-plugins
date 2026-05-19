---
description: Validate SQL or query context against active Metatate governance before execution or release.
---

# Validate Query Context

Use the `metatate` MCP tool `validate-query-context`.

Pass query text as `sql_text`. Include `operation`, `intended_use`,
`actor_role`, `destination_system`, `destination_jurisdiction`, or
`consumer_jurisdiction` when the user's request provides them.

Summarize compliance status, governed assets, policy findings, required changes,
and `validation_id`.

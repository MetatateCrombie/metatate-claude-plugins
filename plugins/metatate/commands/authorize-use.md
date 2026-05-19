---
description: Ask Metatate whether a proposed data use is allowed, denied, conditional, or unknown.
---

# Authorize Data Use

Use the `metatate` MCP tool `authorize-use`.

Collect or infer:

- `table_name`
- `operation`
- `intended_use`
- optional `actor_role`
- optional `columns_csv`
- optional `destination_system`
- optional `destination_jurisdiction`
- optional `consumer_jurisdiction`

If adding audit context, pass it as a JSON object string in `context_json`.

Return the decision, rationale, conditions, prohibitions, obligations, next
actions, and `decision_id`.

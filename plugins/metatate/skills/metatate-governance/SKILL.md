---
name: metatate-governance
description: Use when a user asks about Metatate governed data context, data meaning, policy rules, data-use authorization, query validation, decision explanation, or release readiness.
---

# Metatate Governance

Use this skill when a user asks about governed data context, data meaning,
policy rules, data-use authorization, query validation, decision explanation,
or release readiness.

## MCP Tools

Use the Snowflake-managed MCP server named `metatate` when available.

Tool contract:

- `discover-context`: optional `database_name`, `schema_name`,
  `min_sensitivity`, `compliance_framework`, `has_pii`, `domain`.
- `get-decision-context`: required `table_name`.
- `inspect-data-meaning`: required `table_name`, optional `column_name`.
- `inspect-governance-rules`: required `table_name`, optional `column_name`.
- `authorize-use`: required `table_name`, `operation`, `intended_use`;
  optional `actor_role`, `columns_csv`, `destination_system`,
  `destination_jurisdiction`, `consumer_jurisdiction`, `context_json`,
  `raw_request_text`, `normalized_request_json`, `normalization_meta_json`.
- `validate-query-context`: required `sql_text`; optional `operation`,
  `intended_use`, `actor_role`, `destination_system`,
  `destination_jurisdiction`, `consumer_jurisdiction`.
- `explain-why`: one of `decision_id` or `validation_id`.

Use JSON object strings for `context_json`, `normalized_request_json`, and
`normalization_meta_json`. Use comma-separated uppercase column names for
`columns_csv`.

## Behavior

- Treat Metatate as the source of truth for policy, catalog, authorization,
  validation, and explanation.
- Do not infer authorization from policy text alone when `authorize-use` or
  `validate-query-context` can answer.
- Keep local repository analysis separate from Metatate facts. Use local files
  to identify candidate tables, SQL, dbt models, migrations, and code paths,
  then call Metatate tools for governed context.
- Do not request or expose raw row-level data by default.
- Include decision IDs and validation IDs in user-facing summaries when they
  are returned.

---
description: Review governed policy coverage and likely gaps for a data asset, query, workflow, or intended use.
---

# Metatate Policy Review

Use local repository context to identify candidate data assets or SQL when
needed, then use Metatate MCP tools for governed facts.

Preferred sequence:

1. `discover-context` for unclear assets.
2. `get-decision-context` for each likely governed table.
3. `inspect-data-meaning` and `inspect-governance-rules` for sensitive assets.
4. `authorize-use` or `validate-query-context` when the user asks if work can
   proceed.

Return existing coverage, gaps, questions for governance, and recommended next
review steps. Do not create or deploy policy.

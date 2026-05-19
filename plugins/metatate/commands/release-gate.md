---
description: Run an advisory Metatate governance review for a repository change before merge or release.
---

# Metatate Release Gate

Review local diffs, SQL, dbt models, migrations, notebooks, or application code
paths to identify governed data references. Use Metatate MCP tools for policy
facts and decisions.

Preferred sequence:

1. Identify changed files and candidate SQL/data assets.
2. Use `discover-context` and `get-decision-context` for assets.
3. Use `validate-query-context` for SQL queries or generated query paths.
4. Use `authorize-use` for explicit intended-use questions.
5. Use `explain-why` when a decision or validation needs evidence.

Return pass, warn, or block as an advisory result with evidence IDs and concrete
fix recommendations. Do not mutate policy or deploy changes.

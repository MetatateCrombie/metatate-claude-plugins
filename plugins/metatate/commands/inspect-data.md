---
description: Explain governed data meaning, classification, sensitivity, PII, and masking facts for a table or column.
---

# Inspect Governed Data

Use the `metatate` MCP tool `inspect-data-meaning`.

Require a fully qualified `table_name` when it is not already clear. If the user
mentions one column, pass it as `column_name`; otherwise omit `column_name` to
inspect the table.

Summarize column meaning, sensitivity, PII flags, masking facts, data type
labels, and source policy references.

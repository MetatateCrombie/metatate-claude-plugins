# Example Prompts

Use these examples after the plugin is installed and the `metatate` MCP server
is connected.

## Discover Governed Context

```text
/metatate:discover-context
Find governed customer data with PII in the analytics domain.
```

```text
/metatate:discover-context
Show sensitive assets related to GDPR in database ANALYTICS.
```

## Inspect Data Meaning

```text
/metatate:inspect-data
Explain the governed meaning of METATATE_TEST_DB.PUBLIC.CUSTOMERS.
```

```text
/metatate:inspect-data
What does column EMAIL mean in METATATE_TEST_DB.PUBLIC.CUSTOMERS, and is it PII?
```

## Inspect Governance Rules

```text
/metatate:inspect-rules
Which rules apply to METATATE_TEST_DB.PUBLIC.CUSTOMERS for analytics use?
```

## Authorize Data Use

```text
/metatate:authorize-use
Can role ANALYST read METATATE_TEST_DB.PUBLIC.CUSTOMERS for customer churn analysis?
```

```text
/metatate:authorize-use
Can role DATA_SCIENTIST export METATATE_TEST_DB.PUBLIC.CUSTOMERS to a US-based ML platform?
```

## Validate Query Context

```text
/metatate:validate-query
Validate this SQL for analytics use by role ANALYST:

select email, last_purchase_at
from METATATE_TEST_DB.PUBLIC.CUSTOMERS
where country = 'US';
```

## Explain A Decision

```text
/metatate:explain-decision
Explain decision DECISION_123 and tell me what policy evidence mattered.
```

## Review A Local Change

```text
/metatate:policy-review
Review the SQL changes in this branch and identify any governed data risks.
```

```text
/metatate:release-gate
Run an advisory Metatate release gate for this PR.
```

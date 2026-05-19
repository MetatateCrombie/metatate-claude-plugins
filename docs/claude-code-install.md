# Claude Code Install Guide

This guide is for users after a Snowflake administrator has created the
Metatate OAuth integration.

## Values You Need

Ask your administrator for:

- Snowflake account URL.
- OAuth client ID.
- OAuth client secret.
- Snowflake role to use with Metatate.
- App database, schema, and MCP server name if they differ from
  `METATATE_APP.CORE.METATATE_MCP`.

## 1. Install The Plugin

Open Claude Code and add the marketplace:

```text
/plugin marketplace add MetatateCrombie/metatate-claude-plugins
```

Install the plugin:

```text
/plugin install metatate@metatate-claude-plugins
```

Restart Claude Code if prompted.

## 2. Register The MCP Server

Run this in your terminal:

```bash
claude mcp add-json --scope user --client-secret metatate '{
  "type": "http",
  "url": "https://<account-url>/api/v2/databases/METATATE_APP/schemas/CORE/mcp-servers/METATATE_MCP",
  "oauth": {
    "clientId": "<snowflake-oauth-client-id>",
    "callbackPort": 8080,
    "scopes": "session:role:<snowflake-role>"
  }
}'
```

When prompted, paste the OAuth client secret.

If your administrator gave you a different app database, schema, or MCP server
name, update the URL path.

## 3. Authenticate

Open Claude Code:

```bash
claude
```

Open the MCP menu:

```text
/mcp
```

Select `metatate` and authenticate with Snowflake.

## 4. Confirm It Works

Run:

```text
/metatate:discover-context
```

Then ask for a governed context your Metatate deployment knows about, for
example:

```text
Find governed customer data with PII in the analytics domain.
```

You can also test query validation:

```text
/metatate:validate-query
```

Example:

```text
Validate this SQL for analytics use by role ANALYST:
select * from METATATE_TEST_DB.PUBLIC.CUSTOMERS;
```

## Updating

Update the marketplace metadata:

```text
/plugin marketplace update metatate-claude-plugins
```

Then update the plugin from Claude Code's plugin UI, or reinstall it if your
Claude Code version does not expose update actions in the UI.

## Removing

Remove the MCP server:

```bash
claude mcp remove metatate
```

Then uninstall the plugin:

```text
/plugin uninstall metatate@metatate-claude-plugins
```

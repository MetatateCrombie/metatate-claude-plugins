# Metatate for Claude Code

This Claude Code plugin adds Metatate governance workflows that use the
Snowflake-managed MCP server registered by the Metatate Native App.

## What It Does

The plugin adds Claude Code commands for:

- Discovering governed data context.
- Inspecting data meaning and active governance rules.
- Asking Metatate whether a proposed data use is allowed.
- Validating SQL or query context before execution or release.
- Explaining authorization and validation decisions.
- Running advisory policy reviews and release gates.

## Install

Install from the public marketplace:

```text
/plugin marketplace add MetatateCrombie/metatate-claude-plugins
/plugin install metatate@metatate-claude-plugins
```

## Register The MCP Server

The plugin supplies commands and skills. The Snowflake-managed MCP connection
must be registered separately in Claude Code.

Use the values provided by your Snowflake administrator:

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

Claude Code prompts for the OAuth client secret. Do not place the client secret
directly in the command.

The `session:role:<snowflake-role>` scope is required. It prevents Snowflake
from falling back to the user's default role or secondary role `ALL`.

If you cloned the marketplace repository, you can generate and run the command
with:

```bash
./plugins/metatate/bin/metatate-mcp-add \
  --account-url https://<account-url> \
  --client-id <snowflake-oauth-client-id> \
  --snowflake-role <snowflake-role> \
  --config-scope user \
  --run
```

## Commands

- `/metatate:discover-context`
- `/metatate:inspect-data`
- `/metatate:inspect-rules`
- `/metatate:authorize-use`
- `/metatate:validate-query`
- `/metatate:explain-decision`
- `/metatate:policy-review`
- `/metatate:release-gate`

The plugin expects the MCP server to expose the managed Metatate tools:

- `discover-context`
- `get-decision-context`
- `inspect-data-meaning`
- `inspect-governance-rules`
- `authorize-use`
- `validate-query-context`
- `explain-why`

For full setup instructions, see the marketplace README and docs:

- `docs/snowflake-admin-setup.md`
- `docs/claude-code-install.md`
- `docs/troubleshooting.md`

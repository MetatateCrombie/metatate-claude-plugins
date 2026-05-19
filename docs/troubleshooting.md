# Troubleshooting

## The Role ALL Is Blocked

Error:

```text
The role ALL requested has been explicitly blocked for use with this application by an administrator.
```

Cause:

Snowflake received an OAuth request for the user's default role or secondary
role `ALL` instead of the role intended for Metatate.

Fix:

1. Remove the existing Claude MCP registration.

   ```bash
   claude mcp remove metatate
   ```

2. Register again with an explicit Snowflake role scope.

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

3. Ask your Snowflake administrator to confirm the same role is allowed and
   preauthorized on the OAuth security integration.

Users should not need to change their default Snowflake role to fix this.

## Browser Login Completes But Claude Still Shows Disconnected

Check:

- The callback URI in Snowflake is exactly
  `http://localhost:8080/callback`.
- The Claude MCP registration uses `"callbackPort": 8080`.
- No other local process is occupying port `8080` during authentication.
- The browser completed the redirect back to localhost.

If your team uses a different callback port, update both Snowflake and Claude.

## Claude Cannot Find The Metatate MCP Tools

Check the MCP registration:

```bash
claude mcp get metatate
```

The URL should look like:

```text
https://<account-url>/api/v2/databases/METATATE_APP/schemas/CORE/mcp-servers/METATATE_MCP
```

Ask your administrator to confirm:

```sql
SHOW MCP SERVERS IN SCHEMA METATATE_APP.CORE;
```

Expected tools:

- `discover-context`
- `get-decision-context`
- `inspect-data-meaning`
- `inspect-governance-rules`
- `authorize-use`
- `validate-query-context`
- `explain-why`

## OAuth Client Secret Was Pasted Into Shell History

Rotate the Snowflake OAuth client secret and remove the exposed value from shell
history according to your organization's security process.

Register MCP again using:

```bash
claude mcp add-json --scope user --client-secret metatate '<json-config>'
```

The `--client-secret` flag makes Claude Code prompt for the secret instead of
requiring it in the command.

## Plugin Installed But Slash Commands Are Missing

Check:

```text
/plugin
```

Confirm `metatate@metatate-claude-plugins` is installed and enabled.

Then check:

```text
/help
```

If the commands still do not appear, restart Claude Code and update the
marketplace:

```text
/plugin marketplace update metatate-claude-plugins
```

## Permission Or Policy Result Looks Unexpected

Metatate is the source of truth for governance decisions. Capture:

- User Snowflake role.
- Table or asset name.
- Operation and intended use.
- Decision ID or validation ID returned by Metatate.
- Claude prompt used.

Then review the decision with:

```text
/metatate:explain-decision
```

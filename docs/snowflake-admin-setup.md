# Snowflake Administrator Setup

This guide prepares a Snowflake account so Claude Code can reach the Metatate
Snowflake-managed MCP server through Snowflake OAuth.

## 1. Confirm The Metatate MCP Server Exists

Use an administrative Snowflake role that can inspect the installed Native App.

```sql
SHOW MCP SERVERS IN SCHEMA METATATE_APP.CORE;
```

Expected server:

```text
METATATE_MCP
```

If your Native App database, schema, or server name differs, give users those
actual values. They are used in the Claude MCP URL:

```text
https://<account-url>/api/v2/databases/<app-db>/schemas/<schema>/mcp-servers/<server>
```

## 2. Choose The Claude Snowflake Role

Create or select a least-privilege role for Claude users. The examples below
use:

```text
METATATE_CLAUDE_USER
```

Use a role that is allowed to call the Metatate Native App and managed MCP
server. Do not use `ACCOUNTADMIN`, `SECURITYADMIN`, `ORGADMIN`, or other broad
administration roles for day-to-day Claude use.

Grant the role to each user who should use the Claude integration:

```sql
GRANT ROLE METATATE_CLAUDE_USER TO USER <snowflake_user>;
```

Also grant the Metatate Native App privileges required by your Metatate
deployment to this role. The exact grants depend on how your application
package was installed and configured.

## 3. Create The OAuth Security Integration

Use a Snowflake role that can create security integrations.

```sql
USE ROLE ACCOUNTADMIN;

CREATE SECURITY INTEGRATION METATATE_CLAUDE_CODE_OAUTH
  TYPE = OAUTH
  OAUTH_CLIENT = CUSTOM
  ENABLED = TRUE
  OAUTH_CLIENT_TYPE = 'CONFIDENTIAL'
  OAUTH_REDIRECT_URI = 'http://localhost:8080/callback'
  OAUTH_ALLOW_NON_TLS_REDIRECT_URI = TRUE
  OAUTH_ISSUE_REFRESH_TOKENS = TRUE;
```

Then restrict the OAuth integration to the role users should request from
Claude:

```sql
ALTER SECURITY INTEGRATION METATATE_CLAUDE_CODE_OAUTH
  SET ALLOWED_ROLES_LIST = ('METATATE_CLAUDE_USER')
      PRE_AUTHORIZED_ROLES_LIST = ('METATATE_CLAUDE_USER');
```

The Claude MCP registration must request the matching OAuth scope:

```text
session:role:METATATE_CLAUDE_USER
```

This prevents Snowflake from attempting to authorize the user's default role or
secondary role `ALL`.

## 4. Delegate Authorization For Users

For each Snowflake user who will authenticate from Claude Code:

```sql
ALTER USER <snowflake_user>
  ADD DELEGATED AUTHORIZATION OF ROLE METATATE_CLAUDE_USER
  TO SECURITY INTEGRATION METATATE_CLAUDE_CODE_OAUTH;
```

If your Snowflake account policy relies only on preauthorized roles, this may
not be necessary for every account. It is still a clear rollout pattern for a
controlled user allowlist.

## 5. Retrieve OAuth Client Values

Fetch the OAuth client values and share them through your approved secret
distribution process:

```sql
SELECT SYSTEM$SHOW_OAUTH_CLIENT_SECRETS('METATATE_CLAUDE_CODE_OAUTH');
```

Give users:

- Snowflake account URL, for example `https://<account>.snowflakecomputing.com`.
- OAuth client ID.
- OAuth client secret.
- Snowflake role, for example `METATATE_CLAUDE_USER`.
- App database, schema, and server name if they differ from
  `METATATE_APP.CORE.METATATE_MCP`.

Do not put the client secret in GitHub, Slack history, README files, screenshots,
or issue trackers. Use your normal password or secret sharing process.

## 6. Verify With A Test User

Ask one user to register the MCP server with:

```bash
claude mcp add-json --scope user --client-secret metatate '{
  "type": "http",
  "url": "https://<account-url>/api/v2/databases/METATATE_APP/schemas/CORE/mcp-servers/METATATE_MCP",
  "oauth": {
    "clientId": "<snowflake-oauth-client-id>",
    "callbackPort": 8080,
    "scopes": "session:role:METATATE_CLAUDE_USER"
  }
}'
```

Then have them run `/mcp` in Claude Code and authenticate.

Successful setup means:

- Snowflake login completes.
- Claude Code shows the `metatate` MCP server as connected.
- Claude can call the Metatate tools.

## Operational Notes

- Keep the OAuth callback port and Snowflake redirect URI aligned.
- Rotate the OAuth client secret according to your internal security policy.
- If changing the Claude role, update both the Snowflake OAuth integration and
  the Claude MCP registration.
- If users see a role-blocked error, confirm the MCP config contains
  `session:role:<role>` and that the same role is in
  `ALLOWED_ROLES_LIST` and `PRE_AUTHORIZED_ROLES_LIST`.

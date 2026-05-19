# Metatate Claude Plugins

Claude Code plugin marketplace for Metatate.

The `metatate` plugin brings Metatate governed data workflows into Claude Code
through the Snowflake-managed MCP server installed by the Metatate Snowflake
Native App. It does not run a Metatate-hosted MCP gateway and it does not store
Snowflake credentials in the plugin repository.

## What You Get

- Slash commands for governed data discovery, policy inspection, data-use
  authorization, query validation, decision explanation, policy review, and
  release gating.
- A Claude skill that tells Claude when and how to use the Metatate MCP tools.
- A small local helper script that generates the correct Claude MCP
  registration command for Snowflake OAuth.
- Customer-facing setup docs for Snowflake administrators and Claude Code
  users.

## Repository Layout

```text
metatate-claude-plugins/
  .claude-plugin/
    marketplace.json
  plugins/
    metatate/
      .claude-plugin/
        plugin.json
      bin/
        metatate-mcp-add
      commands/
      skills/
      README.md
  docs/
    claude-code-install.md
    snowflake-admin-setup.md
    troubleshooting.md
  examples/
    prompts.md
  SECURITY.md
  CHANGELOG.md
```

## Requirements

For the Snowflake administrator:

- Metatate Snowflake Native App installed in the target Snowflake account.
- The app exposes the managed MCP server, normally
  `METATATE_APP.CORE.METATATE_MCP`.
- A Snowflake role for Claude users. Use a least-privilege role that is allowed
  to use Metatate, not an account administration role.
- Privilege to create or manage a Snowflake OAuth security integration.

For each Claude Code user:

- Claude Code installed.
- Access to the target Snowflake account in a role authorized for Metatate.
- A Snowflake OAuth client ID from the administrator.
- The OAuth client secret entered only into Claude Code's secure prompt.

## Install The Plugin

In Claude Code, add this public marketplace:

```text
/plugin marketplace add MetatateCrombie/metatate-claude-plugins
```

Install the Metatate plugin:

```text
/plugin install metatate@metatate-claude-plugins
```

Restart Claude Code after installation if Claude prompts you to do so.

## Configure The Snowflake MCP Connection

The plugin and the MCP connection are separate:

- The plugin adds Claude commands and guidance.
- The MCP connection gives Claude Code access to the Snowflake-managed Metatate
  tools.

Register the MCP server in your terminal. Replace the placeholders with values
from your Snowflake administrator:

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

Claude Code will prompt for the OAuth client secret. Do not paste the client
secret into a shell command, README, issue, ticket, or committed file.

The `session:role:<snowflake-role>` scope is required. It makes Snowflake issue
the OAuth session for the intended Metatate role instead of falling back to the
user's default role or secondary role `ALL`.

If you cloned this repository locally, you can use the helper instead of
hand-writing the JSON:

```bash
./plugins/metatate/bin/metatate-mcp-add \
  --account-url https://<account-url> \
  --client-id <snowflake-oauth-client-id> \
  --snowflake-role <snowflake-role> \
  --config-scope user \
  --run
```

## Authenticate

Start Claude Code and open the MCP menu:

```text
/mcp
```

Select the `metatate` server and authenticate. Your browser should open a
Snowflake OAuth login page. After login, Snowflake redirects to Claude Code on
`http://localhost:8080/callback`.

## Smoke Test

After authentication, run:

```text
/metatate:discover-context
```

Ask for a known governed database, schema, domain, sensitivity level, or
compliance framework in your Metatate environment.

Then test one decision workflow:

```text
/metatate:authorize-use
```

Example prompt:

```text
Can role ANALYST use METATATE_TEST_DB.PUBLIC.CUSTOMERS for analytics?
```

Claude should call the Metatate MCP tools and return a governed result with
policy context, rationale, and any decision or validation IDs returned by
Metatate.

## Available Commands

- `/metatate:discover-context`
- `/metatate:inspect-data`
- `/metatate:inspect-rules`
- `/metatate:authorize-use`
- `/metatate:validate-query`
- `/metatate:explain-decision`
- `/metatate:policy-review`
- `/metatate:release-gate`

See [examples/prompts.md](examples/prompts.md) for end-to-end examples.

## MCP Tools Expected From Metatate

The Snowflake-managed MCP server should expose these tools:

- `discover-context`
- `get-decision-context`
- `inspect-data-meaning`
- `inspect-governance-rules`
- `authorize-use`
- `validate-query-context`
- `explain-why`

If any of these are missing, update the Metatate Snowflake Native App before
testing the Claude plugin.

## Administrator Setup

Snowflake administrators should start with
[docs/snowflake-admin-setup.md](docs/snowflake-admin-setup.md).

The important alignment is:

- OAuth integration allows the same role that users pass as
  `session:role:<snowflake-role>`.
- The role has the Metatate privileges required by your Native App deployment.
- The redirect URI is `http://localhost:8080/callback`, unless your team uses a
  different Claude callback port and registers the same port in Snowflake.

## Troubleshooting

Start with [docs/troubleshooting.md](docs/troubleshooting.md).

The most common OAuth error is:

```text
The role ALL requested has been explicitly blocked for use with this application.
```

Fix it by registering Claude MCP with:

```json
"scopes": "session:role:<snowflake-role>"
```

and by ensuring the Snowflake OAuth integration explicitly allows that same
role.

## Security Notes

- This plugin is a thin Claude Code workflow layer.
- Snowflake authenticates the user through OAuth.
- Claude Code stores the OAuth client secret in the user's local Claude
  configuration, not in this repository.
- The plugin should be used with least-privilege Snowflake roles.
- The default workflows operate on metadata, policy context, query text,
  intended use, and decision records. They should not request raw row-level data
  unless your organization explicitly allows that pattern.

See [SECURITY.md](SECURITY.md) for reporting and handling security issues.

## Release Notes

See [CHANGELOG.md](CHANGELOG.md).

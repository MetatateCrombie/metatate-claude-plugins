# Security

## Supported Versions

Security fixes are applied to the latest released version of the marketplace and
plugin.

## Reporting A Vulnerability

Do not open a public GitHub issue for suspected credential exposure,
authorization bypass, tenant isolation problems, or other sensitive findings.

Report security issues through your Metatate support or security contact. If
you do not have a private contact, email the Metatate team through the contact
channel listed on the Metatate website and request a private security intake.

## Credential Handling

- Do not commit Snowflake OAuth client secrets.
- Do not paste OAuth client secrets into shell commands.
- Use `claude mcp add-json --client-secret` so Claude Code prompts for the
  secret.
- Rotate the Snowflake OAuth client secret if it appears in shell history,
  screenshots, logs, issue trackers, or chat tools.

## Data Handling

The plugin is designed for metadata, policy context, intended-use context,
query text, and decision records. It should not request raw row-level data by
default. If your organization enables workflows that inspect raw data, treat
that as a separate governance and audit decision.

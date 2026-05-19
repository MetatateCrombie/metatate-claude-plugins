# Changelog

## 0.1.3

- Refined the README opening, feature summary, and plugin metadata to align
  with Metatate's structured context, portable decision layer, and agent
  coordination positioning.

## 0.1.2

- Added the full no-clone `claude mcp add-json` registration command to the
  main README, so marketplace-only installers do not need a local checkout to
  configure MCP.

## 0.1.1

- Added quick access to the Metatate Snowflake Marketplace listing from the
  main README prerequisites.
- Reworked smoke tests and example prompts to discover governed assets first
  and use customer-specific placeholders instead of demo table names.

## 0.1.0

- Initial public Metatate Claude Code plugin marketplace.
- Added governed data discovery, inspection, authorization, query validation,
  decision explanation, policy review, and release gate commands.
- Added Snowflake-managed MCP setup documentation.
- Added OAuth role-scoped MCP registration guidance using
  `session:role:<snowflake-role>`.
- Added GitHub Actions validation for the marketplace, plugin, helper script,
  role-scoped OAuth output, and Markdown docs.

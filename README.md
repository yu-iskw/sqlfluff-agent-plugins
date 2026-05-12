# SQLFluff agent plugins

Agent skills for using [SQLFluff](https://www.sqlfluff.com/) safely on large SQL and dbt-style projects. This repository publishes a skills-first **sqlfluff** plugin so coding agents can lint, format, debug templating and parsing, tune rules, and plan CI rollout without improvising unsafe CLI usage.

Licensed under the Apache License 2.0. See [LICENSE](LICENSE).

## Prerequisites

- **SQLFluff on your PATH.** The skills assume you can run `sqlfluff` in the environment where the agent runs (for example your project virtualenv or CI image). They do not install SQLFluff for you.
- For **dbt-style** projects, skills are written to **detect templater support** instead of assuming the dbt templater is installed.

## Install

Each platform consumes the same plugin sources under [`plugins/sqlfluff`](plugins/sqlfluff). This repo exposes three marketplace manifests at the root (same plugin entry, different ecosystems):

| Platform    | Marketplace file                                                     |
| ----------- | -------------------------------------------------------------------- |
| Claude Code | [`.claude-plugin/marketplace.json`](.claude-plugin/marketplace.json) |
| Cursor      | [`.cursor-plugin/marketplace.json`](.cursor-plugin/marketplace.json) |
| Codex       | [`.codex-plugin/marketplace.json`](.codex-plugin/marketplace.json)   |

Use a **local clone** of this repository or your own fork that includes these files.

### Claude Code

1. Install the Claude Code CLI if needed: `npm install -g @anthropic-ai/claude-code`
2. From a terminal, register this repository as a plugin marketplace (the CLI accepts either the repo root or the `.claude-plugin` directory):

   ```bash
   claude plugin marketplace add /path/to/sqlfluff-agent-plugins
   # or: claude plugin marketplace add /path/to/sqlfluff-agent-plugins/.claude-plugin
   ```

3. Install the **sqlfluff** plugin for your workspace. The marketplace name in [`.claude-plugin/marketplace.json`](.claude-plugin/marketplace.json) is `sqlfluff-agent-plugins`:

   ```bash
   claude plugin install -s project sqlfluff@sqlfluff-agent-plugins
   ```

   If that form fails with your CLI version, try `claude plugin install -s project sqlfluff` or see `claude plugin install --help`.

### Cursor

This repo’s [`.cursor-plugin/marketplace.json`](.cursor-plugin/marketplace.json) lists the **sqlfluff** plugin with source `./plugins/sqlfluff`. Add this repository (or fork) as a marketplace or install the plugin the way your Cursor build expects.

Authoritative steps change with Cursor releases; use the official guide: [Cursor — Plugins](https://cursor.com/docs/plugins).

### Codex

This repo’s [`.codex-plugin/marketplace.json`](.codex-plugin/marketplace.json) points at the same `./plugins/sqlfluff` tree. Add the marketplace or install the plugin per Codex’s current workflow.

Authoritative steps: [OpenAI Codex — Build a plugin](https://developers.openai.com/codex/plugins/build).

## Usage

After installation, use the agent skills by **name** (for example in natural language: “use **sqlfluff-lint-triage** on `models/staging/orders.sql`” or via your product’s skill or rules UI if it lists them).

Skill IDs included in this plugin:

- `sqlfluff-project-onboarding`
- `sqlfluff-lint-triage`
- `sqlfluff-safe-fix`
- `sqlfluff-template-debug`
- `sqlfluff-parse-debug`
- `sqlfluff-rules-config`
- `sqlfluff-ci-rollout`

Full descriptions and design notes: [plugins/sqlfluff/README.md](plugins/sqlfluff/README.md).

## Contributing

Development setup, validation commands, and how to change skills or manifests: [CONTRIBUTING.md](CONTRIBUTING.md).

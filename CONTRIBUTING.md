# Contributing

Thanks for contributing to **sqlfluff-agent-plugins**. This repository ships the **sqlfluff** plugin under [`plugins/sqlfluff`](plugins/sqlfluff) (skills-first layout). Changes to skills, references, or manifests should keep Claude Code, Cursor, and Codex packaging consistent unless you intentionally scope a change to one platform.

## Development prerequisites

- `git`
- `docker`
- `trunk` CLI
- Optional: `claude` CLI (`npm install -g @anthropic-ai/claude-code`) for full plugin install / list checks used in integration tests

## Setup

1. Fork or branch from this repository.
2. Install Trunk:
   - `curl https://get.trunk.io -fsSL | bash`
3. Verify tools:
   - `trunk --version`
   - `docker --version`

## Local checks

Run these from the **repository root** before opening a pull request:

1. `make format`
2. `make lint`
3. `make test-integration-docker` — builds the integration image and runs all integration tests, including the Claude Code plugin **install** path (marketplace add + install + list / validate)

You can also run scripts directly:

- `./integration_tests/validate-manifest.sh`
- `./integration_tests/run.sh --verbose` (add `--skip-loading` if you only need structural checks)
- `./integration_tests/test-plugin-install.sh` — requires the `claude` CLI, `jq`, and `.claude-plugin/marketplace.json` at the repo root

## Repository and plugin layout

Marketplace manifests (each lists the same `sqlfluff` plugin for its ecosystem):

- [`.claude-plugin/marketplace.json`](.claude-plugin/marketplace.json)
- [`.cursor-plugin/marketplace.json`](.cursor-plugin/marketplace.json)
- [`.codex-plugin/marketplace.json`](.codex-plugin/marketplace.json)

Plugin-specific metadata:

- `plugins/sqlfluff/.claude-plugin/plugin.json`
- `plugins/sqlfluff/.cursor-plugin/plugin.json`
- `plugins/sqlfluff/.codex-plugin/plugin.json`

Shared skills and references:

```text
plugins/sqlfluff/skills/<skill-name>/
├── SKILL.md
└── references/
    └── <reference>.md
```

In-repo authoring guides (optional reading when changing manifests or skills):

- `.claude/skills/implement-agent-skills/` — skill structure and front matter
- `.claude/skills/implement-plugin/` — Claude Code plugin packaging
- `.claude/skills/implement-cursor-plugin/` — Cursor plugin packaging
- `.claude/skills/implement-codex-plugin/` — Codex plugin packaging

## Adding or updating skills

- Add or edit trees under `plugins/sqlfluff/skills/<skill-name>/` with `SKILL.md` plus optional `references/*.md`.
- Keep each skill **self-contained**; do not require another skill on the success path.
- Keep `SKILL.md` concise; put long command matrices or edge cases in `references/`.
- Avoid automatic hooks until rollout behavior is proven on real projects.
- Require explicit user approval before documenting or running `sqlfluff fix --FIX-EVEN-UNPARSABLE`.

Validate structure and front matter (examples):

```bash
bash .claude/skills/implement-agent-skills/scripts/validate-skill-structure.sh plugins/sqlfluff/skills
bash .claude/skills/implement-agent-skills/scripts/check-skill-frontmatter.sh plugins/sqlfluff/skills/sqlfluff-project-onboarding/SKILL.md
```

Run the structure validator for **every** skill directory before release.

### Other plugin components

The **sqlfluff** plugin is currently **skills-only**. If you add agents, hooks, commands, rules, or MCP wiring later, place them under `plugins/sqlfluff/` following the layouts in the `implement-*` skills above, and extend integration tests if loading behavior changes.

## SQLFluff CLI assumptions

Skills assume workflows such as:

```bash
sqlfluff version
sqlfluff dialects --nocolor -v
sqlfluff rules --nocolor -v
sqlfluff lint models/staging/orders.sql --format json --nofail --disable-progress-bar
sqlfluff format models/staging/orders.sql --disable-progress-bar
sqlfluff render models/marts/orders.sql --dialect snowflake --templater jinja
sqlfluff parse models/marts/orders.sql --dialect snowflake --format yaml
```

## Pull request guidelines

1. Keep changes scoped and focused.
2. Update docs when behavior or structure changes (root [README.md](README.md), [plugins/sqlfluff/README.md](plugins/sqlfluff/README.md), or this file as appropriate).
3. Include test evidence in your PR description (commands run and outcomes).
4. Ensure CI passes (`trunk_check` and `integration_tests` workflows).

## Commit guidelines

- Use clear, imperative commit messages.
- Prefer small commits that are easy to review.

## Reporting issues

Open an issue with:

- expected behavior
- actual behavior
- reproduction steps
- logs or screenshots when relevant

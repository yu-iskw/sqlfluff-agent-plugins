# SQLFluff Agent Skills Plugin

Agent skills for using SQLFluff safely on large SQL and dbt-style projects.

This repository packages a skills-first `sqlfluff` plugin under `plugins/sqlfluff`. See `plugins/sqlfluff/README.md` for the full skill catalog and plugin-specific usage notes.

## Repository Layout

```text
.
├── plugins/
│   └── sqlfluff/
│       ├── .claude-plugin/
│       │   └── plugin.json
│       ├── .cursor-plugin/
│       │   └── plugin.json
│       ├── .codex-plugin/
│       │   └── plugin.json
│       ├── skills/
│       │   └── <skill-name>/
│       │       ├── SKILL.md
│       │       └── references/
│       │           └── <reference>.md
│       └── README.md
├── integration_tests/
├── Makefile
└── README.md
```

## SQLFluff Assumptions

The skills are grounded in SQLFluff CLI workflows:

```bash
sqlfluff version
sqlfluff dialects --nocolor -v
sqlfluff rules --nocolor -v
sqlfluff lint models/staging/orders.sql --format json --nofail --disable-progress-bar
sqlfluff format models/staging/orders.sql --disable-progress-bar
sqlfluff render models/marts/orders.sql --dialect snowflake --templater jinja
sqlfluff parse models/marts/orders.sql --dialect snowflake --format yaml
```

For dbt-style projects, the skills instruct agents to detect templater support instead of assuming the dbt templater is installed.

## Validation

Run plugin validation from the repository root:

```bash
./integration_tests/run.sh --skip-loading --verbose
```

Validate skill structure and frontmatter:

```bash
bash .claude/skills/implement-agent-skills/scripts/validate-skill-structure.sh plugins/sqlfluff/skills
bash .claude/skills/implement-agent-skills/scripts/check-skill-frontmatter.sh plugins/sqlfluff/skills/sqlfluff-project-onboarding/SKILL.md
```

Run the skill validators for every skill directory before release. Run repository linting with:

```bash
make lint
```

## Development Notes

- Keep each skill self-contained; do not require another skill on the success path.
- Keep `SKILL.md` concise and put detailed command notes in one-level `references/` files.
- Avoid automatic hooks until rollout behavior is proven on real projects.
- Require explicit user approval before using `sqlfluff fix --FIX-EVEN-UNPARSABLE`.

## License

Apache License 2.0. See `LICENSE`.

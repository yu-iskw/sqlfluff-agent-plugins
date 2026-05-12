---
name: sqlfluff-project-onboarding
description: Use when onboarding to a SQL or dbt-style project and determining the right SQLFluff dialect, templater, config, and baseline lint commands.
compatibility: Requires sqlfluff on PATH.
---

# SQLFluff Project Onboarding

## Purpose

Establish how SQLFluff should run in an unfamiliar SQL or dbt-style repository before linting broadly or applying fixes. Use this to discover the local SQLFluff version, dialect, templater, rule surface, configuration files, and safe baseline commands.

## Workflow

1. **Confirm SQLFluff availability**
   - Run `sqlfluff version`.
   - If SQLFluff is missing, stop and report that the project needs SQLFluff installed before this workflow can continue.

2. **Inspect the available SQLFluff surface**
   - Run `sqlfluff dialects --nocolor -v`.
   - Run `sqlfluff rules --nocolor -v`.
   - Note warehouse-relevant dialects such as `bigquery`, `snowflake`, `databricks`, `redshift`, `postgres`, `duckdb`, `trino`, and `sparksql` when available.

3. **Discover project configuration**
   - Look for SQLFluff config files such as `.sqlfluff`, `pyproject.toml`, `setup.cfg`, or `tox.ini`.
   - Look for dbt indicators such as `dbt_project.yml`, `models/`, `macros/`, `packages.yml`, and `profiles.yml` references.
   - Identify configured `dialect`, `templater`, `rules`, `exclude_rules`, `ignore`, and `.sqlfluffignore` behavior when present.

4. **Check templater assumptions**
   - Do not assume the `dbt` templater is installed. SQLFluff installations may only expose templaters such as `raw`, `jinja`, `python`, and `placeholder`.
   - If the project expects dbt templating but SQLFluff rejects `--templater dbt`, report the missing templater as an environment/setup issue.

5. **Run a narrow baseline lint**
   - Prefer one representative file or a small directory first.
   - Use `--nofail`, `--format json`, and `--disable-progress-bar` for agent-readable exploratory output.

6. **Report the operating model**
   - Summarize the detected dialect, templater, config files, ignore policy, and recommended first commands.
   - Call out uncertainty explicitly instead of guessing.

## Command Patterns

```bash
sqlfluff version
sqlfluff dialects --nocolor -v
sqlfluff rules --nocolor -v
sqlfluff lint models/staging/orders.sql --nofail --format json --disable-progress-bar
```

## Output

Return a concise onboarding summary with:

- SQLFluff version and availability.
- Detected or recommended dialect.
- Detected or recommended templater.
- Configuration files and ignore files found.
- A safe first lint command for the project.
- Any blockers, especially missing templater support.

## Success Criteria

- The agent can run SQLFluff with an explicit dialect/config strategy.
- The agent avoids broad linting or fixing until the project setup is understood.
- The user can see exactly which assumptions are confirmed and which remain open.

## Progressive Disclosure

- Additional CLI details: `references/cli-cheatsheet.md`

---
name: sqlfluff-template-debug
description: Use when SQLFluff linting fails on Jinja or dbt-style templating and the rendered SQL or templater configuration must be diagnosed.
compatibility: Requires sqlfluff on PATH.
---

# SQLFluff Template Debug

## Purpose

Diagnose failures that happen before SQLFluff can lint normal SQL, especially Jinja or dbt-style templating problems. Use this when lint output mentions templating, undefined variables, macros, rendering, `library_path`, or a missing templater.

## Workflow

1. **Reproduce on one file**
   - Choose the smallest failing SQL file.
   - Keep the same dialect, templater, and config used by the failing lint run.

2. **Render the file**
   - Run `sqlfluff render` on the failing file.
   - If testing unsaved editor content through stdin, include `--stdin-filename` so SQLFluff loads config as if the content came from the intended path.

3. **Inspect templater logging**
   - Use `--logger templater` and increased verbosity for templater-specific logs.
   - Use `--library-path` to override Jinja library loading when appropriate.
   - Use `--library-path none` only when disabling library loading is part of the diagnosis.

4. **Check templater availability**
   - Do not assume the `dbt` templater exists. The local SQLFluff install may expose only `raw`, `jinja`, `python`, and `placeholder`.
   - If the project needs dbt templating but SQLFluff rejects it, report the missing integration instead of rewriting project SQL.

5. **Separate diagnosis from rollout suppression**
   - `--ignore templating` can be useful for temporary rollout reports.
   - Do not present ignored templating failures as fixed.

6. **Recommend a concrete fix path**
   - Config correction, macro/library path setup, templater installation, or narrowing the lint scope are valid outcomes.

## Command Patterns

```bash
sqlfluff render models/marts/orders.sql --dialect snowflake --templater jinja
sqlfluff lint models/marts/orders.sql --logger templater -vv --nofail
sqlfluff render - --stdin-filename models/marts/orders.sql --dialect snowflake --templater jinja
sqlfluff render models/marts/orders.sql --library-path none --dialect snowflake --templater jinja
```

## Output

Return:

- The failing file and command.
- Whether rendering succeeds.
- The relevant templater/config issue.
- Whether `dbt` templater support is available or missing when relevant.
- A specific next step.

## Progressive Disclosure

- Templating debug details: `references/templating-debug.md`

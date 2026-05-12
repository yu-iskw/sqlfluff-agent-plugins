# SQLFluff CLI Cheatsheet

## Discovery Commands

```bash
sqlfluff version
sqlfluff dialects --nocolor -v
sqlfluff rules --nocolor -v
```

Use these before assuming a dialect, templater, rule code, or installed SQLFluff feature.

## Core Commands

- `sqlfluff lint [PATHS]...`: lint SQL files, directories, `-` from stdin, or `.` for the current directory.
- `sqlfluff fix [PATHS]...`: fix SQL files. Use cautiously, especially on templated or unparseable files.
- `sqlfluff format [PATHS]...`: autoformat using a known stable subset of fix rules.
- `sqlfluff render PATH`: render templated SQL and print the rendered result.
- `sqlfluff parse PATH`: parse SQL and print the parse tree.
- `sqlfluff rules`: show rules and groups.
- `sqlfluff dialects`: show dialects.

## Common Options

- `--config PATH`: include an additional cfg-format config file.
- `--ignore-local-config`: ignore config files in default search locations.
- `--dialect TEXT`: choose the SQL dialect.
- `--templater raw|jinja|python|placeholder`: choose the templater when available.
- `--stdin-filename PATH`: load config as if stdin came from a real file path.
- `--library-path TEXT`: override Jinja `library_path`; use `none` to disable.
- `--logger templater|lexer|parser|linter|rules|config`: focus verbose logs.
- `--disable-progress-bar`: keep automation logs clean.
- `--processes INTEGER`: parallelize lint/fix/format; `0` means all CPUs.
- `--nofail`: return zero even with violations, useful for rollout and reports.

## dbt-Style Project Caveat

The presence of `dbt_project.yml` does not prove that SQLFluff has the dbt templater installed. If `--templater dbt` is needed, confirm the installed SQLFluff integration supports it before treating render failures as SQL problems.

# SQLFluff Parse Debug

## Structured Parse Output

```bash
sqlfluff parse models/marts/orders.sql --dialect snowflake --format yaml
sqlfluff parse models/marts/orders.sql --dialect snowflake --format json
```

Use YAML for direct reading and JSON for downstream tooling.

## Position and Meta Segments

```bash
sqlfluff parse models/marts/orders.sql --dialect snowflake --format json --include-meta
```

`--include-meta` includes indents, dedents, placeholders, and position fields such as `start_line_no`, `start_line_pos`, `end_line_no`, and `end_line_pos`.

## Reducing Noise

```bash
sqlfluff parse models/marts/orders.sql --dialect snowflake --code-only
```

Use `--code-only` when meta segments distract from the grammar issue.

## Parser Diagnostics

```bash
sqlfluff parse models/marts/orders.sql --dialect snowflake --parse-statistics -vv
sqlfluff parse models/marts/orders.sql --dialect snowflake --logger parser -vv
sqlfluff parse models/marts/orders.sql --dialect snowflake --logger lexer -vv
```

Use these when normal parse output is not enough to identify the failing construct.

## Common Root Causes

- Wrong dialect for warehouse-specific syntax.
- SQL left invalid by templating.
- Unsupported syntax in the installed SQLFluff version.
- Config points SQLFluff at the wrong templater or dialect.

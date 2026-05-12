---
name: sqlfluff-parse-debug
description: Use when SQLFluff cannot parse rendered SQL and the dialect, grammar, or parse tree needs diagnosis.
compatibility: Requires sqlfluff on PATH.
---

# SQLFluff Parse Debug

## Purpose

Diagnose SQLFluff parser failures after templating has been handled or ruled out. Use this when lint output reports parsing errors, dialect mismatch, unexpected grammar, or confusing rule failures that require inspecting the parse tree.

## Workflow

1. **Confirm the input SQL**
   - If templating may be involved, render the file first.
   - Confirm the dialect passed through config or `--dialect`.

2. **Run structured parse output**
   - Use `sqlfluff parse --format yaml` for readable inspection.
   - Use `--format json` when another tool or agent will process the tree.
   - Use `--include-meta` when positions or placeholder segments matter.

3. **Narrow the parse problem**
   - Use `--code-only` to reduce tree noise when meta segments are not relevant.
   - Use `--parse-statistics` and `-vv` for detailed parser terminator debugging.
   - Use `--logger parser` or `--logger lexer` when logs are too broad.

4. **Identify root cause category**
   - Dialect mismatch.
   - Unsupported SQL syntax.
   - Templating left invalid SQL.
   - Config or rule expectation mismatch.

5. **Recommend a fix**
   - Prefer dialect/config changes, SQL rewrite, or a scoped ignore with clear rationale.
   - Do not recommend broad `--ignore parsing` unless the user is deliberately collecting a rollout baseline.

## Command Patterns

```bash
sqlfluff parse models/marts/orders.sql --dialect snowflake --format yaml
sqlfluff parse models/marts/orders.sql --dialect snowflake --format json --include-meta
sqlfluff parse models/marts/orders.sql --dialect snowflake --parse-statistics -vv
sqlfluff parse models/marts/orders.sql --dialect snowflake --logger parser -vv
```

## Output

Return:

- Dialect and config used.
- Whether parsing succeeds.
- The smallest SQL region implicated by the parse output.
- Root cause category.
- Recommended next change or validation command.

## Progressive Disclosure

- Parse debug details: `references/parse-debug.md`

# SQLFluff Rule Families

## Common Families

- `layout`: spacing, indentation, line length, commas, newlines, file boundaries.
- `capitalisation`: keywords, identifiers, functions, literals, datatypes.
- `jinja`: Jinja tag padding.
- `aliasing`: table, column, expression, uniqueness, unused aliases.
- `ambiguous`: DISTINCT, UNION, ordering, joins, column counts, limits.
- `convention`: null checks, join direction, casting, quoted literals, blocked words.
- `references`: FROM references, qualification, keywords, quoting, special characters.
- `structure`: unused CTEs, nested CASE, subqueries, join conditions, unused joins.

## Rollout Sets

These sets are examples. Confirm rule availability with `sqlfluff rules --nocolor -v` before recommending them for a specific SQLFluff version.

Formatting-heavy starter example:

```text
LT01, LT02, LT05, LT12, CP01, CP02, JJ01
```

Correctness-sensitive review example:

```text
AM04, AM05, AM09, RF01, RF02, ST03, ST11
```

Large legacy rollout pattern:

```text
Start with layout/core rules, then add references and structure once parsing and templating are stable.
```

## Inline Ignore Audits

```bash
sqlfluff lint models/staging --warn-unused-ignores --format json --nofail
sqlfluff lint models/staging --disable-noqa --format json --nofail
sqlfluff lint models/staging --disable-noqa-except LT01,LT02 --format json --nofail
```

Use these to distinguish necessary suppressions from stale or overly broad `noqa` comments. `--disable-noqa` audits can be noisy, so start with scoped paths.

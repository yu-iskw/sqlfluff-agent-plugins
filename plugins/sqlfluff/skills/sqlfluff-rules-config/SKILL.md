---
name: sqlfluff-rules-config
description: Use when designing, reviewing, or changing SQLFluff rules and configuration policy for a SQL or dbt-style project.
compatibility: Requires sqlfluff on PATH.
---

# SQLFluff Rules Config

## Purpose

Help teams choose and evolve SQLFluff rule policy without accidentally weakening useful checks or overwhelming a large legacy project. Use this when the user asks which rules to enable, why a rule fires, how to phase in SQLFluff, or how to manage `noqa` usage.

## Workflow

1. **Inspect active rule metadata**
   - Run `sqlfluff rules --nocolor -v`.
   - Capture rule codes, names, groups, aliases, and whether rules are part of `core`.

2. **Classify policy intent**
   - Formatting and readability: layout, capitalisation, Jinja padding.
   - Consistency and convention: aliases, quotes, joins, casting, blocked words.
   - Correctness-sensitive review: ambiguous, references, structure.
   - Dialect-specific rules: T-SQL, Oracle, and warehouse-specific behavior where relevant.

3. **Review current configuration**
   - Inspect configured `rules`, `exclude_rules`, warnings, `ignore`, and inline `noqa` policy.
   - Treat `.sqlfluffignore` as scope policy, not as a fix for real parser or templater failures.

4. **Design a phased rule set**
   - Start with low-risk formatting in large legacy projects.
   - Add references and structure rules once parsing/templating is reliable.
   - Use `--rules` for temporary allowlists and `--exclude-rules` for targeted exclusions.

5. **Handle inline ignores deliberately**
   - Use `--warn-unused-ignores` to find stale `noqa` comments.
   - Use `--disable-noqa` when auditing suppressed violations.
   - Use `--disable-noqa-except` when only selected inline ignores should remain active.

6. **Recommend config changes**
   - Explain the behavior impact of each rule change.
   - Keep rollout and permanent policy separate.

## Command Patterns

```bash
sqlfluff rules --nocolor -v
sqlfluff lint models/staging --rules LT01,LT02,LT12,CP01,JJ01 --format json --nofail
sqlfluff lint models/staging --exclude-rules AM04,RF01 --format json --nofail
sqlfluff lint models/staging --warn-unused-ignores --format json --nofail
sqlfluff lint models/staging --disable-noqa --format json --nofail
```

## Output

Return:

- Current rule policy summary.
- Proposed rollout rule set.
- Rules to exclude temporarily and why.
- Rules that should remain enforced because they protect correctness or maintainability.
- Recommended validation command.

## Progressive Disclosure

- Rule family details: `references/rule-families.md`

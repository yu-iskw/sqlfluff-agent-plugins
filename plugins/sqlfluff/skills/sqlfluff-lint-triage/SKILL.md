---
name: sqlfluff-lint-triage
description: Use when running SQLFluff lint on SQL or dbt-style projects and summarizing violations without immediately editing files.
compatibility: Requires sqlfluff on PATH.
---

# SQLFluff Lint Triage

## Purpose

Run SQLFluff lint in a way that is scoped, repeatable, and useful to an agent. Use this when the user asks what is failing, wants a lint summary, is introducing SQLFluff to a large project, or needs violations grouped into actionable next steps.

## Workflow

1. **Choose the smallest useful scope**
   - Prefer changed SQL files, a target model directory, or one representative file.
   - Use a broad project path only when the user explicitly wants full-project linting.

2. **Use agent-readable output**
   - Prefer `--format json` for local triage.
   - Use `--format yaml` when humans will inspect the output directly.
   - Use `--format sarif`, `github-annotation`, or `github-annotation-native` for CI and PR annotations.

3. **Keep exploratory runs non-blocking**
   - Use `--nofail` when collecting violations during rollout or diagnosis.
   - Use `--disable-progress-bar` to keep logs parseable.
   - Add `--processes 0` only for large runs when full CPU parallelism is acceptable.

4. **Group findings by failure type**
   - Separate templating errors, parsing errors, and lint rule violations.
   - Group lint violations by rule code and file concentration.
   - Identify whether failures are style-only, config-related, dialect-related, or likely SQL issues.

5. **Recommend the next action**
   - For templating failures, recommend rendering/debugging the failing file.
   - For parsing failures, recommend parse debugging and dialect confirmation.
   - For style-only violations, recommend safe formatting or scoped fixing.
   - For noisy legacy projects, recommend phased CI rollout.

## Command Patterns

```bash
sqlfluff lint models/staging/orders.sql --format json --nofail --disable-progress-bar
sqlfluff lint models/staging/orders.sql --format yaml --nofail
sqlfluff lint models --format sarif --write-output sqlfluff.sarif --nofail
```

## Output

Return:

- Command and scope used.
- Total files checked when available.
- Top rule codes and representative files.
- Templating or parse failures separated from normal lint violations.
- Smallest safe next step.

## Guardrails

- Do not apply fixes from this skill.
- Do not hide parsing or templating failures with `--ignore` unless the user only wants a temporary rollout report.
- Do not treat `--nofail` as success; it is only a reporting mode.

## Progressive Disclosure

- Output format details: `references/output-formats.md`

---
name: sqlfluff-ci-rollout
description: Use when adding SQLFluff to CI or making SQLFluff PR checks manageable for a large SQL or dbt-style project.
compatibility: Requires sqlfluff on PATH and CI support for artifacts or annotations when used in CI.
---

# SQLFluff CI Rollout

## Purpose

Introduce SQLFluff to CI in phases so teams get useful feedback without blocking all work on existing violations. Use this when the user asks for CI integration, PR annotations, SARIF output, rollout strategy, or performance tuning for large SQL/dbt projects.

## Workflow

1. **Start report-only**
   - Use `--nofail` while collecting the first baseline.
   - Write structured output to an artifact with `--write-output`.
   - Disable progress bars for cleaner CI logs.

2. **Choose the CI feedback format**
   - Use `github-annotation-native` for GitHub-native annotations.
   - Use `github-annotation` when a generic GitHub annotation format is needed.
   - Use `sarif` when uploading results to code scanning or long-lived artifacts.
   - Use `json` for custom summaries.

3. **Scope the rollout**
   - Prefer changed files, selected directories, or selected rule families.
   - Use `--rules` for allowlisted rollout.
   - Use `--exclude-rules` for a small number of temporary exceptions.

4. **Measure runtime**
   - Use `--processes 0` when full CPU parallelism is acceptable.
   - Use `--persist-timing` to find slow files or rules.

5. **Move toward blocking**
   - Once violations are understood, remove `--nofail` for the selected scope.
   - Keep report-only jobs for broader scopes until they are clean enough to gate.

6. **Document the policy**
   - Record which paths/rules are blocking, which are report-only, and what must happen before expanding the gate.

## Command Patterns

```bash
sqlfluff lint models/staging --format github-annotation-native --annotation-level warning --nofail --disable-progress-bar
sqlfluff lint models/staging --format sarif --write-output sqlfluff.sarif --nofail --disable-progress-bar
sqlfluff lint models/staging --persist-timing sqlfluff-timing.csv --disable-progress-bar
```

## Output

Return:

- Proposed CI mode: report-only, scoped blocking, or full blocking.
- Command examples for the target CI environment.
- Artifact or annotation format.
- Runtime measurement plan.
- Criteria for expanding enforcement.

## Progressive Disclosure

- Rollout patterns: `references/rollout-patterns.md`

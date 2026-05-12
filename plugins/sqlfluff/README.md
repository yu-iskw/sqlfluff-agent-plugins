# SQLFluff Plugin

Agent skills for using SQLFluff safely on large SQL and dbt-style projects.

## Included Skills

- `sqlfluff-project-onboarding`: discover SQLFluff setup, dialects, templaters, config, and baseline commands.
- `sqlfluff-lint-triage`: run scoped lint checks and summarize violations without editing files.
- `sqlfluff-safe-fix`: format and fix SQL conservatively with an explicit safety ladder.
- `sqlfluff-template-debug`: diagnose Jinja and dbt-style templating failures.
- `sqlfluff-parse-debug`: inspect parse trees and dialect/parser failures.
- `sqlfluff-rules-config`: design rule policy, phased rollout sets, and `noqa` audits.
- `sqlfluff-ci-rollout`: introduce SQLFluff to CI with report-only output, annotations, SARIF, and timing.

## Design Notes

This plugin is skills-only in its first version. It does not install hooks or an MCP server. The skills should help agents choose safe SQLFluff workflows before introducing automation that might be noisy on large dbt projects.

The skills assume `sqlfluff` is available on `PATH`. They do not assume the SQLFluff dbt templater is installed; agents should detect templater support and report missing setup explicitly.

## Validation

From the repository root:

```bash
./integration_tests/run.sh --skip-loading --verbose
bash .claude/skills/implement-agent-skills/scripts/validate-skill-structure.sh plugins/sqlfluff/skills
make lint
```

# SQLFluff CI Rollout Patterns

## Report-Only Baseline

```bash
sqlfluff lint models/staging --format json --write-output sqlfluff.json --nofail --disable-progress-bar
```

Use this first when a project has unknown or large existing violation counts.

## GitHub Annotations

```bash
sqlfluff lint models/staging --format github-annotation-native --annotation-level warning --nofail --disable-progress-bar
```

Use `notice` or `warning` for rollout. Use `failure` or `error` only when the job is intended to block.

## SARIF Artifact

```bash
sqlfluff lint models/staging --format sarif --write-output sqlfluff.sarif --nofail --disable-progress-bar
```

Use SARIF when results should be uploaded to code scanning or retained as a structured artifact.

## Scoped Blocking

Start with one of:

- Changed files only.
- `models/staging` or another low-risk directory.
- A small allowlist of formatting rules with `--rules`.
- A broad run with selected temporary exclusions using `--exclude-rules`.

Then remove `--nofail` only for that chosen scope.

## Runtime Analysis

```bash
sqlfluff lint models/staging --persist-timing sqlfluff-timing.csv --disable-progress-bar
```

Use timing output to identify slow files, templater hotspots, or rule families that need separate handling.

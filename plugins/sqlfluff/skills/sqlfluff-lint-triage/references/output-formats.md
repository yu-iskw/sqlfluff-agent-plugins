# SQLFluff Output Formats

## Local Triage

- `human`: default, good for direct terminal reading.
- `json`: best default for agents and scripts.
- `yaml`: readable structured output for humans and agents.
- `none`: suppress lint result output when only exit behavior matters.

## CI and Review

- `sarif`: use when uploading to code scanning or preserving detailed artifacts.
- `github-annotation`: use for GitHub annotation output.
- `github-annotation-native`: use for native GitHub Actions annotation behavior.

## Useful Companions

```bash
sqlfluff lint models/staging --format json --nofail --disable-progress-bar
sqlfluff lint models/staging --format yaml --nofail --disable-progress-bar
sqlfluff lint models/staging --format sarif --write-output sqlfluff.sarif --nofail
sqlfluff lint models/staging --format github-annotation-native --annotation-level warning --nofail
```

- Use `--write-output` with structured formats when the output should become an artifact.
- Use `--annotation-level notice|warning|failure|error` with GitHub annotation formats.
- Use `--nofail` for report-only rollout. It does not mean lint passed.

## Triage Grouping

When summarizing output, separate:

- Templating failures: SQL could not be rendered reliably.
- Parsing failures: rendered SQL could not be parsed with the current dialect.
- Rule violations: SQL parsed and lint rules fired.

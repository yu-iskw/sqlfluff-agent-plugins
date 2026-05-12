# SQLFluff Fix Safety

## Preferred Order

1. Run `sqlfluff lint --format json --nofail` on the target scope.
2. Use `sqlfluff format` for stable formatting-only work.
3. Use `sqlfluff fix --fixed-suffix ...` when reviewable fixed copies are needed.
4. Use `sqlfluff fix --rules ...` for targeted in-place fixes.
5. Use `--FIX-EVEN-UNPARSABLE` only with explicit user approval.

## Safer Commands

```bash
sqlfluff format models/staging/orders.sql --disable-progress-bar
sqlfluff lint models/staging --format json --nofail --disable-progress-bar
sqlfluff fix models/staging --fixed-suffix .fixed --rules LT01,LT02
sqlfluff fix models/staging --rules LT01,LT02,LT12 --disable-progress-bar
```

## Risk Notes

- `format` is usually safer than broad `fix` because it applies a known stable subset of rules.
- `fix --check` asks for terminal input. Avoid it in unattended agent or CI runs.
- `--fixed-suffix` writes fixed copies instead of replacing original paths.
- `--FIX-EVEN-UNPARSABLE` can modify files with templating or parse errors. Treat it as a last resort.

## After Fixing

Always re-run lint on the same scope:

```bash
sqlfluff lint models/staging --format json --nofail --disable-progress-bar
```

Report remaining violations honestly, especially parse and templating errors that prevented safe fixes.

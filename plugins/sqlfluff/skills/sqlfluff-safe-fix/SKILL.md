---
name: sqlfluff-safe-fix
description: Use when formatting or fixing SQLFluff violations safely in SQL or dbt-style projects while minimizing risky rewrites.
compatibility: Requires sqlfluff on PATH.
---

# SQLFluff Safe Fix

## Purpose

Apply SQLFluff formatting or fixes with a conservative safety ladder. Use this when the user asks to format SQL, fix SQLFluff violations, or clean up a limited set of files without changing semantics.

## Workflow

1. **Start with scope and status**
   - Identify the exact files or directories to change.
   - Check for unrelated user edits before changing files.
   - Prefer narrow paths over project-wide fixing.

2. **Prefer stable formatting**
   - Use `sqlfluff format` for formatting-first requests.
   - `format` applies a known subset of stable fix rules and ignores enabled rules while respecting rule exclusions.

3. **Preview broader fixes non-interactively**
   - Use `sqlfluff lint --format json --nofail` to understand violations before fixing.
   - Use `--fixed-suffix` when the user needs reviewable fixed copies instead of in-place edits.
   - Avoid `sqlfluff fix --check` in unattended agent or CI runs because it prompts for terminal input.

4. **Apply scoped fixes**
   - Use `--rules` to fix a targeted rule set.
   - Use `--exclude-rules` to avoid known risky rules.
   - Use `--fixed-suffix` if the user wants generated fixed copies instead of in-place edits.

5. **Avoid unsafe repair**
   - Never use `--FIX-EVEN-UNPARSABLE` unless the user explicitly approves it for the specific files.
   - Explain that this can modify files with templating or parse errors and should be treated as a last resort.

6. **Verify after changes**
   - Re-run lint on the same scope.
   - Report remaining templating, parse, or lint failures.

## Command Patterns

```bash
sqlfluff format models/staging/orders.sql --disable-progress-bar
sqlfluff lint models/staging --format json --nofail --disable-progress-bar
sqlfluff fix models/staging --fixed-suffix .fixed --rules LT01,LT02
sqlfluff fix models/staging --rules LT01,LT02,LT12 --disable-progress-bar
```

## Safety Summary

Prefer `format` on a narrow scope, preview violations with non-interactive lint output, use fixed-copy output when review is needed, and reserve `--FIX-EVEN-UNPARSABLE` for explicit user-approved last-resort repairs. See `references/fix-safety.md` for the full ladder.

## Output

Return:

- Files or directories targeted.
- Which safety level was used.
- Commands run.
- Remaining failures.
- Any files skipped because they were unparseable or unsafe to fix.

## Progressive Disclosure

- Fix safety details: `references/fix-safety.md`

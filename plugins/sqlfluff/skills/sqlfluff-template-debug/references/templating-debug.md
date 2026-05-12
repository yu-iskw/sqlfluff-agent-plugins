# SQLFluff Templating Debug

## Render First

Use `render` when linting fails before rules can run:

```bash
sqlfluff render models/marts/orders.sql --dialect snowflake --templater jinja
```

If the content comes from stdin, preserve path-based config loading:

```bash
sqlfluff render - --stdin-filename models/marts/orders.sql --dialect snowflake --templater jinja
```

## Focus Logs

```bash
sqlfluff lint models/marts/orders.sql --logger templater -vv --nofail
```

Use higher verbosity only when needed. Prefer `--logger templater` so parser and linter logs do not obscure the rendering problem.

## Jinja Library Path

```bash
sqlfluff render models/marts/orders.sql --library-path path/to/sqlfluff_libs
sqlfluff render models/marts/orders.sql --library-path none
```

`--library-path none` disables library loading and is useful only for diagnosis or intentionally isolating a render problem.

## dbt-Style Projects

Check whether SQLFluff supports the needed templater. If a project expects dbt templating but SQLFluff only accepts `raw`, `jinja`, `python`, or `placeholder`, report an environment/setup gap.

## Temporary Ignores

`--ignore templating` can help collect rollout data, but it does not fix templating. Label it as temporary report-only behavior.

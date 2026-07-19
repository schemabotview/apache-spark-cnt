## Common DataFrame operations

The day-to-day toolkit, in five families:

- **Select / project** — `select`, `selectExpr`.
- **Add / rename / drop** — `withColumn`, `withColumnRenamed`, `drop`.
- **Filter** — `filter` is the same as `where`; combine conditions with `&`, `|`, `~`, and **parenthesize each condition** — Python's operator precedence will mislead you otherwise.
- **Nulls** — `isNull`, `isNotNull`, `dropna`, `fillna`, `coalesce(...)`.
- **Cast / sort / dedupe** — `.cast()`, `orderBy(desc(...))`, and `distinct()` versus `dropDuplicates(["col"])`.

These are all *transformations* — lazy, chainable, and optimized together by Catalyst when an action fires.

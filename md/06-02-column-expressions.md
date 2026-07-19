## Column expressions — string, date, array functions

The bulk of the library is vectorized, JVM-resident functions over three column types:

- **String** — `upper` / `lower`, `trim`, `length`, `substring`, `concat_ws(sep, ...)`, `split(col, pattern)`, `regexp_replace`, `regexp_extract(col, pattern, group)`, `lpad` / `rpad`.
- **Date & timestamp** — extractors (`year`, `month`, `dayofweek`, `hour`), `date_format`, `to_date` / `to_timestamp`, `datediff` / `months_between`, `date_add` / `date_sub`, `current_date`, `trunc` / `date_trunc`.
- **Array** — `array(...)`, `array_contains`, `size`, the set ops (`array_distinct` / `union` / `intersect` / `except`), `flatten`, and `explode` — one row per element (`explode_outer` keeps a null array as a single null row).

The theme: reach for a built-in before a UDF — it stays in the JVM and gets optimized.

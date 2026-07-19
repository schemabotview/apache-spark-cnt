## The function library — `pyspark.sql.functions`

Most data work runs through `pyspark.sql.functions` — Spark's library of column-level functions for strings, dates, arrays, conditionals, and aggregations. A few rules:

- **Import explicitly** — `from pyspark.sql.functions import col, when, sum as _sum` — or alias the module: `import pyspark.sql.functions as F`.
- **JVM functions beat Python UDFs.** Every function here runs inside the Spark JVM with Tungsten codegen. A Python UDF needs a serialization round-trip per row through a Python worker — orders of magnitude slower.
- **Watch for builtin collisions.** `sum`, `min`, `max`, `round`, and `filter` all clash with Python builtins — import them with an alias (`sum as _sum`) so a stray call doesn't quietly hit the Python builtin.

Module 04 already covered `select`, `filter`, `withColumn`, casting, sorting, and null handling. This module builds the column *expressions* on top.

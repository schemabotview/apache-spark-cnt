## Python UDFs — `@udf` and `spark.udf.register`

When no built-in can express your logic, fall back to a **Python UDF**. Two registration paths:

- **`@udf(returnType)`** — a decorator for the DataFrame API; use the result as a regular column.
- **`spark.udf.register(name, func, returnType)`** — register a name callable from `spark.sql(...)`.

The cost is real: every row crosses the **JVM ↔ Python boundary twice** — out to a Python worker, back with the result — making a UDF **10–100× slower** than the equivalent built-in on large data.

And a Python UDF is a **Catalyst black box** — the optimizer can't see inside it, so: **no predicate pushdown** across it (the whole column is read first), **no column pruning**, **no null safety** (a built-in returns `null` on `null`; a naive UDF gets `None` and may crash), and **no plan-time type checking**. Always exhaust built-ins, higher-order functions, and Pandas UDFs first.

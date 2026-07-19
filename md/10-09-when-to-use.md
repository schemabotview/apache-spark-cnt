## When to use it vs the Spark DataFrame API

A simple performance rule of thumb decides it:

- **Fast** — if a pandas-on-Spark op maps to a *single* Spark DataFrame operation (`filter`, `groupBy`, `join`, `withColumn`), it compiles to the same plan you'd write by hand. No penalty.
- **Slow** — if it forces a per-row Python callback without a type hint, or materializes the whole frame on the driver, you pay for it. At that scale you're better off in the native DataFrame API.

So the honest guidance: reach for **pandas-on-Spark** to scale an existing pandas codebase, or when a team already thinks in pandas — you keep the familiar syntax and get Spark's distribution nearly for free. Drop to the **native DataFrame API** for new pipelines, for the full feature set, and whenever a step would otherwise trigger a driver collect or a per-row callback. You can flip between them for free with `pandas_api()` and `to_spark()`, so it's never all-or-nothing.

And that closes the course: the whole map — the cluster and execution model, RDDs, DataFrames, I/O, transformations, SQL, streaming, and tuning — is **one engine, wearing whichever surface fits the job**.

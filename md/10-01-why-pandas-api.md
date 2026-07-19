## Why this API exists

Pandas is the lingua franca of single-machine data work — but it caps out at one machine's memory. Rewriting a pandas pipeline into the Spark DataFrame API is mechanical but tedious, and the result looks nothing like the original.

**`pyspark.pandas`** (formerly the **Koalas** project, merged into PySpark in 3.2) gives you the *pandas API* on top of a *Spark backend*. The same `psdf.groupby(...).mean()` that ran on a laptop now runs on a cluster — distributed, lazy, and through Catalyst.

Two facts to anchor on:

- **The surface is pandas; the engine is Spark.** Every `ps.DataFrame` is a thin wrapper around a `pyspark.sql.DataFrame` plus an index. Operations compile to the same Spark plans you'd write by hand.
- **The transformation / action split still applies.** Even though the code reads like eager pandas, most ops are lazy — a line that *looks* like a result (`psdf['new_col'] = ...`) does no work until an action fires.

Import convention: `import pyspark.pandas as ps` — `ps` mirrors pandas' `pd`.

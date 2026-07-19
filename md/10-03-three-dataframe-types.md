## Three DataFrame types — keep them straight

The same three-way distinction from module 04, because it's the axis everything here turns on:

- **pandas DataFrame** (`pandas.DataFrame`) — **driver memory only**; final-mile analysis, plotting, small fixtures.
- **Pandas-on-Spark DataFrame** (`pyspark.pandas.DataFrame`) — **distributed**; pandas syntax at Spark scale.
- **Spark DataFrame** (`pyspark.sql.DataFrame`) — **distributed**; the native API, best performance and most features.

Only the first lives on the driver. The other two are distributed over the **same engine** — Pandas-on-Spark simply adds a **pandas-style index** on top of a Spark DataFrame. That added index is the source of the gotchas later in this module.

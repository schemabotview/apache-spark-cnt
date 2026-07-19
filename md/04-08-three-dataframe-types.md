## Three DataFrame types — keep them straight

Three different things all called "DataFrame" — keep them straight:

- **pandas DataFrame** (`pandas.DataFrame`) — lives entirely in **driver memory**. For final-mile analysis, plotting, and small fixtures.
- **Pandas-on-Spark DataFrame** (`pyspark.pandas.DataFrame`) — **distributed** across executors. When you want pandas *syntax* at Spark *scale*.
- **Spark DataFrame** (`pyspark.sql.DataFrame`) — **distributed**; the native API, best performance and the most features.

The first is single-machine; the other two are distributed over the same engine. Pandas-on-Spark simply adds a pandas-style **index** on top of a Spark DataFrame — which is where the next two sections' gotchas come from.

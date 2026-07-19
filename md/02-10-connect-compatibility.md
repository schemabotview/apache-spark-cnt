## What works — and what doesn't — in Connect mode

The rule follows directly from the design: **anything that describes a serializable *plan* works; anything that needs direct JVM access from the client does not.**

Supported — the DataFrame and SQL surface:

- `DataFrame` / `Dataset` operations, `spark.sql(...)`, and `spark.read` / `df.write`
- Built-in functions, plus regular and pandas UDFs (their code is shipped to the server and run there)
- Structured Streaming and the pandas API on Spark (both 3.5+)

Not supported — anything reaching for the JVM:

- `spark.sparkContext` and the whole **RDD API** (`parallelize`, `rdd.map`) — there is no `SparkContext` on the client
- Internal Py4J handles (`spark._jvm`, `spark._jsc`)
- Injecting JARs from the client — they must be on the server's classpath

One trap worth naming: the two builders look identical but resolve to different classes — `pyspark.sql.session.SparkSession` (classic) versus `pyspark.sql.connect.session.SparkSession` (Connect). If `SPARK_REMOTE` is set, `getOrCreate()` can hand you a Connect session even when your code looks classic. **The exam takeaway:** if a question mentions `SparkContext`, `RDD`, or client-side JARs, it's incompatible with Connect; if it sticks to DataFrame, SQL, and UDFs, it works.

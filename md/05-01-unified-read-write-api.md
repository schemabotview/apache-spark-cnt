## The unified read/write API

Spark reads and writes through a single fluent API. To read: `spark.read.format("parquet").option(...).load("path/")`. To write: `df.write.format("parquet").mode("overwrite").save("path/")`. The short methods — `.csv()`, `.json()`, `.parquet()`, `.orc()` — are just aliases for the longer `.format(...).load(...)` form. Same plumbing, less typing.

Four formats matter for everyday Spark work:

- **CSV** — schema from a header row or explicit; human-readable, slow, text.
- **JSON** — inferred or explicit; good for semi-structured payloads and API dumps.
- **Parquet** — schema embedded in the file footer; columnar, the default for analytics.
- **ORC** — schema in the footer; columnar, common in Hadoop pipelines.

The reader and writer are symmetric — nearly everything you learn about one applies to the other.

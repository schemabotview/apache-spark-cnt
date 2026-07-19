## Your first SparkSession

Everything you do in Spark starts from a **SparkSession** — your handle to the engine for reading data, registering views, and running queries.

Locally you need Java 17 or newer installed, then `pip install pyspark==3.5.3`. The pattern is the same every time: build a session, name the app, point it at a master, and call `getOrCreate()`.

```python
from pyspark.sql import SparkSession

spark = (
    SparkSession.builder
        .appName("spark-course")
        .master("local[*]")
        .getOrCreate()
)
```

**One key gotcha — `getOrCreate()` is sticky.** If a SparkSession already exists in your kernel — from an earlier cell, a previous notebook, anywhere — the builder returns *that* session and silently ignores the new config you just set. To pick up new configuration in a running notebook, call `spark.stop()` first, then build again.

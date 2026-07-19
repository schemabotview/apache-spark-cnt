## Creating RDDs & SparkContext

RDDs predate `SparkSession` and have their own entry point: **`SparkContext`** (`sc` by convention). A `SparkSession` owns one — grab it with `spark.sparkContext`. The mental model: `spark` is the high-level door (DataFrames, SQL); `sc` is the low-level door (RDDs, broadcast variables, accumulators). Same JVM, same cluster, two APIs.

An RDD comes from one of four places — a driver collection, a file, another RDD via transformation, or a DataFrame:

- **`sc.parallelize(seq)`** — distribute a Python list (already in the driver) across executors. Great for tests and demos.
- **`sc.range(start, end)`** — an RDD of longs without materializing a Python list on the driver.
- **`sc.textFile(path)`** — read a text file (or directory) into an RDD where each element is one line.
- **`sc.wholeTextFiles(path)`** — for directories of many small files; each element is a `(filename, content)` pair.
- **`df.rdd`** — drop from a DataFrame to its underlying RDD of `Row` objects for low-level control.

In real pipelines most RDDs are born from transformations on other RDDs, not explicit creation calls.

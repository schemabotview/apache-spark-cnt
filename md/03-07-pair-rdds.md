## Pair RDDs — the (key, value) view

A **Pair RDD** is simply an RDD whose every element is a `(key, value)` tuple. That shape unlocks a family of grouping and aggregation operations that mirror SQL's `GROUP BY`.

- **`reduceByKey(f)`** — combine values per key using `f`; combines **locally before shuffling**.
- **`groupByKey()`** — group all values per key into an iterable; shuffles every value.
- **`mapValues(f)`** / **`flatMapValues(f)`** — transform values, keep keys (narrow).
- **`keys()`** / **`values()`** — project to one side.
- **`sortByKey()`** — sort by key (shuffles).
- **`join(rdd2)`** — inner join on the key.

To build a Pair RDD, `map` your records into `(key, value)` tuples — pick the field you want to group on as the key. The next section digs into the one comparison the exam loves: `reduceByKey` versus `groupByKey`.

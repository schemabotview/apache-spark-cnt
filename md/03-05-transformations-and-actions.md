## Transformations and actions — RDD edition

RDDs follow the same lazy model as DataFrames — two categories.

**Transformations** build the recipe and are lazy (each returns a new RDD):

- `map(f)` — one in, one out
- `flatMap(f)` — one in, zero-or-more out, then flatten
- `filter(f)` — keep elements where `f` is truthy
- `distinct()` — drop duplicates (causes a shuffle)
- `union(rdd2)` — concatenate two RDDs
- `repartition(n)` / `coalesce(n)` — change the partition count

**Actions** trigger execution against the recipe:

- `count()` — number of elements
- `take(n)` / `first()` — the first `n` / the first element
- `collect()` — all elements as a Python list (into driver memory — careful on big data)
- `reduce(f)` — pairwise aggregate
- `saveAsTextFile(path)` — write one file per partition

The rule is unchanged from module 02: nothing runs until an action fires.

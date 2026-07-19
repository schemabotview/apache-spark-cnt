## `groupBy` + `agg`

`groupBy(...)` partitions rows by one or more keys; `agg(...)` applies one or more aggregate functions to each group. Both are lazy — execution waits for an action.

Inside `agg` you use the aggregate functions from the library — `count`, `countDistinct`, `sum`, `avg`, `min`, `max`, `collect_list`, `collect_set` — and you can alias each result: `agg(_sum("amount").alias("total"))`.

Under the hood, `groupBy` triggers a **shuffle** (it's a wide transformation), and Catalyst does a **map-side partial aggregate** first — the same map-side-combine win you saw with `reduceByKey` — which is one more reason to prefer it over hand-rolled RDD grouping.

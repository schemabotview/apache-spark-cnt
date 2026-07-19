## Hello Spark — a multi-step query

With a session in hand, here is a first end-to-end query — read a file, transform it, and pull back a result:

```python
df = spark.read.parquet("transactions.parquet")

result = (
    df.filter(df.amount > 100)
      .groupBy("category")
      .count()
)

result.show()
```

The important thing to notice is *when* work actually happens. The `read`, `filter`, and `groupBy` lines build up a plan and run **nothing** — they are **transformations**, recorded lazily. Only `show()` — an **action** — forces Spark to execute: it plans the job, distributes the tasks, and brings rows back to the driver.

That split is the whole mental model. You describe a pipeline of transformations, and a single action at the end triggers all of it at once. Add or reorder transformations freely; nothing runs until the next action. We unpack exactly why in the next module, on the execution model.

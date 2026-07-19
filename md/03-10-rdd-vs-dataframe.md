## RDD vs DataFrame — when to actually reach for each

The two APIs sit over the same engine, but they buy you very different things:

- **API style** — RDD is functional and row-by-row; DataFrame is declarative and column-based.
- **Optimizer** — RDD has none (your code *is* the plan); DataFrame runs through **Catalyst**.
- **Codegen** — RDD leans on Python lambdas and JVM round-trips; DataFrame gets **Tungsten** whole-stage codegen.
- **Schema** — RDD has none; DataFrame requires one.
- **Performance** — for structured data, DataFrame is faster, often by a lot.

The honest guideline for the rest of this curriculum: **prefer DataFrames.** Drop to RDDs only when you need something the DataFrame API genuinely cannot express — a custom `mapPartitions` pattern, a non-tabular input, or a closure that doesn't fit Spark SQL. This is the RDD-vs-DataFrame lane of the same map you'll spend the next modules inside.

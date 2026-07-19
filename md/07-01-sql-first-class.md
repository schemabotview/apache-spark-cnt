## SQL is a first-class citizen

`spark.sql(...)` takes any ANSI SQL string and routes it through the **same Catalyst optimizer** as the DataFrame API. The two paths produce **identical execution plans** — so pick whichever form reads better, and mix them freely in one application.

One setting worth knowing: **ANSI SQL mode**. Since Spark 3.0 there's a stricter semantics — integer overflow throws instead of silently wrapping, casts are stricter, and divide-by-zero raises. It's off by default (for backward compatibility) but recommended for new pipelines: `spark.conf.set("spark.sql.ansi.enabled", "true")`.

The whole module leans on one idea: SQL isn't a second-class bolt-on — it's the same engine wearing a different syntax.

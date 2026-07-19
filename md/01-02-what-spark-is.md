## What Spark actually is

Apache Spark is a **distributed compute engine**. You write code as if you have one giant table; Spark slices that data into pieces called **partitions**, assigns each partition to a machine — telling it exactly which file and byte range to read — runs your code on every partition in parallel, and brings the results back.

The mental shift from pandas is the important part. Pandas runs your code in one process, against memory it owns directly. Spark runs a *plan* you describe across many processes on many machines, and moves data between them for you. You are no longer calling functions that execute immediately in local memory; you are describing work that Spark will schedule and distribute.

That distinction — one process on owned memory versus a distributed plan — is what every later idea in this course builds on.

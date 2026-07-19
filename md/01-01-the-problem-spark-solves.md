## The problem Spark solves

Imagine a CSV with two hundred gigabytes of bank transactions and a laptop with sixteen gigabytes of RAM. Pandas crashes the moment you try to load it. You could split the file into chunks and loop over them — but the instant you do, you own a small distributed system: which chunks finished, which failed, and how do you run a group-by that spans the *whole* file rather than one chunk at a time?

Spark exists because this pattern — **data too big for one machine, work that has to be coordinated across many** — keeps reappearing once you get past a few hundred gigabytes. Instead of hand-writing your own chunking, retry, and coordination layer, Spark hands you one engine that does all of it, behind a familiar table-shaped API.

That is the whole pitch: you describe the computation as if it were one giant table, and Spark figures out how to split the data, spread the work across a cluster, and bring the answer back.

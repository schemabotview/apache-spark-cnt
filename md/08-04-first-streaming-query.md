## Your first streaming query — rate source → memory sink

The simplest end-to-end streaming demo needs no external system:

- The **rate source** generates rows at a fixed rate with two columns, `timestamp` and `value`.
- The **memory sink** collects results into an in-memory temp view you can query with SQL.

```python
q = (spark.readStream.format("rate").load()
     .writeStream
     .format("memory")            # sink type
     .queryName("counts")         # register as a SQL view
     .outputMode("append")        # emit new rows only
     .start())                    # returns a StreamingQuery handle
q.awaitTermination(5)             # let it run 5 seconds
q.stop()
```

`writeStream` chains the format, a query name, the output mode, and `start()`. The returned handle is how you observe and stop the query — the topic of the sections ahead.

## Schemas — `StructType` vs DDL string

There are two ways to define a schema, and both produce the exact same result — pick whichever reads better in your code:

- **Programmatic** — `StructType([StructField("amount", DecimalType(18, 2)), ...])`. Verbose and explicit; easy to build up from code.
- **DDL string** — `"transaction_id STRING, amount DECIMAL(18,2), ts TIMESTAMP"`. Compact, SQL-flavored, easy to paste.

**Always prefer an explicit schema in production.** Schema *inference* works, but it triggers a full scan of the data just to decide the types — slow on large datasets — and it can guess wrong, like reading a sparse `is_flagged` column as a string when you wanted a boolean.

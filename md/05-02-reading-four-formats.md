## Reading the four formats

The read side is one method with a format and options. Each format has a characteristic set of options:

- **CSV** — `.option("header", True)` to use the first row as names, `.option("sep", ";")` for the delimiter, `.option("inferSchema", True)` to guess types (or pass a schema — preferred).
- **JSON** — one JSON object per line by default; `.option("multiLine", True)` for pretty-printed arrays.
- **Parquet** — no options needed; the schema and column stats travel inside the file.
- **ORC** — same story as Parquet; columnar and self-describing.

Rule of thumb: for CSV and JSON, always think about the schema; for Parquet and ORC, the file already knows. The next section is exactly that schema decision.

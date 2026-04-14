# SQL Projects

Build your own SQL projects here. Each project should live in its own subfolder.

## Suggested Project Ideas

### Beginner
- **Personal Finance Tracker** — Design a schema to track income, expenses, and categories. Write queries to summarize spending by month.
- **Book / Movie Collection** — Create a database of books or movies you own/want to read/watch. Practice filtering and sorting.
- **Sports Stats Explorer** — Download a public sports dataset (e.g., [Kaggle](https://www.kaggle.com/)) and answer questions with SQL.

### Intermediate
- **E-Commerce Analysis** — Use the Northwind or Chinook sample database to write business-intelligence queries (top customers, revenue by product, etc.).
- **NYC Taxi Data Dashboard** — Aggregate and explore the [NYC TLC dataset](https://www.nyc.gov/site/tlc/about/tlc-trip-record-data.page) with window functions and CTEs.
- **GitHub Activity Log** — Export your own GitHub activity and analyze it in SQLite or DuckDB.

### Advanced
- **Slowly Changing Dimensions (SCD)** — Implement SCD Type 1 and 2 patterns in a warehouse schema.
- **Recursive CTEs** — Model a hierarchical org chart or folder structure and query it with recursive queries.
- **Index Optimization Lab** — Take a slow query, profile it with EXPLAIN ANALYZE, and optimize it with proper indexes.

## Project Folder Template

```
projects/
└── my-project-name/
    ├── README.md        ← What the project is, what questions you answer
    ├── schema.sql       ← CREATE TABLE statements
    ├── seed.sql         ← INSERT sample data
    └── queries.sql      ← Your analysis queries with comments
```

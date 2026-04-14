# SQL

Use this folder to store your SQL practice queries, notes, and exercises.

## Suggested Learning Path

| # | Topic | Free Resource |
|---|-------|---------------|
| 1 | SELECT, WHERE, ORDER BY, LIMIT | [Select Star SQL](https://selectstarsql.com/) — free interactive book |
| 2 | GROUP BY, HAVING, aggregate functions | [Mode SQL Tutorial](https://mode.com/sql-tutorial/) |
| 3 | INNER JOIN, LEFT JOIN, FULL OUTER JOIN | [SQLZoo](https://sqlzoo.net/) |
| 4 | Subqueries and CTEs (WITH) | [pgexercises.com](https://pgexercises.com/) |
| 5 | Window functions (RANK, ROW_NUMBER, LAG) | [LeetCode SQL](https://leetcode.com/problemset/database/) |
| 6 | INSERT, UPDATE, DELETE | Any local SQLite or DuckDB file |
| 7 | Indexes and query performance | [Use The Index, Luke](https://use-the-index-luke.com/) |

## Practice Datasets (clone into `../resources/`)

| Dataset | Link | Notes |
|---------|------|-------|
| Chinook | [github.com/lerocha/chinook-database](https://github.com/lerocha/chinook-database) | Music store — SQLite & PostgreSQL |
| Northwind | [github.com/pthom/northwind_psql](https://github.com/pthom/northwind_psql) | Classic ERP schema — PostgreSQL |
| DVD Rental | [postgresqltutorial.com](https://www.postgresqltutorial.com/postgresql-getting-started/postgresql-sample-database/) | PostgreSQL tutorial database |

## Running SQL in Python (DuckDB)

```python
import duckdb

# Query a CSV file directly — no database setup needed
con = duckdb.connect()
df = con.execute("SELECT * FROM '../resources/my-data.csv' LIMIT 10").df()
print(df)
```

## File Naming Convention

Save your queries as `.sql` files with descriptive names, e.g.:
- `01-basics-select-where.sql`
- `03-joins-practice.sql`
- `05-window-functions-rank.sql`

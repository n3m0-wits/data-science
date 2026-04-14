# Data Science Learning Repository

A focused learning repo for SQL and Python. Clone it into [Positron](https://positron.posit.co/) (or any editor) and follow the workflow below to start building skills immediately.

## Directory Structure

```
data-science/
├── resources/    # Clone or download external tutorials, books, and repos here
├── sql/          # Your SQL notes, exercises, and practice queries
├── python/       # Your Python scripts, notebooks, and projects
└── requirements.txt
```

---

## Step-by-Step Learning Workflow

### Step 1 — Set up your environment

1. **Clone this repo** and open it in [Positron](https://positron.posit.co/).
2. Create a virtual environment and install the starter Python packages:
   ```bash
   python -m venv .venv
   source .venv/bin/activate       # Windows: .venv\Scripts\activate
   pip install -r requirements.txt
   ```
3. In Positron, select the `.venv` interpreter so notebooks and scripts use it automatically.

### Step 2 — Download learning materials into `resources/`

The `resources/` folder is your single place for anything you clone or download from the internet — tutorials, sample databases, example repos, cheat sheets.

```bash
cd resources/
git clone https://github.com/lerocha/chinook-database   # SQL sample database
git clone https://github.com/jakevdp/PythonDataScienceHandbook  # free Python DS book
```

> **Convention:** keep each external source in its own sub-folder inside `resources/` so it's easy to update or delete later. Do not edit files inside `resources/` — treat it as read-only reference material.

### Step 3 — Learn SQL (start here if you're new to databases)

Open [`sql/README.md`](sql/README.md) for the full learning path. The recommended order:

| # | Topic | Where to practice |
|---|-------|-------------------|
| 1 | SELECT, WHERE, ORDER BY | [SQLZoo](https://sqlzoo.net/) / [Select Star SQL](https://selectstarsql.com/) |
| 2 | GROUP BY and aggregations | [Mode SQL Tutorial](https://mode.com/sql-tutorial/) |
| 3 | JOINs | [pgexercises.com](https://pgexercises.com/) |
| 4 | Subqueries and CTEs | LeetCode SQL / your own queries in `sql/` |
| 5 | Window functions | [LeetCode SQL](https://leetcode.com/problemset/database/) |
| 6 | DuckDB in Python | Run SQL on CSV files with `duckdb` in a notebook |

Save your practice queries and notes as `.sql` files directly in the `sql/` folder.

### Step 4 — Learn Python for data

Open [`python/README.md`](python/README.md) for the full learning path. The recommended order:

| # | Topic | Resource |
|---|-------|----------|
| 1 | Python basics | [Python for Everybody](https://www.py4e.com/) |
| 2 | pandas and numpy | [Python Data Science Handbook](https://jakevdp.github.io/PythonDataScienceHandbook/) (free) |
| 3 | Visualisation | [Kaggle Learn — Data Visualization](https://www.kaggle.com/learn/data-visualization) |
| 4 | Data cleaning | Real datasets from `resources/` |
| 5 | scikit-learn basics | [Kaggle Learn — Intro to ML](https://www.kaggle.com/learn/intro-to-machine-learning) |

Save your scripts and Jupyter notebooks (`.ipynb`) directly in the `python/` folder.

### Step 5 — Work on a project

Pick a dataset from `resources/` and apply both SQL and Python skills:

1. Use DuckDB or SQLite to explore the data with SQL queries (save in `sql/`).
2. Load the same data into a pandas DataFrame and clean it (save in `python/`).
3. Build a simple visualisation or model.
4. Commit your work regularly so you can track progress over time.

---

## Quick Links

- [SQL Learning Path →](sql/README.md)
- [Python Learning Path →](python/README.md)
- [Resources folder →](resources/README.md)

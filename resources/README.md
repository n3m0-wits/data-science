# Resources

This is the **single folder for all external materials** — tutorials, sample databases, books, and example repos that you clone or download from the internet.

## How to use this folder

1. Clone or download any external resource directly into a sub-folder here.
2. Treat everything inside as **read-only reference material** — don't edit it.
3. Reference it from your `sql/` or `python/` work as needed.

## Suggested resources to add

### SQL datasets & tools

```bash
# Chinook — music store database (SQLite + PostgreSQL)
git clone https://github.com/lerocha/chinook-database

# Northwind — classic ERP sample database (PostgreSQL)
git clone https://github.com/pthom/northwind_psql
```

### Python / Data Science books & examples

```bash
# Python Data Science Handbook (free book with Jupyter notebooks)
git clone https://github.com/jakevdp/PythonDataScienceHandbook

# Hands-On Machine Learning with Scikit-Learn (notebook examples)
git clone https://github.com/ageron/handson-ml3
```

## Naming convention

Use the repo or source name as the sub-folder name so it's easy to identify, e.g.:

```
resources/
├── chinook-database/
├── northwind_psql/
├── PythonDataScienceHandbook/
└── handson-ml3/
```

> **Note:** This folder is excluded from tracking large files. If you download large datasets (CSV, Parquet), add them here and consider adding them to `.gitignore` if you don't want to commit them.

# Python

Use this folder to store your Python scripts, Jupyter notebooks (`.ipynb`), and practice exercises.

## Suggested Learning Path

| # | Topic | Free Resource |
|---|-------|---------------|
| 1 | Python basics (types, loops, functions) | [Python for Everybody](https://www.py4e.com/) |
| 2 | File I/O and standard library (`os`, `pathlib`, `csv`, `json`) | [Automate the Boring Stuff](https://automatetheboringstuff.com/) (free online) |
| 3 | pandas and numpy | [Python Data Science Handbook](https://jakevdp.github.io/PythonDataScienceHandbook/) (free) |
| 4 | Data visualisation (`matplotlib`, `seaborn`, `plotly`) | [Kaggle Learn — Data Visualization](https://www.kaggle.com/learn/data-visualization) |
| 5 | Data cleaning (missing values, type coercion, strings) | Practice with datasets in `../resources/` |
| 6 | Working with APIs (`requests`, JSON) | [Real Python — Requests tutorial](https://realpython.com/python-requests/) |
| 7 | Statistical analysis (`scipy.stats`, `statsmodels`) | [Think Stats](https://greenteapress.com/thinkstats2/html/) (free online) |
| 8 | Machine learning basics (`scikit-learn`) | [Kaggle Learn — Intro to ML](https://www.kaggle.com/learn/intro-to-machine-learning) |

## Key Libraries (all installed via `requirements.txt`)

| Library | What it does |
|---------|-------------|
| `pandas` | DataFrames — load, clean, reshape, and analyse tabular data |
| `numpy` | Fast numerical arrays and math operations |
| `matplotlib` / `seaborn` | Static charts and statistical plots |
| `plotly` | Interactive charts in notebooks |
| `scikit-learn` | Machine learning: regression, classification, clustering |
| `scipy` | Scientific computing, statistical tests |
| `statsmodels` | Statistical models with detailed summaries and p-values |
| `requests` | HTTP calls to REST APIs |
| `sqlalchemy` | Connect Python to SQL databases (PostgreSQL, SQLite, etc.) |
| `duckdb` | Run fast SQL directly on DataFrames and CSV/Parquet files |
| `shap` | Explain individual predictions from any ML model |

## Starting a Notebook

Open Positron, press `Ctrl+Shift+P` → "New Jupyter Notebook", and save it in this folder. Name files descriptively, e.g.:
- `01-pandas-basics.ipynb`
- `03-data-cleaning-chinook.ipynb`
- `08-sklearn-classification.ipynb`

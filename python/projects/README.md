# Python Projects

Build your own Python projects here. Each project should live in its own subfolder.

## Suggested Project Ideas

### Beginner
- **CSV Data Explorer** — Load a CSV with pandas, compute summary statistics, and produce a few charts. Try it with any dataset from [Kaggle](https://www.kaggle.com/datasets).
- **Weather Data Fetcher** — Use the [Open-Meteo API](https://open-meteo.com/) (free, no key needed) to pull historical weather and plot temperature trends.
- **Personal Budget Analyzer** — Export your bank transactions as CSV and use pandas to categorize and visualize your spending.

### Intermediate
- **Web Scraper** — Use `requests` + `BeautifulSoup` to scrape a public website (e.g., book titles from [books.toscrape.com](http://books.toscrape.com/)) and save results to CSV.
- **Stock Price Dashboard** — Pull stock data with `yfinance` and build an interactive chart with `plotly`.
- **Text Analysis** — Analyze word frequency, sentiment, or readability of a book from [Project Gutenberg](https://www.gutenberg.org/).

### Advanced
- **End-to-End ML Pipeline** — Acquire data → clean → feature engineer → train a `scikit-learn` model → evaluate → export predictions.
- **Automated Report Generator** — Use Python to query a database, build charts, and produce a PDF or HTML report automatically.
- **API Data Pipeline** — Build a small ETL that fetches data from a public API on a schedule, stores it in SQLite, and visualizes trends.

## Project Folder Template

```
projects/
└── my-project-name/
    ├── README.md           ← What the project does and how to run it
    ├── requirements.txt    ← Project-specific dependencies (if different from root)
    ├── data/
    │   └── raw/            ← Original, unmodified data files
    ├── notebooks/          ← Exploratory Jupyter notebooks
    └── src/
        └── main.py         ← Main script(s)
```

## Tips

- Keep raw data in `data/raw/` and never overwrite it — write processed data to `data/processed/`
- Add a `requirements.txt` to each project so others (and future you) can reproduce the environment
- Use relative paths via `pathlib.Path` so scripts work regardless of where the repo is cloned

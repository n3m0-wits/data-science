# Notebooks

This directory holds standalone Jupyter (`.ipynb`) and Quarto (`.qmd`) notebooks for exploration, tutorials, and reproducible reports.

## Why This Folder?

Positron has first-class support for both Jupyter notebooks and Quarto documents. Use this space for:
- **Exploratory analysis** — scratch notebooks while you investigate a dataset
- **Tutorials** — notebooks you work through from courses or books
- **Reports** — polished Quarto documents that combine code, prose, and visualizations

## Suggested Notebook Starters

| Notebook | Description |
|----------|-------------|
| `eda-template.ipynb` | Copy this whenever you start exploring a new dataset |
| `sql-in-python.ipynb` | Practice running SQL inside Python with DuckDB or SQLite |
| `visualization-gallery.ipynb` | A reference notebook of your favorite chart types |
| `ml-pipeline-template.ipynb` | End-to-end template: load → clean → model → evaluate |

## Quarto Tips (in Positron)

Quarto is built into Positron. Create a new `.qmd` file and run cells just like a Jupyter notebook. Render to HTML, PDF, or slides:
```bash
quarto render my-report.qmd
```

Useful Quarto resources:
- [Quarto — Hello Positron](https://quarto.org/docs/get-started/hello/positron.html)
- [Quarto Gallery](https://quarto.org/docs/gallery/) — Examples of what you can build

## Organization Tips

- Prefix with a number for sequential analysis: `01-eda.ipynb`, `02-features.ipynb`, `03-model.ipynb`
- For polished reports, use `.qmd` — it renders beautifully and version-controls cleanly
- Add a markdown cell at the top of every notebook with: title, date, data source, and key findings
- Keep large notebooks split by concern — one for EDA, one for modeling, etc.
- Restart & run all before committing to ensure notebooks are reproducible

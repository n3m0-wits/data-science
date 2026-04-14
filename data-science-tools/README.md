# Data Science Tools

This section covers the broader data science ecosystem — exploratory data analysis, machine learning, data visualization, and workflow tooling.

## Subdirectories

| Folder | Purpose |
|--------|---------|
| [`resources/`](resources/README.md) | Tutorials, cheat sheets, and examples copied from other repos or sources |
| [`projects/`](projects/README.md) | Your own end-to-end data science analyses and experiments |

## Suggested Learning Path

1. **Exploratory Data Analysis (EDA)** — Profiling data, spotting outliers, understanding distributions
2. **Feature Engineering** — Encoding categoricals, scaling, creating new features from existing ones
3. **Machine Learning Fundamentals** — Supervised vs. unsupervised, train/test split, cross-validation
4. **Regression Models** — Linear, ridge, lasso, gradient boosting
5. **Classification Models** — Logistic regression, decision trees, random forests, SVMs
6. **Clustering** — k-means, DBSCAN, hierarchical clustering
7. **Model Evaluation** — Metrics (RMSE, accuracy, F1, AUC-ROC), confusion matrices
8. **Model Deployment Basics** — Saving/loading models with `joblib` or `pickle`, simple REST APIs
9. **Data Pipelines** — `scikit-learn` Pipelines, `dbt` for SQL transforms
10. **Big Data Tools (intro)** — Spark concepts, DuckDB for large local datasets

## Key Tools & Libraries

| Tool / Library | Use Case |
|---------------|----------|
| `scikit-learn` | ML algorithms, preprocessing, model evaluation |
| `xgboost` / `lightgbm` | Gradient boosting (often top performers on tabular data) |
| `statsmodels` | Statistical models and econometrics |
| `imbalanced-learn` | Handling imbalanced classification datasets |
| `shap` | Model explainability (SHAP values) |
| `mlflow` | Experiment tracking and model registry |
| `dbt` | SQL-based data transformation pipelines |
| `great_expectations` | Data quality and validation |
| `Quarto` | Reproducible reports combining code + prose (native in Positron) |
| `DuckDB` | Fast in-process OLAP queries on parquet/CSV files |

## Recommended Free Resources

- [Hands-On Machine Learning (O'Reilly)](https://github.com/ageron/handson-ml3) — The definitive ML book (notebooks free on GitHub)
- [fast.ai Practical Deep Learning](https://course.fast.ai/) — Top-down, practical deep learning course
- [Google's Machine Learning Crash Course](https://developers.google.com/machine-learning/crash-course) — Free, well-structured intro
- [Kaggle Learn — Intro to ML & Feature Engineering](https://www.kaggle.com/learn) — Short hands-on courses
- [Made With ML](https://madewithml.com/) — ML engineering concepts end-to-end
- [StatQuest with Josh Starmer (YouTube)](https://www.youtube.com/@statquest) — Best visual explanations of ML concepts

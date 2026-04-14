# Data Science Tools — Projects

Build your own end-to-end data science projects here. Each project should live in its own subfolder.

## Suggested Project Ideas

### Beginner
- **Titanic Survival Predictor** — The classic Kaggle "Hello World" of ML. Practice cleaning, feature engineering, and classification.
- **House Price Regression** — Predict house prices from the [Ames Housing dataset](https://www.kaggle.com/competitions/house-prices-advanced-regression-techniques). Cover the full ML pipeline.
- **Iris/Penguin Classifier** — Multi-class classification with simple algorithms; great for understanding decision boundaries.

### Intermediate
- **Customer Churn Prediction** — Build a binary classifier on a telco churn dataset; practice handling imbalanced classes.
- **Sales Forecasting** — Time series forecasting with `statsmodels` or `Prophet` on retail sales data.
- **A/B Test Analysis** — Simulate an A/B test, perform hypothesis testing, and write up findings in a Quarto report.

### Advanced
- **End-to-End ML Pipeline with MLflow** — Train multiple models, track experiments with MLflow, select the best model, and log it to the registry.
- **NLP Sentiment Classifier** — Fine-tune or apply a pre-trained model to classify text sentiment (e.g., product reviews).
- **Anomaly Detection System** — Detect anomalies in time series data (e.g., server metrics or financial transactions).

## Project Folder Template

```
projects/
└── my-ds-project/
    ├── README.md               ← Problem statement, approach, findings
    ├── requirements.txt        ← Project-specific dependencies
    ├── data/
    │   ├── raw/                ← Original data (never modify)
    │   └── processed/          ← Cleaned / transformed data
    ├── notebooks/
    │   ├── 01-eda.ipynb        ← Exploratory analysis
    │   ├── 02-features.ipynb   ← Feature engineering
    │   └── 03-model.ipynb      ← Model training and evaluation
    ├── src/
    │   ├── preprocess.py
    │   ├── train.py
    │   └── evaluate.py
    └── models/                 ← Serialized model artifacts
```

## Best Practices

- Always **separate EDA from modeling** — use numbered notebooks to show a clear progression
- Write a `README.md` for every project explaining the problem, your approach, and key findings
- Version your datasets if they change — use a `data/raw/` folder and never overwrite original files
- Track experiments with MLflow or at minimum a `results.csv` noting hyperparameters and metrics

# Data Science & Credit Test — Reference Sheet

---

## Table of Contents

1. [Python / Pandas Quick Reference](#1-python--pandas-quick-reference)
2. [Scikit-Learn Reference](#2-scikit-learn-reference)
3. [SQL Reference](#3-sql-reference)
4. [Credit & Risk Concepts](#4-credit--risk-concepts)
5. [Statistics & Metrics Quick Reference](#5-statistics--metrics-quick-reference)
6. [Quick Links to Documentation](#6-quick-links-to-documentation)

---

## 1. Python / Pandas Quick Reference

### 1.1 Imports You Will Almost Certainly Need

```python
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns
from scipy import stats

from sklearn.model_selection import train_test_split, cross_val_score
from sklearn.preprocessing import StandardScaler, LabelEncoder, OneHotEncoder
from sklearn.metrics import (
    accuracy_score, precision_score, recall_score, f1_score,
    confusion_matrix, classification_report, roc_auc_score, roc_curve,
    mean_squared_error, mean_absolute_error, r2_score
)
from sklearn.linear_model import LogisticRegression, LinearRegression
from sklearn.tree import DecisionTreeClassifier
from sklearn.ensemble import RandomForestClassifier, GradientBoostingClassifier
```

### 1.2 DataFrame Creation & I/O

```python
# Reading data
df = pd.read_csv("file.csv")
df = pd.read_excel("file.xlsx", sheet_name="Sheet1")
df = pd.read_sql("SELECT * FROM table", connection)

# Writing data
df.to_csv("output.csv", index=False)
df.to_excel("output.xlsx", index=False)

# Quick creation
df = pd.DataFrame({
    "col_a": [1, 2, 3],
    "col_b": ["x", "y", "z"]
})
```

### 1.3 Inspection & Summary

```python
df.head(10)                # First 10 rows
df.tail(5)                 # Last 5 rows
df.shape                   # (rows, cols)
df.info()                  # Dtypes, non-null counts, memory
df.describe()              # Count, mean, std, min, quartiles, max
df.describe(include="all") # Include categorical columns too
df.dtypes                  # Column data types
df.columns.tolist()        # Column names as list
df.nunique()               # Unique value counts per column
df["col"].value_counts()   # Frequency table for a column
df["col"].value_counts(normalize=True)  # As proportions
```

### 1.4 Missing Values

```python
df.isnull().sum()              # Count NaNs per column
df.isnull().mean()             # Proportion of NaNs per column
df.dropna()                    # Drop rows with any NaN
df.dropna(subset=["col"])      # Drop rows where 'col' is NaN
df.fillna(0)                   # Fill all NaNs with 0
df["col"].fillna(df["col"].median(), inplace=True)  # Median imputation
df.interpolate(method="linear")  # Linear interpolation

# Forward/backward fill (useful for time series)
df.fillna(method="ffill")
df.fillna(method="bfill")
```

### 1.5 Selection, Filtering, Indexing

```python
# Column selection
df["col"]                      # Single column (Series)
df[["col_a", "col_b"]]        # Multiple columns (DataFrame)

# Row filtering
df[df["age"] > 30]
df[(df["age"] > 30) & (df["score"] < 80)]   # AND
df[(df["age"] > 30) | (df["score"] < 80)]   # OR
df[df["city"].isin(["JHB", "CPT", "DBN"])]
df[~df["city"].isin(["JHB"])]               # NOT in
df[df["name"].str.contains("John", na=False)]

# .loc (label-based) and .iloc (integer position-based)
df.loc[0:5, "col_a":"col_c"]        # Inclusive on both ends
df.iloc[0:5, 0:3]                    # Exclusive on end
df.loc[df["age"] > 30, "name"]       # Conditional with column selection

# .query() — often cleaner for complex filters
df.query("age > 30 and score < 80")
df.query("city in ['JHB', 'CPT']")
```

### 1.6 Creating / Modifying Columns

```python
df["new_col"] = df["a"] + df["b"]
df["ratio"] = df["a"] / df["b"]
df["log_income"] = np.log(df["income"] + 1)    # +1 to avoid log(0)

# Conditional column creation
df["flag"] = np.where(df["score"] > 50, 1, 0)

# Multiple conditions
conditions = [
    df["score"] >= 80,
    df["score"] >= 50,
    df["score"] < 50
]
choices = ["A", "B", "C"]
df["grade"] = np.select(conditions, choices, default="Unknown")

# Apply custom function
df["col_clean"] = df["col"].apply(lambda x: x.strip().lower())

# Binning continuous variables
df["age_bin"] = pd.cut(df["age"], bins=[0, 18, 35, 50, 100],
                       labels=["Youth", "Young Adult", "Middle", "Senior"])
df["quantile_bin"] = pd.qcut(df["income"], q=4, labels=["Q1","Q2","Q3","Q4"])
```

### 1.7 Groupby & Aggregation

```python
# Basic groupby
df.groupby("category")["amount"].sum()
df.groupby("category")["amount"].mean()
df.groupby("category")["amount"].agg(["mean", "median", "std", "count"])

# Multiple columns
df.groupby(["region", "product"])["revenue"].sum().reset_index()

# Named aggregation (clean output)
df.groupby("category").agg(
    total_revenue=("revenue", "sum"),
    avg_revenue=("revenue", "mean"),
    customer_count=("customer_id", "nunique")
).reset_index()

# Transform — returns same-shaped output (useful for group-level normalisation)
df["group_mean"] = df.groupby("category")["amount"].transform("mean")
df["pct_of_group"] = df["amount"] / df.groupby("category")["amount"].transform("sum")
```

### 1.8 Merging & Joining

```python
# Merge (SQL-style join)
pd.merge(df1, df2, on="key")                          # inner join (default)
pd.merge(df1, df2, on="key", how="left")               # left join
pd.merge(df1, df2, on="key", how="right")              # right join
pd.merge(df1, df2, on="key", how="outer")              # full outer join
pd.merge(df1, df2, left_on="id", right_on="cust_id")   # different column names

# Concatenation
pd.concat([df1, df2], axis=0)           # Stack rows (union)
pd.concat([df1, df2], axis=1)           # Side by side (column-bind)
pd.concat([df1, df2], ignore_index=True) # Reset index after stacking
```

### 1.9 Pivot Tables & Reshaping

```python
# Pivot table
pd.pivot_table(df, values="revenue", index="region",
               columns="product", aggfunc="sum", fill_value=0)

# Melt (wide → long)
pd.melt(df, id_vars=["id"], value_vars=["jan", "feb", "mar"],
        var_name="month", value_name="sales")

# Crosstab (frequency table)
pd.crosstab(df["gender"], df["product"], margins=True)
pd.crosstab(df["gender"], df["product"], normalize="index")  # Row percentages
```

### 1.10 Correlation & Quick EDA

```python
# Correlation matrix
df.corr()                          # Pearson (default)
df.corr(method="spearman")         # Spearman rank
df[["a","b","c"]].corr()           # Subset of columns

# Correlation heatmap
sns.heatmap(df.corr(), annot=True, cmap="coolwarm", fmt=".2f")
plt.title("Correlation Matrix")
plt.tight_layout()
plt.show()

# Pairplot
sns.pairplot(df[["age", "income", "score", "target"]], hue="target")

# Distribution of a single variable
sns.histplot(df["income"], kde=True, bins=30)
sns.boxplot(x="category", y="income", data=df)
```

### 1.11 String Operations

```python
df["name"].str.lower()
df["name"].str.upper()
df["name"].str.strip()
df["name"].str.replace("old", "new")
df["name"].str.contains("pattern", case=False)
df["name"].str.split("_", expand=True)        # Split into columns
df["name"].str.len()
df["name"].str.extract(r"(\d+)")              # Regex extract
```

### 1.12 Date / Time Operations

```python
df["date"] = pd.to_datetime(df["date_str"])
df["year"] = df["date"].dt.year
df["month"] = df["date"].dt.month
df["day_of_week"] = df["date"].dt.dayofweek       # 0=Monday
df["day_name"] = df["date"].dt.day_name()
df["quarter"] = df["date"].dt.quarter

# Time differences
df["days_since"] = (pd.Timestamp.now() - df["date"]).dt.days

# Resampling time series
ts = df.set_index("date")
ts.resample("M").mean()       # Monthly average
ts.resample("Q").sum()        # Quarterly sum
```

### 1.13 Sorting & Ranking

```python
df.sort_values("col", ascending=False)
df.sort_values(["col_a", "col_b"], ascending=[True, False])
df["rank"] = df["score"].rank(ascending=False, method="dense")
df.nlargest(10, "revenue")
df.nsmallest(5, "cost")
```

### 1.14 Duplicates

```python
df.duplicated().sum()                        # Count duplicate rows
df.duplicated(subset=["id"]).sum()           # Duplicates on specific columns
df.drop_duplicates()
df.drop_duplicates(subset=["id"], keep="last")
```

### 1.15 Type Conversions

```python
df["col"] = df["col"].astype(int)
df["col"] = df["col"].astype(float)
df["col"] = df["col"].astype(str)
df["col"] = df["col"].astype("category")
pd.to_numeric(df["col"], errors="coerce")    # Invalid → NaN
```

---

## 2. Scikit-Learn Reference

### 2.1 Train/Test Split

```python
X = df.drop("target", axis=1)
y = df["target"]

X_train, X_test, y_train, y_test = train_test_split(
    X, y, test_size=0.2, random_state=42, stratify=y
)
# stratify=y ensures proportional class distribution in both sets
```

### 2.2 Preprocessing

```python
# Scaling (important for distance-based models: KNN, SVM, Logistic)
from sklearn.preprocessing import StandardScaler, MinMaxScaler

scaler = StandardScaler()   # z = (x - mean) / std
X_train_scaled = scaler.fit_transform(X_train)   # fit on train only!
X_test_scaled = scaler.transform(X_test)          # transform test with train params

minmax = MinMaxScaler()     # scales to [0, 1]
X_train_mm = minmax.fit_transform(X_train)
X_test_mm = minmax.transform(X_test)

# Encoding categorical variables
from sklearn.preprocessing import LabelEncoder, OneHotEncoder

# Label Encoding (ordinal or binary categories)
le = LabelEncoder()
df["gender_enc"] = le.fit_transform(df["gender"])

# One-Hot Encoding (nominal categories)
df_encoded = pd.get_dummies(df, columns=["city", "product"], drop_first=True)
# drop_first=True avoids the dummy variable trap (multicollinearity)
```

### 2.3 Common Models — Classification

```python
# Logistic Regression
from sklearn.linear_model import LogisticRegression
model = LogisticRegression(max_iter=1000, random_state=42)
model.fit(X_train, y_train)
y_pred = model.predict(X_test)
y_prob = model.predict_proba(X_test)[:, 1]   # Probability of class 1

# Decision Tree
from sklearn.tree import DecisionTreeClassifier
dt = DecisionTreeClassifier(max_depth=5, min_samples_leaf=10, random_state=42)

# Random Forest
from sklearn.ensemble import RandomForestClassifier
rf = RandomForestClassifier(n_estimators=100, max_depth=10, random_state=42)

# Gradient Boosting
from sklearn.ensemble import GradientBoostingClassifier
gb = GradientBoostingClassifier(n_estimators=200, learning_rate=0.1,
                                 max_depth=3, random_state=42)

# XGBoost (if available)
from xgboost import XGBClassifier
xgb = XGBClassifier(n_estimators=200, learning_rate=0.1,
                     max_depth=3, use_label_encoder=False,
                     eval_metric="logloss", random_state=42)

# K-Nearest Neighbours
from sklearn.neighbors import KNeighborsClassifier
knn = KNeighborsClassifier(n_neighbors=5)

# Support Vector Machine
from sklearn.svm import SVC
svm = SVC(kernel="rbf", probability=True, random_state=42)
```

### 2.4 Common Models — Regression

```python
from sklearn.linear_model import LinearRegression, Ridge, Lasso

lr = LinearRegression()
ridge = Ridge(alpha=1.0)        # L2 regularisation
lasso = Lasso(alpha=0.1)        # L1 regularisation (feature selection)

from sklearn.ensemble import RandomForestRegressor, GradientBoostingRegressor

rfr = RandomForestRegressor(n_estimators=100, random_state=42)
gbr = GradientBoostingRegressor(n_estimators=200, learning_rate=0.1, random_state=42)
```

### 2.5 Evaluation Metrics — Classification

```python
from sklearn.metrics import (
    accuracy_score, precision_score, recall_score, f1_score,
    confusion_matrix, classification_report, roc_auc_score, roc_curve,
    log_loss
)

# Predictions
y_pred = model.predict(X_test)
y_prob = model.predict_proba(X_test)[:, 1]

# Metrics
print("Accuracy: ", accuracy_score(y_test, y_pred))
print("Precision:", precision_score(y_test, y_pred))
print("Recall:   ", recall_score(y_test, y_pred))
print("F1 Score: ", f1_score(y_test, y_pred))
print("AUC-ROC:  ", roc_auc_score(y_test, y_prob))
print("Log Loss: ", log_loss(y_test, y_prob))

# Full classification report
print(classification_report(y_test, y_pred))

# Confusion matrix
cm = confusion_matrix(y_test, y_pred)
sns.heatmap(cm, annot=True, fmt="d", cmap="Blues",
            xticklabels=["Predicted 0","Predicted 1"],
            yticklabels=["Actual 0","Actual 1"])

# Confusion matrix layout:
#                  Predicted 0    Predicted 1
# Actual 0         TN             FP
# Actual 1         FN             TP

# ROC Curve
fpr, tpr, thresholds = roc_curve(y_test, y_prob)
plt.plot(fpr, tpr, label=f"AUC = {roc_auc_score(y_test, y_prob):.3f}")
plt.plot([0, 1], [0, 1], "k--")
plt.xlabel("False Positive Rate")
plt.ylabel("True Positive Rate")
plt.title("ROC Curve")
plt.legend()
plt.show()
```

**Key Metric Definitions:**

| Metric | Formula | When to use |
|--------|---------|-------------|
| Accuracy | (TP+TN) / (TP+TN+FP+FN) | Balanced classes |
| Precision | TP / (TP+FP) | Cost of false positives is high |
| Recall (Sensitivity) | TP / (TP+FN) | Cost of false negatives is high (e.g., credit default) |
| Specificity | TN / (TN+FP) | Correctly identifying negatives |
| F1 Score | 2 × (Precision × Recall) / (Precision + Recall) | Imbalanced classes |
| AUC-ROC | Area under the ROC curve | Overall model discrimination |

### 2.6 Evaluation Metrics — Regression

```python
from sklearn.metrics import mean_squared_error, mean_absolute_error, r2_score

y_pred = model.predict(X_test)

print("MAE: ", mean_absolute_error(y_test, y_pred))
print("MSE: ", mean_squared_error(y_test, y_pred))
print("RMSE:", np.sqrt(mean_squared_error(y_test, y_pred)))
print("R²:  ", r2_score(y_test, y_pred))

# Adjusted R² (manual calc)
n = len(y_test)
p = X_test.shape[1]
r2 = r2_score(y_test, y_pred)
adj_r2 = 1 - (1 - r2) * (n - 1) / (n - p - 1)
```

### 2.7 Cross-Validation

```python
from sklearn.model_selection import cross_val_score, StratifiedKFold

# Quick cross-validation
scores = cross_val_score(model, X, y, cv=5, scoring="accuracy")
print(f"Mean: {scores.mean():.4f}, Std: {scores.std():.4f}")

# Stratified K-Fold (preserves class distribution)
skf = StratifiedKFold(n_splits=5, shuffle=True, random_state=42)
scores = cross_val_score(model, X, y, cv=skf, scoring="roc_auc")

# Common scoring options: 
# "accuracy", "precision", "recall", "f1", "roc_auc",
# "neg_mean_squared_error", "neg_mean_absolute_error", "r2"
```

### 2.8 Hyperparameter Tuning

```python
from sklearn.model_selection import GridSearchCV, RandomizedSearchCV

# Grid Search (exhaustive)
param_grid = {
    "n_estimators": [50, 100, 200],
    "max_depth": [3, 5, 10, None],
    "min_samples_leaf": [1, 5, 10]
}
grid = GridSearchCV(RandomForestClassifier(random_state=42),
                    param_grid, cv=5, scoring="roc_auc", n_jobs=-1)
grid.fit(X_train, y_train)
print("Best params:", grid.best_params_)
print("Best score: ", grid.best_score_)
best_model = grid.best_estimator_

# Randomized Search (faster for large grids)
from scipy.stats import randint, uniform
param_dist = {
    "n_estimators": randint(50, 300),
    "max_depth": [3, 5, 10, None],
    "min_samples_leaf": randint(1, 20)
}
rand_search = RandomizedSearchCV(
    RandomForestClassifier(random_state=42),
    param_dist, n_iter=50, cv=5, scoring="roc_auc",
    random_state=42, n_jobs=-1
)
rand_search.fit(X_train, y_train)
```

### 2.9 Feature Importance

```python
# Tree-based models
importances = model.feature_importances_
feat_imp = pd.Series(importances, index=X.columns).sort_values(ascending=False)
feat_imp.head(15).plot(kind="barh")
plt.title("Feature Importances")
plt.show()

# Logistic Regression coefficients
coef_df = pd.DataFrame({
    "feature": X.columns,
    "coefficient": model.coef_[0]
}).sort_values("coefficient", ascending=False)
```

### 2.10 Pipelines

```python
from sklearn.pipeline import Pipeline
from sklearn.compose import ColumnTransformer

# Prevents data leakage: scaling/encoding is fit only on training data
numeric_features = ["age", "income"]
categorical_features = ["city"]

preprocessor = ColumnTransformer([
    ("num", StandardScaler(), numeric_features),
    ("cat", OneHotEncoder(drop="first", handle_unknown="ignore"), categorical_features)
])

pipe = Pipeline([
    ("preprocessor", preprocessor),
    ("classifier", LogisticRegression(max_iter=1000))
])

pipe.fit(X_train, y_train)
y_pred = pipe.predict(X_test)
```

### 2.11 Handling Imbalanced Classes

```python
# Common in credit modelling: few defaults relative to non-defaults

# Option 1: Class weights
model = LogisticRegression(class_weight="balanced", max_iter=1000)

# Option 2: SMOTE (Synthetic Minority Oversampling)
from imblearn.over_sampling import SMOTE
smote = SMOTE(random_state=42)
X_resampled, y_resampled = smote.fit_resample(X_train, y_train)

# Option 3: Adjust decision threshold
y_prob = model.predict_proba(X_test)[:, 1]
threshold = 0.3   # Lower threshold → more predicted positives → higher recall
y_pred_custom = (y_prob >= threshold).astype(int)
```

---

## 3. SQL Reference

### 3.1 Basic Queries

```sql
-- Select all columns
SELECT * FROM customers;

-- Select specific columns
SELECT customer_id, first_name, last_name FROM customers;

-- Aliases
SELECT first_name AS fname, last_name AS lname FROM customers;

-- Distinct values
SELECT DISTINCT city FROM customers;

-- Limiting results
SELECT * FROM customers LIMIT 10;            -- MySQL/PostgreSQL/SQLite
SELECT TOP 10 * FROM customers;              -- SQL Server
```

### 3.2 WHERE Clause & Filtering

```sql
SELECT * FROM loans WHERE amount > 10000;
SELECT * FROM loans WHERE status = 'default';
SELECT * FROM loans WHERE amount BETWEEN 5000 AND 20000;
SELECT * FROM loans WHERE city IN ('Johannesburg', 'Cape Town', 'Durban');
SELECT * FROM loans WHERE city NOT IN ('Johannesburg');
SELECT * FROM loans WHERE name LIKE 'J%';          -- starts with J
SELECT * FROM loans WHERE name LIKE '%son';         -- ends with son
SELECT * FROM loans WHERE name LIKE '%van%';        -- contains van
SELECT * FROM loans WHERE email IS NULL;
SELECT * FROM loans WHERE email IS NOT NULL;

-- Compound conditions
SELECT * FROM loans
WHERE amount > 10000
  AND status = 'active'
  AND (term = 12 OR term = 24);
```

### 3.3 ORDER BY & LIMIT

```sql
SELECT * FROM loans ORDER BY amount DESC;
SELECT * FROM loans ORDER BY status ASC, amount DESC;

-- Top N per some criterion
SELECT * FROM loans ORDER BY amount DESC LIMIT 5;
```

### 3.4 Aggregate Functions

```sql
SELECT
    COUNT(*)            AS total_loans,
    COUNT(DISTINCT customer_id) AS unique_customers,
    SUM(amount)         AS total_amount,
    AVG(amount)         AS avg_amount,
    MIN(amount)         AS min_amount,
    MAX(amount)         AS max_amount,
    ROUND(AVG(amount), 2) AS avg_rounded
FROM loans;

-- COUNT(*) counts all rows including NULLs
-- COUNT(column) counts non-NULL values only
```

### 3.5 GROUP BY & HAVING

```sql
-- Total loans and average amount per status
SELECT
    status,
    COUNT(*)     AS loan_count,
    AVG(amount)  AS avg_amount,
    SUM(amount)  AS total_amount
FROM loans
GROUP BY status;

-- HAVING filters on aggregated results (WHERE filters before aggregation)
SELECT
    customer_id,
    COUNT(*)     AS num_loans,
    SUM(amount)  AS total_borrowed
FROM loans
GROUP BY customer_id
HAVING COUNT(*) > 3
ORDER BY total_borrowed DESC;

-- Multi-column groupby
SELECT
    region,
    product_type,
    COUNT(*)       AS num_loans,
    AVG(interest_rate) AS avg_rate
FROM loans
GROUP BY region, product_type
ORDER BY region, avg_rate DESC;
```

### 3.6 JOINS

```sql
-- INNER JOIN: Only matching rows from both tables
SELECT
    c.customer_id,
    c.first_name,
    l.loan_id,
    l.amount
FROM customers c
INNER JOIN loans l ON c.customer_id = l.customer_id;

-- LEFT JOIN: All rows from left table, matched rows from right (NULLs where no match)
SELECT
    c.customer_id,
    c.first_name,
    l.loan_id,
    l.amount
FROM customers c
LEFT JOIN loans l ON c.customer_id = l.customer_id;

-- RIGHT JOIN: All rows from right table
SELECT
    c.customer_id,
    c.first_name,
    l.loan_id
FROM customers c
RIGHT JOIN loans l ON c.customer_id = l.customer_id;

-- FULL OUTER JOIN: All rows from both tables
SELECT
    c.customer_id,
    l.loan_id
FROM customers c
FULL OUTER JOIN loans l ON c.customer_id = l.customer_id;

-- CROSS JOIN: Cartesian product (every combination)
SELECT c.name, p.product_name
FROM customers c
CROSS JOIN products p;

-- SELF JOIN: Joining a table to itself
-- Example: Find employees and their managers
SELECT
    e.name AS employee,
    m.name AS manager
FROM employees e
LEFT JOIN employees m ON e.manager_id = m.employee_id;

-- Multiple joins
SELECT
    c.first_name,
    l.amount,
    p.payment_date,
    p.payment_amount
FROM customers c
INNER JOIN loans l ON c.customer_id = l.customer_id
INNER JOIN payments p ON l.loan_id = p.loan_id
WHERE l.status = 'active';

-- Join with aggregation
SELECT
    c.customer_id,
    c.first_name,
    COUNT(l.loan_id)  AS num_loans,
    SUM(l.amount)     AS total_borrowed
FROM customers c
LEFT JOIN loans l ON c.customer_id = l.customer_id
GROUP BY c.customer_id, c.first_name
ORDER BY total_borrowed DESC;
```

**Join Cheat Sheet:**

| Join Type | Returns |
|-----------|---------|
| INNER JOIN | Only rows with matches in both tables |
| LEFT JOIN | All rows from left + matches from right (NULL if no match) |
| RIGHT JOIN | All rows from right + matches from left (NULL if no match) |
| FULL OUTER JOIN | All rows from both tables (NULL where no match) |
| CROSS JOIN | Every combination of rows (Cartesian product) |
| SELF JOIN | Table joined to itself |

### 3.7 Subqueries

```sql
-- Subquery in WHERE
SELECT * FROM customers
WHERE customer_id IN (
    SELECT customer_id FROM loans WHERE amount > 50000
);

-- Subquery in FROM (derived table)
SELECT avg_by_region.region, avg_by_region.avg_amount
FROM (
    SELECT region, AVG(amount) AS avg_amount
    FROM loans
    GROUP BY region
) AS avg_by_region
WHERE avg_by_region.avg_amount > 20000;

-- Correlated subquery (references outer query)
-- Customers whose total loans exceed the average for their region
SELECT c.customer_id, c.first_name, c.region
FROM customers c
WHERE (
    SELECT SUM(l.amount) FROM loans l WHERE l.customer_id = c.customer_id
) > (
    SELECT AVG(total) FROM (
        SELECT SUM(amount) AS total
        FROM loans
        GROUP BY customer_id
    ) sub
);

-- EXISTS (more efficient than IN for large datasets)
SELECT c.first_name
FROM customers c
WHERE EXISTS (
    SELECT 1 FROM loans l
    WHERE l.customer_id = c.customer_id AND l.status = 'default'
);
```

### 3.8 Common Table Expressions (CTEs)

```sql
-- Basic CTE
WITH high_value_customers AS (
    SELECT
        customer_id,
        SUM(amount) AS total_borrowed
    FROM loans
    GROUP BY customer_id
    HAVING SUM(amount) > 100000
)
SELECT
    c.first_name,
    c.last_name,
    hvc.total_borrowed
FROM high_value_customers hvc
JOIN customers c ON hvc.customer_id = c.customer_id
ORDER BY hvc.total_borrowed DESC;

-- Multiple CTEs
WITH
loan_summary AS (
    SELECT
        customer_id,
        COUNT(*) AS num_loans,
        SUM(amount) AS total_amount,
        AVG(interest_rate) AS avg_rate
    FROM loans
    GROUP BY customer_id
),
default_summary AS (
    SELECT
        customer_id,
        COUNT(*) AS num_defaults
    FROM loans
    WHERE status = 'default'
    GROUP BY customer_id
)
SELECT
    c.first_name,
    ls.num_loans,
    ls.total_amount,
    COALESCE(ds.num_defaults, 0) AS defaults
FROM customers c
JOIN loan_summary ls ON c.customer_id = ls.customer_id
LEFT JOIN default_summary ds ON c.customer_id = ds.customer_id;

-- Recursive CTE (e.g. for hierarchies)
WITH RECURSIVE org_chart AS (
    SELECT employee_id, name, manager_id, 1 AS level
    FROM employees
    WHERE manager_id IS NULL   -- root: CEO
    UNION ALL
    SELECT e.employee_id, e.name, e.manager_id, oc.level + 1
    FROM employees e
    JOIN org_chart oc ON e.manager_id = oc.employee_id
)
SELECT * FROM org_chart ORDER BY level, name;
```

### 3.9 Window Functions (VERY commonly tested)

```sql
-- ROW_NUMBER: Unique sequential number per partition
SELECT
    customer_id,
    loan_id,
    amount,
    ROW_NUMBER() OVER (PARTITION BY customer_id ORDER BY amount DESC) AS row_num
FROM loans;

-- RANK & DENSE_RANK
-- RANK: gaps after ties (1, 2, 2, 4)
-- DENSE_RANK: no gaps (1, 2, 2, 3)
SELECT
    customer_id,
    amount,
    RANK()       OVER (ORDER BY amount DESC) AS rnk,
    DENSE_RANK() OVER (ORDER BY amount DESC) AS dense_rnk
FROM loans;

-- NTILE: Divide into N roughly equal groups
SELECT
    customer_id,
    amount,
    NTILE(4) OVER (ORDER BY amount) AS quartile
FROM loans;

-- Running totals / cumulative sum
SELECT
    loan_id,
    payment_date,
    payment_amount,
    SUM(payment_amount) OVER (
        PARTITION BY loan_id ORDER BY payment_date
        ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW
    ) AS cumulative_paid
FROM payments;

-- LAG / LEAD: Access previous/next row values
SELECT
    customer_id,
    payment_date,
    payment_amount,
    LAG(payment_amount, 1)  OVER (PARTITION BY customer_id ORDER BY payment_date) AS prev_payment,
    LEAD(payment_amount, 1) OVER (PARTITION BY customer_id ORDER BY payment_date) AS next_payment,
    payment_amount - LAG(payment_amount, 1) OVER (
        PARTITION BY customer_id ORDER BY payment_date
    ) AS payment_change
FROM payments;

-- FIRST_VALUE / LAST_VALUE
SELECT
    customer_id,
    loan_id,
    amount,
    FIRST_VALUE(amount) OVER (
        PARTITION BY customer_id ORDER BY loan_date
    ) AS first_loan_amount,
    LAST_VALUE(amount) OVER (
        PARTITION BY customer_id ORDER BY loan_date
        ROWS BETWEEN UNBOUNDED PRECEDING AND UNBOUNDED FOLLOWING
    ) AS last_loan_amount
FROM loans;

-- Moving average
SELECT
    loan_date,
    amount,
    AVG(amount) OVER (
        ORDER BY loan_date
        ROWS BETWEEN 2 PRECEDING AND CURRENT ROW
    ) AS moving_avg_3
FROM loans;

-- Top N per group (common test pattern)
-- Get the largest loan per customer
WITH ranked AS (
    SELECT
        customer_id,
        loan_id,
        amount,
        ROW_NUMBER() OVER (PARTITION BY customer_id ORDER BY amount DESC) AS rn
    FROM loans
)
SELECT * FROM ranked WHERE rn = 1;
```

**Window Function Frame Clauses:**

| Frame | Meaning |
|-------|---------|
| ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW | From start to current row |
| ROWS BETWEEN 2 PRECEDING AND CURRENT ROW | Current and 2 previous rows |
| ROWS BETWEEN UNBOUNDED PRECEDING AND UNBOUNDED FOLLOWING | Entire partition |

### 3.10 CASE Statements

```sql
-- Simple CASE
SELECT
    loan_id,
    amount,
    CASE
        WHEN amount >= 100000 THEN 'Large'
        WHEN amount >= 50000  THEN 'Medium'
        ELSE 'Small'
    END AS loan_size
FROM loans;

-- CASE in aggregation (pivot-style)
SELECT
    customer_id,
    SUM(CASE WHEN status = 'active'  THEN 1 ELSE 0 END) AS active_loans,
    SUM(CASE WHEN status = 'default' THEN 1 ELSE 0 END) AS defaulted_loans,
    SUM(CASE WHEN status = 'closed'  THEN 1 ELSE 0 END) AS closed_loans
FROM loans
GROUP BY customer_id;

-- CASE in ORDER BY
SELECT * FROM loans
ORDER BY
    CASE status
        WHEN 'default' THEN 1
        WHEN 'active'  THEN 2
        WHEN 'closed'  THEN 3
    END;
```

### 3.11 String Functions (dialect varies)

```sql
-- Concatenation
SELECT first_name || ' ' || last_name AS full_name FROM customers;    -- PostgreSQL/SQLite
SELECT CONCAT(first_name, ' ', last_name) AS full_name FROM customers; -- MySQL/SQL Server

-- Case
SELECT UPPER(name), LOWER(name) FROM customers;

-- Substring
SELECT SUBSTRING(phone FROM 1 FOR 3) AS area_code FROM customers;  -- PostgreSQL
SELECT SUBSTR(phone, 1, 3) AS area_code FROM customers;            -- SQLite/MySQL

-- Length
SELECT LENGTH(name) FROM customers;       -- PostgreSQL/MySQL/SQLite
SELECT LEN(name) FROM customers;          -- SQL Server

-- Trim
SELECT TRIM(name), LTRIM(name), RTRIM(name) FROM customers;

-- Replace
SELECT REPLACE(phone, '-', '') FROM customers;

-- COALESCE (first non-NULL value)
SELECT COALESCE(email, phone, 'No Contact') AS contact FROM customers;

-- NULLIF (returns NULL if arguments are equal)
SELECT NULLIF(score, 0) FROM students;   -- Avoids division by zero: x / NULLIF(y, 0)

-- CAST / type conversion
SELECT CAST(amount AS DECIMAL(10,2)) FROM loans;
SELECT CAST(loan_date AS DATE) FROM loans;
```

### 3.12 Date Functions

```sql
-- Current date/time
SELECT CURRENT_DATE;                     -- PostgreSQL/MySQL/SQLite
SELECT GETDATE();                        -- SQL Server
SELECT NOW();                            -- MySQL/PostgreSQL

-- Date extraction
SELECT EXTRACT(YEAR FROM loan_date) AS yr FROM loans;       -- PostgreSQL/MySQL
SELECT YEAR(loan_date) AS yr FROM loans;                    -- SQL Server/MySQL
SELECT strftime('%Y', loan_date) AS yr FROM loans;          -- SQLite

-- Date arithmetic
SELECT loan_date + INTERVAL '30 days' FROM loans;           -- PostgreSQL
SELECT DATE_ADD(loan_date, INTERVAL 30 DAY) FROM loans;     -- MySQL
SELECT DATEADD(day, 30, loan_date) FROM loans;              -- SQL Server

-- Difference between dates
SELECT AGE(CURRENT_DATE, loan_date) FROM loans;             -- PostgreSQL
SELECT DATEDIFF(CURRENT_DATE, loan_date) FROM loans;        -- MySQL (returns days)
SELECT DATEDIFF(day, loan_date, GETDATE()) FROM loans;      -- SQL Server

-- Truncate to month/year
SELECT DATE_TRUNC('month', loan_date) AS month_start FROM loans;  -- PostgreSQL
```

### 3.13 Set Operations

```sql
-- UNION: Combine results, remove duplicates
SELECT customer_id FROM loans_2023
UNION
SELECT customer_id FROM loans_2024;

-- UNION ALL: Combine results, keep duplicates (faster)
SELECT customer_id, amount FROM loans_2023
UNION ALL
SELECT customer_id, amount FROM loans_2024;

-- INTERSECT: Rows in both queries
SELECT customer_id FROM loans_2023
INTERSECT
SELECT customer_id FROM loans_2024;

-- EXCEPT / MINUS: Rows in first but not second
SELECT customer_id FROM loans_2023
EXCEPT
SELECT customer_id FROM loans_2024;
```

### 3.14 INSERT, UPDATE, DELETE

```sql
-- INSERT
INSERT INTO customers (first_name, last_name, email)
VALUES ('John', 'Smith', 'john@example.com');

-- INSERT from SELECT
INSERT INTO archived_loans
SELECT * FROM loans WHERE status = 'closed';

-- UPDATE
UPDATE loans SET status = 'default'
WHERE days_past_due > 90;

-- UPDATE with JOIN
UPDATE loans l
SET l.risk_score = c.credit_score
FROM customers c
WHERE l.customer_id = c.customer_id;

-- DELETE
DELETE FROM loans WHERE status = 'cancelled';
```

### 3.15 CREATE TABLE & Constraints

```sql
CREATE TABLE loans (
    loan_id       INT PRIMARY KEY,
    customer_id   INT NOT NULL,
    amount        DECIMAL(12, 2) NOT NULL,
    interest_rate DECIMAL(5, 4),
    term_months   INT DEFAULT 12,
    status        VARCHAR(20) CHECK (status IN ('active','default','closed')),
    loan_date     DATE NOT NULL,
    FOREIGN KEY (customer_id) REFERENCES customers(customer_id)
);

-- Add column
ALTER TABLE loans ADD COLUMN risk_grade VARCHAR(2);

-- Create index
CREATE INDEX idx_loans_customer ON loans(customer_id);
```

### 3.16 Commonly Tested SQL Patterns

**Pattern 1: Percentage of total**
```sql
SELECT
    region,
    SUM(amount) AS region_total,
    ROUND(100.0 * SUM(amount) / (SELECT SUM(amount) FROM loans), 2) AS pct_of_total
FROM loans
GROUP BY region
ORDER BY pct_of_total DESC;
```

**Pattern 2: Year-over-year comparison**
```sql
WITH yearly AS (
    SELECT
        EXTRACT(YEAR FROM loan_date) AS yr,
        SUM(amount) AS total
    FROM loans
    GROUP BY EXTRACT(YEAR FROM loan_date)
)
SELECT
    yr,
    total,
    LAG(total) OVER (ORDER BY yr) AS prev_year,
    ROUND(100.0 * (total - LAG(total) OVER (ORDER BY yr))
          / LAG(total) OVER (ORDER BY yr), 2) AS yoy_growth_pct
FROM yearly;
```

**Pattern 3: Running default rate**
```sql
SELECT
    loan_date,
    COUNT(*) AS total,
    SUM(CASE WHEN status = 'default' THEN 1 ELSE 0 END) AS defaults,
    ROUND(100.0 * SUM(CASE WHEN status = 'default' THEN 1 ELSE 0 END)
          / COUNT(*), 2) AS default_rate_pct
FROM loans
GROUP BY loan_date
ORDER BY loan_date;
```

**Pattern 4: Find duplicates**
```sql
SELECT email, COUNT(*) AS cnt
FROM customers
GROUP BY email
HAVING COUNT(*) > 1;
```

**Pattern 5: Gaps in sequences**
```sql
SELECT
    loan_id + 1 AS gap_start,
    next_id - 1 AS gap_end
FROM (
    SELECT loan_id, LEAD(loan_id) OVER (ORDER BY loan_id) AS next_id
    FROM loans
) sub
WHERE next_id - loan_id > 1;
```

**Pattern 6: Cumulative distribution**
```sql
SELECT
    amount,
    PERCENT_RANK() OVER (ORDER BY amount) AS pct_rank,
    CUME_DIST()    OVER (ORDER BY amount) AS cumulative_dist
FROM loans;
```

---

## 4. Credit & Risk Concepts

### 4.1 Key Credit Risk Parameters

**Probability of Default (PD)**
The likelihood that a borrower will default within a given time horizon (usually 12 months).
- Estimated from: credit scoring models, transition matrices, historical default rates.
- Regulatory range: 0% < PD ≤ 100%.
- Point-in-Time (PIT) PD: reflects current economic conditions.
- Through-the-Cycle (TTC) PD: average over a full economic cycle.

**Loss Given Default (LGD)**
The proportion of exposure that is lost if the borrower defaults.
- LGD = 1 − Recovery Rate.
- Depends on: collateral, seniority of debt, workout process efficiency.
- Typical range: 10%–60% for secured lending; higher for unsecured.

**Exposure at Default (EAD)**
The total amount owed at the time of default.
- For drawn facilities: EAD ≈ current outstanding balance.
- For undrawn commitments: EAD = drawn + CCF × undrawn, where CCF is the Credit Conversion Factor.

**Expected Loss (EL)**

$$EL = PD \times LGD \times EAD$$

This is the average loss a lender expects and provisions for. It is NOT the "worst case" — that is addressed by Unexpected Loss / Economic Capital.

### 4.2 Credit Scoring Concepts

**Logistic Regression Scorecard**
The standard in credit scoring. The log-odds of default are modelled as a linear function of features:

$$\ln\left(\frac{p}{1-p}\right) = \beta_0 + \beta_1 x_1 + \beta_2 x_2 + \cdots + \beta_k x_k$$

where $p = P(\text{default})$.

**Weight of Evidence (WoE)**
A common feature transformation in credit scoring:

$$WoE_i = \ln\left(\frac{\text{Distribution of Non-Events}_i}{\text{Distribution of Events}_i}\right)$$

where "Events" = defaults and "Non-Events" = non-defaults in bin $i$.

- Positive WoE → more non-defaults (lower risk).
- Negative WoE → more defaults (higher risk).

**Information Value (IV)**
Measures the predictive power of a feature:

$$IV = \sum_{i=1}^{n} \left(\text{Distr. of Non-Events}_i - \text{Distr. of Events}_i\right) \times WoE_i$$

| IV Range | Predictive Power |
|----------|-----------------|
| < 0.02 | Not useful |
| 0.02 – 0.1 | Weak |
| 0.1 – 0.3 | Medium |
| 0.3 – 0.5 | Strong |
| > 0.5 | Suspicious (possible overfitting) |

**WoE & IV Calculation in Python:**
```python
def calc_woe_iv(df, feature, target):
    """Calculate WoE and IV for a binned feature."""
    grouped = df.groupby(feature)[target].agg(["sum", "count"])
    grouped.columns = ["events", "total"]
    grouped["non_events"] = grouped["total"] - grouped["events"]
    
    total_events = grouped["events"].sum()
    total_non_events = grouped["non_events"].sum()
    
    grouped["dist_events"] = grouped["events"] / total_events
    grouped["dist_non_events"] = grouped["non_events"] / total_non_events
    
    # Add small constant to avoid log(0)
    grouped["woe"] = np.log(
        (grouped["dist_non_events"] + 1e-6) / (grouped["dist_events"] + 1e-6)
    )
    grouped["iv"] = (grouped["dist_non_events"] - grouped["dist_events"]) * grouped["woe"]
    
    total_iv = grouped["iv"].sum()
    return grouped, total_iv
```

### 4.3 Model Discrimination Metrics

**Gini Coefficient**
Measures the model's ability to discriminate between defaults and non-defaults.

$$\text{Gini} = 2 \times AUC - 1$$

| Gini | Interpretation |
|------|---------------|
| < 0.2 | Poor |
| 0.2 – 0.4 | Acceptable |
| 0.4 – 0.6 | Good |
| 0.6 – 0.8 | Very Good |
| > 0.8 | Excellent (check for data leakage) |

**Kolmogorov-Smirnov (KS) Statistic**
Maximum separation between the cumulative distribution of defaults and non-defaults.

```python
from scipy.stats import ks_2samp

defaults = y_prob[y_test == 1]
non_defaults = y_prob[y_test == 0]
ks_stat, ks_pvalue = ks_2samp(defaults, non_defaults)

# Or manually from ROC:
ks_stat = max(tpr - fpr)
```

| KS | Interpretation |
|----|---------------|
| < 20 | Poor |
| 20 – 40 | Acceptable |
| 40 – 60 | Good |
| 60 – 75 | Very Good |
| > 75 | Excellent (check for overfitting) |

**Population Stability Index (PSI)**
Measures how much a score distribution has shifted between development and validation/monitoring samples.

$$PSI = \sum_{i=1}^{n}\left(\text{Actual}_i - \text{Expected}_i\right) \times \ln\left(\frac{\text{Actual}_i}{\text{Expected}_i}\right)$$

| PSI | Interpretation |
|-----|---------------|
| < 0.1 | No significant shift |
| 0.1 – 0.25 | Moderate shift — investigate |
| > 0.25 | Significant shift — model may need recalibration |

```python
def calc_psi(expected, actual, bins=10):
    """Calculate Population Stability Index."""
    breakpoints = np.quantile(expected, np.linspace(0, 1, bins + 1))
    breakpoints[0] = -np.inf
    breakpoints[-1] = np.inf
    
    expected_counts = np.histogram(expected, bins=breakpoints)[0] / len(expected)
    actual_counts = np.histogram(actual, bins=breakpoints)[0] / len(actual)
    
    # Avoid division by zero
    expected_counts = np.clip(expected_counts, 1e-6, None)
    actual_counts = np.clip(actual_counts, 1e-6, None)
    
    psi = np.sum((actual_counts - expected_counts) * np.log(actual_counts / expected_counts))
    return psi
```

### 4.4 Regulatory & Basel Concepts

**Basel II/III IRB Approach**
Banks using the Internal Ratings-Based (IRB) approach estimate their own PD, LGD, EAD to calculate Risk-Weighted Assets (RWA).

$$RWA = K \times EAD \times 12.5$$

where $K$ is the capital requirement from the Basel formula (a function of PD, LGD, correlation parameter, and maturity).

**Stages of IFRS 9 Expected Credit Loss (ECL)**

| Stage | Trigger | ECL Horizon |
|-------|---------|-------------|
| Stage 1 | Performing (initial recognition) | 12-month ECL |
| Stage 2 | Significant Increase in Credit Risk (SICR) | Lifetime ECL |
| Stage 3 | Credit-impaired (default) | Lifetime ECL |

Key: transition from Stage 1 → Stage 2 occurs when credit risk has increased significantly since origination (relative assessment, not absolute).

**Days Past Due (DPD) & Default Definition**
Under Basel / IFRS 9, a borrower is typically considered in default when:
- More than 90 days past due on a material obligation, OR
- The bank considers it unlikely the borrower will pay in full (Unlikely To Pay, UTP).

### 4.5 Credit Scorecard Scaling

A common convention converts log-odds into a "points" score:

$$\text{Score} = \text{Offset} + \text{Factor} \times \ln\left(\frac{p}{1-p}\right)$$

where:
- Offset and Factor are chosen so that a specific odds ratio corresponds to a target score.
- Common choice: "600 points at 50:1 odds, with 20 points to double the odds (PDO = 20)."

$$\text{Factor} = \frac{PDO}{\ln(2)}$$

$$\text{Offset} = \text{Target Score} - \text{Factor} \times \ln(\text{Target Odds})$$

### 4.6 Common Credit Test Questions

**Q: What is the difference between PD, LGD, and EAD?**
A: PD is the probability the borrower defaults. LGD is the fraction of exposure lost if default occurs. EAD is the total exposure at default. Together: $EL = PD \times LGD \times EAD$.

**Q: Why use WoE transformation?**
A: WoE converts categorical or binned variables to a continuous scale that has a monotonic relationship with the log-odds of default. It handles missing values naturally (as a separate bin) and makes the model more interpretable.

**Q: How do you validate a credit scorecard?**
A: Discrimination (Gini, KS, AUC-ROC), Calibration (observed vs predicted default rates per score band), Stability (PSI for score distribution, characteristic stability for individual features), out-of-time testing.

**Q: What is the difference between through-the-cycle (TTC) and point-in-time (PIT) PD?**
A: TTC PD is an average over a full economic cycle — it is relatively stable. PIT PD reflects current economic conditions — it fluctuates with the macro environment. Basel II IRB requires TTC-like PDs; IFRS 9 requires PIT PDs.

**Q: What causes a loan to move from Stage 1 to Stage 2 under IFRS 9?**
A: A Significant Increase in Credit Risk (SICR) since origination. Typical indicators: a significant increase in PD since origination, 30+ DPD (rebuttable presumption), qualitative factors like forbearance, restructuring, or industry downturn. The assessment is relative — a low-risk borrower moving from PD 0.1% to 0.5% may trigger SICR, even though 0.5% is still low in absolute terms.

**Q: What is the difference between Expected Loss and Unexpected Loss?**
A: Expected Loss (EL) is the mean/average loss — provisioned for in P&L. Unexpected Loss (UL) is the volatility around EL, covering tail risk — this is what economic capital and regulatory capital address.

**Q: Why is Gini > 0.8 suspicious?**
A: Extremely high Gini often indicates data leakage (a feature that directly reveals the target), overfitting, or a feature that would not be available at the time of scoring in production.

---

## 5. Statistics & Metrics Quick Reference

### 5.1 Hypothesis Testing Refresher

| Test | Use Case | Python |
|------|----------|--------|
| t-test (independent) | Compare means of two groups | `scipy.stats.ttest_ind(a, b)` |
| t-test (paired) | Compare means of matched pairs | `scipy.stats.ttest_rel(a, b)` |
| Chi-squared test | Test independence of categorical vars | `scipy.stats.chi2_contingency(table)` |
| ANOVA (one-way) | Compare means across 3+ groups | `scipy.stats.f_oneway(g1, g2, g3)` |
| Mann-Whitney U | Non-parametric alternative to t-test | `scipy.stats.mannwhitneyu(a, b)` |
| Shapiro-Wilk | Test for normality | `scipy.stats.shapiro(data)` |
| Pearson correlation | Linear correlation significance | `scipy.stats.pearsonr(x, y)` |
| Spearman correlation | Rank-based correlation | `scipy.stats.spearmanr(x, y)` |

Standard interpretation: reject the null hypothesis if p-value < significance level (typically 0.05).

### 5.2 Type I and Type II Errors

| | H₀ is True | H₀ is False |
|------|------------|------------|
| **Reject H₀** | Type I Error (α) — False Positive | Correct (Power = 1 − β) |
| **Fail to Reject H₀** | Correct | Type II Error (β) — False Negative |

In credit context:
- Type I Error: Classifying a good borrower as a default (reject a good loan).
- Type II Error: Classifying a bad borrower as good (approve a bad loan). This is typically the more costly error.

### 5.3 Variance Inflation Factor (VIF)

Measures multicollinearity. VIF > 5 (or > 10) suggests problematic collinearity.

```python
from statsmodels.stats.outliers_influence import variance_inflation_factor

vif_data = pd.DataFrame()
vif_data["feature"] = X.columns
vif_data["VIF"] = [
    variance_inflation_factor(X.values, i) for i in range(X.shape[1])
]
print(vif_data.sort_values("VIF", ascending=False))
```

### 5.4 Common Probability Distributions

| Distribution | Use Case | Python |
|-------------|----------|--------|
| Normal | Continuous data, CLT | `scipy.stats.norm` |
| Binomial | Number of successes in n trials | `scipy.stats.binom` |
| Poisson | Count of events in fixed interval | `scipy.stats.poisson` |
| Exponential | Time between events | `scipy.stats.expon` |
| Uniform | Equal probability | `scipy.stats.uniform` |
| Beta | PD calibration, proportions | `scipy.stats.beta` |

---

## 6. Quick Links to Documentation

### Python & Pandas
- Pandas API Reference: https://pandas.pydata.org/docs/reference/index.html
- Pandas User Guide: https://pandas.pydata.org/docs/user_guide/index.html
- NumPy Reference: https://numpy.org/doc/stable/reference/index.html

### Scikit-Learn
- Scikit-Learn User Guide: https://scikit-learn.org/stable/user_guide.html
- All Estimators (Models): https://scikit-learn.org/stable/modules/classes.html
- Metrics: https://scikit-learn.org/stable/modules/model_evaluation.html
- Preprocessing: https://scikit-learn.org/stable/modules/preprocessing.html
- Pipeline: https://scikit-learn.org/stable/modules/compose.html

### SQL
- PostgreSQL Documentation: https://www.postgresql.org/docs/current/
- PostgreSQL SQL Commands: https://www.postgresql.org/docs/current/sql-commands.html
- MySQL Reference Manual: https://dev.mysql.com/doc/refman/8.0/en/
- SQLite Documentation: https://www.sqlite.org/docs.html
- SQL Window Functions (PostgreSQL): https://www.postgresql.org/docs/current/tutorial-window.html
- W3Schools SQL (quick syntax lookup): https://www.w3schools.com/sql/

### Statistics
- SciPy Stats: https://docs.scipy.org/doc/scipy/reference/stats.html
- Statsmodels: https://www.statsmodels.org/stable/api.html

### Visualization
- Matplotlib: https://matplotlib.org/stable/api/index.html
- Seaborn: https://seaborn.pydata.org/api.html

### XGBoost
- XGBoost Parameters: https://xgboost.readthedocs.io/en/stable/parameter.html

### Imbalanced-Learn (SMOTE etc.)
- API Reference: https://imbalanced-learn.org/stable/references/index.html

---

*Good luck.*

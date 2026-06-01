# Piscine Python — Data Science

A **42** data-science bootcamp ("piscine"), working end-to-end on real data: from
building a PostgreSQL data warehouse, through exploratory visualization and
preprocessing, to training and comparing classification models.

The work is split into five modules. Modules 1–3 use an **e-commerce
customer-events** dataset stored in PostgreSQL; modules 4–5 switch to the
**Knight** dataset (classifying characters as **Jedi** or **Sith** from 30
numeric attributes).

> Suggested repository name: **`piscine-data-science`** (the project database is
> named `piscineds`).

---

## Table of contents

- [Datasets](#datasets)
- [Tech stack](#tech-stack)
- [Setup](#setup)
- [Modules](#modules)
  - [Module 1 — Build the database (SQL)](#module-1--build-the-database-sql)
  - [Module 2 — Consolidate & clean (SQL)](#module-2--consolidate--clean-sql)
  - [Module 3 — Visualization & clustering](#module-3--visualization--clustering)
  - [Module 4 — Preprocessing (Knight dataset)](#module-4--preprocessing-knight-dataset)
  - [Module 5 — Machine learning (Knight dataset)](#module-5--machine-learning-knight-dataset)

---

## Datasets

**1. E-commerce events** (PostgreSQL, modules 1–3)
A `customers` table of user events with columns `event_time`, `event_type`
(`view` / `cart` / `purchase` / `remove_from_cart`), `product_id`, `price`,
`user_id`, `user_session`, enriched with an `items` table (`product_id`,
`category_id`, `category_code`, `brand`).

**2. Knight** (CSV, modules 4–5)
30 numeric attributes (`Sensitivity`, `Hability`, `Strength`, …) plus a `knight`
label. `Train_knight.csv` is labeled (`Jedi` / `Sith`); `Test_knight.csv` is
unlabeled.

---

## Tech stack

- **PostgreSQL** — data warehouse (loaded via `COPY` from CSV).
- **Python** — `pandas`, `numpy`, `matplotlib`, `seaborn`, `psycopg2`,
  `SQLAlchemy`, `scikit-learn`, `statsmodels`.

There is no `requirements.txt` in the repo yet; you can create one with, e.g.:

```
pandas
numpy
matplotlib
seaborn
psycopg2-binary
SQLAlchemy
scikit-learn
statsmodels
```

---

## Setup

A PostgreSQL instance must be running with the e-commerce CSVs available to the
server (the SQL scripts `COPY` from `/customer` and `/items`). The Python
scripts in module 3 connect to a database named `piscineds`.

```bash
python3 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt   # after creating it (see above)
```

Most plotting scripts force the `TkAgg` matplotlib backend, so a display (or X
forwarding) is needed to see the windows.

---

## Modules

### Module 1 — Build the database (SQL)

| Exercise | File | What it does |
|---|---|---|
| ex02 | `table.sql` | Create one monthly table (`data_2022_oct`) with explicit column types and `COPY` the CSV into it. |
| ex03 | `automatic_table.sql` | A `PL/pgSQL` block that loops over every `.csv` in `/customer`, creating and filling one table per file automatically. |
| ex04 | `items_table.sql` | Create the `items` table and load `item.csv`. |

### Module 2 — Consolidate & clean (SQL)

| Exercise | File | What it does |
|---|---|---|
| ex01 | `customers_table.sql` | Build a single `customers` table by inserting every monthly `data_202*` table into it. |
| ex02 | `remove_duplicates.sql` | Remove near-duplicate events (identical rows within a 1-second window) using a `LAG` window function. |
| ex03 | `fusion.sql` | Enrich `customers` with item attributes (`category_id`, `category_code`, `brand`) via a `MERGE` on `product_id`. |

### Module 3 — Visualization & clustering

Python scripts that query PostgreSQL and plot the results.

| Exercise | File | What it does |
|---|---|---|
| ex00 | `pie.py` | Pie chart of the event-type distribution. |
| ex01 | `chart.py` | Purchases over time (daily counts across a date range). |
| ex02 | `mustache.py` | Price statistics (count/mean/std/quartiles) and a box plot ("moustache"). |
| ex03 | `Building.py` | Bar chart of purchase frequency per customer (binned). |
| ex04 | `elbow.py` | Elbow method to choose `k` for K-means (uses customer purchase/session/spend features). |
| ex05 | `Clustering.py` | Customer segmentation with K-means. |

### Module 4 — Preprocessing (Knight dataset)

| Exercise | File | What it does |
|---|---|---|
| ex00 | `Histogram.py` | Per-feature histograms of the Knight data. |
| ex01 | `Correlation.py` | Correlation of every feature with the target (`Jedi`/`Sith` mapped to 1/0). |
| ex02 | `points.py` | PCA scatter plots to visualize class separation. |
| ex03 | `standardization.py` | Z-score standardization (`StandardScaler`) + before/after plots. |
| ex04 | `Normalization.py` | Min-max normalization (`MinMaxScaler`) + before/after plots. |
| ex05 | `split.py` | Stratified train/validation split → `Training_knight.csv`, `Validation_knight.csv`. |

`split.py` usage:

```bash
python 4/ex05/split.py Train_knight.csv
```

### Module 5 — Machine learning (Knight dataset)

| Exercise | File | What it does |
|---|---|---|
| ex00 | `Confusion_Matrix.py` | Confusion matrix + precision / recall / F1 / accuracy, **computed by hand**. |
| ex01 | `Heatmap.py` | Correlation-coefficient heatmap of the features. |
| ex02 | `variances.py` | PCA explained and cumulative variance per component. |
| ex03 | `Feature_Selection.py` | Feature selection via Variance Inflation Factor (VIF). |
| ex04 | `Tree.py` | Decision-tree / random-forest classifier (with grid search), reporting F1. |
| ex05 | `KNN.py` | K-nearest-neighbors classifier, F1 vs. `k`. |
| ex06 | `democracy.py` | Voting ensemble combining logistic regression, KNN and random forest. |

`Confusion_Matrix.py` usage:

```bash
python 5/ex00/Confusion_Matrix.py predictions.txt truth.txt
```

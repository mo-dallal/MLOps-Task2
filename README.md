# 🚀 MLOps Training 2026/2027 — Task 2

<p align="center">
  <b>Olist E-Commerce — End-to-End Machine Learning Pipeline</b>
</p>

<p align="center">
  <i>From raw relational data to a production-ready classification workflow</i>
</p>

---

## 📌 Project Overview

This repository contains the complete implementation of **Task 2** for the MLOps Training 2026/2027 program.

The objective is to transform the Olist e-commerce database into a clean, reproducible machine-learning workflow capable of predicting whether an order will be **late or delivered on time**.

The project follows a sequential pipeline:

**Database → Data Integration → Label Creation → Data Split → EDA → Feature Engineering → Model Training → Tuning → Evaluation**

---

## 🎯 Objective

Build a machine-learning classification pipeline that predicts:

- 🟢 **0 — On Time**
- 🔴 **1 — Late**

The target is created by comparing:

`order_delivered_customer_date`

against

`order_estimated_delivery_date`

An order is considered **late** when the actual delivery date is later than the estimated delivery date.

---

## 🗂️ Project Structure

```text
MLOps-Task2/
│
├── 📓 notebooks/
│   ├── 01_read_join_tables.ipynb
│   ├── 02_create_labels.ipynb
│   ├── 03_train_validation_test_split.ipynb
│   ├── 04_eda_training_only.ipynb
│   ├── 05_feature_engineering.ipynb
│   └── 06_train_tune_evaluate.ipynb
│
├── 📦 artifacts/
│   └── charts/
│
├── 📄 requirements.txt
├── 📄 README.md
└── 📄 .gitignore
```

---

## 🔬 Pipeline

### 1️⃣ Read & Join Tables

`01_read_join_tables.ipynb`

- Connects to the Olist PostgreSQL database.
- Reads the available Olist tables.
- Inspects table shapes and duplicate records.
- Checks important keys.
- Aggregates one-to-many tables before joining.
- Creates a final **one-row-per-order ML table**.
- Saves the resulting dataset as an artifact.

---

### 2️⃣ Create Labels

`02_create_labels.ipynb`

Creates the target variable:

```text
late = 1 → Late
late = 0 → On Time
```

The notebook also:

- Validates the label using real order records.
- Displays class counts.
- Checks class proportions.
- Calculates delivery delay in days.

---

### 3️⃣ Train / Validation / Test Split

`03_train_validation_test_split.ipynb`

The data is divided chronologically into:

| Dataset | Portion |
|---|---:|
| Training | 70% |
| Validation | 15% |
| Test | 15% |

A chronological split is used to better represent a real production scenario where the model learns from historical orders and predicts future orders.

The notebook also verifies that the temporal order is preserved.

---

### 4️⃣ Exploratory Data Analysis

`04_eda_training_only.ipynb`

EDA is intentionally performed using **training data only**.

The analysis includes:

- Missing-value analysis
- Numerical statistics
- Categorical cardinality
- Rare categories
- Target distribution
- Target relationships
- Customer/seller state analysis
- Purchase weekday analysis
- Purchase month analysis
- Correlation analysis
- Visualizations

Generated charts are stored under:

```text
artifacts/charts/
```

---

### 5️⃣ Feature Engineering

`05_feature_engineering.ipynb`

Production-safe features are created from information available at prediction time.

Examples include:

- Purchase year
- Purchase month
- Purchase weekday
- Purchase hour
- Number of order items
- Freight value
- Number of unique products
- Number of unique sellers
- Payment information
- Customer state
- Seller state
- Order status

The preprocessing pipeline includes:

- Missing-value imputation
- Numerical scaling
- Categorical encoding
- Handling unseen categories

The fitted preprocessing object is saved so the same transformations can be reused later.

---

### 6️⃣ Model Training & Evaluation

`06_train_tune_evaluate.ipynb`

The notebook contains:

### Baseline

A simple **Most-Frequent Classifier** is used as the baseline.

### Machine Learning Model

A **Logistic Regression** classifier is trained using:

- Class balancing
- Validation-based hyperparameter tuning
- Multiple `C` values

### Evaluation Metrics

The final model is evaluated using:

- Accuracy
- Precision
- Recall
- F1-score
- ROC-AUC
- Confusion Matrix
- Classification Report

The test set is kept untouched until the final evaluation stage.

---

## 🧠 Data Leakage Prevention

Several measures are used to reduce data leakage:

- EDA is performed only on training data.
- The test set is not used for model selection.
- Preprocessing is fitted on training data only.
- Validation data is used for model selection/tuning.
- Test data is used only for the final evaluation.
- Post-delivery information is excluded from prediction features.

This makes the workflow closer to how a model would behave in a real production environment.

---

## 🛠️ Technologies Used

| Technology | Purpose |
|---|---|
| 🐍 Python | Programming language |
| 🐼 Pandas | Data manipulation |
| 🔢 NumPy | Numerical computing |
| 📊 Matplotlib | Data visualization |
| 🤖 Scikit-learn | Machine learning |
| 🗄️ PostgreSQL | Database |
| 🔌 SQLAlchemy | Database connection |
| 💾 Joblib | Model/preprocessor persistence |
| 📓 Jupyter Notebook | Experimentation & documentation |
| 🐙 Git / GitHub | Version control |

---

## ⚙️ Installation

Clone the repository:

```bash
git clone https://github.com/YOUR_USERNAME/MLOps-Task2.git
cd MLOps-Task2
```

Install dependencies:

```bash
pip install -r requirements.txt
```

---

## 🗄️ Database Configuration

The notebooks expect the Olist PostgreSQL database created during the previous task.

By default, the connection is:

```text
postgresql+psycopg2://postgres:postgres@localhost:5432/olist
```

If your PostgreSQL configuration is different, set:

```bash
OLIST_DB_URL
```

Example:

```bash
set OLIST_DB_URL=postgresql+psycopg2://USERNAME:PASSWORD@localhost:5432/olist
```

Do **not** commit database passwords or other secrets to GitHub.

---

## ▶️ How to Run

Run the notebooks in this exact order:

```text
01 → 02 → 03 → 04 → 05 → 06
```

Each notebook generates artifacts required by the next stage.

### Recommended workflow

```bash
jupyter notebook
```

Then open:

```text
notebooks/
```

and execute the notebooks sequentially.

---

## 📦 Generated Artifacts

During execution, the pipeline generates artifacts such as:

```text
artifacts/
│
├── 01_ml_table.csv
├── 01_table_profile.csv
├── 02_labeled_table.csv
├── 03_train.csv
├── 03_validation.csv
├── 03_test.csv
├── 04_eda_findings.md
├── 05_preprocessor.joblib
├── 05_feature_list.csv
├── 06_trained_model.joblib
├── 06_validation_results.csv
├── 06_test_results.csv
└── 06_baseline_vs_model.csv
```

These artifacts make the workflow reproducible and allow each stage to consume the output of the previous stage.

---

## 📈 Model Selection Strategy

The project follows a simple and transparent evaluation strategy:

```text
Training Data
     │
     ▼
Train Models
     │
     ▼
Validation Data
     │
     ▼
Hyperparameter Selection
     │
     ▼
Final Model
     │
     ▼
Test Data
     │
     ▼
Final Evaluation
```

The baseline is reported first so the machine-learning model can be evaluated against a meaningful reference point.

---

## 🔐 Reproducibility

The project is designed to be reproducible by:

- Using a fixed random seed where sampling is required.
- Saving preprocessing objects.
- Saving trained model artifacts.
- Keeping notebooks in a fixed execution order.
- Separating training, validation, and test data.
- Recording validation and test results.

---

## 👨‍💻 Author

**Mostafa Dallal**

Computer Systems Engineering Student  
Palestine Technical University – Kadoorie (PTUK)

---

## ⭐ Project Status

**Status:** ✅ Completed

**Task:** MLOps Training 2026/2027 — Task 2

**Pipeline:** End-to-End Classification Workflow

**Dataset:** Olist Brazilian E-Commerce Dataset

---

<p align="center">
  <b>Built with Python • PostgreSQL • Scikit-learn • Jupyter • GitHub</b>
</p>

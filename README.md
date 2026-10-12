# Comparative Evaluation of Regression Models Using a Diabetes Dataset

A Python machine learning demonstration comparing **Linear Regression**, **Random Forest Regression**, and **XGBoost Regression** on a public dataset. The project illustrates a reproducible workflow for exploring a clinical-style regression problem and comparing models with consistent evaluation metrics.

## Research question

How does predictive performance compare across Linear Regression, Random Forest, and XGBoost when predicting a quantitative diabetes progression measure using baseline demographic and clinical characteristics?

## Data and variables

The built-in [scikit-learn diabetes regression dataset](https://scikit-learn.org/stable/modules/generated/sklearn.datasets.load_diabetes.html) contains **442 observations**, **10 baseline predictors**, and a continuous outcome measured one year after baseline. This project uses five prespecified predictors: `age`, `sex`, `bmi`, `bp`, and `s6` (blood sugar), and `target` as the dependent variable. Predictors are scaled in the default dataset; the target is **not a documented HbA1c measurement**. The analysis includes 353 training and 89 test observations (`random_state=42`).

## Methods

1. Inspect dataset structure, missing values, and duplicate rows.
2. Describe selected variables; inspect correlations, VIF, and potential IQR outliers. IQR-flagged observations were retained.
3. Split once into training (80%) and held-out test (20%) subsets.
4. Train an ordinary least-squares Linear Regression benchmark, Random Forest (`n_estimators=100`, `random_state=42`), and XGBoost (`n_estimators=100`, `max_depth=3`, `learning_rate=0.05`, `objective='reg:squarederror'`, `random_state=42`).
5. Compare held-out mean absolute error (MAE), root mean squared error (RMSE), and R², then inspect model-specific feature importance and residuals.

## Reported test-set results

| Model | MAE ↓ | RMSE ↓ | R² ↑ |
|---|---:|---:|---:|
| Linear Regression | 49.893 | **60.074** | **0.319** |
| Random Forest | **48.606** | 62.789 | 0.256 |
| XGBoost | 48.886 | 60.241 | 0.315 |

Linear Regression is the **preferred model for this demonstration**, given the lowest RMSE, highest R², and straightforward interpretation. Random Forest has the lowest MAE. These results are descriptive comparisons on one test split and **do not establish statistical superiority**.

![MAE and RMSE comparison](figures/model_performance_comparison.png)

## Reproduce the analysis

Requires Python 3.10+ and Jupyter Notebook. In a terminal in the repository folder:

```bash
python -m venv .venv
# Windows PowerShell: .venv\Scripts\Activate.ps1
# macOS/Linux: source .venv/bin/activate
python -m pip install -r requirements.txt
jupyter notebook notebooks/diabetes_model_comparison.ipynb
```

In Jupyter, select **Kernel → Restart Kernel and Run All Cells**. The notebook retains outputs from the author's analysis; regenerated results may vary slightly across package versions. Requirements specify supported package ranges rather than exact versions.

## Repository contents

- `notebooks/diabetes_model_comparison.ipynb` — annotated exploratory analysis, modeling, evaluation, and plots
- `docs/research_summary.docx` — research write-up and discussion
- `figures/model_performance_comparison.png` — model comparison visualization
- `requirements.txt` — Python dependencies

## Limitations

The dataset is small (n=442) with five prespecified predictors. Models were not systematically tuned or assessed using cross-validation or external validation. Repeated inspection of the same test results can bias model-selection decisions; further evaluation should use training-only cross-validation and an untouched final evaluation set. Importance scores from Random Forest and XGBoost use different model-specific calculations and are not directly comparable clinical effect estimates. No clinical use, treatment recommendation, or real-world generalizability is claimed.

## Data source and attribution

The diabetes dataset is distributed with scikit-learn and is associated with Efron, Hastie, Johnstone, and Tibshirani (2004), *Least Angle Regression*, *Annals of Statistics*, 32(2), 407–499. [DOI](https://doi.org/10.1214/009053604000000067).

This is an educational/research methods demonstration, not a warfarin dosing algorithm. No patient-level protected data are included.

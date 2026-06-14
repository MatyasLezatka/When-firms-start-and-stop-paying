# Machine Learning Prediction of Dividend Initiations and Omissions

**Master's Thesis** · Utrecht University School of Economics · June 2026  
**Author:** Matyáš Ležatka  
**Supervisor:** Prof. dr. Marie Dutordoir

---

## Overview

This repository contains the complete data pipeline and machine learning analysis for the thesis *When Firms Start and Stop Paying: Machine Learning Evidence on Dividend Initiations and Omissions*. Using Compustat Fundamentals Annual data on U.S. industrial firms (1963–2025), six classifiers are trained and evaluated under a rolling walk-forward cross-validation design to predict first-time dividend initiations and dividend omissions.

The primary evaluation metric is Precision-Recall AUC (PR-AUC), selected for robustness to severe class imbalance. SHAP attributions provide global and local model interpretability, structured around eight dividend theory blocks drawn from the corporate finance literature.

### Sample

| Task | Firm-years | Events | Event rate | Imbalance ratio |
|---|---|---|---|---|
| Initiation | 94,127 | 2,858 | 3.04% | 31.9:1 |
| Omission | 72,646 | 2,925 | 4.03% | 23.8:1 |

Estimation window: 1990–2025. Out-of-sample test years: 2000–2025 (26 rolling folds).

---

## Repository structure

```
ml-dividend-initiation-omission/
│
├── 01_data_sourcing.ipynb            # Compustat download, feature engineering, ML CSV export
├── 02_descriptive_analysis.ipynb     # Summary statistics, event-count figures, Table 5.1–5.2
├── 03_ml_analysis.ipynb              # Horse-race, SHAP, investor screening, firm case studies
│
├── data/
│   ├── README.md                     # Data access and WRDS reproduction instructions
│   ├── ml_dividend_initiations_1990_2025.csv   # 94,127 firm-years, 34 lagged features + target
│   ├── ml_dividend_omissions_1990_2025.csv     # 72,646 firm-years, 34 lagged features + target
│   └── ml_feature_list.csv           # Feature mapping (current-year name → lagged column name)
│
├── results/
│   ├── figures/                      # All main and supplementary figures (PNG/PDF)
│   └── tables/                       # Model output tables (LaTeX and CSV)
│
├── requirements.txt
└── .gitignore
```

---

## Notebooks

| Notebook | Purpose | Runtime |
|---|---|---|
| `01_data_sourcing.ipynb` | Downloads Compustat via WRDS, applies sample filters, engineers 34 features, exports ML CSVs | ~5 min (with cached raw data) |
| `02_descriptive_analysis.ipynb` | Produces descriptive statistics and event-count charts for thesis §5.1 | ~2 min |
| `03_ml_analysis.ipynb` | Full rolling CV horse-race, SHAP attribution, portfolio screening, case studies | ~30–36 hours |

Notebooks must be run in sequence: `01` exports the CSVs that `02` and `03` read. Pre-computed ML CSVs are provided in `data/` so that `03_ml_analysis.ipynb` can be run without re-running `01`.

---

## Models

Six classifiers are evaluated in a horse-race under identical rolling cross-validation:

| Model | Hyperparameter tuning | Imbalance handling |
|---|---|---|
| Logistic Regression (unpenalized) | None | Balanced class weights |
| Ridge (L2) | `C` over {0.05, 0.06, …, 2.05} | Balanced class weights |
| LASSO (L1) | Fixed `C = 0.1` | Balanced class weights |
| Random Forest | Trees, min-leaf, max-depth | Balanced class weights |
| XGBoost | Trees, learning rate, depth, subsampling | `scale_pos_weight = n_neg / n_pos` per fold |
| LightGBM | Learning rate, n-leaves, depth | `scale_pos_weight = n_neg / n_pos` per fold |

No SMOTE or undersampling is used, preserving the chronological integrity of the rolling window.

---

## Features

Thirty-four predictors, all lagged one fiscal year (`_lag1`), spanning eight theory blocks:

| Block | Theoretical basis | Features |
|---|---|---|
| Life-cycle and maturity | DeAngelo et al., (2006) | RE/TE, RE/TA, TE/TA, Size, Log Mkt Cap, Sales Growth, Asset Growth, Listing Age |
| Growth options and investment | Fama and French (2001) | Market-to-Book, CapEx/Assets, R&D/Assets, Price/Sales |
| Signaling and profitability | Bhattacharya (1979) | ROA, ROE, OpCF/Assets, ORA, Profit Margin, Accruals |
| Free cash flow and agency | Jensen (1986) | Cash/Assets, FCF/Assets, Repurchase/Assets, Goodwill/Assets |
| Conservatism and risk | Hoberg and Prabhala (2009) | Leverage, LTDA, Current Ratio, Earnings Volatility, Interest Coverage, FAT, LCTAT, Labor Intensity |
| Corporate taxation | Chetty and Saez (2005) | GAAP ETR, Cash ETR |
| Transaction cost | Kale et al. (2012) | Share Turnover |
| Catering and behavioral | Baker & Wurgler (2004) | Dividend Premium (market-wide) |

---

## Validation design

```
Training: [t − 10, t − 1]   →   Test: year t
Test years: 2000–2025 (26 folds)
Inner CV: 5 chronological folds within each training window (hyperparameter tuning)
Preprocessing: fit on training data only, applied to held-out test fold
```

Preprocessing stack per classifier group:
- **Linear models (Logit, Ridge, LASSO):** median imputation with missing-value indicators, then standardisation
- **Random Forest:** median imputation only
- **XGBoost / LightGBM:** raw feature matrix (native missing-value handling)

---

## Data

Raw data are sourced from Compustat Fundamentals Annual (WRDS, `comp.funda`, INDL format, 1963–2025). The raw file is proprietary and cannot be redistributed. See [`data/README.md`](data/README.md) for the WRDS query and extraction instructions.

Pre-computed ML-ready CSVs are provided in `data/`. These files are the direct input to `03_ml_analysis.ipynb` and reproduce all thesis results without requiring a WRDS subscription or re-running the data pipeline.

> **Reproducibility note:** Re-running `01_data_sourcing.ipynb` from a fresh WRDS download may produce marginally different sample counts due to pipeline refinements made after the ML analysis was completed. All thesis results correspond to the pre-computed CSVs provided in `data/`.

---

## Setup

### Requirements

Python 3.13. Install all dependencies with:

```bash
pip install -r requirements.txt
```

### Reproducing the ML results

Pre-computed CSVs are already in `data/`. Launch JupyterLab from the repository root and run `03_ml_analysis.ipynb`:

```bash
cd ml-dividend-initiation-omission
jupyter lab
```

### Re-running the full pipeline from raw data (WRDS required)

A WRDS subscription is required. Set `FORCE_DOWNLOAD = True` in the first code cell of `01_data_sourcing.ipynb`, or use your existing WRDS credentials against the cached raw file. Run notebooks in order: `01` → `02` → `03`.

---

## Key results

| Metric | Initiation best model | Omission best model |
|---|---|---|
| PR-AUC | XGBoost / LightGBM | XGBoost / LightGBM |
| Primary signal block | Life-cycle and maturity | Conservatism and risk |

Full performance tables and SHAP figures are in `results/`.

---

## Citation

If you use this code or data pipeline in your work, please cite:

```bibtex
@mastersthesis{lezatka2026dividend,
  author    = {Leza̡tka, Matyáš},
  title     = {When Firms Start and Stop Paying: Machine Learning Evidence
               on Dividend Initiations and Omissions},
  school    = {Utrecht University School of Economics},
  year      = {2026},
  supervisor = {Dutordoir, Marie},
  type      = {Master's thesis}
}
```

---

## License

Code in this repository is released for academic and research use. The underlying raw Compustat data are subject to WRDS and S&P Global licensing terms and cannot be redistributed.

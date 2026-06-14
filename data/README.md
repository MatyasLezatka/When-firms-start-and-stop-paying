# Data Directory

## Pre-computed ML CSVs (included — use these to reproduce thesis results)

| File | Rows | Contents |
|---|---|---|
| `ml_dividend_initiations_1990_2025.csv` | 94,127 | 34 lagged features + `target_initiation` (1 = first-time dividend payer) |
| `ml_dividend_omissions_1990_2025.csv` | 72,646 | 34 lagged features + `target_omission` (1 = dividend stopped after 2+ years) |
| `ml_feature_list.csv` | 34 | Feature name mapping: `feature_current_year` → `feature_model_column` (`_lag1` suffix) |

These files are the direct input to `03_ml_analysis.ipynb`. All thesis results are reproducible from them without WRDS access or re-running the data pipeline.

All 34 predictor columns carry the `_lag1` suffix — they represent year `t − 1` values used to predict the year `t` target. Do not lag them again inside the modelling notebook.

---

## Raw Compustat data (proprietary — not redistributable)

The raw source is Compustat Fundamentals Annual (WRDS database, table `comp.funda`).

**Access:** A WRDS institutional subscription is required. Apply at [wrds.wharton.upenn.edu](https://wrds.wharton.upenn.edu).

**Extraction filters used:**

| Parameter | Value |
|---|---|
| `indfmt` | `'INDL'` (industrial format) |
| `datafmt` | `'STD'` (standardised) |
| `popsrc` | `'D'` (domestic) |
| `consol` | `'C'` (consolidated) |
| `fic` | `'USA'` |
| `curcd` | `'USD'` |
| Date range | 1963-01-01 to 2025-12-31 |

The complete SQL query is embedded in cell 2 of `01_data_sourcing.ipynb`. Running that cell with valid WRDS credentials fetches and caches the raw extract as `compustat_funda_dividend_ml_raw_1963_2025.csv.gz` (not included in this repository).

---

## Full engineered panel (large — not included)

`dividend_full_engineered_panel_1963_2025.csv` contains 332,358 firm-years (1963–2025) with both current-year features and lagged predictors. It is produced by `01_data_sourcing.ipynb` and read by `02_descriptive_analysis.ipynb` for same-year feature reporting. The file is excluded from this repository because of its size but can be reproduced by running `01_data_sourcing.ipynb` with WRDS access.

---

## Reproducibility note

Re-running `01_data_sourcing.ipynb` from a fresh WRDS download may produce marginally different sample counts relative to those in the thesis (94,127 initiation / 72,646 omission firm-years). This reflects minor pipeline refinements made after the ML analysis was completed. The pre-computed CSVs in this directory correspond to the exact data used for all thesis results.

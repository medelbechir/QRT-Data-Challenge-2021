# QRT Data Challenge (2021)
## Reconstruction of Liquid Asset Performance

This repository contains my complete submission for the **QRT Data Challenge (2021)**.
The objective is to predict the **next-day sign of returns of a liquid asset**
using cross-sectional information from a universe of illiquid assets.

The repository includes the full modeling pipeline, rendered outputs, datasets,
benchmark material, and a concise technical overview of the solution.

---

## Problem description

For each observation identified by a trading day (`ID_DAY`) and a target liquid
asset (`ID_TARGET`), the task is to predict the sign of the next-day return
`RET_TARGET` using returns from 100 illiquid assets (`RET_*`).

The official evaluation metric is the **magnitude-weighted directional accuracy**:

Score =
sum_i |y_i| · 1[sign(ŷ_i) = sign(y_i)]
------------------------------------
          sum_i |y_i|

---

## Methodology overview

### Validation protocol
- **GroupKFold cross-validation**, grouped by `ID_DAY` (day-wise split)
- All label-dependent steps (proxy selection, threshold calibration) are computed
  **inside training folds only** or using **out-of-fold predictions**

### Feature engineering
For each target asset, a separate model is trained using the same feature space.
Contextual features are computed in a target-specific and fold-specific manner:

- Raw illiquid returns (`RET_*`)
- Market state feature:
  - Cross-sectional volatility (`RET_MKT_STD`)
- Correlation proxy (training only):
  - Illiquid asset with the highest absolute correlation with `RET_TARGET`
  - Used solely to prevent information leakage
- Proxy-excluded contextual averages:
  - Market-level average
  - Sector-level average (when sector information is available)
- Relative return features:
  - Deviations from market and sector contextual averages

This results in **303 features per target asset**.

---

## Models

For each target asset, a weighted ensemble is trained combining:

- **Ridge Regression**
  - Median imputation
  - Feature standardization
  - Regularization to control variance
- **XGBoost Regressor**
  - Boosted trees to reduce bias
  - Native handling of missing values
- **ExtraTrees Regressor**
  - Bagged randomized trees to reduce variance
  - Native handling of missing values

During training, target values are clipped to limit the influence of extreme
returns, while evaluation is performed using the original sign-based metric.

---

## Post-hoc threshold calibration

Since the evaluation metric depends only on the **sign** of predictions, a global
additive decision threshold is calibrated using **out-of-fold predictions only**.
A smoothed score curve is used to select a robust threshold, which is then applied
consistently at inference time.

---

## Results

- **GroupKFold cross-validation (by day)**: ~74.8% magnitude-weighted directional accuracy
- **Public leaderboard**: Top 3 position at the time of submission
- Cross-validation and leaderboard scores are closely aligned, indicating limited
  overfitting

A concise technical summary of the approach and key results is provided in the PDF
overview.

---

## Repository structure

QRT-Data-Challenge-2021/
├── README.md
├── QRT_Challenge.ipynb
├── QRT_Challenge.html
├── data/
│   ├── data.zip
│   ├── README.md
│   └── supplementary_data/
│       ├── supplementary_data.csv
│       └── Benchmark_2021_QRT_Challenge.ipynb
└── Overview_of_my_solution.pdf

---

## Notes

This work was developed as part of a quantitative research recruiting exercise.
The focus is on methodological correctness, robustness, and clarity rather than
domain-specific financial jargon.

All results reported in this repository are obtained without data leakage and are
fully reproducible from the provided material.

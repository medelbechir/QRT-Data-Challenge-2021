# QRT Data Challenge (2021) — Reconstruction of Liquid Asset Performance

Submission repository for the **QRT Data Challenge (2021)**.  
The objective is to predict the sign of a liquid asset return using 100 illiquid returns


## Highlights
- **ENS Challenge Data – QRT Data Challenge 2021:**  
  3rd place on the public leaderboard at the time of submission  
  ([official challenge page](https://challengedata.ens.fr/challenges/44))
- **Day-wise cross-validation:** GroupKFold split by `ID_DAY`
- **Target-specific training:** one model per `ID_TARGET`
- **Ensemble model:** Ridge + XGBoost + ExtraTrees
- **Robust sign decision:** threshold calibrated on out-of-fold predictions only


## Quick links
- **Notebook (full pipeline):** `QRT_Challenge.ipynb`
- **Rendered notebook :** `QRT_Challenge.html`
- **Short technical overview (PDF):** `Overview_of_my_solution.pdf`


## Repository structure
```text
QRT-Data-Challenge-2021/
│
├── QRT_Challenge.ipynb
├── QRT_Challenge.html
├── Overview_of_my_solution.pdf
│
└── data/
    ├── data.zip
    ├── README.md
    └── supplementary_data/
        ├── supplementary_data.csv
        └── Benchmark_2021_QRT_Challenge.ipynb
```

## Notes

- This work was developed as part of a quantitative research recruiting exercise.
- The PDF provides a concise summary; all details and diagnostics are in the notebook.
- For a quick review without execution, open `QRT_Challenge.html`.

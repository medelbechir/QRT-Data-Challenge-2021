## Data

This folder contains the datasets provided for the **QRT Data Challenge (2021)**.

### Main datasets
- `data.zip`
  - `X_train`: training features (returns of illiquid assets)
  - `y_train`: training targets (returns of the liquid asset)
  - `X_test`: test features

The data is provided in compressed form to keep the repository lightweight.

### Supplementary data
- `supplementary_data/`
  - `supplementary_data.csv`: additional asset-level information
  - `Benchmark_2021_QRT_Challenge.ipynb`: official benchmark notebook provided with the challenge

### Notes
- The data is used exclusively for reproducibility of the experiments in this repository.
- No data preprocessing or modification is performed outside the notebook.

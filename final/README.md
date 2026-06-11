# SWCE: Severity-Weighted Calibration Error
## ICU Mortality Prediction Calibration Framework

### Overview

Severity-Weighted Calibration Error (SWCE) is a post-hoc calibration evaluation framework for imbalanced clinical risk prediction. The central finding is a calibration paradox: a model can achieve the best aggregate calibration score while still showing the most clinically dangerous underestimation in the highest-acuity subgroup.

This repository version is publication-focused and includes the two core notebooks plus archived outputs used for replication and reporting.

### Repository Structure

```
final/
├── notebooks/
│   ├── SWCE_MIMIC_IV.ipynb       # MIMIC-IV analysis notebook
│   ├── SWCE_MIMIC_III.ipynb      # MIMIC-III replication notebook
│   └── results/
│       ├── combined/             # cross-dataset summary outputs
│       ├── drive_downloads/      # archived exported figures/tables
│       ├── mimic3/               # MIMIC-III output artifacts
│       ├── mimic4/               # MIMIC-IV output artifacts
│       └── README.md
├── requirements.txt
├── .gitignore
└── README.md
```

### Published Scope Notes

- Included: analysis notebooks, requirements, and result artifacts.
- Excluded from this repo snapshot: manuscript/LaTeX bundle and consolidated pipeline notebook.

### Datasets

| Dataset   | Patients | Features | Split                | Severe Band       | Mortality |
|-----------|----------|----------|----------------------|-------------------|-----------|
| MIMIC-IV  | 72,001   | 14       | 70/10/20 stratified  | SOFA >= 16, n=69  | 63.8%     |
| MIMIC-III | 45,278   | 14       | 70/10/20 stratified  | SOFA >= 16, n=35  | 74.3%     |

Access to both datasets requires PhysioNet credentialed approval: https://physionet.org/

### Models Evaluated

| Model                | Abbreviation | F1 (MIMIC-IV) | AUC (MIMIC-IV) |
|----------------------|--------------|---------------|----------------|
| Logistic Regression  | LR           | 0.4043        | 0.7909         |
| Random Forest        | RF           | 0.4364        | 0.8183         |
| XGBoost              | XGB          | 0.4152        | 0.7913         |
| LightGBM             | LGBM         | 0.4415        | 0.8252         |
| LSTM                 | LSTM         | 0.4314        | 0.8143         |

### Key Results

- Calibration paradox: RF has best aggregate ECE (0.0098; 95% CI 0.006-0.015) but worst severe-band gap (-0.174), missing 43/69 severe patients at threshold 0.50.
- Metric equivalence at aggregate level: ECE, AECE, and SWCE produce identical model rankings (Spearman rho = 1.00) across tested beta values.
- Recalibration impact: per-band isotonic recalibration substantially reduces severe-band error, including strong gains on MIMIC-III.
- Replication: the paradox reproduces on MIMIC-III, indicating a structural calibration safety issue rather than a single-cohort artifact.

### Reproducing Results

1. Obtain MIMIC-IV v2.2 and MIMIC-III v1.4 access from PhysioNet.
2. Install dependencies from the repository root:

   ```bash
   pip install -r final/requirements.txt
   ```

3. Open and run notebooks in this order:
   - final/notebooks/SWCE_MIMIC_IV.ipynb
   - final/notebooks/SWCE_MIMIC_III.ipynb
4. Update data paths in notebook configuration cells to your local MIMIC storage.
5. Review outputs under final/notebooks/results/.

### Requirements

- Python 3.9+
- Package versions listed in final/requirements.txt

### Citation

Paper under review; citation metadata will be added upon publication.

### License and Data Use

Code and outputs in this repository are shared for research reproducibility. MIMIC data access and use remain governed by PhysioNet credentialing and data use agreements.

# SWCE: Severity-Weighted Calibration Error
## ICU Mortality Prediction Calibration Framework

### Overview

Severity-Weighted Calibration Error (SWCE) is a formally defined post-hoc calibration framework that incorporates outcome-severity weighting to evaluate probability calibration in imbalanced clinical prediction settings. This work demonstrates a **calibration paradox**: the model with the best aggregate Expected Calibration Error (ECE) simultaneously produces the most dangerous severe-band calibration gap, systematically underestimating mortality in the highest-acuity ICU subgroup. Standard aggregate metrics — regardless of weighting scheme — cannot reliably detect this failure, establishing severity-stratified calibration evaluation as a practical requirement for safe clinical model deployment.

### Repository Structure

```
final/
├── paper/
│   ├── main.tex                  ← submission-ready LaTeX manuscript
│   ├── cover.tex                 ← cover letter
│   ├── IEEEtran.cls              ← IEEE transaction class file
│   ├── ieeeaccess.cls            ← IEEE Access class file
│   ├── bullet.png, logo.png, notaglinelogo.png  ← journal assets
│   └── figures/                  ← all publication figures (Fig 1–9)
├── notebooks/
│   ├── final_pipeline.ipynb      ← consolidated end-to-end pipeline
│   └── results/                  ← all saved CSV tables and plot outputs
│       ├── mimic4/               ← MIMIC-IV results (tables + plots)
│       ├── mimic3/               ← MIMIC-III results (tables + plots)
│       └── combined/             ← cross-dataset replication summaries
├── .gitignore
├── requirements.txt
└── README.md                     ← this file
```

### Datasets

| Dataset  | Patients | Features | Split         | Severe Band         | Mortality |
|----------|----------|----------|---------------|---------------------|-----------|
| MIMIC-IV | 72,001   | 14       | 70/10/20 stratified | SOFA ≥ 16, n=69  | 63.8%     |
| MIMIC-III| 45,278   | 14       | 70/10/20 stratified | SOFA ≥ 16, n=35  | 74.3%     |

**Access:** Both datasets require PhysioNet credentialed access. See [physionet.org](https://physionet.org/) for details.

### Models Evaluated

| Model               | Abbreviation | F1 (MIMIC-IV) | AUC (MIMIC-IV) |
|---------------------|-------------|---------------|----------------|
| Logistic Regression  | LR          | 0.4043        | 0.7909         |
| Random Forest        | RF          | 0.4364        | 0.8183         |
| XGBoost              | XGB         | 0.4152        | 0.7913         |
| LightGBM             | LGBM        | 0.4415        | 0.8252         |
| LSTM                 | LSTM        | 0.4314        | 0.8143         |

### Key Results

- **Calibration Paradox:** RF achieves best aggregate ECE (0.0098 [95% CI: 0.006–0.015]) but worst severe-band gap (−0.174), missing 43/69 (62.3%) severe patients at T=0.50, while LR and LSTM miss zero.
- **Metric Equivalence:** ECE, AECE, and SWCE produce identical aggregate rankings (Spearman ρ = 1.00) across all β values tested — aggregate weighting cannot resolve subgroup failures.
- **Recalibration:** Per-band isotonic regression substantially reduces RF's gap on MIMIC-III (−0.185 → +0.010) and markedly attenuates it on MIMIC-IV (−0.174 → −0.058).
- **Temporal Replication:** The paradox replicates on MIMIC-III (RF gap −0.185, 8/35 severe patients missed), confirming the finding is structural, not dataset-specific.

### Reproducing Results

1. **Obtain Data:** Request access to MIMIC-IV v2.2 and MIMIC-III v1.4 from PhysioNet.
2. **Install Dependencies:**
   ```bash
   pip install -r requirements.txt
   ```
3. **Configure Data Paths:** Update the data loading cells in `notebooks/final_pipeline.ipynb` to point to your local MIMIC data directory.
4. **Run the Pipeline:** Execute `final_pipeline.ipynb` end-to-end. Sections are clearly marked:
   - Sections 1–3: Environment setup and data preprocessing
   - Section 4: Model training (all 5 models)
   - Sections 5–6: Calibration evaluation and severity band analysis
   - Section 7: Recalibration experiments
   - Section 8: Generate all tables and figures
5. **Output:** Results are saved to `notebooks/results/` and figures match those in `paper/figures/`.

### Requirements

- Python ≥ 3.9
- See `requirements.txt` for full dependency list with version pins.

### Citation

> Paper under review — citation will be added upon acceptance.

### License

This project is licensed under the MIT License. See the MIMIC data use agreements for dataset-specific restrictions.

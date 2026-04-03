# Remaining Useful Life Prediction of Turbofan Engines
## Metaheuristic-Optimised LSTM with SHAP-Based Explainability

[![Python](https://img.shields.io/badge/Python-3.8%2B-blue)](https://python.org)
[![TensorFlow](https://img.shields.io/badge/TensorFlow-2.x-orange)](https://tensorflow.org)
[![License: MIT](https://img.shields.io/badge/License-MIT-green)](LICENSE)

> **Paper:** *Remaining Useful Life Prediction of Turbofan Engines Using
> Metaheuristic-Optimised LSTM with SHAP-Based Explainability*  
> Sultan Zeybek, Büşra Öztürk

---

## Overview

This repository contains the full source code and experimental notebooks
for the paper. The proposed framework integrates:

- **LSTM-based RUL prediction** on the NASA C-MAPSS benchmark (FD001–FD004)
- **Genetic Algorithm (GA)** and **Particle Swarm Optimisation (PSO)** for
  automated LSTM hyperparameter search
- **SHAP (KernelExplainer)** for sensor-level feature attribution linked to
  turbofan fault modes
- **Multi-run statistical validation** (Shapiro-Wilk, Levene, ANOVA/Kruskal-Wallis,
  Tukey HSD / Dunn–Bonferroni)

---

## Repository Structure

```
turbofan-rul-lstm-shap/
├── turbofan_rul_lstm_shap.ipynb   # Main notebook (all experiments)
├── requirements.txt               # Python dependencies
├── data/
│   └── CMAPSS/                    # Place NASA C-MAPSS .txt files here
│       ├── train_FD001.txt
│       ├── test_FD001.txt
│       ├── RUL_FD001.txt
│       └── ... (FD002, FD003, FD004)
├── outputs/                       # Generated figures and CSV results
└── README.md
```

---

## Dataset

The NASA C-MAPSS dataset is publicly available at:  
https://data.nasa.gov/Aerospace/CMAPSS-Jet-Engine-Simulated-Data/ff5v-kuh6

Download the six `.txt` files and place them in `data/CMAPSS/`.

| Sub-dataset | Training engines | Test engines | Operating conditions | Fault modes |
|-------------|-----------------|--------------|----------------------|-------------|
| FD001       | 100             | 100          | 1                    | 1 (HPC)     |
| FD002       | 260             | 259          | 6                    | 1 (HPC)     |
| FD003       | 100             | 100          | 1                    | 2           |
| FD004       | 248             | 249          | 6                    | 2           |

---

## Installation

```bash
git clone https://github.com/YOUR_USERNAME/turbofan-rul-lstm-shap.git
cd turbofan-rul-lstm-shap
pip install -r requirements.txt
```

### Requirements

```
tensorflow>=2.10
numpy>=1.21
pandas>=1.3
scikit-learn>=1.0
shap>=0.41
matplotlib>=3.5
scipy>=1.7
scikit-posthocs>=0.7
```

---

## Usage

### Local

```bash
jupyter notebook turbofan_rul_lstm_shap.ipynb
```

Set `DATA_DIR` in **Section 2 (Configuration)** to the folder containing
the C-MAPSS `.txt` files.

### Google Colab

Open the notebook in Colab and update `DATA_DIR` to your Google Drive path:

```python
from google.colab import drive
drive.mount('/content/drive')
DATA_DIR = '/content/drive/MyDrive/path/to/CMAPSS'
```

---

## Reproducing Paper Results

| Step | Notebook section |
|------|-----------------|
| Data preprocessing | Section 3 |
| PSO optimisation (FD001) | Section 5, 7 |
| GA optimisation (FD001) | Section 6, 7 |
| Multi-run validation (FD001–FD004) | Section 8 |
| Summary table (Table 10 in paper) | Section 9 |
| Statistical tests | Section 10 |
| Convergence plots (Figure 5) | Section 11 |
| Box plots (Figures 6–9) | Section 12 |
| SHAP analysis (Figures 10–17) | Section 13 |

> **Computational note:** Full optimisation on FD004 requires up to 185 minutes
> on a single GPU (Google Colab T4). The `OPT_EPOCHS` parameter in
> Section 2 can be reduced for faster preliminary runs.

---

## Key Results

| Dataset | Model    | Mean RMSE | CV (%) |
|---------|----------|-----------|--------|
| FD001   | PSO-LSTM | 15.107    | **2.60** |
| FD001   | Bi-LSTM  | **14.861** | 3.46  |
| FD003   | **GA-LSTM**  | **14.841** | **7.14** |
| FD004   | Bi-LSTM  | **34.286** | **5.71** |

PSO-LSTM achieves the lowest run-to-run variance on FD001 (CV = 2.60%),
while GA-LSTM provides the best accuracy and stability on FD003 (lowest
mean RMSE, CV = 7.14%), providing empirical support for the No Free Lunch theorem.

---

## Citation

If you use this code, please cite:

```bibtex
@article{zeybek2025rul,
  author  = {Zeybek, Sultan and \"{O}zt\"{u}rk, B\"{u}\c{s}ra},
  title   = {Remaining Useful Life Prediction of Turbofan Engines Using
             Metaheuristic-Optimised {LSTM} with {SHAP}-Based Explainability},
  journal = {Pattern Analysis and Applications},
  year    = {2025}
}
```

---

## License

This project is licensed under the MIT License — see [LICENSE](LICENSE) for details.

---

## Contact

**Sultan Zeybek**  
Department of Artificial Intelligence and Data Engineering  
Istanbul University, Istanbul, Turkey  
sultan.zeybek@istanbul.edu.tr

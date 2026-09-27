# Heart Disease Prediction: Logistic Regression vs. Machine Learning

![Python](https://img.shields.io/badge/Python-3.10%2B-3776AB?logo=python&logoColor=white)
![statsmodels](https://img.shields.io/badge/statsmodels-econometrics-4B8BBE)
![scikit-learn](https://img.shields.io/badge/scikit--learn-ML-F7931E?logo=scikitlearn&logoColor=white)

An applied econometrics project that predicts the **presence** and **severity** of heart disease from patients' clinical characteristics, comparing interpretable logistic regression (binary and multinomial) with Random Forest and Gradient Boosting.

Built for the *Advanced Econometrics* course, MSc in Applied Statistics and Data Science, Bucharest University of Economic Studies (ASE).

**Data:** [Heart Disease UCI Dataset (Kaggle)](https://www.kaggle.com/datasets/redwankarimsony/heart-disease-data) — 920 patients, 16 variables.

---

## Objective

The project answers two distinct clinical questions:

| Part | Question | Target | Problem type |
|---|---|---|---|
| Part I | Does the patient have heart disease? | 0 / 1 | Binary classification |
| Part II | How severe is the disease? | 0, 1, 2, 3, 4 | Multinomial classification |

In both parts, logistic regression (the main econometric model) is benchmarked against Random Forest and Gradient Boosting to show the trade-off between **interpretability** and **predictive performance**.

---

## Key results

### Part I — Binary classification (test set)

| Model | Accuracy | F1 | ROC AUC |
|---|---|---|---|
| Random Forest | **0.848** | **0.864** | **0.931** |
| Gradient Boosting | 0.832 | 0.852 | 0.912 |
| Logistic Regression | 0.815 | 0.830 | 0.905 |

**Strongest predictors** in the logistic regression (standardized features, p < 0.01):

| Feature | Description | Odds ratio |
|---|---|---|
| `major_vessels` | Number of major vessels colored by fluoroscopy | 2.26 |
| `st_depression` | ST depression induced by exercise | 1.96 |
| `thalassemia` | Thalassemia test result | 1.86 |
| `colesterol_ridicat` | Engineered flag: cholesterol above 240 mg/dl | 1.82 |
| `sex` | Male patients have higher risk | 1.73 |

### Part II — Multinomial classification

Performance drops sharply on the 5-class severity task (accuracy ≈ 48%, macro F1 ≈ 0.34). Distinguishing severity levels 1–4 is intrinsically hard because the classes are heavily imbalanced (class 4 has only 28 patients), and roughly 45% of patients belong to class 0.

---

## Conclusions

- **Binary logistic regression is the most suitable model for clinical screening.** It combines strong performance (AUC > 0.90) with clinically meaningful odds ratios.
- **Tree-based models win only marginally** (+0.026 AUC for Random Forest) at the cost of transparency — a worthwhile trade-off only when interpretability is not a priority.
- **Exercise-test indicators** (`st_depression`, `max_heart_rate`) are more informative than static measurements (`cholesterol`, `resting_bp`).
- **Asymptomatic patients show the highest risk**, consistent with the "silent" presentation of heart disease described in the medical literature.

---

## Methodology

1. **Setup** — imports and global configuration
2. **Data loading** — CSV import, column renaming
3. **Exploratory data analysis** — distributions, correlations, visualizations
4. **Missing values** — feature-specific strategy (median / mode / new "missing" category)
5. **Feature engineering** — derived variables `colesterol_ridicat` (high cholesterol flag) and `scor_risc` (risk score)
6. **Encoding and standardization** — label encoding, feature scaling
7. **Part I — Binary classification**
   - Logistic regression with `statsmodels` (full statistical summary)
   - Multicollinearity check with VIF
   - Test-set evaluation: accuracy, precision, recall, F1, ROC AUC
   - Comparison with Random Forest and Gradient Boosting
   - Error analysis (false positives vs. false negatives)
   - Coefficient interpretation (log-odds and odds ratios)
8. **Part II — Multinomial classification**
   - Multinomial logistic regression
   - Comparison with ML models
9. **Final conclusions**

---

## Limitations

- Results are based on a single train/test split; cross-validation would give more robust estimates.
- The multinomial model barely outperforms a majority-class baseline; class weighting, resampling (e.g., SMOTE) or an ordinal model would be natural next steps, since severity is an ordered outcome.
- The UCI data was collected decades ago from four hospitals, so the model is **not intended for real clinical use** without validation on contemporary data and medical approval.
- Notebook commentary and some engineered feature names are in Romanian; this README summarizes them in English.

---

## Repository structure

```
.
├── Proiect Econometrie Avansata.ipynb   # Main notebook with the full analysis
├── heart_disease_uci.csv                # Dataset (920 patients, 16 variables)
├── requirements.txt                     # Pinned Python dependencies
├── .gitignore
└── README.md
```

## How to run

**Prerequisites:** Python 3.10+, Git, and Jupyter, VS Code or PyCharm.

```bash
# 1. Clone the repository
git clone https://github.com/ruximarian/Proiect-Econometrie-Avansata.git
cd Proiect-Econometrie-Avansata

# 2. Create and activate a virtual environment
python -m venv .venv
.\.venv\Scripts\activate        # Windows (PowerShell)
source .venv/bin/activate       # macOS / Linux

# 3. Install dependencies (pinned versions, ~2–3 minutes)
pip install -r requirements.txt

# 4. Launch Jupyter
jupyter notebook
```

Open `Proiect Econometrie Avansata.ipynb` and select **Kernel → Restart & Run All**. In VS Code or PyCharm, select the `.venv` interpreter as the notebook kernel.

## Tech stack

| Library | Version | Purpose |
|---|---|---|
| pandas | 3.0.3 | Tabular data manipulation |
| numpy | 2.4.6 | Numerical operations |
| matplotlib | 3.10.9 | Plotting |
| seaborn | 0.13.2 | Statistical visualization |
| scikit-learn | 1.8.0 | Random Forest, Gradient Boosting, metrics |
| statsmodels | 0.14.6 | Logistic regression with statistical inference |
| jupyter | 1.1.1 | Notebook environment |

## Authors

**Ruxandra-Elena Mărian** — [GitHub](https://github.com/ruximarian)
**Diana-Maria Lazăr**
MSc Applied Statistics and Data Science, Bucharest University of Economic Studies (ASE)

*This project was completed for educational purposes.*

# Advanced Causal Inference: Estimating Treatment Effects using Doubly Robust Methods

This project implements two advanced causal inference estimators—**Double Machine Learning (DML)** and **Targeted Maximum Likelihood Estimation (TMLE)**—to estimate the Average Treatment Effect (ATE) and a subgroup Conditional Average Treatment Effect (CATE) using a synthetic dataset designed to mimic confounded observational data.

The goal is to obtain robust causal estimates even when nuisance models (propensity or outcome models) may be misspecified, and to perform diagnostic checks such as covariate balance and overlap evaluation.

---

## 📁 Project Structure

```text
project-root/
│
├── dml_tml_project.ipynb          # Main executed notebook with all outputs
├── synthetic_causal_dataset.csv   # Synthetic dataset generated in notebook
├── results_summary.json           # Saved ATE/CATE estimates, SEs, and CIs
├── README.md                      # Project summary (this file)
└── plots/
      ├── dist_x1_by_treatment.png
      ├── dist_x2_by_treatment.png
      ├── propensity_distribution.png
      ├── smd_before_after.png
      ├── ate_comparison.png
      └── other diagnostic plots saved during execution
```

---

## 🚀 Methods Implemented

### 🔹 1. Double Machine Learning (DML)
- Uses **cross-fitting** with K-folds.
- Separately estimates:
  - Outcome regression: \(\hat m(W, A)\)
  - Propensity score: \(\hat g(W)\)
- Uses orthogonal scores for robustness.
- Final ATE computed via residual-on-residual regression.
- Standard errors computed using the influence function.

### 🔹 2. Targeted Maximum Likelihood Estimation (TMLE)
- Fits initial outcome regression \(Q(A,W)\)
- Fits propensity model \(g(W)\)
- Performs **logistic fluctuation / targeting step**
- Produces doubly robust, asymptotically efficient ATE estimate
- Computes SE and 95% CI using influence function

---

## 📊 Diagnostics Performed

- **Covariate balance before and after weighting**  
  → SMD plot: `smd_before_after.png`

- **Propensity score distribution**  
  → `propensity_distribution.png`

- **EDA distributions of covariates by treatment**  
  → `dist_x1_by_treatment.png`, `dist_x2_by_treatment.png`

- **ATE comparison plot (DML vs TMLE)**  
  → `ate_comparison.png`

These diagnostics help assess overlap, confounding structure, and robustness.

---

## 📈 Outputs

All final numeric results are stored in:

```
results_summary.json
```

This file includes:

- `dml_ate`, `dml_se`, `dml_ci`
- `tmle_ate`, `tmle_se`, `tmle_ci`
- `sub_dml_ate`, `sub_dml_se`, `sub_dml_ci`
- `sub_tmle_ate`, `sub_tmle_se`, `sub_tmle_ci`

The notebook (`dml_tml_project.ipynb`) prints all estimates and shows the diagnostic plots.

---

## 🎯 Subgroup Analysis (CATE)

The subgroup chosen for CATE estimation is:

```
x1 > 0
```

Both DML and TMLE are used to estimate the treatment effect within this subgroup.  
Interpretation and reasoning for subgroup choice are included in the full `report.md`.

---

## 🧠 Purpose & Learning Objectives

This project demonstrates:

- Doubly robust causal inference methods  
- Use of machine learning models inside causal estimators  
- Cross-fitting for bias reduction  
- Targeting step of TMLE  
- Diagnostics for balance & overlap  
- Subgroup treatment effect interpretation  
- Sensitivity reasoning for real-world causal studies  

---

## 💻 How to Run

1. Open the notebook:

```
dml_tml_project.ipynb
```

2. Install dependencies (if needed):

```bash
pip install numpy pandas matplotlib seaborn scikit-learn statsmodels
```

3. Run all cells.  
All plots and `results_summary.json` will be regenerated automatically inside the `plots/` folder.

---

## 📄 Additional Report

A full written analysis including:
- model assumptions  
- ATE results interpretation  
- robustness checks  
- subgroup CATE analysis  
- causal interpretation  

is provided in **report.md**.

---

## 👤 Author

**Rajesh M**


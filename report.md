# Report — Advanced Causal Inference: DML & TMLE

## 1. Project overview
This project estimates the Average Treatment Effect (ATE) and Conditional Average Treatment Effect (CATE) using two doubly-robust methods:
- **Double Machine Learning (DML)** with cross-fitting
- **Targeted Maximum Likelihood Estimation (TMLE)**

We use a synthetic dataset (`synthetic_causal_dataset.csv`) that simulates a randomized-looking experiment with confounding via a propensity model. The goal is to estimate the causal effect of `treatment` on binary `outcome` and evaluate robustness and diagnostics.

---

## 2. Data & EDA
- Observations: **N = [REPLACE_N]**  
- Treatment rate (mean): **[REPLACE_TREAT_RATE]**

Key EDA performed:
- Covariate distributions by treatment (plots: `dist_x1_by_treatment.png`, `dist_x2_by_treatment.png`)  
- Propensity score distribution (plot: `propensity_distribution.png`)  
- Standardized mean differences before and after weighting (plot: `smd_before_after.png`)

Pre-adjustment standardized mean differences (SMD):
- x1: [REPLACE_SMD_x1_pre]  
- x2: [REPLACE_SMD_x2_pre]  
- x3: [REPLACE_SMD_x3_pre]  
- x4: [REPLACE_SMD_x4_pre]

Post-weighting SMDs (IPW stabilized):  
- x1: [REPLACE_SMD_x1_post]  
- x2: [REPLACE_SMD_x2_post]  
- x3: [REPLACE_SMD_x3_post]  
- x4: [REPLACE_SMD_x4_post]

Interpretation: describe whether balance improved and whether overlap is adequate.

---

## 3. Methods (implementation details)
**Nuisance models**:
- Propensity (`g`) model: `LogisticRegression` (or `GradientBoostingClassifier`) — predicted probability saved in `propensity`.  
- Outcome (`m` or `Q`) model: `GradientBoostingRegressor` (or appropriate learner).

**DML**:
- K-fold cross-fitting (K = 5).  
- Saved m_hat and g_hat for all observations.  
- Final ATE estimated by regressing residualized outcome on residualized treatment (closed-form influence-based SE reported).

**TMLE**:
- Initial Q(A,W) estimated on full data by `GradientBoostingRegressor`.
- Propensity g fit on full data.
- Logistic fluctuation (epsilon) fitted via a GLM with offset = logit(Q).
- Targeted estimates Q1*, Q0* computed; TMLE ATE = mean(Q1* - Q0*).
- Influence-function-based SE for TMLE computed.

All code is contained in `dml_tml_project.ipynb` and produces saved plots and `results_summary.json`.

---

## 4. Main results (from results_summary.json)
**DML estimate**  
- ATE (DML): **[DML_ATE]**  
- SE (DML): **[DML_SE]**  
- 95% CI (DML): **([DML_CI_LOW], [DML_CI_HIGH])**

**TMLE estimate**  
- ATE (TMLE): **[TMLE_ATE]**  
- SE (TMLE): **[TMLE_SE]**  
- 95% CI (TMLE): **([TMLE_CI_LOW], [TMLE_CI_HIGH])**

Interpretation:
- If point estimates are similar and CIs overlap → estimates are robust to modeling choices.
- If they differ substantially → discuss possible reasons (nuisance model misspecification, lack of overlap).

Plot: `plots/ate_comparison.png`.

---

## 5. Subgroup analysis (CATE) — subgroup defined as `x1 > 0`
**Subgroup DML**  
- ATE_sub (DML): **[SUB_DML_ATE]**  
- SE_sub: **[SUB_DML_SE]**  
- 95% CI: **([SUB_DML_CI_LOW], [SUB_DML_CI_HIGH])**

**Subgroup TMLE**  
- ATE_sub (TMLE): **[SUB_TMLE_ATE]**  
- SE_sub: **[SUB_TMLE_SE]**  
- 95% CI: **([SUB_TMLE_CI_LOW], [SUB_TMLE_CI_HIGH])**

Causal interpretation: summarize whether treatment effect is larger/smaller in subgroup `x1>0`, plausibility and business meaning.

---

## 6. Diagnostics & robustness checks
- Covariate balance before/after IPW: see `smd_before_after.png`. Comment on remaining imbalances.  
- Propensity overlap: see `propensity_distribution.png`. If mass near 0 or 1, note limited overlap and consider trimming.  
- Sensitivity notes: if propensity model misspecified, ATE could be biased; DML/TMLE are doubly-robust but still rely on at least one nuisance model being well-specified.

---

## 7. Practical recommendations
- If DML and TMLE agree → confident in ATE; consider targeted rollout to subgroup with positive CATE.  
- If disagreements → investigate nuisance model choices, try alternative learners (e.g., random forests), trimming or reweighting, and run sensitivity analyses.

---

## 8. Files included
- `dml_tml_project.ipynb` — notebook with runnable code and visible outputs  
- `synthetic_causal_dataset.csv` — dataset  
- `results_summary.json` — numeric outputs  
- `plots/` — all saved diagnostic & result plots  
- `report.md` — this document  

---

## 9. Author
**Rajesh M**


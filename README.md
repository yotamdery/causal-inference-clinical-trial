# Causal Inference in Clinical Trial

**Author**: Yotam Dery  
**Date**: July 27, 2025  
**Assignment Provider**: PhaseV Trials Inc.

---

## 📌 Problem Statement

You are given a clinical trial dataset with:
- Covariates/features `X1` to `X15`
- A binary treatment indicator `W`
- A continuous outcome `Y`

The data is modeled as:

\[
Y = g(X) + W \cdot \tau(X) + \varepsilon
\]

Where:
- `g(X)`: Prognostic term — expected outcome without treatment
- `τ(X)`: Predictive term — treatment effect heterogeneity
- `W`: Treatment indicator
- `ε`: Noise

---

## 🎯 Objectives

1. Identify features that contribute to **g(X)** and **τ(X)**
2. Estimate the contributions of these covariates
3. Justify modeling choices
4. Interpret results and discuss limitations

---

## 🧠 Analytical Approach

- **EDA**: Feature distributions, outliers, and balance checks across treatment groups.
- **Propensity Score Estimation**:
  - Logistic Regression to estimate P(W=1|X)
  - AUC ≈ 0.48 → Treatment assignment nearly random
- **IPW (Inverse Probability Weighting)**:
  - Used Horvitz–Thompson estimator to calculate ATE
  - Estimated ATE ≈ **5.70**
- **Modeling g(X)**:
  - Trained XGBoost on control group (W=0)
  - Identified top prognostic features (X5, X4, X2)
- **Modeling τ(X)**:
  - T-Learner (train separate models on W=1 and W=0)
  - Estimated ITEs: τ̂(X) = Ŷ₁(X) - Ŷ₀(X)
  - Trained new model on τ̂(X) to extract predictive features
- **Interpretation**:
  - Used SHAP to explain τ̂(X)
  - Visual comparison of importance for g(X) vs τ(X)

---

## 📊 Key Results

- **g(X)** Top Features: X5, X4, X2, X1
- **τ(X)** Top Features: X5, X6
- **ATE (IPW-adjusted)**: ~ **5.70**
- **SHAP** confirmed the role of X5 and X6 in treatment effect heterogeneity

---

## ✅ Modeling Choices Justified

- **XGBoost**: Robust to non-linearity, feature interactions, and missing values
- **SHAP**: Transparent, local + global explanations of feature contributions
- **IPW**: Simple and efficient, preserves full dataset, appropriate given balance
- **T-Learner**: Interpretable way to model heterogeneous treatment effects

---

## ⚠️ Limitations & Future Work

- Dataset was **small** (n ≈ 500) → limits statistical power and subgroup analysis
- No cross-validation used for XGBoost models
- No confidence intervals on ATE or τ̂(X)
- Future directions:
  - Add cross-validation and regularization
  - Estimate variance and CIs (e.g., bootstrapping)
  - Explore doubly robust methods (EconML, CausalML)

---

## 🖥️ Files

- `main.ipynb` — Full notebook with code, visualizations, and SHAP analysis
- `PhaseV_Causal_Inference_Deck_Updated.pptx` — Slide deck for presentation
- `simulated_data.csv` — Input dataset (assumed to be provided)

---

## 📬 Contact

For any follow-up or explanation, please reach out via GitHub or LinkedIn.


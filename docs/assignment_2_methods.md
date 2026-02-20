# Disclosure Timing and Market Reactions to Data Breaches: Research Design Document

## 1. Research Overview

This research asks if how quickly a company discloses a data breach will affect the stock market's reaction. I used an event study design that was built around a natural experiment. It was the FCC's 2007 rule that required communications companies to disclose breaches within seven days while other industries follow state laws that allow anywhere from 30 to 90 days. That regulatory gap gives me a clean way to test the timing effects. OLS regression handles the four main hypothesis tests and machine learning (random forests and gradient boosting) checks for nonlinear patterns that OLS might miss.

## 2. Data Description and Variables

### Data Sources and Sample

I pulled the breach records from the Privacy Rights Clearinghouse, stock returns from CRSP, and firm financials from Compustat. After filtering for breaches that had complete fields in all required fields (report date, start date, resolution date, records affected, and stock ticker), there were 1,054 breaches between 2006 and 2025 left. Matching them to CRSP trading data left 926 breaches (87.9% match rate). Including financial controls reduced the regression sample to 898 due to 28 firms not having Compustat data (3% of the sample). The primary analysis covers 2007–2023, when the FCC's seven-day rule was in effect.

Of the 898 observations, 200 involve FCC-regulated firms, 198 were disclosed within seven days, and 117 involved health data. The mean 30-day CAR is −0.74%, ranging from −42.56% to +34.05%.

### Unit of Analysis

Each observation is a single breach at a single firm, with financial characteristics measured in the fiscal year prior to the breach.

### Dependent Variable

The dependent variable is the 30-day cumulative abnormal return (CAR), calculated using a market model estimated over a 120-day pre-breach window. Abnormal returns are the difference between actual and expected returns; cumulative abnormal returns sum these residuals from day 0 (disclosure date) through day 30.

### Independent Variables

- **Immediate Disclosure (H1)**: Binary indicator, 1 if disclosed within seven days. Expected to reduce market penalty. 198 breaches (19%) meet this criterion.
- **FCC Regulated (H2)**: Binary indicator, 1 if FCC-regulated firm. Expected to worsen reaction. 200 breaches (22%) involve FCC firms.
- **Prior Breaches (H3)**: Count of prior breaches. Expected to increase penalty. 42% of firms have prior breaches.
- **Health Data Breach (H4)**: Binary indicator, 1 if HIPAA-covered health information. Expected to worsen reaction. 117 breaches (11%) involve health data.

### Control Variables

Firm size (log assets), Leverage (debt/assets), ROA (operating income/assets), all from prior fiscal year.

## 3. Feature Engineering and Data Preprocessing

### Feature Creation

Engineered features: disclosure lag, breach complexity (data types × records), and crisis indicators (2008–09, 2020–21). Outliers where CAR exceeds ±3 SD are flagged for sensitivity testing.

### Feature Selection

All variance inflation factors < 2.0. OLS uses seven variables; ML uses full variable set with importance rankings compared to OLS significance.

### Transformations

Firm size log-transformed; continuous variables standardized for ML; categorical variables one-hot encoded. Class weighting tested in RF for imbalanced disclosure timing (19%).

## 4. Model Selection and Justification

### OLS Regression (Primary Causal Model)

OLS with HC3 standard errors is the primary approach. Models 1–4 test each hypothesis separately; Model 5 combines all effects. The FCC's rule functions as a natural experiment—regulatory status is determined by SIC code, not breach characteristics—enabling causal interpretation.

### Random Forest (ML Validation)

Random Forest validates regressions and captures nonlinear relationships OLS misses. If relationships are primarily linear, RF should not substantially exceed OLS.

### Gradient Boosting (Alternative ML)

Gradient Boosting verifies findings aren't algorithm-specific. Similar feature importance across methods strengthens confidence.

**Computational Requirements:** OLS <1 second; RF/GB tuning ~15–20 minutes on standard laptop.

## 5. Hyperparameter Tuning Strategy

Randomized search (100 iterations) with 5-fold cross-validation on training set.

**Random Forest:** n_estimators [100, 200, 500], max_depth [10, 20, 30, None], min_samples_split [2, 5, 10], min_samples_leaf [1, 2, 4]

**Gradient Boosting:** n_estimators [100, 200, 500], learning_rate [0.01, 0.05, 0.1], max_depth [3, 5, 7], subsample [0.7, 0.8, 1.0]

## 6. Validation Strategy

ML: 5-fold stratified cross-validation, 70% training / 15% validation / 15% testing. OLS: full sample with clustered SEs. Data leakage prevented by pre-breach market model window, prior-year financial variables, and training-set-only feature engineering.

## 7. Baseline Model and Evaluation Metrics

### Baseline

Logistic regression predicting CAR sign (positive/negative) using only controls. Expected: 55–60% accuracy, ROC-AUC ≈ 0.55.

### Evaluation

OLS: Coefficient direction/magnitude, p-values, CI, adjusted R². ML: out-of-sample R², RMSE, MAE. Baseline: Precision, Recall, F1, ROC-AUC.

### Success Criteria

(1) ≥2 of 4 hypotheses significant (p<0.05); (2) RF R² > 0.15; (3) ML feature importance aligns with OLS significance; (4) Findings stable across alternative windows/definitions.

## 8. Model Interpretability

OLS coefficients: −2.2% FCC coefficient means FCC breaches experience 2.2pp worse reactions. RF feature importance (0–100%) shows relative contribution. SHAP values reveal how features push individual predictions above/below mean.

## 9. Project Timeline

All milestones completed January 2026.

| Milestone | Timeline | Deliverable |
|-----------|----------|-------------|
| Data Collection | Week 1–2 | Clean matched dataset |
| EDA | Week 3 | EDA notebook |
| Market Model & OLS | Week 4–5 | CARs; Models 1–5 |
| ML Validation | Week 5–6 | RF/GB trained, tuned |
| Robustness Testing | Week 6–7 | Alternative specifications |
| Documentation | Week 7–8 | Complete analysis, code, docs |

## 10. Limitations and Mitigation

**Limitations:** Sample restricted to public firms (N=926); 30-day window misses longer effects; timing measure may not reflect true lag; unobserved firm differences.

**Mitigation:** Matched vs. unmatched comparisons; alternative event windows (5d, 30d BHAR); multiple SE specs (HC3, clustered); timing thresholds (3–90 days); fixed effects; day-0 placebo test.

## 11. Reproducibility

Python 3.11+ with scikit-learn, pandas, statsmodels, SHAP. Seeds: 42. Notebooks: 01_data_collection, 02_eda, 03_market_model, 04_ols_regression, 05_ml_validation, 06_robustness_checks. Dependencies via pyproject.toml, uv.lock. Data: `data/raw/` (documented), `data/processed/` (with dictionary). Git tracking with descriptive commits.

---

**Last Updated:** February 20, 2026

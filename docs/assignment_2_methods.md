# Disclosure Timing and Market Reactions to Data Breaches: Research Design Document

This assignment is the Methods section of my first essay, just simplified.

## 1. Research Overview

This study asks if how quickly a firm (I chose this word over company intentionally) discloses a data breach will affect the stock market's reaction, which extends Myers and Majluf's (1984) information asymmetry framework to cybersecurity disclosure decisions. I used an event study design that was built around a natural experiment: the FCC's 2007 rule requiring telecommunication firms to disclose within 7 days, while other industries follow state laws that usually allow anywhere from 30–90 days. That regulatory gap gave me a clean way to test timing effects, similar to the quasi-natural experiment approach used by Cao et al. (2024). OLS regression handles the four main hypothesis tests, and machine learning checks for non-linear patterns that OLS might miss.

## 2. Data Description and Variables

### Data Sources and Sample

I pulled the information for this research from three sources: Privacy Rights Clearinghouse (1,054 breaches), a database validated in prior breach research (Edwards et al., 2016), CRSP (stock returns), and Compustat (firm financials). After matching the firms with trading data, 926 breaches remained; adding financial controls dropped 28 breaches that had missing Compustat data, which left 898 for the regression. The sample covered 2007–2023 — Out of the 898 breaches in the sample, 200 involved FCC-regulated firms, 198 were disclosed within 7 days, and 117 involved health data. The mean 30-day cumulative abnormal return is -0.74%, ranging from -42.56% to +34.05%.

### Unit of Analysis

Each observation in this research is a single breach at a single firm, with financial characteristics measured in the fiscal year before the breach.

### Variables and Expected Relationships

The Dependent Variable is the 30-day cumulative abnormal return (CAR) from a market model with a 120-day pre-breach estimation window.

The Independent Variables are - Immediate Disclosure (H1) (19%): Expected to reduce the penalty — fast disclosure signals transparency, consistent with Spence's (1973) signaling framework where lower-cost signals convey higher quality. FCC Regulated (H2) (22%): Expected to worsen the reaction — mandatory deadlines may signal severity to investors. Prior Breaches (H3) (mean = 3.36): Expected to increase the penalty — repeat breaches read as governance failure. Health Data Breach (H4) (11%): Expected to worsen the reaction — extra regulatory and reputational exposure driven by heightened stakeholder urgency and legitimacy (Mitchell et al., 1997). The controls for this research are: Firm size (log assets), leverage (debt/assets), ROA (prior fiscal year). I also test an FCC × Immediate Disclosure interaction.

## 3. Feature Engineering and Data Preprocessing

### Feature Creation

I engineered several additional features that included disclosure lag measured as days from discovery to announcement, breach complexity calculated as the interaction of data types and records affected, and crisis-period indicators for 2008–09 and 2020–21. Outliers where the cumulative abnormal return exceeded three standard deviations were flagged for sensitivity testing.

### Feature Selection

All variance inflation factors (VIF) stayed below 2.0. OLS used 7 variables while the machine learning models included all available features. PCA was only tested if it explained more than 85% of the variance and actually improved Random Forest performance.

### Transformations

Firm size is log-transformed, and continuous variables are standardized to a mean of zero and standard deviation of one for the machine learning models, while categorical variables are one-hot encoded. The 28 missing Compustat observations were dropped through listwise deletion, which accounted for about 3% of the sample. Because only 19% of breaches qualified as immediate disclosure, I tested class weighting in the Random Forest to account for that imbalance.

## 4. Model Selection and Justification

### OLS Regression (Causal Model)

OLS regression with firm-clustered standard errors served as the primary analytical tool. Models 1 through 4 tested each hypothesis individually, while Model 5 combined all four into a single specification. OLS is appropriate here because the natural experiment supports interpreting the coefficients as causal effects rather than just associations, following the event study methodology established in prior breach disclosure research (Gordon et al., 2024; Tsang et al., 2023).

### Assumptions and Verification

I verified OLS assumptions by checking linearity through partial regression plots, constant variance using Breusch-Pagan tests with HC3 and clustered standard errors, and multicollinearity by confirming all VIFs remain below 2.0. Residual normality was assessed with Q-Q plots and Shapiro-Wilk, and influential observations were identified using Cook's D. Omitted variable concerns were addressed through robustness checks and fixed effects.

### Random Forest (ML Validation)

The Random Forest started with 500 trees and was tuned via grid search over the number of estimators (100, 200, 500), maximum depth (5, 10, 20, unconstrained), minimum samples to split (2, 5, 10), and minimum samples per leaf (1, 2, 4). I chose Random Forest because it captures non-linear patterns and interactions automatically, produces feature importance rankings comparable to the regression results, and handles outliers well. If the relationships are truly linear, Random Forest should not meaningfully outperform OLS, which is itself useful information about the data's structure.

### Gradient Boosting (Alternative ML)

Gradient Boosting was tuned across the number of estimators at 100, 200, and 500, learning rates at 0.01, 0.05, and 0.1, and maximum depth at 3, 5, 7, and 10. The purpose of including it was to see whether a fundamentally different algorithm reached similar conclusions as the Random Forest.

### Computational Requirements

OLS ran in under a second. Full grid search for RF and GB took about 15–20 minutes on a standard laptop.

## 5. Validation Strategy

The machine learning models used 5-fold stratified cross-validation, maintaining similar proportions of FCC-regulated, health data, and timing variables in each fold. The data was split into 70% training (649 observations), 15% validation (139), and 15% testing (138). OLS used the full sample with clustered standard errors. Data leakage was not a concern because the estimation window ended before the breach, financial controls came from the prior year, and feature engineering was fit only on training data. I also compared the 926 matched breaches against the 128 unmatched breaches on firm characteristics and applied propensity-score weighting where the two groups differed significantly at the p < 0.05 level, consistent with the matching approaches used in breach disclosure studies (Cao et al., 2024).

## 6. Baseline and Evaluation Metrics

### Baseline Model

The baseline model was a logistic regression that predicted whether the cumulative abnormal return was positive or negative, using only the three control variables. The expected accuracy was between 55 and 60 percent, with a ROC-AUC around 0.55 to 0.60, which is barely better than a coin flip. That set the bar for any improvement from adding timing and regulatory variables. Performance was tracked through accuracy, precision, recall, F1-score, and the confusion matrix.

### Evaluation Metrics and Success Criteria

For OLS, I evaluated results based on coefficient size and direction, p-values, 95% confidence intervals, R², and residual diagnostics. For the machine learning models, the primary metric was out-of-sample R², supplemented by RMSE, MAE, and feature importance rankings.

I considered this analysis successful if at least two of the four hypotheses were significant at p < 0.05, the Random Forest out-of-sample R² exceeded 0.15, and the ML feature importance rankings aligned with what the regression identifies as significant. The results should also hold across alternative event windows, and the H1 timing effect stayed under 1% and remained non-significant across all seven threshold definitions.

## 7. Model Interpretability

OLS coefficients were directly interpretable, while RF feature importance on a 0–100% scale showed each variable's relative contribution to predictions. SHAP values on 200 test observations revealed how features push individual predictions up or down. The key check was whether the ML rankings matched what the regression identifies as significant.

## 8. Project Timeline

All data collection, preprocessing, exploratory analysis, model development, and robustness testing were completed as of January 2026 as part of my dissertation's first essay. The full pipeline took roughly 4–6 weeks, with OLS and ML work running in parallel.

## 9. Limitations and Robustness Mitigation

### Limitations

The sample covers only publicly traded firms (N = 926). The 30-day window captures initial reactions but misses longer-term effects. The timing measure (reported discovery to announcement) may not reflect the true lag. And while the FCC rule provides a useful natural experiment, telecom firms could differ from other industries in ways beyond disclosure speed, a concern echoed in research showing firms strategically time breach announcements to reduce market penalties (Foerderer & Schuetz, 2022).

### Mitigation Strategies

To mitigate these limitations, I compared matched and unmatched breaches for selection bias, tested alternative event windows including 5-day CAR and 30-day buy-and-hold abnormal returns, and ran multiple standard error specifications including HC3, clustered, and Newey-West. I varied the timing threshold across seven definitions from 3 to 90 days and added time and industry fixed effects to control for unobserved heterogeneity. A placebo test at day zero checked for spurious effects, since no firm can disclose before discovering a breach.

## 10. Reproducibility

### Code and Documentation

The entire analysis runs in Python 3.11 or later using scikit-learn, pandas, and statsmodels, with all code documented and the random seed fixed at 42. The analysis is organized across six notebooks and all package versions are pinned in the requirements file.

### Environment and Archiving

Raw data is stored in the raw data directory with access dates recorded, while processed data is kept in a separate processed directory alongside a variable dictionary. All changes are tracked through Git with meaningful commit messages.

---

## References

Cao, H., Phan, H. V., & Silveri, S. (2024). Data breach disclosures and stock price crash risk: Evidence from data breach notification laws. *International Review of Financial Analysis*, 93, 103164.

Edwards, B., Hofmeyr, S., & Forrest, S. (2016). Hype and heavy tails: A closer look at data breaches. *Journal of Cybersecurity*, 2(1), 3–14.

Foerderer, J., & Schuetz, S. W. (2022). Data breach announcement timing. *Management Science*, 68(10), 7298–7322.

Gordon, L. A., Loeb, M. P., Zhou, L., & Wilford, A. L. (2024). Empirical evidence on disclosing cyber breaches in an 8-K report: Initial exploratory evidence. *Journal of Accounting and Public Policy*, 46, 107226.

Liu, C., & Babar, M. A. (2024). Corporate cybersecurity risk and data breaches: A systematic review of empirical research. *Australian Journal of Management*, 1–31.

Michel, A., Oded, J., & Shaked, I. (2020). Do security breaches matter? The shareholder puzzle. *European Financial Management*, 26(2), 288–315.

Mitchell, R. K., Agle, B. R., & Wood, D. J. (1997). Toward a theory of stakeholder identification and salience: Defining the principle of who and what really counts. *Academy of Management Review*, 22(4), 853–886.

Myers, S. C., & Majluf, N. S. (1984). Corporate financing and investment decisions when firms have information the investors do not have. *Journal of Financial Economics*, 13(2), 187–221.

Spence, M. (1973). Job market signaling. *The Quarterly Journal of Economics*, 87(3), 355–374.

Tsang, R. C. W., Baldwin, A. A., Hair, J. F., Jr., Affuso, E., & Lahtinen, K. D. (2023). The informativeness of sentiment types in risk factor disclosures: Evidence from firms with cybersecurity breaches. *Journal of Information Systems*, 37(3), 157–190.

---

**Last Updated:** February 23, 2026

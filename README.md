# Data Breach Disclosure Timing and Market Reactions

## Project Overview

This research investigates how quickly companies disclose data breaches and whether the timing of disclosure affects stock market reactions. Using an event study design with a natural experiment—the FCC's 2007 rule requiring telecommunications companies to disclose breaches within 7 days—this project analyzes 926 publicly-traded firms to test whether immediate disclosure benefits or harms firm value.

## Research Question

**What factors determine market reactions to data breach disclosures among publicly-traded firms, and does immediate disclosure of breach information benefit or harm firm value?**

### Research Design

- **Methodology**: Event study with natural experiment
- **Sample**: 926 breaches with complete stock market data (2006-2025)
- **Dependent Variable**: 30-day cumulative abnormal return (CAR)
- **Key Hypothesis**: Immediate disclosure (≤7 days) reduces negative market reactions compared to delayed disclosure
- **Approaches**: OLS regression (hypothesis testing) + Machine Learning (Random Forests, Gradient Boosting for validation)

For detailed methodology, see [`docs/assignment_2_methods.md`](docs/assignment_2_methods.md).

## Installation

### Prerequisites

- Python 3.11 or higher
- `uv` package manager ([install here](https://docs.astral.sh/uv/getting-started/installation/))

### Setup Instructions

1. **Clone the repository:**
   ```bash
   git clone <your-repo-url>
   cd BA798_TIM
   ```

2. **Install dependencies:**
   ```bash
   uv sync
   ```

   This will create a virtual environment and install all required packages with locked versions (reproducible environment).

3. **Activate the environment:**
   ```bash
   source .venv/bin/activate  # On Windows: .venv\Scripts\activate
   ```

### Dependencies

All dependencies are managed through `pyproject.toml`:
- **Data Processing**: pandas, numpy, scipy
- **ML/Statistics**: scikit-learn, statsmodels, xgboost
- **Visualization**: matplotlib, seaborn
- **Model Interpretation**: shap
- **Jupyter**: jupyter, ipython

For reproducibility, exact versions are locked in `uv.lock`. Do not modify this file manually.

## Repository Structure

```
.
├── README.md                              # This file
├── pyproject.toml                         # Project metadata & dependencies
├── uv.lock                                # Locked dependency versions
├── .gitignore                             # Git ignore rules
│
├── docs/
│   └── assignment_2_methods.md            # Complete methodology document
│
├── notebooks/
│   ├── 01_data_collection.ipynb          # Data collection & preprocessing
│   ├── 02_eda.ipynb                      # Exploratory data analysis
│   └── README.md                          # Notebook guide
│
├── data/
│   ├── raw/                               # Original, immutable data
│   ├── processed/                         # Cleaned, transformed data
│   └── README.md                          # Data documentation
│
├── src/                                   # Source code
│   ├── __init__.py
│   ├── data/                              # Data loading & preprocessing
│   ├── features/                          # Feature engineering
│   ├── models/                            # Model definitions
│   └── utils/                             # Utility functions
│
└── tests/                                 # Unit tests (optional)
    └── __init__.py
```

## Data Sources

This project integrates four major data sources:

1. **DataBreaches.gov** - Privacy Rights Clearinghouse breach registry
   - 1,054 breaches (2006-2025)
   - Includes disclosure timing, affected populations, incident descriptions
   - Link: https://www.privacyrights.org/data-breaches

2. **CRSP (Center for Research in Security Prices)** - Daily stock data
   - 926 firms with complete stock price data (1999-2025)
   - Used to calculate cumulative abnormal returns (CAR)
   - Requires WRDS institutional access

3. **Compustat** - Annual financial statements
   - Firm financials for control variables (size, leverage, ROA)
   - SIC industry classification for FCC regulatory assignment
   - Requires WRDS institutional access

4. **SEC EDGAR** - Executive officer filings (Form 8-K)
   - Material event disclosures
   - Used for executive turnover analysis
   - Publicly available: https://www.sec.gov/edgar/

**Data Completeness**: See [`data/README.md`](data/README.md) for data dictionary and preprocessing notes.

## Usage

### Run Analysis Pipeline

1. **Data Collection & Preprocessing** (Milestone 1)
   ```bash
   jupyter notebook notebooks/01_data_collection.ipynb
   ```

2. **Exploratory Data Analysis** (Milestone 2)
   ```bash
   jupyter notebook notebooks/02_eda.ipynb
   ```

3. **Event Study & OLS Regression** (Milestones 3-4)
   ```bash
   jupyter notebook notebooks/03_market_model.ipynb
   jupyter notebook notebooks/04_ols_regression.ipynb
   ```

4. **ML Validation** (Milestone 5)
   ```bash
   jupyter notebook notebooks/05_ml_validation.ipynb
   ```

5. **Robustness Checks** (Milestone 6)
   ```bash
   jupyter notebook notebooks/06_robustness_checks.ipynb
   ```

### Run Tests

```bash
pytest tests/
```

## Project Timeline

| Milestone | Target Date | Deliverable |
|-----------|-------------|-------------|
| Data Collection & Preprocessing | Week 1-2 | Clean matched dataset |
| Exploratory Data Analysis | Week 3 | EDA notebook with insights |
| Market Model & OLS Regression | Week 4-5 | CARs; Models 1-5 with results |
| ML Validation | Week 5-6 | Random Forest & Gradient Boosting trained & tuned |
| Robustness Testing | Week 6-7 | Alternative specifications tested |
| Documentation & Finalization | Week 7-8 | Complete analysis, code, documentation |

## Key Results

(To be populated after analysis)

- **H1 (Immediate Disclosure)**:
- **H2 (FCC Regulation)**:
- **H3 (Prior Breaches)**:
- **H4 (Health Data)**:

## Reproducibility

### Code Standards
- Python 3.11+
- All code committed to git with meaningful commit messages
- Jupyter notebooks follow 01_*, 02_* naming convention
- Random seed: 42 (for consistent results)

### Environment Replication
Users can replicate the exact environment by running:
```bash
uv sync
```

This installs the exact versions specified in `uv.lock`.

### Data Documentation
- `data/raw/README.md`: Description of raw data sources and formats
- `data/processed/README.md`: Data dictionary for processed datasets
- Variable definitions documented in [`docs/assignment_2_methods.md`](docs/assignment_2_methods.md)

## Contact Information

**Researcher**: Timothy D. Spivey
**Email**: tspivey@southalabama.edu
**Course**: BA798 - Machine Learning for Business Research
**Institution**: University of South Alabama

## AI Assistance Disclosure

This project used Claude (Anthropic) AI assistance for:
- Repository structure organization and setup
- README.md formatting and structure
- Git workflow and commit messages
- Markdown formatting and documentation layout

All methodology content, research design, citations, and analysis decisions are the original work of Timothy D. Spivey.

---

**Last Updated**: February 2026
**License**: MIT

For questions about the research design, see [`docs/assignment_2_methods.md`](docs/assignment_2_methods.md).
For questions about the code structure, see each module's docstrings.

# Notebooks

Analysis notebooks in execution order:

1. **01_data_collection.ipynb** - Data acquisition and preprocessing
   - Load breach data from Privacy Rights Clearinghouse
   - Match to CRSP stock returns and Compustat financial data
   - Output: Cleaned, merged dataset ready for analysis

2. **02_eda.ipynb** - Exploratory Data Analysis
   - Describe sample characteristics and variable distributions
   - Visualize relationships between key variables
   - Identify patterns and potential outliers
   - Output: Summary statistics and visualizations

## Setup

To run notebooks, ensure dependencies are installed:
```bash
uv sync
uv run jupyter notebook
```

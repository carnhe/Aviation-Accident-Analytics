# Aviation Accident Analysis

Exploratory data analysis (EDA) and insights on aviation accidents. This project examines patterns over time, contributing factors, and severity outcomes to inform safety improvements.

## Project Structure

```
.
├── MA0218_Mini_Project.ipynb        # Main analysis notebook
├── requirements.txt                 # Python dependencies
├── .gitignore                       # Ignore rules for repo hygiene
├── data/
│   ├── README.md                    # Download steps can be found here
│   └── (place datasets here)
└── reports/
    └── figures/                     # Charts
```

## Data
This project uses a synthetic sample dataset (sample_AviationData.csv) created for demonstration purposes, since the original Kaggle dataset is no longer publicly available. The sample file preserves the structure of the original data so the notebook can run without modification.

Data sources referenced in the notebook:
- sample_AviationData.csv - synthetic, generated for demo use only.
- Original Kaggle dataset (no longer available): https://www.kaggle.com/khsamaha/aviation-accident-database-synopses
- https://www.ntsb.gov/Pages/AviationQueryHelp.aspx
- https://stackoverflow.com/questions/17071871/how-do-i-select-rows-from-a-dataframe-based-on-column-values
- https://stackoverflow.com/questions/24251219/pandas-read-csv-low-memory-and-dtype-options
- https://www3.ntu.edu.sg/home/ehchua/programming/howto/Regexe.html#zz-1.4

Note:
The sample_AviationData.csv file is not real accident data and should only be used to reproduce the analysis workflow. For actual aviation accident data, please use the official NTSB database.

## Quickstart

```bash
# 1) Create venv (optional)
python -m venv .venv && source .venv/bin/activate  # on Windows: .venv\Scripts\activate

# 2) Install deps
pip install -r requirements.txt

# 3) Open the notebook
jupyter lab  # or: jupyter notebook
```

## Tech Stack

- Python (pandas, numpy, matplotlib/plotly, scikit-learn as applicable)
- Jupyter Notebook

## What’s inside

1) Data cleaning & preparation
- Standardized categorical fields like Injury.Severity (grouped “Minor” & “Serious” into “Non-Fatal”)
- Reclassified Aircraft.Category and filled missing values with context-based labels (e.g., “NA” for unpowered aircraft)
- Unified Aircraft.Damage categories and merged all public aircraft types
- Cleaned Weather.Condition entries and normalized text
- Adjusted Broad.phase.of.flight values for consistent analysis

2) Exploratory data analysis (EDA)
- Counted number of fatal vs. non-fatal injuries by year
- Measured correlation (Cramér’s V) between injury severity and other categorical variables (e.g., aircraft category, phase of flight, weather condition)
- Summarized injury severity distribution across U.S. states

3) Data Visualizations
- Bar chart of fatal vs. non-fatal injuries by year
- Choropleth map of injury severity by U.S. state
- Heatmap for Cramer’s V correlation between injury severity and other categorical variables

4) Machine Learning
- K-Nearest Neighbors (KNN) – Classification
    Built a classification model to predict injury severity (Fatal vs. Non-Fatal) using encoded categorical variables (e.g., aircraft category, phase of flight, weather condition) and scaled numeric features.
    - Preprocessing: one-hot encoding, missing value imputation, standardization.
    - Tuned optimal k via grid search.
    - Results:
        - Accuracy: 85%
        - Precision: Fatal = 0.62, Non-Fatal = 0.91
        - Recall: Fatal = 0.60, Non-Fatal = 0.91
        - F1-score: Fatal = 0.61, Non-Fatal = 0.91
        - Macro avg F1 = 0.76, Weighted avg F1 = 0.85

- ARIMA – Time Series Forecasting
    Developed an ARIMA model to forecast monthly aviation accident counts (binary-coded as Fatal = 1, Non-Fatal = 0).
    - Stationarity checked using Augmented Dickey-Fuller (ADF) test; differencing applied as needed.
    - Model parameters (p, d, q) = (1, 0, 1) selected via AIC minimization.
    - Results:
        - RMSE: 7.80
        - MSE: 60.89
        - MAPE: not computed due to zero values in test set
    - Visual inspection showed reasonable short-term forecasting performance.
- SARIMA – Seasonal Time Series Forecasting
    Extended ARIMA to capture seasonality in accident counts.
    - ADF test confirmed stationarity (ADF statistic = -12.09, p-value ≈ 2.16e-22).
    - Best model selected: SARIMA(1, 1, 1) × (0, 1, 1, 12) with AIC = 1564.23.
    - Results:
        - RMSE: 0.177 (training)
        - RMSE: 0.031 (testing)
    - Improved seasonal trend capture compared to ARIMA, especially for recurring patterns across years.

## Results & Findings

1) Fatal accidents are a smaller proportion overall compared to non-fatal ones. The strongest link with severity is Aircraft Damage — accidents where the aircraft was “Destroyed” are far more likely to be fatal, while “Substantial” damage is most often non-fatal.
2) Aircraft Category shows little correlation with severity, but single-engine fixed-wing aircraft dominate accident counts.
3) Landing and Takeoff phases have the highest total accident counts, while Cruise and Maneuvering phases have a higher proportion of fatal outcomes.
4) Most accidents occur in good weather (Visual conditions), and a significant portion of these are fatal — indicating that human and mechanical factors play a major role.
5) Purpose of Flight is moderately linked to severity: Personal flights have the highest counts overall, while Air Taxi and Business flights show higher fatality ratios.
6) KNN classification of fatal vs. non-fatal accidents achieved 85% accuracy, with strong recall for Non-Fatal cases (0.91) but lower recall for Fatal cases (0.60), suggesting class imbalance.
7) ARIMA forecasting captured short-term accident trends with RMSE ≈ 7.80, but struggled with seasonal patterns.
8) SARIMA model improved seasonal trend capture, achieving much lower RMSE (0.177 training, 0.031 testing) than ARIMA, indicating better fit for cyclical accident patterns.

## Reproducibility

- Requires a CSV dataset (AviationData.csv) from the NTSB Aviation Accident Database.
- All preprocessing is performed inside the Jupyter Notebook — no extra scripts needed.
- Data cleaning includes category merging, missing value handling, and label standardization before visualizations.

To reproduce:
1) Download the dataset from NTSB website.
2) Place it in the data/ folder.
3) Run the notebook from top to bottom.

## License

MIT License

## Acknowledgements

- Data source: National Transportation Safety Board (NTSB)
- Stack Overflow community for code snippets on Pandas filtering and CSV loading
- NTU programming notes on regex handling for string cleaning
# Macroeconomic Forecasting & Policy Research

A Python-based macroeconomic forecasting engine that builds, validates, and compares models for forecasting key macro variables — GDP growth, inflation, unemployment, and interest rates. Includes train-test backtesting, scenario analysis with fan charts, and a Diebold-Mariano test for statistical significance.

---

## What this project does

1. **Pulls data** from FRED (US macro) and the World Bank API (India macro)
2. **Builds three models** — Random Walk benchmark, AR(4), and VAR
3. **Backtests** on pre-2020 training data, evaluates on 2020–2024 test data
4. **Runs a Diebold-Mariano test** to statistically verify whether the VAR beats the benchmark
5. **Produces scenario forecasts** — baseline, upside, and downside paths with 80% confidence bands
6. **Saves publication-ready charts** to the `outputs/` folder

---

## Results

### Model Comparison (RMSE — lower is better)

| Variable | RW RMSE | VAR RMSE | Improvement % | DM Statistic | p-value | Stat. Significant? |
|---|---|---|---|---|---|---|
| Real GDP       | 2.6415 | 2.5629 | 3.0 | -0.8125 | 0.4266 | No |
| CPI            | 1.3144 | 0.9615 | 26.8 | -4.0786 | 0.0006 | Yes |
| Unemployment   | 35.3066 | 35.0438 | 0.7 | -0.8627 | 0.399 | No |
| Fed Funds Rate | 123.6874 | 122.6876 | 0.8 | -2.1874 | 0.0414 | Yes |


---

### Charts

**Macro Overview — All 4 Indicators (2000–2024)**

![Macro Overview](outputs/macro_overview.png)

---

**VAR Model — Forecast vs Actual (Test Period)**

![VAR Forecast](outputs/var_forecast.png)

---

**Scenario Fan Charts — Baseline, Upside & Downside**

![Scenario Fan Charts](outputs/scenario_fan_charts.png)

---

**GDP Scenario Chart (Report-Ready)**

![GDP Scenarios](outputs/gdp_scenario_chart.png)

---

**Diebold-Mariano Test Results**

![DM Test](outputs/dm_test_results.png)

---

## Models

### 1. Random Walk (Benchmark)
Predicts next quarter = current quarter. No learning, no parameters. Every other model is judged against this.

### 2. AR(4) — AutoRegressive Model
Forecasts each variable using its own last 4 quarters. Single-variable, simple, interpretable.

```
GDP(t) = α + β₁·GDP(t-1) + β₂·GDP(t-2) + β₃·GDP(t-3) + β₄·GDP(t-4) + ε
```

### 3. VAR — Vector AutoRegression
Forecasts all four variables simultaneously. Each variable depends on the recent history of all other variables, capturing macro interdependencies (e.g. how an interest rate shock feeds into unemployment).

```
Y(t) = A₁·Y(t-1) + A₂·Y(t-2) + ... + Aₚ·Y(t-p) + ε
```
Lag order selected automatically using AIC.

---

## Scenario Analysis

Three forecast paths are produced over the test horizon:

| Scenario | Assumption | GDP shock | CPI shock |
|---|---|---|---|
| **Baseline** | Model forecast, no additional shock | — | — |
| **Upside** | Positive productivity shock | +1.5% | -0.3% |
| **Downside** | Stagflation / oil price spike | -2.0% | +1.5% |

Shocks phase in linearly over 4 quarters to simulate gradual transmission.

---

## Statistical Validation

### Diebold-Mariano Test
The DM test (Diebold & Mariano, 1995) tests whether the difference in forecast accuracy between the VAR and Random Walk is statistically significant or could be due to chance.

- **H₀**: VAR and Random Walk have equal predictive accuracy
- **H₁**: VAR is significantly more accurate
- **Rejection threshold**: p-value < 0.05

A negative DM statistic means the VAR has lower forecast loss than the Random Walk. Results are saved to `outputs/diebold_mariano_results.csv`.

---

## Data Sources

| Dataset | Source | Frequency | Variables |
|---|---|---|---|
| US Real GDP | FRED (`GDPC1`) | Quarterly | GDP index |
| US CPI | FRED (`CPIAUCSL`) | Monthly → Quarterly | Consumer prices |
| US Unemployment | FRED (`UNRATE`) | Monthly → Quarterly | Unemployment rate |
| Federal Funds Rate | FRED (`FEDFUNDS`) | Monthly → Quarterly | Policy interest rate |
| India GDP Growth | World Bank (`NY.GDP.MKTP.KD.ZG`) | Annual → Quarterly | GDP growth % |
| India Inflation | World Bank (`FP.CPI.TOTL.ZG`) | Annual → Quarterly | CPI % |
| India Unemployment | World Bank (`SL.UEM.TOTL.ZS`) | Annual → Quarterly | Unemployment % |

All data is downloaded once and saved locally to `data/` (excluded from version control).

---

## Project Structure

```
Macro_Forecasting_Research/
├── notebooks/
│   └── 01_data.ipynb          # Full pipeline: data → models → charts → results
├── outputs/
│   ├── macro_overview.png         # All 4 indicators plotted
│   ├── forecast_vs_actual.png     # AR model vs random walk
│   ├── var_forecast.png           # VAR model vs actual
│   ├── scenario_fan_charts.png    # All 4 variables, 3 scenarios + CI
│   ├── gdp_scenario_chart.png     # GDP only, publication style
│   ├── dm_test_results.png        # DM test bar chart
│   ├── model_comparison.csv       # RMSE table
│   ├── scenario_summary.csv       # End-of-horizon scenario values
│   ├── diebold_mariano_results.csv
│   └── final_model_evaluation.csv # Combined RMSE + DM results
├── data/                          # Downloaded CSVs (gitignored)
├── requirements.txt
├── .gitignore
└── README.md
```

---

## Setup & Usage

**1. Clone the repository**
```bash
git clone https://github.com/KartikBadesara/Macro_Forecasting_Research.git
cd Macro_Forecasting_Research
```

**2. Create a virtual environment**
```bash
python -m venv .venv

# Windows
.venv\Scripts\Activate.ps1

# Mac/Linux
source .venv/bin/activate
```

**3. Install dependencies**
```bash
pip install -r requirements.txt
```

**4. Register the Jupyter kernel**
```bash
python -m ipykernel install --user --name=macro_env --display-name "Macro Project"
```

**5. Open the notebook**

Open `notebooks/01_data.ipynb` in VS Code, select the **Macro Project** kernel, and run all cells.

Data downloads automatically on first run and is cached locally. All outputs are saved to `outputs/`.

---

## Dependencies

```
pandas
numpy
matplotlib
statsmodels
pandas-datareader
scikit-learn
scipy
wbdata
ipykernel
jupyter
```

Install with: `pip install -r requirements.txt`

---

## Key References

- Diebold, F.X. and Mariano, R.S. (1995). *Comparing Predictive Accuracy*. Journal of Business & Economic Statistics.
- Sims, C.A. (1980). *Macroeconomics and Reality*. Econometrica. *(Original VAR paper)*
- Stock, J.H. and Watson, M.W. (2001). *Vector Autoregressions*. Journal of Economic Perspectives.

---

## Author

**Kartik Badesara**
[github.com/KartikBadesara](https://github.com/KartikBadesara)

---

*Built with Python · pandas · statsmodels · scipy*

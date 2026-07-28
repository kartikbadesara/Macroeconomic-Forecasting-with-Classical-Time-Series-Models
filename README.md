# Macroeconomic Forecasting with Classical Time Series Models

This project explores classical time-series forecasting techniques for predicting key U.S. macroeconomic indicators using publicly available FRED data.

I compare three forecasting approaches—Random Walk, Autoregressive (AR), and Vector Autoregression (VAR)—to evaluate whether modeling relationships between economic variables improves forecasting accuracy.

The project focuses on GDP growth, inflation (CPI), unemployment, and the Federal Funds Rate.

---

## Why I Built This

I wanted to better understand how traditional econometric models are applied to real-world macroeconomic forecasting. Rather than using machine learning immediately, I chose to start with widely used statistical models and compare their strengths and limitations on real economic data.

---

![Macro Overview](macro_overview.png)

---

## What this project does

1. Pulls data from FRED for GDP, CPI, unemployment, and the Fed funds rate (2000-2024)
2. Builds three models: Random Walk benchmark, AR(4), and VAR
3. Backtests on pre-2020 training data, evaluates on 2020-2024 test data (includes the COVID shock)
4. Runs a Diebold-Mariano test to statistically verify whether the VAR beats the benchmark
5. Produces scenario forecasts: baseline, upside, and downside paths with 80% confidence bands

---

## Results

### Model Comparison (RMSE - lower is better)

| Variable | RW RMSE | VAR RMSE | Improvement % | DM Statistic | p-value | Stat. Significance |
|---|---|---|---|---|---|---|
| Real GDP | 2.6415 | 2.5629 | 3.0 | -0.8125 | 0.4266 | No |
| CPI | 1.3144 | 0.9615 | 26.8 | -4.0786 | 0.0006 | Yes |
| Unemployment | 35.3066 | 35.0438 | 0.7 | -0.8627 | 0.399 | No |
| Fed Funds Rate | 123.6874 | 122.6876 | 0.8 | -2.1874 | 0.0414 | Yes |


### Charts

**GDP Scenario Forecast**

![GDP Scenario](gdp_scenario_chart.png)

**VAR Model - Forecast vs Actual**

![VAR Forecast](outputs/var_forecast.png)

**Scenario Fan Charts**

![Scenario Fan Charts](outputs/scenario_fan_charts.png)

**Diebold-Mariano Test Results**

![DM Test](outputs/dm_test_results.png)

---

## Models

### 1. Random Walk (Benchmark)

Predicts next quarter = current quarter. No learning, no parameters. Every other model is judged against this.

### 2. AR(4) - AutoRegressive Model

Forecasts each variable using its own last 4 quarters. Single-variable, simple, interpretable.

```
GDP(t) = a + b1*GDP(t-1) + b2*GDP(t-2) + b3*GDP(t-3) + b4*GDP(t-4) + e
```

### 3. VAR - Vector AutoRegression

Forecasts all four variables simultaneously. Each variable depends on the recent history of all other variables, capturing macro interdependencies (e.g. how an interest rate shock feeds into unemployment).

```
Y(t) = A1*Y(t-1) + A2*Y(t-2) + ... + Ap*Y(t-p) + u(t)
```


---

## Scenario Analysis

Three forecast paths are produced over the test horizon:

| Scenario | Assumption | GDP shock | CPI shock |
|---|---|---|---|
| Baseline | Model forecast, no additional shock | - | - |
| Upside | Positive productivity shock | +1.5% | -0.3% |
| Downside | Oil price spike / stagflation | -2.0% | +1.5% |

Shocks phase in linearly over 4 quarters to simulate gradual transmission.

---

## Statistical Validation

### Diebold-Mariano Test

The DM test (Diebold and Mariano, 1995) tests whether the difference in forecast accuracy between the VAR and Random Walk is statistically significant or could be due to chance.

- H0: VAR and Random Walk have equal predictive accuracy
- H1: VAR is significantly more accurate
- Rejection threshold: p-value < 0.05

A negative DM statistic means the VAR has lower forecast loss than the Random Walk. 

---

## Data Sources

| Dataset | Source | Frequency | Variable |
|---|---|---|---|
| US Real GDP | FRED (GDPC1) | Quarterly | GDP index |
| US CPI | FRED (CPIAUCSL) | Monthly to Quarterly | Consumer prices |
| US Unemployment | FRED (UNRATE) | Monthly to Quarterly | Unemployment rate |
| Federal Funds Rate | FRED (FEDFUNDS) | Monthly to Quarterly | Policy interest rate |


---

## Key Learnings

- Working with economic time-series data
- Implementing AR and VAR models
- Performing rolling forecast evaluation
- Comparing models using RMSE and the Diebold–Mariano test
- Understanding the trade-offs between univariate and multivariate forecasting

---

## Dependencies

pandas
numpy
matplotlib
statsmodels
pandas-datareader
scikit-learn
scipy
ipykernel
jupyter

---

## Key References

- Diebold, F.X. and Mariano, R.S. (1995). Comparing Predictive Accuracy. Journal of Business and Economic Statistics.
- Sims, C.A. (1980). Macroeconomics and Reality. Econometrica.
- Stock, J.H. and Watson, M.W. (2001). Vector Autoregressions. Journal of Economic Perspectives.

---

# Conclusion

Among the models evaluated, the VAR model generally produced stronger forecasts for variables that are economically interconnected, while simpler models remained competitive for highly persistent series.

This project helped me understand the complete forecasting workflow—from collecting macroeconomic data to evaluating competing models using statistical tests.

Future work could include Bayesian VAR, machine learning models, and additional macroeconomic indicators.

---

## Author

Kartik Badesara
github.com/KartikBadesara

---


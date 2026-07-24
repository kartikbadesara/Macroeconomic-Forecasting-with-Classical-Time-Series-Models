# Macro Forecasting Research

I built this to teach myself how macroeconomic forecasting actually works — not just the theory, but the full pipeline from pulling real data to producing statistically validated results.

The project forecasts four US macro variables (real GDP, CPI inflation, unemployment, the Fed funds rate) using a VAR model, and tests whether it meaningfully beats a random walk using the Diebold-Mariano test.

---

![Macro Overview](macro_overview.png)

---

## What it does

- Pulls FRED data for GDP, CPI, unemployment, and the Fed funds rate (2000–2024)
- Trains on pre-2020 data, tests on 2020–2024 out-of-sample (includes the COVID shock)
- Compares Random Walk, AR(4), and VAR models
- Runs a Diebold-Mariano test to check if the VAR's improvement is statistically significant
- Builds three scenarios (baseline, upside, downside) with 80% confidence bands

---

## Results

![GDP Scenario](gdp_scenario_chart.png)

Full results are in `outputs/final_model_evaluation.csv`.

---

## Models

**Random Walk** — predicts zero change each period. The benchmark every model has to beat.

**AR(4)** — forecasts each variable from its own last 4 quarters.

**VAR(p)** — models all four variables together, each depending on the recent history of the others. Lag order picked by AIC.

**Scenarios** — shocks applied to the baseline:
- Upside: positive productivity shock (+1.5pp GDP, −0.3pp inflation)
- Downside: oil price spike (−2.0pp GDP, +1.5pp inflation)

---

## Setup

```bash
git clone https://github.com/KartikBadesara/Macro_Forecasting_Research.git
cd Macro_Forecasting_Research
python -m venv .venv && .venv\Scripts\Activate.ps1
pip install -r requirements.txt
```

Open `notebooks/01_data.ipynb`, select the Macro Project kernel, and run all cells.

Data downloads automatically on first run and is saved locally. Charts go to `outputs/`.

---

## Stack

Python · pandas · statsmodels · scipy · pandas-datareader

---

## References

- Diebold & Mariano (1995). Comparing Predictive Accuracy. *JBES*
- Sims (1980). Macroeconomics and Reality. *Econometrica*
- FRED — fred.stlouisfed.org

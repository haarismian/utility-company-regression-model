# Hotazel Steam: Forecasting Model Comparison

**Author:** Haaris Mian

A regression-based forecasting project that models monthly revenue for a steam/energy operation using historical production and weather (degree-day) data, then compares two candidate models on out-of-sample accuracy.

## Project Overview

This notebook builds and evaluates two simple linear regression models to forecast monthly revenue:

- **Model 1** — predicts revenue from **production** volume
- **Model 2** — predicts revenue from **heating degree days (heatDD)**, a proxy for cold-weather demand

The goal is to determine which single predictor produces more reliable out-of-sample revenue forecasts, using a proper train/test split rather than in-sample fit alone.

## Data

The dataset (`AICPA_regressionAnalysisData.csv`) contains 48 months of data (Jan 2011–Dec 2014) with the following columns:

| Column | Description |
|---|---|
| `type` | Splits rows into `dt4training` (2011–2013, 36 months) or `dt4testing` (2014, 12 months) |
| `date` | Month-end date |
| `revenue` | Monthly revenue (target variable) |
| `production` | Monthly production volume |
| `coolDD` | Cooling degree days (warm-weather demand proxy) |
| `heatDD` | Heating degree days (cold-weather demand proxy) |

Revenue shows a clear seasonal pattern, peaking in winter months and dipping in spring/fall shoulder seasons.

## Methodology

1. **Load and inspect the data**, converting `date` to a proper datetime type.
2. **Visualize revenue over time** to confirm seasonality.
3. **Check correlations** between revenue and each candidate predictor:
   - `production`: ≈0.63
   - `heatDD`: ≈0.69
   - `coolDD`: ≈-0.17 (weak — excluded from both models)
4. **Split the data** into training (2011–2013) and testing (2014) sets using the `type` column.
5. **Train two OLS regression models** (via `statsmodels`), one on `production`, one on `heatDD`.
6. **Forecast the 2014 test months** with each model and score accuracy using **MAPE** (Mean Absolute Percentage Error).
7. **Visually compare** both models' forecasts against actual 2014 revenue.

## Results

| Model | Predictor | R² (train) | MAPE (test) |
|---|---|---|---|
| Model 1 | `production` | 0.397 | 25.42% |
| Model 2 | `heatDD` | 0.428 | 21.65% |

**Model 2 (heatDD) outperforms Model 1 (production)** on out-of-sample accuracy, despite both having modest R² values. This suggests that cold-weather demand is a somewhat stronger driver of revenue than raw production volume for this operation — though neither single-variable model explains the majority of the variance, pointing to opportunities for a multivariate model in future work.

## Tech Stack

- Python
- `pandas` / `numpy` — data handling
- `matplotlib` — visualization
- `statsmodels` — OLS regression

## How to Run

1. Place `AICPA_regressionAnalysisData.csv` in the same directory as the notebook (or update the file path).
2. Open the notebook in Google Colab or Jupyter.
3. Run all cells sequentially — each step builds on the previous one (data load → EDA → train/test split → Model 1 → Model 2 → comparison plot).

## Possible Extensions

- Combine `production` and `heatDD` into a single multivariate regression model
- Test for multicollinearity between predictors (the condition number in Model 1's summary output flags this as worth investigating)
- Try a time-series-aware approach (e.g., SARIMA) to explicitly model seasonality
- Cross-validate across multiple train/test splits rather than a single fixed year holdout

## Contact

Haaris Mian

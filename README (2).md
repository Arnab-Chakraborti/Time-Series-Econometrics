# Forecast evaluation and causal impact: from financial returns to promotion lift

Two notebooks on one theme: **judge a model honestly against a baseline, quantify its uncertainty, and separate cause from correlation.**

> Developed locally from September 2026 and extended in October 2026; uploaded to GitHub in October 2026.

## Notebooks

| Notebook | Question | Data |
|---|---|---|
| `financial_time_series_forecasting.ipynb` | Does ARIMA beat a naive baseline on daily SPY returns, and can the diagnostics tell us in advance? | SPY daily prices (Yahoo Finance via `yfinance`), plus one autocorrelated contrast series |
| `promo_lift_did_synthetic.ipynb` | How much does a promotion really lift sales, and when do simple comparisons and difference-in-differences fail? | **Synthetic** store x week panel with a known true effect |

## What is in the forecasting notebook
- Stationarity and volatility diagnostics (ADF, ACF/PACF implemented from scratch and checked against `statsmodels`, Ljung-Box, ARCH-LM, EWMA volatility).
- ARIMA order selection on **training data only**.
- **Walk-forward validation** with an automated no-lookahead test, benchmarked against zero-change and rolling-mean forecasts using MAE, RMSE, WAPE, bias, MASE, directional accuracy and **Diebold-Mariano tests**.
- **Conformal prediction intervals**, constant-width and volatility-scaled, checked for coverage overall and by volatility regime.
- A contrast experiment on a strongly autocorrelated series.

## What is in the causal notebook
- A simulated retail panel with store size, trends, seasonality and a holiday spike.
- Naive estimates vs **difference-in-differences** (two-way fixed effects, cluster-robust SEs) against the known truth.
- **Event study** and **placebo test** to check parallel trends.
- A deliberate parallel-trends violation, the effect on the estimate, and a partial fix.
- A 100-dataset Monte Carlo: bias, confidence-interval coverage and placebo rejection rates.

## Results
Headline numbers are printed in Section 12 of the forecasting notebook and Section 5 of the causal notebook, generated from the data that was actually loaded.

<!-- TODO before publishing: paste the headline numbers from your own run here (data source/date, ARIMA relative RMSE and DM p-value, interval coverage, contrast-series result). -->

## Limitations
- One asset and one-day horizon. Daily equity returns are close to the efficient-market limit, so the main result is about method, not about a forecastable signal.
- Interval guarantees are approximate and can degrade under regime shifts.
- The causal notebook is a simulation: it shows how the methods behave, not the size of any real promotion effect. It assumes a constant effect, no spillovers and no stockouts.

## Run it
```bash
pip install -r requirements.txt
jupyter notebook
```
The forecasting notebook downloads SPY on first run and caches it in `data/` (git-ignored). If the download fails and no cache exists it falls back to a simulated series and prints a warning, so check the "Data source" line before reading any results.

## Next steps
Apply the same framework to retail demand data (WAPE/MASE by SKU, Croston/ETS baselines, a LightGBM global model, quantile loss), compare with synthetic control / CausalImpact, and replace EWMA with GARCH.

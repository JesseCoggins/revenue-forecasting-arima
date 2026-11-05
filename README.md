# Forecasting daily changes in hospital revenue

I tested how forecast timing changes the decision for hospital finance staff. On **146 held-out days**, ARIMA updated each day had RMSE **0.4408 million dollars** of daily revenue change, compared with **0.4860 million** for assuming no change. When all 146 predictions were issued together at day 585, no-change performed slightly better.

[Open the forecast notebook](notebooks/revenue_arima_forecast.ipynb) · [Inspect the 731-day source series](data/hospital_revenue_raw.csv) · [View the revenue history](assets/report-extracts/)

## What was evaluated

The D603 data dictionary reports revenue in millions of dollars. The CSV contains days 1–731. First differencing produced 730 day-to-day changes. I fitted an AR(1) model to the first 584 changes and issued all 146 test forecasts from day 585. The first prediction corresponds to day 586 and the last to day 731.

| Forecast on the 146 test days | Mean absolute error (millions of dollars) | Root mean squared error (millions of dollars) |
|---|---:|---:|
| ARIMA on daily changes | 0.3765 | 0.4887 |
| Assume no daily change | 0.3746 | 0.4860 |

## Forecasts updated each day

The daily evaluation refits the same AR(1) model form on an expanding history before predicting the next day. Each new fit sees only observations available before its forecast. The test dates remain days 586–731.

| Forecast cadence | MAE ($ millions of daily change) | RMSE ($ millions of daily change) |
|---|---:|---:|
| ARIMA, refitted before each next day | 0.3531 | 0.4408 |
| Assume no daily change | 0.3746 | 0.4860 |

Daily operations and a budget prepared months in advance have different information available. These results support ARIMA for the tested daily update schedule, while the fixed-origin comparison favors no change. Neither evaluation measures error in monthly totals. [All 146 paired forecasts](data/forecast_evaluation.csv)

## Model and 30-day forecast

The Augmented Dickey–Fuller test gave a p-value of 0.1997 on levels and about 5.11 × 10⁻³⁰ after differencing. The exploratory correlation plots informed the choice of one autoregressive lag. The holdout model is `ARIMA(1,0,0)` on daily changes.

For a forecast issued after day 731, the notebook refits that model form to all 731 revenue levels as `ARIMA(1,1,0)` with drift. It projects days **732–761** and calculates 95% prediction intervals directly on revenue levels. There are no observed revenues for those future dates in this dataset.

## Reproduce the analysis

The evaluation used Python 3.13.1, statsmodels 0.14.4 and NumPy 2.2.2. [Numerical environment](requirements-verified.txt)

Install the [Python requirements](requirements.txt) in a Jupyter environment and run the notebook with `notebooks/` as the working directory. The prepared series are included. [Training changes](data/train_dataset.csv) · [Holdout changes](data/test_dataset.csv) · [Cleaned series](data/cleaned_dataset.csv)

*WGU M.S. in Data Analytics coursework, D603 Machine Learning.*

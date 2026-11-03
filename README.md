# Forecasting daily changes in hospital revenue

I tested whether a hospital finance team could forecast changes in daily receipts more accurately than assuming receipts would stay flat. On **146 held-out days**, the no-change forecast was slightly more accurate on both measures. ARIMA’s average absolute error was **0.3765 million dollars** in daily revenue change, compared with **0.3746 million** for the baseline.

[Open the forecast notebook](notebooks/revenue_arima_forecast.ipynb) · [Inspect the 731-day source series](data/hospital_revenue_raw.csv) · [View the original coursework figures](assets/report-extracts/)

## What was evaluated

The D603 data dictionary reports revenue in millions of dollars. The CSV contains days 1–731. First differencing produced 730 day-to-day changes. I fitted an AR(1) model to the first 584 changes and issued all 146 test forecasts from day 585. The first prediction corresponds to day 586 and the last to day 731.

| Forecast on the 146 test days | Mean absolute error (millions of dollars) | Root mean squared error (millions of dollars) |
|---|---:|---:|
| ARIMA on daily changes | 0.3765 | 0.4887 |
| Assume no daily change | 0.3746 | 0.4860 |

The simpler forecast gives finance staff a useful comparison when deciding whether to maintain a more complex model. A monthly budgeting decision would also require evaluating errors in revenue levels and monthly totals over several historical periods.

## Model and 30-day forecast

The Augmented Dickey–Fuller test gave a p-value of 0.1997 on levels and about 5.11 × 10⁻³⁰ after differencing. The exploratory correlation plots informed the choice of one autoregressive lag. The holdout model is `ARIMA(1,0,0)` on daily changes.

For a forecast issued after day 731, the notebook refits that model form to all 731 revenue levels as `ARIMA(1,1,0)` with drift. It projects days **732–761** and calculates 95% prediction intervals directly on revenue levels. There are no observed revenues for those future dates in this dataset.

## Coursework and portfolio verification

The submitted notebook reported holdout MAE of 0.3766 and RMSE of 0.4887. Its integer start position skipped the first forecast step. Its 30-day figure also drew predictions from the training cutoff against the final observed revenue. The public version corrects both forecast origins, adds the no-change comparison and retains the original coursework figures for reference.

Install the [Python requirements](requirements.txt) in a Jupyter environment and run the notebook with `notebooks/` as the working directory. The prepared series are included. [Training changes](data/train_dataset.csv) · [Holdout changes](data/test_dataset.csv) · [Cleaned series](data/cleaned_dataset.csv)

*WGU M.S. in Data Analytics coursework, D603 Machine Learning.*

# Forecasting daily changes in hospital revenue

I used a hospital’s first **731 days of revenue** to build a short-term baseline for finance staff. A useful planning question is whether a change in receipts is unusual enough to investigate. On a chronological holdout, the model’s mean absolute error was **0.3766 in the dataset’s units of daily revenue change**.

[Open the forecast notebook](notebooks/revenue_arima_forecast.ipynb) · [Inspect the source series](data/hospital_revenue_raw.csv) · [View the coursework figures](assets/report-extracts/)

## Why model the change?

The original series had a trend. The Augmented Dickey–Fuller test gave a p-value of 0.1997 on revenue levels and about 5.11 × 10⁻³⁰ after first differencing. I used the autocorrelation and partial autocorrelation plots to choose a lag-one autoregressive model. In code, this is `ARIMA(1,0,0)` on daily changes, corresponding to one order of differencing on the original series.

## Results

I used the first **584 daily changes** for training and the following **146** for evaluation. Keeping the order of observations preserved the forecasting task.

| Held-out metric | Daily revenue change |
|---|---:|
| Mean absolute error (MAE) | 0.3766 |
| Root mean squared error (RMSE) | 0.4887 |

These scores apply to the change from one day to the next. A finance team using a forecast to set a monthly budget would also want to measure error in revenue levels and accumulated totals.

The notebook illustrates a 30-day revenue path by adding forecasts of daily changes to the last observed level. The fitted model ends at the training cutoff. For a forecast issued on day 731, update the model with the holdout observations first. The plotted revenue-level band adds up the 95% bounds for individual daily changes and should be treated as illustrative.

## Review or rerun

Install the [Python requirements](requirements.txt) in a Jupyter environment and run the notebook with `notebooks/` as the working directory. It contains the stationarity tests, model fit, holdout predictions and plots. [Training series](data/train_dataset.csv) · [Holdout series](data/test_dataset.csv) · [Cleaned data](data/cleaned_dataset.csv)

*WGU M.S. in Data Analytics coursework, D603 Machine Learning.*

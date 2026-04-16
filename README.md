# Revenue Forecasting with ARIMA

## Overview
This project forecasts short-term hospital revenue using daily historical revenue data from the organization's first two years of operation. The analysis applies time series preparation, stationarity testing, decomposition, ARIMA modeling, and forecast evaluation to support operational and financial planning.

## Coursework Context
This project was completed as part of my M.S. in Data Analytics program at Western Governors University (WGU). Screenshots from the original written submission are preserved in `assets/report-extracts/`.

## Business Question
Can historical daily revenue from the hospital's first two years of operation be used to accurately forecast short-term future revenue?

## Dataset
- Source: `hospital_revenue_raw.csv`
- Observations: 731 daily revenue records
- Columns: `Day`, `Revenue`
- Missing values: none
- Duplicate rows: none

## Workflow
1. Inspected the revenue series for completeness and continuity
2. Visualized the daily revenue trend over time
3. Tested stationarity with the Augmented Dickey-Fuller test
4. Applied first-order differencing to stabilize the series
5. Re-tested stationarity on the differenced series
6. Split the differenced series chronologically into 584 training observations and 146 testing observations
7. Reviewed ACF and PACF plots to guide model selection
8. Fit an ARIMA model and generated both test-period and future forecasts
9. Evaluated performance with RMSE and MAE

## Model
- Original-series interpretation: ARIMA(1,1,0)
- Implemented fit: ARIMA(1,0,0) on the differenced series
- Rationale: the PACF showed a strong lag-1 signal while the ACF decayed gradually, supporting an AR(1) structure after differencing

## Stationarity
- ADF statistic before differencing: -2.2183
- p-value before differencing: 0.1997
- ADF statistic after differencing: -17.3748
- p-value after differencing: 5.11e-30

## Results
- RMSE: 0.4887
- MAE: 0.3766
- Forecast horizon beyond the dataset: 30 additional days
- Confidence intervals: 95%

## Selected Visuals

![ARIMA report visual](assets/report-extracts/report_image_01.png)

## Included Files
- `notebooks/revenue_arima_forecast.ipynb`
- `data/hospital_revenue_raw.csv`
- `data/cleaned_dataset.csv`
- `data/train_dataset.csv`
- `data/test_dataset.csv`
- `requirements.txt`

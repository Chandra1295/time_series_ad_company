Time Series Ad Company
Overview
Ad Company is an ads and marketing-based company helping businesses elicit maximum clicks at minimum cost. AdEase is an ad infrastructure designed to help businesses promote themselves easily, effectively, and economically. The interplay of three AI modules—Design, Dispense, and Decipher—makes this an end-to-end, three-step digital advertising solution for all.

You are part of the Data Science team at Ad Company, tasked with analyzing per-page view reports for different Wikipedia pages over 550 days. The goal is to forecast the number of views to predict and optimize ad placement for clients. The dataset includes 145,000 Wikipedia pages with daily view counts. Clients belong to different regions and require insights into how their ads will perform on pages in various languages.

Dataset Description
train_1.csv
Each row represents an article, and each column represents a date.

Values indicate the number of visits on that date.

Page Name Format:

Copy
SPECIFIC_NAME_LANGUAGE.wikipedia.org_ACCESS_TYPE_ACCESS_ORIGIN
SPECIFIC_NAME: Article name

LANGUAGE: Wikipedia language version

ACCESS_TYPE: Device type used (desktop/mobile)

ACCESS_ORIGIN: Request origin (spider or browser)

Exog_Campaign_eng.csv
Contains data on campaign events that might impact page views.

Applies only to English pages.

1 indicates a campaign/event, and 0 means no event.

This can be used as an external factor while training models for forecasting.

Summary of Model Performance
1. Initial ARIMA Models
Tried different ARIMA models with varying parameters.

Best AIC (lower is better) for ARIMA: 7449.09 with (1,1,1).

Applying transformations (e.g., log transformation) resulted in a much lower AIC (804.73), but likely didn’t work well overall.

2. Switching to SARIMAX (with Seasonality & Exogenous Variables)
Added seasonal components and an external factor (exogenous variable) to improve predictions.

Initial SARIMAX model (order=(4,1,3), seasonal_order=(1,1,1,7)) gave AIC 7457.98, slightly worse than ARIMA but improved overall accuracy:

MAE: 299.53

RMSE: 422.72

MAPE: 5.84%

3. Optimizing SARIMAX Parameters
Best-found parameters:

Order: (0,1,4)

Seasonal Order: (1,1,4,7)

Reduced AIC to 6706.99, a significant improvement.

Accuracy improved:

MAE: 247.27 (Lower = Better)

RMSE: 307.09 (Lower = Better)

MAPE: 0.048% (Very small error, excellent result!)

4. Prophet Model Performance
Tested Prophet, a model designed for time-series forecasting.

Results comparing without and with an exogenous variable:

RMSE Without Exogenous Variable: 478.21

RMSE With Exogenous Variable: 416.20

Adding an external factor improved Prophet’s accuracy, but SARIMAX still performed better overall.

Key Takeaways
ARIMA was a good starting point, but adding seasonality and external variables (SARIMAX) significantly improved performance.

SARIMAX outperformed Prophet in terms of RMSE, making it the best choice for this dataset.

Final Model: SARIMAX with (0,1,4) and seasonal (1,1,4,7) is the most accurate so far.


Inferences from Data Visualizations
1. Language Distribution
English dominates with the highest number of pages, followed by Japanese, German, and French.

2. Access Types
All-access (51.4%) is the most common, followed by mobile-web (24.9%) and desktop (23.6%).

3. Access Origins
All-agents account for 75.8% of traffic, while spiders (bots) make up 24.2%.

Business Implications
English Pages: High traffic and low MAPE make them ideal for ad placement.

Chinese Pages: Low traffic suggests limited ad potential unless targeting specific demographics.

Russian Pages: Moderate traffic.

Spanish Pages: High traffic.

French, German, Japanese Pages: Moderate traffic suitable for targeted campaigns.

Time Series Decomposition
Purpose
Time series decomposition breaks down a series into components like trend, seasonality, and residuals. This helps in:

Identifying underlying patterns.

Understanding the impact of each component on the series.

Improving forecasting accuracy by modeling each component separately.

We used an additive model for decomposition in this case.

Level of Differencing for Stationarity
Differencing
First-order differencing was sufficient to achieve stationarity for most series.

Seasonal differencing was used when seasonality was present, with the lag determined by the seasonal frequency (e.g., 7 for weekly data).

Differences Between ARIMA, SARIMA, and SARIMAX
1. ARIMA (AutoRegressive Integrated Moving Average)
Combines autoregression (AR), differencing (I), and moving average (MA).

Suitable for non-seasonal data.

Model: ARIMA(p, d, q).

2. SARIMA (Seasonal ARIMA)
Extends ARIMA to include seasonal components.

Captures seasonal patterns in data.

Model: SARIMA(p, d, q)(P, D, Q, s), where s is the seasonal period.

3. SARIMAX (Seasonal ARIMA with Exogenous Variables)
Incorporates external variables (e.g., oil prices, temperature) to improve forecasts.

Useful when external factors influence the time series.

Comparison of Views Across Languages
The mean number of views (popularity) across languages is as follows:

English: Highest traffic.

Spanish: Second highest but with higher MAPE.

Russian: Moderate traffic with reliable forecasts.

German: Moderate traffic.

Japanese: Moderate traffic.

French: Moderate traffic.

Chinese: Lowest traffic.

Alternative Methods to Grid Search for Model Selection
Beyond grid search, the following methods can be used to estimate model parameters:

Domain Knowledge: Leverage business expertise to set initial parameter estimates.

ACF and PACF Plots:

Use ACF to identify the MA component (q).

Use PACF to identify the AR component (p).

Augmented Dickey-Fuller Test: Determine the differencing order (d) for stationarity.

Automated Tools: Use libraries like pmdarima (auto-ARIMA) to automate parameter selection.

Bayesian Optimization: Efficiently search the parameter space using probabilistic models.

Cross-Validation: Validate model performance on multiple time series splits to avoid overfitting.


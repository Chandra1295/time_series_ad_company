# Time Series Ad Company: - Forecasting Wikipedia Page Views for Ad Placement Optimization

## Overview
Ad Company is a cutting-edge ad infrastructure designed to help businesses promote themselves easily, effectively, and economically. The system is powered by three AI modules—**Design**, **Dispense**, and **Decipher**—making it an end-to-end, three-step digital advertising solution for businesses. As part of the Data Science team, the goal is to analyze per-page view reports for different Wikipedia pages over 550 days and forecast the number of views to predict and optimize ad placement for clients.

## Dataset Description

### 1. **train_1.csv**
Each row represents an article, and each column represents a date. The values in the columns indicate the number of visits on that date.

#### Page Name Format:
`SPECIFIC_NAME_LANGUAGE.wikipedia.org_ACCESS_TYPE_ACCESS_ORIGIN`

- **SPECIFIC_NAME**: Article name
- **LANGUAGE**: Wikipedia language version
- **ACCESS_TYPE**: Device type used (desktop/mobile)
- **ACCESS_ORIGIN**: Request origin (spider or browser)

### 2. **Exog_Campaign_eng.csv**
Contains data on campaign events that might impact page views. It applies only to English pages.  
- **1**: Represents a campaign/event
- **0**: No event

This can be used as an external factor while training models for forecasting.

## Summary of Model Performance

### 1. **Initial ARIMA Models**
Tried different ARIMA models with varying parameters.

- **Best AIC (lower is better)** for ARIMA: 7449.09 with (1,1,1).
- **Transformations** (e.g., log transformation) resulted in a much lower AIC (804.73), but didn’t work well overall.

### 2. **Switching to SARIMAX (with Seasonality & Exogenous Variables)**
Added seasonal components and an external factor (exogenous variable) to improve predictions.

- **Initial SARIMAX Model**: `order=(4,1,3)`, `seasonal_order=(1,1,1,7)`
- **AIC**: 7457.98 (slightly worse than ARIMA but improved overall accuracy)
- **MAE**: 299.53
- **RMSE**: 422.72
- **MAPE**: 5.84%

### 3. **Optimizing SARIMAX Parameters**
Best-found parameters:

- **Order**: (0,1,4)
- **Seasonal Order**: (1,1,4,7)

This resulted in:

- **AIC**: 6706.99 (significant improvement)
- **MAE**: 247.27 (lower = better)
- **RMSE**: 307.09 (lower = better)
- **MAPE**: 0.048% (excellent result!)

### 4. **Prophet Model Performance**
Tested **Prophet**, a model designed for time-series forecasting.

- **RMSE Without Exogenous Variable**: 478.21
- **RMSE With Exogenous Variable**: 416.20

While adding an external factor improved Prophet’s accuracy, **SARIMAX** still performed better overall.

### Key Takeaways
- **ARIMA** was a good starting point, but adding seasonality and external variables (via SARIMAX) significantly improved performance.
- **SARIMAX** outperformed Prophet in terms of RMSE, making it the best choice for this dataset.
- **Final Model**: SARIMAX with `(0,1,4)` and seasonal `(1,1,4,7)` is the most accurate model so far.
  
---

## Inferences from Data Visualizations

### 1. **Language Distribution**
- **English**: Highest number of pages.
- **Japanese**: Second highest.
- **German** and **French**: Moderate number of pages.

### 2. **Access Types**
- **All-access** (51.4%) is the most common.
- **Mobile-web** (24.9%) and **desktop** (23.6%) follow.

### 3. **Access Origins**
- **All-agents** account for 75.8% of traffic.
- **Spiders (bots)** account for 24.2% of traffic.

---

## Business Implications

- **English Pages**: High traffic and low MAPE, ideal for ad placement.
- **Chinese Pages**: Low traffic, limited ad potential unless targeting specific demographics.
- **Russian Pages**: Moderate traffic, reasonable ad placement potential.
- **Spanish Pages**: High traffic, good for targeted campaigns.
- **French, German, Japanese Pages**: Moderate traffic, suitable for targeted campaigns.

---

## Time Series Decomposition

### Purpose
Time series decomposition breaks down a series into components like trend, seasonality, and residuals. This helps in:
- Identifying underlying patterns.
- Understanding the impact of each component on the series.
- Improving forecasting accuracy by modeling each component separately.

We used an **additive model** for decomposition.

### Level of Differencing for Stationarity

#### Differencing
- **First-order differencing** was sufficient for stationarity for most series.
- **Seasonal differencing** was used when seasonality was present, with the seasonal frequency (e.g., 7 for weekly data).

---

## Differences Between ARIMA, SARIMA, and SARIMAX

### 1. **ARIMA** (AutoRegressive Integrated Moving Average)
- Suitable for non-seasonal data.
- **Model**: ARIMA(p, d, q).

### 2. **SARIMA** (Seasonal ARIMA)
- Extends ARIMA to include seasonal components.
- **Model**: SARIMA(p, d, q)(P, D, Q, s), where s is the seasonal period.

### 3. **SARIMAX** (Seasonal ARIMA with Exogenous Variables)
- Incorporates external variables (e.g., campaign events, weather data).
- Useful when external factors influence the time series.

---

## Comparison of Views Across Languages

The mean number of views (popularity) across languages is as follows:
- **English**: Highest traffic.
- **Spanish**: Second highest but with higher MAPE.
- **Russian**: Moderate traffic with reliable forecasts.
- **German**: Moderate traffic.
- **Japanese**: Moderate traffic.
- **French**: Moderate traffic.
- **Chinese**: Lowest traffic.

---

## Alternative Methods to Grid Search for Model Selection

Beyond grid search, the following methods can be used to estimate model parameters:
- **Domain Knowledge**: Leverage business expertise to set initial parameter estimates.
- **ACF and PACF Plots**: Use ACF to identify the MA component (q) and PACF to identify the AR component (p).
- **Augmented Dickey-Fuller Test**: Determine the differencing order (d) for stationarity.
- **Automated Tools**: Use libraries like `pmdarima` (auto-ARIMA) to automate parameter selection.
- **Bayesian Optimization**: Efficiently search the parameter space using probabilistic models.
- **Cross-Validation**: Validate model performance on multiple time series splits to avoid overfitting.

---

## Conclusion
The project successfully employed time-series forecasting models (ARIMA, SARIMAX, Prophet) to predict Wikipedia page views, helping optimize ad placement for clients. After experimenting with multiple models, **SARIMAX** emerged as the most accurate and robust choice for this task, offering significant improvements in forecasting accuracy. The final model not only offers solid predictions but also provides valuable insights into how ad campaigns can be optimized across different languages and regions.

---

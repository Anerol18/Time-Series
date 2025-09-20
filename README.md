# Time Series Forecasting – Second Submission
**Author:** Lorena Romeo  
**Date:** 2025-01-01  

# Purpose of the Project
The goal of this project is to forecast electricity consumption (kW) for one day using two different approaches:  
1. **Without using outdoor temperature**  
2. **Including outdoor temperature** as a feature  

The forecasts consist of **96 rows** (one for each 15-minute interval) and **two columns** (one per forecast).  

In addition to traditional time series methods, a **Neural Network (NN) model** was also developed and is attached separately.

# Data Investigation
- **Dataset:** 15-minute interval readings of power consumption and temperature from 01/01/2010 to 20/02/2010  
- **Variables:**  
  - `Timestamp` – date and time of the reading  
  - `Power_kW` – electricity consumption in kilowatts  
  - `Temp_C` – temperature in Celsius  

### Key Steps
- Libraries loaded: `readxl`, `lubridate`, `dplyr`, `forecast`, `ggplot2`, `xts`, `zoo`, `randomForest`, `xgboost`, `vars`, `Metrics`, etc.  
- Timestamp cleaned using a custom function to handle Excel and character formats  
- Zero values in power consumption treated as missing data (`NA`) and interpolated

# Data Exploration & Cleaning
- Time series object created for power consumption (`elec_ts`) with **96 observations per day**  
- Decomposition of the series revealed:  
  - Strong daily seasonality  
  - Slight decreasing trend  
  - Residuals with regular spikes, indicating autocorrelation  
- Outliers (power = 0) replaced by interpolated values  
- Differencing applied (1–2 times) to remove non-stationarity

# Modelization

## 1. SARIMA Models
- **Auto ARIMA:** `ARIMA(5,0,0)(0,1,0)[96]`  
- **Manually Tuned SARIMA:** `ARIMA(1,1,7)(1,1,1)[96]` – best performing model without temperature  
- Residuals checked; RMSE, AIC, BIC calculated for comparison

## 2. Holt-Winters Exponential Smoothing
- **Additive Model:** RMSE = 11.531, AIC = 944.89, BIC = 954.67  
- **Multiplicative Model:** Slightly better RMSE (11.295) but additive chosen due to stable seasonal variations

## 3. Including Temperature
- SARIMA model extended with temperature as a regressor: `ARIMA(1,1,7)(1,1,1)[96]` with xreg = Temp_C  
- RMSE slightly improved; residuals analyzed for autocorrelation

## 4. Linear Regression
- Simple regression `Power ~ Temperature` tested but residuals showed patterns; model discarded

## 5. Machine Learning
- **Random Forest:** Used `Timestamp` and `Temp_C` as features  
  - RMSE = 45.83 → worse than SARIMA/HW models  
- **Neural Network (NN) model:** Developed separately and attached for further comparison

# Forecasting
- Forecasts generated for **21st February 2010** using:  
  1. **Holt-Winters Additive (best traditional model) without temperature**  
  2. **SARIMA with temperature as regressor**  
  3. **NN model** – see separate file for implementation and predictions  

- Forecast plots visualized alongside historical data

# Conclusion
- **Best traditional approach without temperature:** Holt-Winters Additive model  
- **Best approach with temperature:** SARIMA with temperature regressor  
- **Machine Learning/NN models:** Useful as alternative; performance evaluated in separate attachment  
- Forecasts are ready for deployment and comparison

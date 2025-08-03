# Store Sales Time Series Forecasting

## Problem Statement
The goal of this project is to forecast daily grocery store sales using historical sales data. Accurate forecasting supports inventory optimization, reduces overstock and stockouts, improves financial planning, and enhances operational efficiency.

## Dataset
Source: [Kaggle - Store Sales Time Series Forecasting](https://www.kaggle.com/competitions/store-sales-time-series-forecasting)

The main dataset contains daily sales (`sales`) for various store and item combinations, along with metadata such as dates, store numbers, item numbers, and promotion indicators.

---

## Project Workflow

1. **Data Loading:**  
   Downloaded the dataset programmatically via Kaggle API for reproducibility.

2. **Exploratory Data Analysis (EDA) & Outlier Handling:**  
   Analyzed sales distributions and detected outliers using the Interquartile Range (IQR) method.  
   Outliers were capped at thresholds to prevent skewing the model, maintaining the natural data shape while reducing noise from rare or erroneous spikes.

3. **Feature Engineering:**  
   Engineered multiple key features to capture temporal patterns:  
   - **Calendar features:** day of week and month to model weekly and monthly seasonality.  
   - **Lag features:** sales lagged by 7 and 14 days to capture autocorrelation and repeating customer behavior.  
   - **Rolling statistics:** 7-day rolling mean sales to smooth short-term fluctuations.

4. **Data Aggregation:**  
   Modeled both aggregated total sales across all stores/items and optionally focused on single store-item time series to demonstrate forecasting at different granularities.

5. **Train-Test Split:**  
   Used an 80/20 chronological split to simulate forecasting future sales and properly evaluate model performance on unseen data.

6. **Modeling:**  
   Developed a SARIMAX (Seasonal AutoRegressive Integrated Moving Average with eXogenous regressors) model, incorporating both trend and seasonality alongside calendar-based features.  
   Model hyperparameters were selected through visual inspection and iterative tuning to balance fit and generalization.

7. **Evaluation & Visualization:**  
   Evaluated model accuracy with Root Mean Squared Error (RMSE), Mean Absolute Error (MAE), and Mean Absolute Percentage Error (MAPE).  
   Visualized actual vs. forecasted sales and residuals to assess predictive quality and bias.

---

## Feature Engineering Explained

- **Day of Week and Month:**  
  Capture recurring weekly sales cycles (e.g., weekends often have different shopping behaviors) and seasonal monthly variation.

- **Lag Features (7 & 14 days):**  
  Embed recent sales history to leverage autoregressive patterns typical in retail data where past behavior informs future sales.

- **Rolling Mean (7 days):**  
  Smooth sales data to reduce noise and highlight trends, improving the model’s stability.

---

## Outlier Handling Rationale

- Sales outliers can be caused by promotions, holidays, or data errors.  
- To prevent these rare events from disproportionately influencing the model, outliers were capped at the IQR-based boundaries [Q1 - 1.5×IQR, Q3 + 1.5×IQR].  
- This approach preserves the underlying data distribution while improving model robustness.

---

## Model Selection & Hyperparameter Tuning

- SARIMAX was selected for its interpretability and ability to model both non-stationary trends and recurring seasonal patterns, along with including exogenous calendar features.  
- Hyperparameters (p,d,q) and seasonal components (P,D,Q,s) were chosen based on autocorrelation analyses and experimentation for optimal error performance.

---

## Results & Interpretation

- **RMSE:** 28,015 (~10.8% of mean daily sales)  
- **MAE:** 19,247 units  
- **MAPE:** 22.3%  

Given the average daily sales of approximately 258,422 units with a standard deviation around 24,466, the forecasting errors are reasonable, reflecting the inherent volatility in retail sales.

Residual analysis indicates errors are generally unbiased with no obvious temporal trends, supporting model reliability.

---

## Real-World Implications

- This level of accuracy supports informed decisions in inventory management by reducing overstock and stockouts.  
- Forecasts can assist workforce planning and promotional scheduling, enabling responsive and cost-effective retail operations.

---

## Final Observations & Learnings

- Including calendar and lagged features significantly improved forecasting performance and aligned with intuitive business seasonality.  
- Outlier capping enhanced model stability, particularly during unusual sales spikes.  
- SARIMAX provided an explainable and reliable forecasting framework but is sensitive to parameters and outliers.  
- Future work could explore machine learning models like XGBoost or Prophet for improved flexibility and accuracy, as well as more granular forecasts by item or store.

---

## Next Steps

- Enhance feature set with promotions, holidays, and external economic factors.  
- Experiment with ensemble or deep learning methods.  
- Deploy forecasting model as an API or dashboard for real-time business use.

---

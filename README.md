# Rossmann Sales Forecasting — Time Series Modeling (Capstone Project)

## Project Summary

Rossmann operates thousands of stores across Europe. Because many pharmaceutical products have short shelf lives, accurate daily sales forecasting is critical. In this project, we build a statistical forecasting pipeline to predict **daily sales for the next 6 weeks (42 days)** for selected Rossmann stores, taking into account promotions, customers, holidays, and competition.

## Objectives

- Develop models to forecast store-level sales for a 42-day horizon.  
- Quantify the influences of key variables:  
  - Customer footfall  
  - Promotional campaigns (`Promo`, `Promo2`)  
  - Holidays (state and school)  
  - Competition distance and opening times  
- Verify time series properties:  
  - Stationarity (ADF / KPSS tests)  
  - Cointegration among variables (Johansen test)  
- Compare and select among modeling approaches: ARIMA / SARIMAX and VAR / VECM.  
- Evaluate performance using **MAPE (Mean Absolute Percentage Error)**.  
- Present interpretable business insights from the best model.

## Data

### 1. `train.csv`  

Contains daily-level sales and store operation variables:

| Column         | Description |
|----------------|-------------|
| `Store`        | Store identifier |
| `DayOfWeek`    | Day of the week (1–7) |
| `Date`         | Date of observation |
| `Sales`        | Sales (target variable) |
| `Customers`    | Number of customers |
| `Open`         | Whether store was open (0 / 1) |
| `Promo`         | Ongoing promotion flag |
| `StateHoliday` | State holiday indicator |
| `SchoolHoliday`| School holiday indicator |

### 2. `store.csv`  

Metadata for each store:

| Column                    | Description |
|---------------------------|-------------|
| `StoreType`               | Store type (a / b / c / d) |
| `Assortment`              | Assortment category (a / b / c) |
| `CompetitionDistance`     | Distance to nearest competitor store |
| `CompetitionOpenSince[Month/Year]` | When competitor opened |
| `Promo2`                  | Whether store participates in long-term promotion |
| `Promo2Since[Year/Week]`  | Start time for Promo2 participation |
| `PromoInterval`           | Months when Promo2 renewals happen |

## Approach & Methodology

1. **Data Loading & Cleaning**  
   - Merged `train` and `store` tables by `Store`.  
   - Addressed missing or zero values in features.  
2. **Exploratory Data Analysis (EDA)**  
   - Distribution analysis, correlation checks, time-series trend plotting, holiday/seasonal effects.  
3. **Outlier Handling**  
   - Removed extreme values above the 99th percentile for `Sales` and `Customers`.  
4. **Feature Engineering & Transformation**  
   - Created calendar features (year, month, week).  
   - Standardized `Sales` and `Customers` to z-scores (for VAR stability).  
5. **Stationarity Testing**  
   - ADF / KPSS tests on original and transformed series.  
   - Differenced data if non-stationary.  
6. **Cointegration Analysis**  
   - Johansen test to detect long-run equilibrium relationships among `Sales`, `Customers`, and promotions.  
7. **Model Training & Selection**  
   - ARIMA / SARIMAX as baselines.  
   - VAR / VECM for multivariate modeling.  
   - Lag selection via AIC / BIC.  
   - Residual diagnostics and impulse response analysis.  
8. **Forecasting & Evaluation**  
   - Forecast 42 days ahead.  
   - Inverse-transform to original scale.  
   - Compute MAPE (excluding closed-store days).  
9. **Insights & Business Interpretation**  
   - Use coefficients & impulse responses to interpret the effect of promotions, customers, etc.  
   - Highlight model strengths, weaknesses, and recommendations.

## Key Findings & Insights

- **Promotional campaigns** have statistically significant positive impact on sales (shown via impulse response).  
- **Customers and Sales** display cointegration — changes in customer footfall lead sales over time.  
- **Holiday periods** show notable sales spikes, especially around public and major holidays.  
- **Competition distance** is negatively correlated with sales (closer competition means lower sales for a given store).  
- Among models tested, **VAR (or VECM depending on cointegration)** yielded lowest MAPE on holdout data — performance in the range of ~12–15%.

## Evaluation Metric

**MAPE (Mean Absolute Percentage Error)** was used:

MAPE = (100 / n) * Σ | (y_t - ŷ_t) / y_t |

Where:

- n is the number of observations
- y_t is the actual value at time t
- ŷ_t is the predicted value at time t

- Excludes days when store is closed (`Open = 0`).  
- Handles zero-sales days via filtering or adjusted denominator.

## Tools & Dependencies

- **Python** (3.x)  
- **Libraries**: `pandas`, `numpy`, `matplotlib`, `seaborn`, `statsmodels`, `scikit-learn`, `pickle`, `warnings`

You can pin versions in a `requirements.txt` as follows (sample):

```txt
pandas>=1.5
numpy>=1.21
matplotlib
seaborn
statsmodels
scikit-learn
```

## Setup & Usage Instructions

1. **Clone the repository**

```bash
git clone https://github.com/Ritesh-GitHub-Ranjan/sales-forecasting.git
cd sales-forecasting
````

1. **Install dependencies**

```bash
pip install -r requirements.txt
```

1. **Open Jupyter Notebook**

```bash
jupyter notebook
```

1. **Run the main notebook**

Open `Rossmann_Sales_Forecasting_Case_Study_final.ipynb` (or whichever notebook is your final version) and execute cells sequentially.

## Repository Structure

```txt
├── Dataset/                   # Contains train.csv, store.csv
├── Rossmann_Sales_Forecasting_base_01.ipynb
├── Rossmann_Sales_Forecasting_Case_Study_final.ipynb
├── README.md
└── requirements.txt
```

## Future Enhancements

- Enrich model with **external variables** (weather, macro data, local events).
- Deploy forecasting as a **web API** (Flask/FastAPI) for real-time use.
- Test advanced algorithms: **XGBoost**, **LSTM**, **Temporal Fusion Transformer**.
- Extend forecast beyond 6 weeks or build rolling forecasts.
- Build a dashboard for non-technical stakeholders to view forecasts and insights.

## 👤 Author

### **Ritesh Ranjan**

Data Science & Machine Learning Enthusiast

📧 [Gmail](mailto:ranjanritesh1729@gmail.com) | 🌐 [LinkedIn](https://www.linkedin.com/in/ritesh-nitk/) | [GitHub](https://github.com/Ritesh-GitHub-Ranjan)

---

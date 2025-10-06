# Rossmann Sales Forecasting — Time Series Modeling(Capstone Project)

## Problem Statement

Rossmann is a European drug distributor operating **over 3,000 stores** across seven countries.  
Accurate sales forecasting is crucial because many pharmaceutical products have a **short shelf life**.  
Currently, individual store managers manually predict sales for the next six weeks — a process that is inconsistent and often inaccurate.

This project builds a **statistical and machine learning pipeline** to forecast **daily sales for the next 42 days (6 weeks)** for selected Rossmann stores, while accounting for promotions, holidays, customers, and competition.

---

## Objectives

- Forecast **daily store-level sales** for the next 6 weeks.  
- Quantify effects of:
  - Customer traffic  
  - Promotions (`Promo`, `Promo2`)  
  - Holidays (state and school)  
  - Competition distance and timing  
- Check statistical properties:
  - **Stationarity** of series (ADF, KPSS tests)  
  - **Cointegration** (Johansen test)  
- Build and compare models:
  - ARIMA / SARIMAX  
  - VAR / VECM  
- Evaluate forecasts using **MAPE**.  

---

## Data Description

The dataset includes two files:

### `train.csv`

Daily sales & store operations data.
| Column | Description |

|--------|-------------|
| `Store` | Unique store ID |
| `DayOfWeek` | Day of week (1 = Monday … 7 = Sunday) |
| `Date` | Transaction date |
| `Sales` | Daily sales (target) |
| `Customers` | Number of customers |
| `Open` | Store open flag (0/1) |
| `Promo` | Promo campaign flag |
| `StateHoliday` | a = public, b = Easter, c = Christmas, 0 = None |
| `SchoolHoliday` | Whether school was closed |

### `store.csv`

Store metadata.
| Column | Description |

|--------|-------------|
| `StoreType` | Store type (a/b/c/d) |
| `Assortment` | Assortment level (a = basic, b = extra, c = extended) |
| `CompetitionDistance` | Distance to nearest competitor |
| `CompetitionOpenSince[Month/Year]` | Competition opening date |
| `Promo2` | Continuous promotion (0/1) |
| `Promo2Since[Year/Week]` | Start of Promo2 |
| `PromoInterval` | Renewal months of Promo2 |

---

## Methodology

### 1. Data Import & Cleaning

- Loaded `train.csv` and `store.csv`.  
- Handled missing values:  
  - Replaced some columns with **0** (e.g., `Promo2SinceWeek`, `CompetitionDistance`)  
  - Imputed others using **mode** (e.g., `CompetitionOpenSinceMonth`).  
- Fixed data discrepancies (e.g., competition distance = 0).  

### 2. Exploratory Data Analysis (EDA)

- **Univariate analysis**: distributions of store types, assortment, promos.  
- **Bivariate analysis**: correlation of sales with promotions, holidays, customers.  
- **Time series plots**: identified seasonality, weekly cycles, holiday spikes.  

### 3. Preprocessing

- Removed **outliers above 99th percentile** for `Sales` and `Customers`.  
- Standardized key features (`Sales`, `Customers`) using `StandardScaler`.  
- Created calendar features (year, month, week).  

### 4. Statistical Tests

- **ADF & KPSS tests**: checked stationarity of sales and customer series.  
- **Differencing**: applied where non-stationarity detected.  
- **Johansen cointegration**: confirmed long-run relationship between Sales, Customers, and Promo.  

### 5. Modeling

- **ARIMA / SARIMAX**: baseline time series models.  
- **VAR (Vector Autoregression)**: multivariate forecasting, capturing interactions.  
- Lag order selected via AIC/BIC.  
- Compared models on validation holdout.  

### 6. Forecasting

- Generated **42-day forecasts** (6 weeks) for selected stores.  
- Reversed transformations (differencing, scaling) to obtain sales in original scale.  
- Visualized forecast vs. actual sales.  

### 7. Evaluation

- Metric: **MAPE (Mean Absolute Percentage Error)**.  
- Excluded closed-store days (`Open=0`).  
- VAR showed superior performance with average MAPE ~12–15%.  

---

## Key Insights

- **Promotions** significantly boost sales, confirmed by impulse response analysis.  
- **Customers and Sales are strongly cointegrated** (Granger causality p < 0.05).  
- **Holidays** lead to demand spikes — especially public holidays and Christmas.  
- **Competition distance** negatively correlated with sales (closer competition reduces sales).  

---

## Tech Stack

- **Language**: Python 3.10+  
- **Core Libraries**:  
  - Data Processing: `pandas`, `numpy`  
  - Visualization: `matplotlib`, `seaborn`  
  - Time Series Modeling: `statsmodels` (`ARIMA`, `SARIMAX`, `VAR`, `Johansen`)  
  - Preprocessing: `scikit-learn`  
  - Others: `pickle`, `warnings`  

---

## How to Run

1. Clone the repository:

```bash
git clone https://github.com/<your-username>/rossmann-sales-forecasting.git
cd rossmann-sales-forecasting
````

1. Install requirements:

```bash
pip install -r requirements.txt
```

1. Run Jupyter Notebook:

```bash
jupyter notebook
```

1. Open and execute:
   `Rossmann_Sales_Forecasting_Case_Study.ipynb`

---

## Future Enhancements

- Incorporate **external datasets** (weather, macroeconomic factors).
- Deploy as an **API** (Flask/FastAPI) for real-time sales forecasting.
- Extend forecasting horizon beyond 6 weeks.
- Explore **ML/DL models** (XGBoost, LSTM, Temporal Fusion Transformer).

---

## Author

**Ritesh Ranjan**
Data Science & Machine Learning Enthusiast
📧 [Gmail](mailto:ranjanritesh1729@gmail.com) | 🌐 [LinkedIn](https://www.linkedin.com/in/ritesh-nitk/) | [GitHub](https://github.com/Ritesh-GitHub-Ranjan)

---

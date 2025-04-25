# BA_Bootcamp_10_Prophet_Challenge

# (Facebook) Prophet Challenge


# 📈 Facebook Prophet Challenge: Demand for Shelter 🏠  
**Forecasting NYC’s Shelter Demand Using Facebook Prophet + Time Series Cross-Validation + Hyperparameter Tuning**

---

## 📘 Challenge Overview

> **"Predicting the future is always difficult."**

In this unique case study, we dive into time-series forecasting using **Facebook Prophet** to model the **daily demand for shelter in New York City**. We also explore:

- Time Series Cross-Validation
- Forecast Visualization
- Advanced Parameter Tuning

The project is built using Python with `Prophet`, `Pandas`, `sklearn`, and other standard libraries.

---

## 📂 Repository Structure

| File Name                  | Description                                               |
|---------------------------|-----------------------------------------------------------|
| `Prophet Challenge.pdf`   | Problem Statement and Challenge Details                   |
| `Prophet Challenge.ipynb` | Python Notebook with Full Workflow and Code               |
| `DHS_weekly.csv`          | Dataset: Weekly shelter demand and associated variables   |
| `README.md`               | You are here! Repository overview and interpretation      |

---

## 🔧 Step-by-Step Breakdown

### 1️⃣ Data Preparation

- Renamed columns to fit Prophet’s requirements:
  - `ds`: Date (in `YYYY-MM-DD` format)
  - `y`: Target variable – Total Individuals in Shelter
- Holiday Effects included:
  - **Easter** and **Thanksgiving**, configured with windows `[-7, 7]`
- Additional regressors used:  
  `Christmas`, `Temperature`

---

### 2️⃣ Train-Test Split

- The final **4 weeks** are reserved for the **test set**
- Remaining weekly data used for training
- This structure mirrors a real-world forecasting scenario

---

### 3️⃣ Model Building & Forecasting

- Prophet Model Configurations:
  - `growth`: linear
  - `yearly_seasonality`: True
  - `seasonality_mode`: multiplicative
  - Custom prior scales for regularization
- Added external **regressors** and **holiday effects**
- Built a **future dataframe** to include test periods and regressors

#### 📌 Coefficients Summary

| Regressor    | Mode           | Center     | Coef Lower | Coef     | Coef Upper |
|--------------|----------------|------------|------------|----------|------------|
| Christmas    | Multiplicative | 0.000000   | -0.000204  | -0.000204| -0.000204  |
| Temperature  | Multiplicative | 14.934939  | -0.000222  | -0.000222| -0.000222  |

#### 📌 Thanksgiving Holiday Effect Post-2020

| Date       | Thanksgiving Effect |
|------------|---------------------|
| 2020-11-22 | 0.002178            |
| 2020-11-29 | -0.000985           |
| 2020-12-06 | -0.003979           |

#### 📈 Forecast Output (Test Weeks)

| Week Index | Forecasted Demand (yhat) |
|------------|--------------------------|
| 362        | 385,778                  |
| 363        | 384,876                  |
| 364        | 382,666                  |
| 365        | 381,604                  |

---

### 4️⃣ Accuracy Assessment

- **Metric**: Root Mean Squared Error (RMSE)
- **Test RMSE**:  
  ✅ `84,923.58` (lower RMSE = better fit)

---

### 5️⃣ Visualization Highlights

- Prophet's built-in plots used for:
  - Overall forecast trend
  - Individual seasonality components
- Forecast visually aligned well with true demand
- Holiday spikes clearly reflected in component plots

---

## 🔍 Cross-Validation

Used Prophet’s `cross_validation` utility:

| Horizon | Initial | Type       |
|---------|---------|------------|
| 4 weeks | 300 weeks | Rolling   |

Sample Output:

| ds         | yhat      | y        | cutoff     |
|------------|-----------|----------|------------|
| 2019-10-20 | 419,439.04| 420,265  | 2019-10-13 |
| 2020-11-22 | 392,501.89| 377,413  | 2020-10-25 |
| 2020-12-06 | 389,022.61| 375,444  | 2020-11-08 |

📊 Visualized **Mean Absolute Percentage Error (MAPE)** using Prophet’s utility.

---

## 🛠️ Parameter Tuning

Used **Grid Search** on key Prophet parameters:

| Parameter               | Values                      |
|-------------------------|-----------------------------|
| `seasonality_mode`      | additive, multiplicative    |
| `seasonality_prior_scale` | 5, 10, 20               |
| `holidays_prior_scale`  | 5, 10, 20                   |
| `changepoint_prior_scale`| 0.01, 0.05, 0.1            |

- Total combinations tested: **54**
- Evaluated models using **RMSE on cross-validation set**

#### 🔍 Sample Tuning Results

| changepoint_prior_scale | holidays_prior_scale | seasonality_mode | seasonality_prior_scale | RMSE     |
|--------------------------|----------------------|------------------|--------------------------|----------|
| 0.01                     | 5                    | multiplicative   | 5                        | 8,947.15 |
| 0.05                     | 10                   | additive         | 10                       | 8,593.82 |
| 0.1                      | 20                   | multiplicative   | 20                       | 8,799.42 |

---

## ✅ Key Learnings

- Facebook Prophet is powerful, but **data prep is crucial**.
- Events and regressors significantly **improve forecast accuracy**.
- **Cross-validation and tuning** are essential for robust time-series modeling.
- Visualizations provide deep insights into seasonality and anomalies.

---

## 🚀 Future Work

- Incorporate **weather forecasts** for better future temperature modeling
- Explore **monthly** or **daily** granularity
- Use **XGBoost** or **LSTM** for comparison with Prophet’s performance

---

## 📎 References

- [Facebook Prophet Documentation](https://facebook.github.io/prophet/)
- [NYC Open Data – DHS Shelter Report](https://data.cityofnewyork.us/Social-Services/DHS-Daily-Report/k46n-sa2m)
- [Cross-validation for time-series](https://facebook.github.io/prophet/docs/diagnostics.html)

---

## 👨‍💻 Author

**Binny Babu**  
Data Scientist | Analyst | Time Series Explorer  
📧 binnybabu@gmail.com

---

_Star this repo 🌟 if you find it helpful!_

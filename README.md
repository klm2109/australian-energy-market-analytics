# ⚡ Electricity Demand & Spot Price Analysis


------------------------------------------------------------------------

## 📌 Overview

This project explores electricity demand and wholesale spot-price
behaviour using **5-minute electricity market data** from August 2026.

The project progresses through **data preparation, exploratory data
analysis, statistical relationship analysis, extreme-price analysis,
feature engineering, and machine learning**.

Two machine-learning problems are investigated:

1.  📈 **Regression:** predicting continuous Regional Reference Price
    (RRP).
2.  🚨 **Classification:** identifying periods with elevated price-spike
    risk.

------------------------------------------------------------------------

## 🎯 Project Objectives

1.  How does electricity demand vary throughout the day?
2.  How do weekday and weekend demand patterns differ?
3.  How do electricity spot prices vary over time?
4.  What is the relationship between electricity demand and spot price?
5.  How does spot-price behaviour change across demand levels?
6.  When are extreme positive price spikes most likely?
7.  Under what conditions do negative electricity prices occur?
8.  Can machine-learning models predict electricity spot prices?
9.  Can machine-learning models identify elevated price-spike risk?

------------------------------------------------------------------------

## 📂 Dataset

  Variable           Description
  ------------------ -------------------------------------------------
  `SETTLEMENTDATE`   Date and time of the market interval
  `TOTALDEMAND`      Total electricity demand (MW)
  `RRP`              Regional Reference Price (AUD/MWh)
  `hour`             Hour of day
  `day_number`       Day of week
  `is_weekend`       Weekday/weekend indicator
  `demand_level`     Low, Medium, High or Very High demand
  `price_spike`      Indicator for unusually high-price observations

Additional temporal and cyclical features are derived during the
analysis.

------------------------------------------------------------------------

# 🔎 Exploratory & Statistical Analysis

## 1. 🧹 Data Preparation

Key preprocessing steps include:

-   parsing settlement timestamps;
-   checking missing and duplicate observations;
-   validating demand and RRP values;
-   extracting hour and day-of-week information;
-   identifying weekday/weekend observations;
-   constructing demand categories; and
-   creating indicators for extreme-price events.

## 2. ⚡ Electricity Demand Analysis

The analysis examines hourly demand, peak and minimum periods,
day-of-week effects, weekday/weekend differences, and intraday profiles.

Demand shows a clear daily cycle, with morning and evening increases and
lower demand around the middle of the day. Weekday demand is generally
stronger than weekend demand, particularly during morning hours.

## 3. 💰 Electricity Spot Price Analysis

RRP is analysed through time-series and distributional visualisations.
Spot prices are substantially more volatile than electricity demand and
include both extreme positive observations and negative prices.

## 4. 🔗 Demand--Price Relationship

A positive relationship is observed between total demand and RRP.

A simple exploratory linear relationship gives approximately:

**R² ≈ 0.371**

Demand therefore explains a meaningful portion of price variation, but
considerable variation remains associated with other market conditions.

Median and average spot prices also increase progressively from **Low**
to **Very High** demand.

## 5. 🚨 Extreme Price Analysis

### 🔺 Positive Price Spikes

Price-spike events are concentrated during particular periods of the
day. The strongest observed spike risk occurs during the **late
afternoon and evening**, particularly around **17:00--18:00**.

Spike probability also rises substantially with demand, with the **Very
High** demand category showing the greatest risk.

### 🔻 Negative Electricity Prices

Negative prices show an almost opposite pattern. They occur primarily
during **lower-demand periods** and are concentrated around midday and
early afternoon.

The highest observed negative-price probability occurs around **14:00**,
and negative prices are overwhelmingly associated with the **Low
demand** category.

> **Key insight:** Low-demand periods are more exposed to negative
> electricity prices, while very high-demand periods are substantially
> more exposed to extreme positive-price events.

------------------------------------------------------------------------

# 🤖 Machine Learning Extension

The machine-learning extension tests whether the patterns found during
exploratory analysis can be translated into useful predictive models.

> 📈 **Can demand and temporal features predict continuous electricity
> spot prices?**

> 🚨 **Can demand and temporal features identify periods at elevated
> risk of a price spike?**

------------------------------------------------------------------------

## 6. 🧠 Feature Engineering

Predictive features include:

-   `TOTALDEMAND`
-   `is_weekend`
-   `hour_sin`
-   `hour_cos`
-   `dow_sin`
-   `dow_cos`

Hour and day-of-week are cyclical variables, so sine and cosine
transformations are used to preserve their recurring structure.

A **chronological train-test split** is used so that earlier
observations train the models and later observations evaluate them.

------------------------------------------------------------------------

# 📈 7. Electricity Spot-Price Prediction

The first machine-learning task treats **RRP** as a continuous target.

### 🌳 Regression Models

-   Mean Baseline
-   Linear Regression
-   Decision Tree Regressor
-   Random Forest Regressor

### 📏 Evaluation Metrics

-   **MAE** --- Mean Absolute Error
-   **RMSE** --- Root Mean Squared Error
-   **R²** --- Coefficient of Determination

  Model                 MAE (AUD/MWh)   RMSE (AUD/MWh)          R²
  ------------------- --------------- ---------------- -----------
  Mean Baseline                36.147           45.857      -0.070
  Linear Regression            30.598           39.034       0.225
  **Decision Tree**        **28.661**       **38.935**   **0.229**
  Random Forest                31.761           41.516       0.123

### 🏆 Best Regression Model --- Decision Tree

The **Decision Tree Regressor** achieves the lowest MAE and RMSE and the
highest R² among the tested regression models.

-   **MAE:** 28.661 AUD/MWh
-   **RMSE:** 38.935 AUD/MWh
-   **R²:** 0.229

The trained models outperform the mean baseline on MAE and RMSE, showing
that demand and temporal features contain useful predictive information.

However, the modest R² demonstrates that these features alone cannot
explain most spot-price variation.

### 📊 Actual vs Predicted RRP

The Decision Tree captures several broad price regimes, but its
predictions are smoother and more step-like than actual RRP.

The largest errors occur during sudden extreme-price movements.

> **Interpretation:** Demand and temporal variables are useful for
> estimating general electricity-price conditions, but additional market
> information is required to accurately predict short-lived extreme
> events.

------------------------------------------------------------------------

# 🚨 8. Price-Spike Classification

The second task is binary classification:

  Class   Meaning
  ------- -----------------
  `0`     ⚪ Normal Price
  `1`     🔴 Price Spike

Because spikes are relatively rare, this is an **imbalanced
classification problem**. Accuracy alone is therefore insufficient.

### 🤖 Classification Models

-   Logistic Regression
-   Decision Tree Classifier
-   Random Forest Classifier

### 📏 Evaluation Metrics

-   Precision
-   Recall
-   F1-score
-   ROC-AUC
-   Confusion Matrix
-   ROC Curve

  --------------------------------------------------------------------------
  Model               Precision         Recall             F1        ROC-AUC
  -------------- -------------- -------------- -------------- --------------
  **Logistic          **0.467**      **0.393**      **0.427**      **0.899**
  Regression**                                                

  Decision Tree           0.396          0.365          0.380          0.645

  Random Forest           0.464          0.180          0.259          0.883
  --------------------------------------------------------------------------

### 🏆 Best Classification Model --- Logistic Regression

**Logistic Regression** provides the strongest overall classification
performance.

Its **ROC-AUC ≈ 0.90** indicates strong ability to rank higher-risk
observations above lower-risk observations across classification
thresholds.

------------------------------------------------------------------------

# 🔢 9. Logistic Regression Confusion Matrix

                             Predicted Normal   Predicted Price Spike
  ------------------------ ------------------ -----------------------
  **Actual Normal**                      1298                      80
  **Actual Price Spike**                  108                      70

The model:

-   ✅ correctly identifies **1,298 normal intervals**;
-   🚨 correctly detects **70 price spikes**;
-   ⚠️ produces **80 false spike warnings**; and
-   ❌ misses **108 actual spikes**.

For the spike class:

-   **Precision ≈ 0.47**
-   **Recall ≈ 0.39**
-   **F1 ≈ 0.43**

Overall accuracy is approximately **88%**, but this should be
interpreted cautiously because normal observations greatly outnumber
spikes.

At the default threshold, the model detects roughly four out of every
ten observed price spikes.

------------------------------------------------------------------------

# 📉 10. ROC Curve

Approximate ROC-AUC values are:

-   🥇 **Logistic Regression --- 0.90**
-   🥈 **Random Forest --- 0.88**
-   🥉 **Decision Tree --- 0.65**

Logistic Regression provides the strongest discrimination.

Its high ROC-AUC but moderate recall at the default threshold suggests
that **threshold optimisation** could be useful when missing a price
spike is more costly than generating a false alarm.

------------------------------------------------------------------------

# 🔍 11. Logistic Regression Model Interpretation

Because Logistic Regression is the strongest classifier, its
**coefficients** are used for interpretation rather than tree-based
`feature_importances_`.

-   A **positive coefficient** is associated with higher predicted
    log-odds of a price spike.
-   A **negative coefficient** is associated with lower predicted
    log-odds.
-   Larger absolute coefficients indicate stronger predictive influence
    within the fitted model.

The cyclical pairs `hour_sin`/`hour_cos` and `dow_sin`/`dow_cos` should
be interpreted jointly.

> ⚠️ Coefficients describe **predictive associations**, not causal
> effects.

------------------------------------------------------------------------

# 💡 Key Findings

-   ⚡ Electricity demand follows a clear intraday cycle.
-   📅 Weekday and weekend demand profiles differ.
-   💰 Spot prices are substantially more volatile than demand.
-   📈 Higher demand is generally associated with higher RRP.
-   🔺 Median prices increase from Low to Very High demand.
-   🚨 Positive price spikes are concentrated in high-demand
    morning/evening periods.
-   🌆 Spike risk is especially elevated around **17:00--18:00**.
-   🔻 Negative prices predominantly occur during Low demand.
-   ☀️ Negative-price risk is concentrated around midday and early
    afternoon.
-   🌳 **Decision Tree Regression** is the strongest tested model for
    continuous RRP prediction.
-   📊 **Logistic Regression** is the strongest tested model for spike
    classification.
-   🎯 Logistic Regression achieves approximately **0.90 ROC-AUC**.
-   ⚠️ Moderate recall shows that rare extreme events remain difficult
    to detect at the default threshold.
-   🧠 Demand and temporal features are informative but do not fully
    explain electricity-market volatility.

------------------------------------------------------------------------

# 🛠️ Technologies & Tools

## 💻 Programming

![Python](https://img.shields.io/badge/Python-3776AB?style=for-the-badge&logo=python&logoColor=white)

## 📊 Data Analysis

![Pandas](https://img.shields.io/badge/Pandas-150458?style=for-the-badge&logo=pandas&logoColor=white)
![NumPy](https://img.shields.io/badge/NumPy-013243?style=for-the-badge&logo=numpy&logoColor=white)

## 🤖 Machine Learning

![Scikit-Learn](https://img.shields.io/badge/Scikit--Learn-F7931E?style=for-the-badge&logo=scikitlearn&logoColor=white)
![Regression](https://img.shields.io/badge/Regression-4285F4?style=for-the-badge)
![Classification](https://img.shields.io/badge/Classification-FF6F00?style=for-the-badge)
![Feature
Engineering](https://img.shields.io/badge/Feature%20Engineering-6A5ACD?style=for-the-badge)

## 📈 Visualisation

![Matplotlib](https://img.shields.io/badge/Matplotlib-11557C?style=for-the-badge)
![Data
Visualization](https://img.shields.io/badge/Data%20Visualization-2196F3?style=for-the-badge)

## ⚙️ Development

![Jupyter](https://img.shields.io/badge/Jupyter-F37626?style=for-the-badge&logo=jupyter&logoColor=white)
![Git](https://img.shields.io/badge/Git-F05032?style=for-the-badge&logo=git&logoColor=white)
![GitHub](https://img.shields.io/badge/GitHub-181717?style=for-the-badge&logo=github&logoColor=white)

------------------------------------------------------------------------

# 🧩 Skills Demonstrated

![EDA](https://img.shields.io/badge/Exploratory%20Data%20Analysis-4CAF50?style=flat-square)
![Statistics](https://img.shields.io/badge/Statistical%20Analysis-00897B?style=flat-square)
![Feature
Engineering](https://img.shields.io/badge/Feature%20Engineering-673AB7?style=flat-square)
![Regression](https://img.shields.io/badge/Regression-1976D2?style=flat-square)
![Classification](https://img.shields.io/badge/Classification-E65100?style=flat-square)
![Model
Evaluation](https://img.shields.io/badge/Model%20Evaluation-455A64?style=flat-square)
![Energy
Analytics](https://img.shields.io/badge/Energy%20Analytics-FFC107?style=flat-square)

------------------------------------------------------------------------

# 🗂️ Repository Structure

``` text
electricity-demand-price-analysis/
│
├── data/
│   └── electricity_data.csv
│
├── notebooks/
│   ├── 01_data_preparation.ipynb
│   ├── 02_demand_analysis.ipynb
│   ├── 03_price_analysis.ipynb
│   └── 04_machine_learning.ipynb
│
├── README.md
└── requirements.txt
```

> Exact file names may differ depending on the final repository
> organisation.

------------------------------------------------------------------------

# ⚠️ Limitations

The project uses a relatively short observation period and a limited set
of explanatory variables.

Electricity prices can also be influenced by:

-   renewable-generation output;
-   generator availability;
-   transmission constraints;
-   interconnector conditions;
-   weather;
-   generator bidding behaviour; and
-   broader market events.

The observed relationships should therefore be interpreted primarily as
**associations rather than causal effects**.

The machine-learning models are exploratory predictive models rather
than production forecasting systems.

------------------------------------------------------------------------

# 🚀 Future Work

Potential extensions include:

-   🌡️ incorporating temperature and other weather variables;
-   ☀️ adding solar and wind generation;
-   ⏱️ creating lagged RRP and demand features;
-   📈 adding rolling averages and recent demand changes;
-   🧠 comparing gradient-boosting and additional algorithms;
-   🎯 optimising the price-spike probability threshold;
-   ⚖️ investigating additional class-imbalance techniques;
-   🔬 applying permutation importance or SHAP;
-   📅 extending the dataset across multiple months or years;
-   🔄 using time-series cross-validation;
-   📉 developing dedicated time-series forecasting models; and
-   🚨 modelling positive spikes and negative-price events separately.

------------------------------------------------------------------------

# 🏁 Conclusion

This project demonstrates an end-to-end data-science workflow for
analysing electricity-market behaviour.

Exploratory analysis reveals strong temporal patterns in electricity
demand and spot prices. Higher demand is generally associated with
higher prices and increased positive price-spike risk, while negative
prices are concentrated during lower-demand periods around midday and
early afternoon.

For continuous spot-price prediction, the **Decision Tree Regressor**
performs best among the tested regression models, although the modest R²
and Actual-vs-Predicted analysis show that demand and temporal
information alone cannot explain sudden market volatility.

For price-spike classification, **Logistic Regression** provides the
strongest overall results, achieving approximately **0.90 ROC-AUC**. Its
moderate recall nevertheless shows that reliably detecting rare
extreme-price events remains challenging.

Overall, the project highlights both the value and limitations of
applying statistical analysis and machine learning to electricity-market
data and provides a foundation for richer energy forecasting using
market, renewable-generation and weather information.

# ⚡ Electricity Demand & Spot Price Analysis

## Overview

This project explores electricity demand and wholesale spot-price behaviour using
5-minute electricity market data.

The analysis investigates how electricity demand changes throughout the day,
how spot prices behave under different demand conditions, and when extreme
events such as price spikes and negative electricity prices are most likely to
occur.

The project progresses from exploratory data analysis and visualisation to
statistical relationship analysis, with a machine-learning extension used to
investigate whether electricity prices and extreme-price events can be predicted
from demand and temporal features.

---

## Project Objectives

The main questions investigated are:

1. How does electricity demand vary throughout the day?
2. How do weekday and weekend demand patterns differ?
3. How do electricity spot prices vary over time and throughout the day?
4. What is the relationship between electricity demand and spot price?
5. How does spot-price behaviour change across different demand levels?
6. When are extreme positive price spikes most likely to occur?
7. Under what conditions do negative electricity prices occur?
8. Can machine-learning models predict electricity spot prices or price-spike events?

---

## Dataset

The analysis uses electricity-market observations recorded at **5-minute
intervals** during August 2026.

Important variables include:

| Variable | Description |
|---|---|
| `SETTLEMENTDATE` | Date and time of the market interval |
| `TOTALDEMAND` | Total electricity demand (MW) |
| `RRP` | Regional Reference Price ($/MWh) |
| `hour` | Hour of day extracted from the timestamp |
| `day_number` | Day of week |
| `is_weekend` | Indicator for weekday/weekend |
| `demand_level` | Demand category: Low, Medium, High or Very High |
| `price_spike` | Indicator identifying unusually high-price observations |

Additional features are derived during the analysis to investigate temporal,
demand and extreme-price patterns.

---

## Analysis Workflow

### 1. Data Preparation

The raw electricity-market data is cleaned and prepared for analysis.

Key preprocessing steps include:

- parsing settlement timestamps;
- checking missing and duplicate observations;
- validating electricity demand and RRP values;
- extracting hour and day-of-week information;
- identifying weekday and weekend observations; and
- constructing additional analytical features.

---

### 2. Electricity Demand Analysis

Electricity demand is analysed across time to identify recurring consumption
patterns.

The analysis includes:

- average electricity demand by hour;
- peak and minimum demand periods;
- average demand by day of week;
- weekday versus weekend demand patterns; and
- intraday demand profiles.

The results show a clear daily demand cycle, with morning and evening increases
and lower demand around the middle of the day.

Weekday demand is generally stronger than weekend demand, particularly during
the morning period.

---

### 3. Electricity Spot Price Analysis

Spot-price behaviour is examined using both time-series and distributional
analysis.

The analysis includes:

- electricity spot prices over time;
- daily average spot prices;
- average spot price by hour;
- distribution of RRP values; and
- identification of unusually high and negative prices.

Spot prices exhibit substantially greater volatility than electricity demand,
with occasional extreme positive-price observations and periods of negative
pricing.

---

### 4. Demand–Price Relationship

The relationship between total electricity demand and RRP is investigated using
scatter plots, regression analysis and demand categories.

A positive relationship is observed between electricity demand and spot price.

A simple linear model produces:

**R² ≈ 0.371**

indicating that electricity demand explains a meaningful portion of spot-price
variation, but substantial variation remains attributable to other market
conditions.

Demand observations are also divided into four equally sized categories:

- Low
- Medium
- High
- Very High

Median and average spot prices increase progressively across these demand
categories.

---

### 5. Extreme Price Analysis

The project separately investigates unusually high prices and negative prices.

#### Positive Price Spikes

Price-spike events are concentrated during particular periods of the day.

The highest spike risk occurs during the late afternoon and evening, with a
particularly strong concentration around **17:00–18:00**.

Price-spike probability also increases substantially with electricity demand.
The Very High demand category experiences a much greater spike rate than the
Low, Medium and High categories.

#### Negative Electricity Prices

Negative prices show almost the opposite pattern.

They are primarily concentrated during lower-demand periods and occur most
frequently around the middle of the day.

The probability of a negative price reaches its highest level at approximately
**14:00**, while negative prices are overwhelmingly associated with the Low
demand category.

Together, these findings indicate that:

> **Low-demand periods are more exposed to unusually low or negative electricity
> prices, while very high-demand periods are substantially more exposed to
> extreme positive-price events.**

---

## Machine Learning Extension

The next stage of the project applies machine-learning methods to determine
whether the patterns identified during exploratory analysis can be translated
into predictive models.

Two prediction problems are considered.

### Spot Price Prediction

A regression task is used to predict continuous RRP values using features such
as:

- electricity demand;
- hour of day;
- day of week; and
- weekday/weekend status.

Candidate models include:

- Linear Regression
- Polynomial Regression
- Decision Tree Regression
- Random Forest Regression

Model performance can be evaluated using MAE, RMSE and R².

### Price-Spike Prediction

Price-spike prediction is formulated as a binary classification problem.

Candidate models include:

- Logistic Regression
- Decision Tree Classifier
- Random Forest Classifier

Because price spikes represent a relatively small proportion of observations,
model evaluation focuses on metrics such as **precision, recall, F1-score,
confusion matrices and ROC-AUC**, rather than relying only on classification
accuracy.

A chronological train-test split is used where appropriate so that earlier
observations are used to predict later market behaviour.

---

## Key Findings

The analysis identifies several important patterns:

- Electricity demand follows a clear intraday cycle.
- Weekday and weekend demand profiles differ, particularly during morning hours.
- Electricity spot prices are substantially more volatile than electricity demand.
- Higher electricity demand is generally associated with higher spot prices.
- Demand alone does not fully explain spot-price behaviour (`R² ≈ 0.371`).
- Median electricity prices rise progressively from Low to Very High demand.
- Positive price spikes are concentrated around high-demand morning and evening periods.
- Price-spike risk increases sharply under Very High electricity demand.
- Negative prices predominantly occur during low-demand periods.
- Negative-price risk is concentrated around midday and early afternoon.
- Extreme positive and negative prices therefore exhibit distinctly different demand and time-of-day patterns.

---

## Technologies

The project is developed using **Python** and Jupyter Notebook.

Main libraries include:

```text
pandas
numpy
matplotlib
scikit-learn
```

These tools are used for data cleaning, transformation, exploratory analysis,
visualisation, statistical modelling and machine learning.

---

## Repository Structure

```text
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

> File names and repository structure may differ depending on the final project
> organisation.

---

## Limitations

This analysis focuses on a relatively short observation period and uses a
limited set of explanatory variables.

Electricity prices are influenced by many factors beyond total demand,
including generation availability, renewable generation, transmission
constraints, weather, generator bidding behaviour and other electricity-market
conditions.

Therefore, relationships identified in this project should primarily be
interpreted as **associations rather than causal effects**.

The machine-learning models should similarly be interpreted as exploratory
predictive models rather than production electricity-price forecasting systems.

---

## Future Work

Several extensions could improve the project:

- incorporate weather variables such as temperature;
- include renewable-generation information;
- investigate lagged electricity demand and price features;
- develop time-series forecasting models;
- compare additional machine-learning algorithms;
- perform feature-importance analysis;
- investigate model performance across different time periods; and
- extend the dataset across multiple months or years.

---

## Conclusion

This project demonstrates an end-to-end data-science workflow for analysing
electricity-market behaviour.

Exploratory analysis reveals strong temporal patterns in electricity demand and
spot prices, while relationship analysis shows that higher demand is associated
with higher prices and increased positive price-spike risk. In contrast,
negative electricity prices are concentrated during lower-demand periods around
the middle of the day.

The machine-learning extension builds on these findings by investigating whether
demand and temporal characteristics can be used to predict electricity prices
and identify periods at elevated risk of extreme pricing.

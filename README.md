# YZM2011 — Machine Learning Project

## Problem Description

Turkey's electricity producers must submit hourly generation bids to the EPİAŞ day-ahead market before 12:00 each day, committing to production for the following 24 hours. Wrong bids result in imbalance penalties or wasted capacity. This project aims to predict Turkey's total hourly electricity consumption (`consumption_mwh`) 24 hours in advance using historical consumption, production, and weather data.

## Dataset Source

- **EPİAŞ Transparency Platform** — hourly electricity consumption and production data for Turkey (January 2021 – January 2026, ~43,824 rows)
- **Open-Meteo Archive API** — daily mean temperature, relative humidity, and wind speed for Istanbul, Ankara, and Izmir, averaged as a national proxy

## How the Three Projects Connect

| Project | Due | Focus | Role |
|---------|-----|-------|------|
| **P1 — Problem & EDA** | Week 6 | Problem formulation, data cleaning, exploration, visualization | Understand the data: distributions, seasonality, correlations, and outliers. Identifies the key features (temperature, hour of day, weekday/holiday) that drive consumption. |
| **P2 — Regression** | Week 10 | Linear/polynomial regression, feature engineering, evaluation | Uses insights from P1 to build and compare regression models for 24-hour-ahead forecasting. |
| **P3 — Classification or Beyond** | Week 15 | Classification/Regression, model selection, report | Selects the best model from P2, evaluates it rigorously, and wraps it into a usable forecasting pipeline. |


---

## P2 — Regression Modeling: Key Findings

**Feature Engineering**
Ten new features were added on top of the P1 columns: polynomial temperature (`temp²`), temperature × humidity interaction, cyclical sin/cos encoding for hour, month and day-of-week (replacing raw integers to properly represent the circular nature of time), peak-hour binary flag, week-over-week lag difference, and temperature bin dummies.

**Models Trained & Compared**

| Model | CV R² | Test R² | Test RMSE |
|-------|-------|---------|-----------|
| Baseline (lag_1d only) | 0.726 | 0.742 | 3,332 MWh |
| Multiple Linear Regression | 0.899 | 0.913 | 1,933 MWh |
| Polynomial Regression (deg=2) | −0.55 | 0.947 | 1,508 MWh |
| **Ridge (L2) ← chosen** | **0.906** | **0.912** | **1,943 MWh** |
| Lasso (L1) | 0.904 | 0.911 | 1,959 MWh |

**Best Model: Ridge (L2)**
Ridge was selected based on the highest cross-validation R² (0.906) across TimeSeriesSplit folds — the most reliable generalisation metric for a time-series problem. Polynomial degree 2 achieved the lowest test RMSE (1,508 MWh) but its negative CV R² (−0.55) indicates instability in smaller training folds. Ridge's test RMSE of 1,943 MWh represents a **42% improvement** over the lag-only baseline.

**Lasso Feature Selection**
Lasso automatically zeroed 6 of 24 features: `lag_diff`, `temp_avg`, `hum_avg`, `season_Spring`, `season_Winter`, and `temp_bin_warm` — confirming that these are redundant once the lag, quadratic temperature, and cyclical encoding features are present.

**Limitations & Next Steps (P3)**
Lag features (`lag_1d`, `lag_7d`) dominate all models, confirming that Turkey's electricity demand follows a strong 24-hour and 7-day cycle. Linear models achieve ~5% MAPE, close to but not yet at the EPİAŞ operational target of ±2–3%. P3 will explore non-linear models (gradient boosting, neural networks) using the same 80/20 chronological split and Ridge's 1,943 MWh RMSE as the benchmark.

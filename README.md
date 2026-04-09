# YZM2011 — Machine Learning Project

## Problem Description

Turkey's electricity producers must submit hourly generation bids to the EPİAŞ day-ahead market before 12:00 each day, committing to production for the following 24 hours. Wrong bids result in imbalance penalties or wasted capacity. This project aims to predict Turkey's total hourly electricity consumption (`consumption_mwh`) 24 hours in advance using historical consumption, production, and weather data.

## Dataset Source

- **EPİAŞ Transparency Platform** — hourly electricity consumption and production data for Turkey (January 2021 – January 2026, ~43,824 rows)
- **Open-Meteo Archive API** — daily mean temperature, relative humidity, and wind speed for Istanbul, Ankara, and Izmir, averaged as a national proxy

## How the Three Projects Connect

| Project | Focus | Role |
|---------|-------|------|
| **P1 — EDA** | Exploratory Data Analysis | Understand the data: distributions, seasonality, correlations, and outliers. Identifies the key features (temperature, hour of day, weekday/holiday) that drive consumption. |
| **P2 — Modeling** | Feature engineering & ML models | Uses insights from P1 to build and compare regression models (e.g. Linear Regression, Random Forest, XGBoost) for 24-hour-ahead forecasting. |
| **P3 — Evaluation & Deployment** | Model evaluation & application | Selects the best model from P2, evaluates it rigorously (RMSE, MAE, MAPE), and wraps it into a usable forecasting pipeline. |

The three projects form an end-to-end ML pipeline: **explore → model → deploy**.

---
title: Forecasting and Anomaly Detection
slug: forecasting-and-anomaly-detection
description: Sales forecasting and unsupervised anomaly detection on Walmart weekly retail data.
aliases:
- /p/forcasting-and-anomaly-detection/
image: forecasting.png
date: 2022-06-27T00:00:00+00:00
weight: 4
categories:
- Machine Learning
tags:
- Machine Learning
- Deep Learning
- Time Series
- Statistics
- Regression
- Anomaly Detection
---

**[GitHub Repository](https://github.com/U77w41/Forcasting-and-Anomaly-Detection)**

## Overview

Retail demand forecasting requires balancing macroeconomic indicators, regional promotional events, and seasonal holidays. At the same time, unexpected sales spikes or drops need to be isolated to separate genuine demand shifts from supply chain bottlenecks or data collection errors.

This project uses the historical Walmart sales dataset (covering 45 stores across diverse regions) to benchmark multiple forecasting approaches and evaluate unsupervised anomaly detection techniques on retail time series.

## Dataset & Feature Engineering

The dataset tracks weekly department-level sales alongside store attributes and regional economic indicators:

* **Temporal Signals:** Week of year, month, and major holiday indicators (Super Bowl, Labor Day, Thanksgiving, Christmas).
* **Store Characteristics:** Store type (A, B, C) and store size.
* **Macroeconomic Factors:** Regional Consumer Price Index (CPI), unemployment rates, and local fuel prices.
* **Promotions:** Anonymized markdown discounts (MarkDown1 through MarkDown5).

Preprocessing included forward-filling missing markdown values, cyclical encoding of week numbers, and scaling continuous economic variables.

## Forecasting Models

We evaluated several regression and time series models on held-out test periods:

### Linear & Regularized Regression

* **Multiple Linear Regression:** Established a baseline mapping features directly to weekly revenue.
* **Polynomial Feature Expansion:** Captured non-linear interactions between weather (temperature), holiday proximity, and markdown events.
* **L1 & L2 Regularization (Lasso, Ridge, Elastic Net):** Penalized collinear features—particularly among markdowns and store size tiers—to control overfitting and improve test set generalization.

### Time Series Forecasting

* **ARMA / ARIMA:** Modeled the sequential auto-regressive structure and moving averages on stationary, differenced weekly sales series to capture recurring annual seasonality.

## Anomaly Detection Approaches

To identify unusual sales swings without labeled incident logs, we applied three unsupervised techniques:

* **k-Nearest Neighbors (k-NN) Distance:** Evaluates each data point's Euclidean distance to its $k$-th nearest neighbor in feature space. Samples with large neighborhood distances represent rare sales conditions.
* **DBSCAN (Density-Based Spatial Clustering):** Groups dense clusters of regular sales activity while isolating points in low-density regions as noise/anomalies.
* **LSTM Autoencoders:** Sequences of weekly sales were passed through an LSTM encoder-decoder architecture trained exclusively on typical demand patterns. High reconstruction errors flag non-standard anomalies where sales deviate sharply from temporal expectations.

## Data Version Control (DVC)

To keep model weights, intermediate data splits, and feature matrices reproducible across runs without committing multi-megabyte CSVs into Git, the project uses [Data Version Control (DVC)](https://dvc.org/). DVC tracks pointer files in Git while storing large artifacts in remote storage, making every experiment pipeline reproducible from raw data to evaluation metrics.

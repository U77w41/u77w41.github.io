---
title: 'NSEScraper: An Open-Source Scraper for nseindia.com'
slug: nsescraper
description: Open-source Python package on PyPI to fetch real-time intraday candlestick and tick data from the National Stock Exchange of India.
aliases:
- /p/nsescraper-an-open-source-scrapper-for-nseindia.com-website./
image: nse_homepage.png
date: 2024-08-03T17:37:11+00:00
weight: 2
categories:
- Open Source
tags:
- Data Mining
- Web Scraping
- Stock Market
- Python
---

**[PyPI Package](https://pypi.org/project/nsescraper/) · [GitHub Repository](https://github.com/u77w41/nsescraper)**

## Overview

The National Stock Exchange of India (NSE) website provides live market feeds, but extracting clean intraday price histories for quantitative analysis or algorithmic backtesting often requires navigating session headers, cookie rotation, and nested JSON endpoints.

**nsescraper** is a lightweight, open-source Python library that queries NSE endpoints directly and returns cleaned, timestamped pandas DataFrames for any listed index or equity. It supports standard 1-minute OHLC bars, custom multi-minute intervals, and second-by-second tick data.

## Installation

Install the package directly from PyPI using pip:

```bash
pip install nsescraper
```

Dependencies: Python 3.8+ and `pandas`.

## Features

* **Real-time Intraday Feeds:** Extract the current trading day's prices for indices (Nifty 50, Bank Nifty, Midcap, Sectoral indices) and all NSE-listed stocks.
* **1-Minute OHLC Candles:** Formats timestamped Open, High, Low, and Close bars across the full market session.
* **1-Second Tick Data:** Capture granular tick-level price changes throughout active market hours.
* **Custom Candlestick Resampling:** Parameterize candle intervals to custom durations (e.g., 5-minute, 15-minute, or 60-minute bars).
* **Direct pandas Integration:** Every function returns a standard pandas DataFrame ready for technical analysis, plotting, or CSV export.

## Usage & Examples

Import the package functions:

```python
from nsescraper import intraday_index, intraday_stock, historical_index, historical_stock
```

### 1. 1-Minute OHLC Candlestick Data

To fetch the current day's 1-minute candles for an index:

```python
df_nifty = intraday_index("nifty 50")
print(df_nifty.head())
```

```text
+-----+---------------------+---------+---------+---------+---------+
|     | DATETIME            |    open |    high |     low |   close |
+=====+=====================+=========+=========+=========+=========+
|   0 | 2024-08-02 09:00:00 | 25010.9 | 25085.6 | 24777.0 | 24945.8 |
|   1 | 2024-08-02 09:01:00 | 24946.0 | 24950.0 | 24826.8 | 24830.0 |
|   2 | 2024-08-02 09:02:00 | 24816.5 | 24816.5 | 24784.0 | 24792.3 |
|   3 | 2024-08-02 09:03:00 | 24791.5 | 24791.5 | 24777.6 | 24789.8 |
+-----+---------------------+---------+---------+---------+---------+
```

To fetch 1-minute candles for an individual stock:

```python
df_stock = intraday_stock("britannia")
print(df_stock.head())
```

```text
+-----+---------------------+---------+---------+---------+---------+
|     | DATETIME            |    open |    high |     low |   close |
+=====+=====================+=========+=========+=========+=========+
|   0 | 2024-08-02 09:15:00 | 5645.9  | 5695.8  | 5645.9  | 5695.8  |
|   1 | 2024-08-02 09:16:00 | 5707.8  | 5732.95 | 5706.7  | 5729.85 |
|   2 | 2024-08-02 09:17:00 | 5715.0  | 5737.9  | 5709.15 | 5729.85 |
+-----+---------------------+---------+---------+---------+---------+
```

### 2. High-Frequency 1-Second Tick Data

Pass `tick=True` to retrieve tick-by-tick market logs:

```python
# Tick data for index
df_midcap_ticks = intraday_index("nifty midcap 100", tick=True)

# Tick data for equity
df_reddy_ticks = intraday_stock("dr reddy", tick=True)
print(df_reddy_ticks.head())
```

```text
+------+---------------------+---------+
|      | DATETIME            |    Tick |
+======+=====================+=========+
|    0 | 2024-08-02 09:15:24 | 6891.20 |
|    1 | 2024-08-02 09:15:30 | 6886.10 |
|    2 | 2024-08-02 09:15:37 | 6900.05 |
|    3 | 2024-08-02 09:15:44 | 6900.60 |
+------+---------------------+---------+
```

### 3. Custom Timeframe Candlesticks

Use the `candlestick` argument to specify an arbitrary minute interval (e.g., 10-minute candles):

```python
df_energy_10m = intraday_index("nifty energy", candlestick=10)
print(df_energy_10m.head())
```

```text
+----+---------------------+---------+---------+---------+---------+
|    | DATETIME            |    open |    high |     low |   close |
+====+=====================+=========+=========+=========+=========+
|  0 | 2024-08-02 09:00:00 | 44954.4 | 44954.4 | 44481.8 | 44481.8 |
|  1 | 2024-08-02 09:10:00 | 44481.8 | 44627.7 | 44258.1 | 44625.6 |
|  2 | 2024-08-02 09:20:00 | 44618.6 | 44684.2 | 44537.8 | 44551.7 |
+----+---------------------+---------+---------+---------+---------+
```

### 4. Historical Data Retrieval

To fetch longer-term end-of-day price histories:

```python
df_hist_nifty = historical_index("NIFTY 50")
df_hist_abb = historical_stock("ABB")
```

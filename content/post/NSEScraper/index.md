---
title: "NSEScraper an open-source scrapper for nseindia.com website."
description: nsescraper is an open-source python package to scrap stock/index data from nseindia.com.
date: 2024-08-03T17:37:11Z
image: nse_homepage.png
math: 
license: 
categories:
    - Projects
tags:
    - Data Mining
    - Web Scrapping
    - Stock Market
weight: 6 
hidden: false
comments: true
draft: false
---

# Installation

To install using pip, use

``` python
pip install nsescraper
```

**nsescraper** requires [Python 3](https://www.python.org/) and [Pandas](https://pandas.pydata.org/) to execute.

# Features:

**nsescraper** package contains different functions such as:

- **intraday_index**: Scrapes intra day data for any index from NSE
- **intraday_stock**: Scrapes intra day data for any listed stock from NSE

# Usage

* **Import the library**:

``` python
from nsescraper import *
```

* **Choose a method:**

To scrap current days nse index/stock data as 1 minute candle format

``` python
intraday_index("nifty 50")
```

``` python
intraday_stock("britannia")
```

To scrap current days nse index/stock data as 1 second tick format

``` python
intraday_index("nifty midcap 100", tick= True)
```

``` python
intraday_stock("dr reddy", tick= True)
```

To scrap current days nse index/stock data as required length minute candle

```python
intraday_index("nifty energy", candlestick= 10)
```

```python
intraday_stock("Zomato", candlestick= 69)
```

To scrap historical nse index/stock data

```python
historical_index('NIFTY 50')
```

```python
historical_stock('ABB')
```
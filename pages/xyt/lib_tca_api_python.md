---
title: TCA Analysis from Python
keywords: python, sample, data access, xythub, xyt hub, xyt-hub, big xyt, big-xyt, documentation, API documentation, TCA API, execution analytics
summary: "This is the API documentation for xythub-tca Python package."
sidebar: xyt_sidebar
permalink: lib_tca_api_python.html
folder: xyt
---

Installation
------------

To install package from file:
```sh
> pip install xythub-tca-X.Y.Z.tar.gz
```

### Requirements

This library requires Python 2.7 or 3.0+ to run.


Configuration
-------------

To configure authentication settings set following attributes in the
`ApiSettings` class:
```python
from xythub import ApiSettings

ApiSettings.user_id = USERNAME
ApiSettings.user_password = PASSWORD
```

Browsing Universe
-------------
### Symbols lookup

Looking up symbols on given data source by pattern:
```python
from xythub import lookup_symbols

symbols = lookup_symbols(source='ACTIV', pattern='apple')
```

Looking up symbols on given data source by pattern and dates range:
```python
from xythub import lookup_symbols

symbols = lookup_symbols(source='ACTIV', pattern='apple',
                         first_day='2016.01.01', last_day='2016.12.31')
```

Looking symbols on given data source:
```python
from xythub import lookup_symbols

symbols = lookup_symbols(source='TCA')
```

#### Advanced searching

More advanced search limiting to the specific exchanges on the data source:
```python
symbols = lookup_symbols(source='ACTIV', pattern='zal', exchange_filters=['XETRA', 'BXTR'],
                         first_day='2016.01.01', last_day='2016.12.31')
```

#### Input parameters

| Parameter              | Type                   | Required  | Description                                      |
|------------------------|------------------------|-----------|--------------------------------------------------|
| source                 | string                 | x         | Data source.                                     |
| pattern                | string                 |           | Lookup pattern.                                  |
| exchange_filters       | [string]               |           | Narrow lookup to one or more exchange names.     |
| region_filters         | [string]               |           | Narrow lookup to one or more region names.       |
| entity_class_filters   | [string]               |           | Narrow lookup to one or more entities.           |
| first_day              | pd.Timestamp / string  |           | First day that should be taken into account.     |
| last_day               | pd.Timestamp / string  |           | Last day that should be taken into account.      |
| page_limit             | int                    |           | Size of the paging window.                       |
| cursor                 | string                 |           | Paging cursor returned by the previous call.     |


#### Output columns

| Column           | Type           | Description                |
|------------------|----------------|----------------------------|
| source           | string         | Source identifier.         |
| region           | string         | Region identifier.         |
| exchange         | string         | Exchange identifier.       |
| symbol           | string         | Symbol.                    |
| name             | string         | Full item name.            |
| isin             | string         | ISIN.                      |
| local_code       | string         | Exchange local code.       |
| currency         | string         | Item currency.             |
| market_segment   | string         | Market segment.            |
| entity_class     | string         | Entity class.              |
| item_type        | ItemType       | Item type.                 |
| first_day        | pd.Timestamp   | First available day.       |
| last_day         | pd.Timestamp   | Last available day.        |

Transaction Cost Analysis (TCA) API
-------
### Tick Analytics

```python
tick_analytics(
    source='TCA',
    symbols=['ORA.P'],
    metrics={
        'Market': MarketAggregate(
            day='2019.10.10',
            from_time='10:00:00.000',
            to_time='10:10:00.000',
            fields=['VWAP']
        )
    }
).to_pandas()
```
#### Input parameters

| Parameter              | Type                   | Required  | Description                                      |
|------------------------|------------------------|-----------|--------------------------------------------------|
| source                 | string                 | x         | Data source.                                     |
| symbols                | [string]               | x         | List of symbols                                  |
| metrics                | dict [Str: Metrics]    | x         | Metrics to be calculated                         |

#### Output columns

| Column           | Type           | Description                |
|------------------|----------------|----------------------------|
| To be defined    | ....           | ...........                |

### Supported metrics

- Interval Analysis
    - Trades (Market) Measures:
        - Volume Weighted Average Price (VWAP)
        - Traded Volume
        - Trades Count
        - OHLC measures
        - Participation Weighted Price (PVWAP)
        - Relative Performance Measure (RPM)
    - Quotes (Order Book) Measures:
        - Average Spread Size
        - Book Size
- Point-in-time Analysis
    - Bid/Ask related Measures




License
-------

[Apache License Version 2.0](http://opensource.org/licenses/Apache-2.0)

{% include links.html %}

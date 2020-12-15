---
title: TCA Python API
keywords: python, sample, data access, xythub, xyt hub, xyt-hub, big xyt, big-xyt, documentation, API documentation, API, single API, tick data
summary: "This is the API documentation for xyt hub Python package."
sidebar: xyt_sidebar
permalink: lib_tca_api_python.html
folder: xyt
---

### 0. Prerequisites

API version: `1.1.8.0`

-------------

This library requires Python 2.7 or 3.0+ to run.

-------------

To install package from file:
```sh
> pip install xythub-tca-X.Y.Z.tar.gz
```

### 0.1. Library imports

```python
from xythub import ApiSettings, lookup_symbols

from xythub_tca.data import tick_analytics
from xythub_tca.metrics import *
```

### 0.2. API authentication


To configure authentication settings set following attributes in the
`ApiSettings` class:
```python
ApiSettings.user_id = 'YOUR_USERNAME'
ApiSettings.user_password = 'YOUR_PASSWORD'
```

### 1. Symbols lookup

For more details, please visit [xyt hub API documentation](https://xythub.github.io/lib_python.html#symbols-lookup).

#### Example:
Looking up symbols on given data source:
```python
symbols = lookup_symbols(source='TCA')
```

Looking up symbols on given data source by pattern and dates range limiting to the specific exchanges on the data source:
```python
symbols = lookup_symbols(
    source='TCA',
    exchange_filters=['XETRA', 'BXTR'],
    first_day='2016.01.01',
    last_day='2016.12.31'
).to_pandas()
```

|    | source   | region   | exchange   | symbol     | isin         | currency   | mic   | primary_market   | operating_mic   |
|----|----------|----------|------------|------------|--------------|------------|-------|------------------|-----------------|
|  0 | TCA      | EMEA     | XETRA      | 00XJ.XE    | DE000A1NZLJ4 | EUR        | XETA  | XETR             | XETR            |
|  1 | TCA      | EMEA     | XETRA      | 00XK.XE    | DE000A1NZLK2 | EUR        | XETA  | XETR             | XETR            |
|  2 | TCA      | EMEA     | XETRA      | 00XL.XE    | DE000A1NZLL0 | EUR        | XETA  | XETR             | XETR            |
|  3 | TCA      | EMEA     | BXTR       | 00XJd.BXTR | DE000A1NZLJ4 | EUR        | BOTC  | XETR             | BCXE            |
|  4 | TCA      | EMEA     | BXTR       | 00XKd.BXTR | DE000A1NZLK2 | EUR        | BOTC  | XETR             | BCXE            |

<br>

### 2. Transaction Cost Analysis (TCA)

Main function used for getting TCA result

```python
tick_analytics(
    source='TCA',
    symbols=['DPW.XE', 'DPWd.CHIX'],
    metrics={
        'MarketAggregate': MarketAggregate(
            fields=['VOLUME'],
            from_datetime=pd.Timestamp('2019-10-10 10:00:00.000'),
            to_datetime=pd.Timestamp('2019-10-10 12:00:00.000'),
        )
    }
).to_pandas()
```

#### Input parameters

| Parameter              | Type                   | Required  | Description                                      |
|------------------------|------------------------|-----------|--------------------------------------------------|
| source                 | string                 | x         | Data source                                      |
| symbols                | [string]               | x         | List of symbols                                  |
| metrics                | dict [Str: Metrics]    | x         | Metrics to be calculated                         |    
| flags                  | [TickDataFlag]         |           | List of flags                                    |                 

#### Output columns

Output columns depend on the chosen metrics.

<br>

### 2.1. Supported Metrics

| type   | metric                | description                                           |
|--------|-----------------------|-------------------------------------------------------|
| quotes | [BookSnapshot](#BookSnapshot)        | best bid/ask data at given moment                     |
| quotes | [BookAggregate](#BookAggregate)       | best bid/ask data aggregated across time              |
| quotes | [BookDepth](#BookDepth)           | bid/ask data at given moment aggregated across levels |
| trades | [MarketAggregate](#MarketAggregate)     | aggregated trade data across time                     |
| trades | [PVWAP](#PVWAP)               | VWAP at given participation rate                      |
| trades | [RPM](#RPM)                 | relative performance measure based on count or volume |
| trades | [Volatility](#Volatility)          | intraday volatility estimation                        |
| trades | [MarketParticipation](#MarketParticipation) | detailed PVWAP metric combining volume                |

#### <a name="BookSnapshot">BookSnapshot</a>

Best bid/offer (EBBO, NBBO) data at given moment.

Tick data on a given data source and symbols, using Book Snapshot metric limiting to specific datetime and fields:

```python
tick_analytics(
    source='TCA',
    symbols=['DPW.XE', 'DPWd.CHIX', 'DPWd.TU', 'DPWd.AQX', 'DPWd.BTE'],
    metrics={
        'PBBO': BookSnapshot(
            symbols=['DPW.XE'],
            datetime=pd.Timestamp('2019-10-10 10:00:00.00'),
            fields=['MID', 'ASK', 'BID']
        ),
        'EBBO': BookSnapshot(
            symbols=['DPW.XE', 'DPWd.CHIX', 'DPWd.TU', 'DPWd.AQX', 'DPWd.BTE'],
            datetime=pd.Timestamp('2019-10-10 10:00:00.00'),
            fields=['MID', 'ASK', 'BID']
        ),
    }
).to_pandas()
```

*Input parameters:*

| Parameter              | Type         | Required  | Description                                  |
|------------------------|--------------|-----------|----------------------------------------------|
| datetime               | pd.Timestamp | x         | Requested datetime                           |
| fields                 | [string]     | x         | List of [fields](#BookSnapshot_field_values) |
| symbols                | [string]     |           | List of symbols                              |
| eod_symbol             | string       |           | Symbol for OHLC prints, required if datetime outside Continuous Session |

<br>

<a name="BookSnapshot_field_values">*Available field values:*</a>

| Field          | Description      |
|----------------|------------------|
| MID            | Mid              |
| ASK            | Ask              |
| BID            | Bid              |
| SPREAD         | Spread           |
| ASK_SIZE       | Ask Size         |
| BID_SIZE       | Bid Size         |
| ASK_VENUE      | Ask Venue        |
| BID_VENUE      | Bid Venue        |
| AT_TOUCH       | At touch spread  |

<br>

#### <a name="BookAggregate">BookAggregate</a>

Best bid/offer (EBBO, NBBO) data aggregated across time.

Tick data on a given data source and symbols, using Book Aggregate metric limiting to specific datetime range and fields:

```python
tick_analytics(
    source='TCA',
    symbols=['DPW.XE', 'DPWd.CHIX'],
    metrics={
        'BookAggregate': BookAggregate(
            from_datetime=pd.Timestamp('2019-10-10 10:00:00.00'),
            to_datetime=pd.Timestamp('2019-10-10 12:00:00.00'),
            fields=['AVG_SPREAD', 'AVG_ASK', 'AVG_BID']
        )
    }
).to_pandas()
```

*Input parameters:*

| Parameter              | Type         | Required  | Description                                   |
|------------------------|--------------|-----------|-----------------------------------------------|
| from_datetime          | pd.Timestamp | x         | Start of the requested datetime range         |
| to_datetime            | pd.Timestamp | x         | End of the requested datetime range           |
| fields                 | [string]     | x         | List of [fields](#BookAggregate_field_values) |
| symbols                | [string]     |           | List of symbols                               |

<br>

<a name="BookAggregate_field_values">*Available field values:*</a>

| Field             | Description                |
|-------------------|----------------------------|
| AVG_SPREAD        | Average spread             |
| AVG_BID           | Average bid                |
| AVG_ASK           | Average ask                |
| AVG_BID_SIZE      | Average bid size           |
| AVG_ASK_SIZE      | Average ask size           |

<br>

#### <a name="BookDepth">BookDepth</a>

Bid/offer data at given moment aggregated across levels (til level 10).

Tick data on a given data source and symbols, using Book Depth metric limiting to specific datetime and fields:

```python
tick_analytics(
    source='TCA',
    symbols=['DPW.XE', 'DPWd.CHIX'],
    metrics={
        'BookDepth': BookDepth(
            datetime=pd.Timestamp('2019-10-10 10:00:00.00'),
            fields=['BID_WAVG', 'ASK_WAVG'],
        )
    }
).to_pandas()
```

Tick data on a given data source and symbols, using Book Depth metric limiting to specific datetime, fields,
level limit and size limit:

```python
tick_analytics(
    source='TCA',
    symbols=['DPW.XE', 'DPWd.CHIX'],
    metrics={
        'BookDepth': BookDepth(
            datetime=pd.Timestamp('2019-10-10 10:00:00.00'),
            fields=['BID_WAVG', 'ASK_WAVG'],
            level_limit=10,
            size_limit=200
        )
    }
).to_pandas()
```

*Input parameters:*

| Parameter              | Type         | Required  | Description                               |
|------------------------|--------------|-----------|-------------------------------------------|
| datetime               | pd.Timestamp | x         | Requested datetime                        |
| fields                 | [string]     | x         | List of [fields](#BookDepth_field_values) |
| level_limit            | int          |           | Level limit                               |
| size_limit             | float        |           | Size limit                                |
| symbols                | [string]     |           | List of symbols                           |

<br>

<a name="BookDepth_field_values">*Available field values:*</a>

| Field             | Description                             |
|-------------------|-----------------------------------------|
| BID_WAVG          | Volume weighted average bid price       |
| ASK_WAVG          | Volume weighted average ask price       |
| BID_SIZE_SUM      | Aggregated bid size                     |
| ASK_SIZE_SUM      | Aggregated ask size                     |

<br>

#### <a name="MarketAggregate">MarketAggregate</a>

Aggregated trade data across time.

Tick data on a given data source and symbols, using Market Aggregate metric limiting to specific fields and datetime range:

```python
tick_analytics(
    source='TCA',
    symbols=['DPW.XE', 'DPWd.CHIX'],
    metrics={
        'MarketAggregate': MarketAggregate(
            fields=['VOLUME'],
            from_datetime=pd.Timestamp('2019-10-10 10:00:00.000'),
            to_datetime=pd.Timestamp('2019-10-10 12:00:00.000')
        )
    }
).to_pandas()
```

Tick data on a given data source and symbols, using Market Aggregate metric limiting to specific fields, datetime range and lower price limit:

```python
tick_analytics(
    source='TCA',
    symbols=['DPW.XE', 'DPWd.CHIX'],
    metrics={
        'MarketAggregate': MarketAggregate(
            fields=['VOLUME'],
            from_datetime=pd.Timestamp('2019-10-10 10:00:00.000'),
            to_datetime=pd.Timestamp('2019-10-10 12:00:00.000'),
            lower_price_limit=10
        )
    }
).to_pandas()
```

*Input parameters:*

| Parameter              | Type                   | Required  | Description                                     |
|------------------------|------------------------|-----------|-------------------------------------------------|
| fields                 | [string]               | x         | List of [fields](#MarketAggregate_field_values) |
| from_datetime          | pd.Timestamp           | x         | Start of the requested datetime range           |
| to_datetime            | pd.Timestamp           | x         | End of the requested datetime range             |
| lower_price_limit      | float                  |           | Minimum price                                   |
| upper_price_limit      | float                  |           | Maximum price                                   |
| trade_types            | [string]               |           | List of trade types                             |
| exclude_trade_types    | [string]               |           | List of trade types to exclude                  |
| split_by_trade_type    | bool                   |           | Split by trade type selection                   |
| use_last_snapshot      | bool                   |           | Use last snapshot                               |
| symbols                | [string]               |           | List of symbols                                 |

<br>

<a name="MarketAggregate_field_values">*Available field values:*</a>

| Field             | Description                   |
|-------------------|-------------------------------|
| COUNT             | Number of trades              |
| VOLUME            | Market Volume                 |
| VALUE             | Market Turnover               |
| TWAP              | Time weighted average price   |
| VWAP              | Volume weighted average price |
| TRADE_DIRECTION   | Trade direction               |
| FIRST             | First price                   |
| MIN               | Lowest price                  |
| MAX               | Highest price                 |
| LAST              | Last price                    |
| LAST_TS           | Last trade size               |
| MEDIAN_TS         | Median trade size             |

<br>

#### <a name="PVWAP">PVWAP</a>

Volume weighted average price at given participation rate.

Tick data on a given data source and symbols, using PVWAP metric limiting to specific datetime range start,
trade size and participation rate:

```python
tick_analytics(
    source='TCA',
    symbols=['DPW.XE', 'DPWd.CHIX'],
    metrics={
        'PVWAP': PVWAP(
            from_datetime=pd.Timestamp('2019-10-10 10:00:00.00'),
            trade_size=100,
            participation_rate=0.5
        )
    }
).to_pandas()
```

Tick data on a given data source and symbols, using PVWAP metric limiting to datetime range,
trade size, participation rate and lower price limit:

```python
tick_analytics(
    source='TCA',
    symbols=['DPW.XE', 'DPWd.CHIX'],
    metrics={
        'PVWAP': PVWAP(
            from_datetime=pd.Timestamp('2019-10-10 10:00:00.00'),
            trade_size=100,
            participation_rate=0.5,
            lower_price_limit=10,
            to_datetime=pd.Timestamp('2019-10-10 12:00:00.00')
        )
    }
).to_pandas()
```

*Input parameters:*

| Parameter              | Type                  | Required  | Description                            |
|------------------------|-----------------------|-----------|----------------------------------------|
| from_datetime          | pd.Timestamp          | x         | Start of the requested datetime range  |
| trade_size             | int                   | x         | Size of trade                          |
| participation_rate     | float                 | x         | Participation rate                     |
| lower_price_limit      | float                 |           | Minimum price                          |
| upper_price_limit      | float                 |           | Maximum price                          |
| trade_types            | [string]              |           | List of trade types                    |
| symbols                | [string]              |           | List of symbols                        |
| to_datetime            | pd.Timestamp          |           | End of the requested datetime range    |

<br>

#### <a name="RPM">RPM</a>

Relative performance measure based on count or volume.

Tick data on a given data source and symbols, using RPM metric limiting to specific datetime range, benchmark price and fields:

```python
tick_analytics(
    source='TCA',
    symbols=['DPW.XE', 'DPWd.CHIX'],
    metrics={
        'RPM': RPM(
            from_datetime=pd.Timestamp('2019-10-10 10:00:00.00'),
            to_datetime=pd.Timestamp('2019-10-10 12:00:00.00'),
            benchmark_price=100,
            fields=['TRADES', 'SHARES']
        )
    }
).to_pandas()
```

Tick data on a given data source and symbols, using RPM metric limiting to specific datetime range, benchmark price,
 fields and price range:

```python
tick_analytics(
    source='TCA',
    symbols=['DPW.XE', 'DPWd.CHIX'],
    metrics={
        'RPM': RPM(
            from_datetime=pd.Timestamp('2019-10-10 10:00:00.00'),
            to_datetime=pd.Timestamp('2019-10-10 12:00:00.00'),
            benchmark_price=100,
            fields=['TRADES', 'SHARES'],
            lower_price_limit=95,
            upper_price_limit=105
        )
    }
).to_pandas()
```

*Input parameters:*

| Parameter              | Type         | Required  | Description                           |
|------------------------|--------------|-----------|---------------------------------------|
| from_datetime          | pd.Timestamp | x         | Start of the requested datetime range |
| to_datetime            | pd.Timestamp | x         | End of the requested datetime range   |
| benchmark_price        | float        | x         | Benchmark price                       |
| fields                 | [string]     | x         | List of [fields](#RPM_field_values)   |
| lower_price_limit      | float        |           | Minimum price                         |
| upper_price_limit      | float        |           | Maximum price                         |
| trade_types            | [string]     |           | List of trade types                   |
| symbols                | [string]     |           | List of symbols                       |

<br>

<a name="RPM_field_values">*Available field values:*</a>

| Field          | Description     |
|----------------|-----------------|
| TRADES         | RPM based on count          |
| SHARES         | RPM based on volume          |
| TRADES_INCLUDE_EQUAL     | RPM based on count, including trades with price equal to benchmark      |
| SHARES_INCLUDE_EQUAL     | RPM based on volume, including trades with price equal to benchmark      |

<br>

#### <a name="Volatility">Volatility</a>

Intraday volatility estimation.

Tick data on a given data source and symbols, using Volatility metric limiting to specific fields and datetime range:

```python
tick_analytics(
    source='TCA',
    symbols=['DPW.XE', 'DPWd.CHIX'],
    metrics={
        'Volatility': Volatility(
            fields=['REALIZED', 'VARIANCE'],
            from_datetime=pd.Timestamp('2019-10-10 10:00:00.00'),
            to_datetime=pd.Timestamp('2019-10-10 12:00:00.00'),
        )
    }
).to_pandas()
```

Tick data on a given data source and symbols, using Volatility metric limiting to specific fields, datetime range and bin size:

```python
tick_analytics(
    source='TCA',
    symbols=['DPW.XE', 'DPWd.CHIX'],
    metrics={
        'Volatility': Volatility(
            fields=['REALIZED', 'VARIANCE'],
            from_datetime=pd.Timestamp('2019-10-10 10:00:00.00'),
            to_datetime=pd.Timestamp('2019-10-10 12:00:00.00'),
            bin_size=100
        )
    }
).to_pandas()
```

*Input parameters:*

| Parameter              | Type                   | Required  | Description                                |
|------------------------|------------------------|-----------|--------------------------------------------|
| fields                 | [string]               | x         | List of [fields](#Volatility_field_values) |
| from_datetime          | pd.Timestamp           | x         | Start of the requested datetime range      |
| to_datetime            | pd.Timestamp           | x         | End of the requested datetime range        |
| bin_size               | int                    |           | Bin size in seconds                        |
| annualization_factor   | float                  |           | Annualization factor                       |
| trade_types            | [string]               |           | List of trade types                        |
| symbols                | [string]               |           | List of symbols                            |

<br>

<a name="Volatility_field_values">*Available field values:*</a>

| Field             | Description                |
|-------------------|----------------------------|
| REALIZED          | Realized                   |
| VARIANCE          | Variance                   |
| PARKINSON         | Parkinson                  |
| GARMAN_KLASS      | Garman Klass               |
| ROGERS_SATCHELL   | Rogers Satchell            |

<br>

#### <a name="MarketParticipation">MarketParticipation</a>

Enhanced version of PVWAP metric including volume.

Tick data on a given data source and symbols, using Market Participation metric limiting to specific datetime range,
trade size, participation rate and fields:

```python
tick_analytics(
    source='TCA',
    symbols=['DPW.XE', 'DPWd.CHIX'],
    metrics={
        'MarketParticipation': MarketParticipation(
            from_datetime=pd.Timestamp('2019-10-10 10:00:00.00'),
            to_datetime=pd.Timestamp('2019-10-10 12:00:00.00'),
            trade_size=100,
            participation_rate=0.5,
            fields=['PVWAP', 'VOLUME']
        )
    }
).to_pandas()
```

Tick data on a given data source and symbols, using Market Participation metric limiting to specific datetime range,
trade size, participation rate, price limit and fields:

```python
tick_analytics(
    source='TCA',
    symbols=['DPW.XE', 'DPWd.CHIX'],
    metrics={
        'MarketParticipation': MarketParticipation(
            from_datetime=pd.Timestamp('2019-10-10 10:00:00.00'),
            to_datetime=pd.Timestamp('2019-10-10 12:00:00.00'),
            trade_size=100,
            participation_rate=0.5,
            price_limit=200,
            fields=['PVWAP', 'VOLUME']
        )
    }
).to_pandas()
```

*Input parameters:*

| Parameter              | Type                   | Required  | Description                                         |
|------------------------|------------------------|-----------|-----------------------------------------------------|
| fields                 | [string]               | x         | List of [fields](#MarketParticipation_field_values) |
| from_datetime          | pd.Timestamp           | x         | Start of the requested datetime range               |
| to_datetime            | pd.Timestamp           | x         | End of the requested datetime range                 |
| trade_size             | int                    | x         | Size of trade                                       |
| participation_rate     | float                  | x         | Participation rate                                  |
| price_limit            | float                  |           | Price limit                                         |
| trade_types            | [string]               |           | List of trade types                                 |
| symbols                | [string]               |           | List of symbols                                     |


<br>

<a name="MarketParticipation_field_values">*Available field values:*</a>

| Field                   | Description                |
|-------------------------|----------------------------|
| PVWAP                   | PVWAP                      |
| VOLUME                  | Volume cumulated until order is filled                    |
| VOLUME_ABOVE_PVWAP      | Volume above PVWAP value, cumulated until order is filled         |
| VOLUME_BELOW_PVWAP      | Volume below PVWAP value, cumulated until order is filled         |
| VOLUME_ABOVE_LIMIT      | Volume above limit value, cumulated until order is filled         |
| VOLUME_BELOW_LIMIT      | Volume below limit value, cumulated until order is filled         |
| VOLUME_TILL_END              | Volume cumulated until to_datetime parameter                  |
| VOLUME_TILL_END_ABOVE_PVWAP  | Volume above PVWAP value, cumulated until to_datetime parameter     |
| VOLUME_TILL_END_BELOW_PVWAP  | Volume below PVWAP value, cumulated until to_datetime parameter     |

<br>

#### Example

```python
# Set parameters
FROM_DATETIME = pd.Timestamp('2019-10-10 10:00:00.00')
TO_DATETIME = pd.Timestamp('2019-10-10 12:00:00.00')
LIMIT_PRICE = 10

tick_data = tick_analytics(
    source='TCA',
    symbols=['ORA.P'],
    metrics={
        'Market': MarketAggregate(
            from_datetime=FROM_DATETIME,
            to_datetime=TO_DATETIME,
            fields=['VOLUME', 'VWAP']
        ),

        'MarketLimit': MarketAggregate(
            from_datetime=FROM_DATETIME,
            to_datetime=TO_DATETIME,
            fields=['VOLUME', 'VWAP'],
            lower_price_limit=LIMIT_PRICE
        ),

        'PVWAP_05': PVWAP(
            from_datetime=FROM_DATETIME,
            trade_size=100,
            participation_rate=0.05
        ),

        'BookSnapshot': BookSnapshot(
            datetime=FROM_DATETIME,
            fields=['MID', 'ASK', 'BID']
        ),

        'RPM': RPM(
            from_datetime=FROM_DATETIME,
            to_datetime=TO_DATETIME,
            benchmark_price=LIMIT_PRICE,
            fields=['SHARES', 'TRADES']
        )
    }
).to_pandas()
```

<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Market_VOLUME</th>
      <th>Market_VWAP</th>
      <th>MarketLimit_VOLUME</th>
      <th>MarketLimit_VWAP</th>
      <th>PVWAP_05</th>
      <th>BookSnapshot_MID</th>
      <th>BookSnapshot_ASK</th>
      <th>BookSnapshot_BID</th>
      <th>RPM_SHARES</th>
      <th>RPM_TRADES</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>0</td>
      <td>362440</td>
      <td>14.462757</td>
      <td>362440</td>
      <td>14.462757</td>
      <td>14.43</td>
      <td>14.4275</td>
      <td>14.43</td>
      <td>14.425</td>
      <td>0.0</td>
      <td>0.0</td>
    </tr>
  </tbody>
</table>
</div>
<br>

{% include links.html %}

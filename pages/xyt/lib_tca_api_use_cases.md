---
title: TCA Python API use cases
keywords: python, sample, data access, xythub, xyt hub, xyt-hub, big xyt, big-xyt, documentation, API documentation, API, single API, tick data
summary: "This is the API documentation for xyt hub Python package."
sidebar: xyt_sidebar
permalink: lib_tca_api_use_cases.html
folder: xyt
---

### Supported Metrics

| type   | metric                | description                                           | fields |
|--------|-----------------------|-------------------------------------------------------|--------|
| quotes | [BookSnapshot](#BookSnapshot)        | best bid/ask data at given moment                     |MID, ASK, BID, SPREAD, ASK_SIZE, BID_SIZE, ASK_VENUE, BID_VENUE, AT_TOUCH |
| quotes | [BookAggregate](#BookAggregate)       | best bid/ask data aggregated across time              |AVG_SPREAD, AVG_BID, AVG_ASK, AVG_ASK_SIZE, AVG_BID_SIZE |
| quotes | [BookDepth](#BookDepth)           | bid/ask data at given moment aggregated across levels |BID_WAVG, ASK_WAVG, BID_SIZE_SUM, ASK_SIZE_SUM |
| trades | [MarketAggregate](#MarketAggregate)     | aggregated trade data across time                     |COUNT, VOLUME, VALUE, VWAP, FIRST, LAST, MIN, MAX, TTS_MEDIAN |
| trades | [PVWAP](#PVWAP)               | VWAP at given participation rate                      | |
| trades | [RPM](#RPM)                 | relative performance measure based on count or volume |TRADES, SHARES, TRADES_ALL, SHARES_ALL |
| trades | [Volatility](#Volatility)          | intraday volatility estimation                        |REALIZED, VARIANCE, PARKINSON, GARMAN_KLASS, ROGERS_SATCHELL |
| trades | [MarketParticipation](#MarketParticipation) | detailed PVWAP metric combining volume                |PVWAP, VOLUME, VOLUME_LOWER_PVWAP, VOLUME_UPPER_PVWAP, VOLUME_LOWER_LIMIT, VOLUME_UPPER_LIMIT |

#### <a name="BookSnapshot">BookSnapshot</a>

Mid price at order arrival time.

```python
BookSnapshot(
    datetime=ARRIVAL_TIME,
    fields=['MID']
)
```

Reversion stats after order last fill time.

```python
BookSnapshot(
    datetime=LAST_FILL_TIME + pd.to_timedelta(1, unit="m"),
    fields=['ASK', 'BID', 'MID']
)
```

EBBO, spread, bid and ask size at trade time.

```python
BookSnapshot(
    datetime=TRADE_TIME,
    fields=['ASK', 'BID', 'SPREAD', 'BID_SIZE', 'ASK_SIZE']
)
```

<br>

#### <a name="BookAggregate">BookAggregate</a>

Average spread in bps during order execution

```python
BookAggregate(
    from_datetime=ARRIVAL_TIME,
    to_datetime=END_TIME,
    fields=['AVG_SPREAD']
)
```
Average spread in bps from first till last last fill

```python
BookAggregate(
    from_datetime=FIRST_FILL_TIME,
    to_datetime=LAST_FILL_TIME,
    fields=['AVG_SPREAD']
)
```

<br>

#### <a name="BookDepth">BookDepth</a>

What price would we get if we want to execute full order size (iceberg trade)

```python
BookDepth(
    datetime=TRADE_TIME,
    fields=['BID_WAVG', 'ASK_WAVG'],
    size_limit=ORDER_SIZE
)
```

How the book size changed before and after the trade

```python
BookDepth(
    datetime=TRADE_TIME - pd.to_timedelta(1, unit='s'),
    fields=['BID_SIZE_SUM', 'ASK_SIZE_SUM'],
    level_limit=5
)
BookDepth(
    datetime=TRADE_TIME + pd.to_timedelta(1, unit='s'),
    fields=['BID_SIZE_SUM', 'ASK_SIZE_SUM'],
    level_limit=5
)
```

<br>

#### <a name="MarketAggregate">MarketAggregate</a>

VWAP and Volume from arrival to last fill time

```python
MarketAggregate(
    fields=['VWAP', 'VOLUME'],
    from_datetime=ARRIVAL_TIME,
    to_datetime=LAST_FILL_TIME
)
```

VWAP and Volume from arrival to last fill time, only on Lit, with given lower price limit

```python
MarketAggregate(
    fields=['VWAP', 'VOLUME'],
    from_datetime=ARRIVAL_TIME,
    to_datetime=LAST_FILL_TIME,
    lower_price_limit=ORDER_LIMIT,
    trade_types=['Lit']    
)
```

Last price in Lit market
```python
MarketAggregate(
    fields=['LAST'],
    from_datetime=OPEN_TIME,
    to_datetime=CLOSE_TIME,
    trade_types=['Lit']    
)
```

Volume weighted last price in Lit market, if potentially multiple trades could have been executed at that time.

```python
MarketAggregate(
    fields=['VWAP'],
    from_datetime=OPEN_TIME,
    to_datetime=CLOSE_TIME,
    trade_types=['Lit'],
    use_last_snapshot=True
)
```

<br>

#### <a name="PVWAP">PVWAP</a>

What would be my price if I traded at participation rate 15%

```python
PVWAP(
    from_datetime=ARRIVAL_TIME,
    trade_size=ORDER_SIZE,
    participation_rate=0.15
)
```


What would be my price if I traded at participation rate 15%, only on Lit, with given lower price limit

```python
PVWAP(
    from_datetime=ARRIVAL_TIME,
    trade_size=ORDER_SIZE,
    participation_rate=0.15,
    lower_price_limit=ORDER_LIMIT,
    trade_types=['Lit']
)
```

<br>

#### <a name="RPM">RPM</a>

Relative performance measure: trade count with prices lower than order price / trade count with prices different than order price

```python
RPM(
    from_datetime=ARRIVAL_TIME,
    to_datetime=LAST_FILL_TIME,
    benchmark_price=ORDER_PRICE,
    fields=['TRADES']
)
```

Relative performance measure, only Lit market, with given lower price limit: traded volume with prices lower than order price / total traded volume

```python
RPM(
    from_datetime=ARRIVAL_TIME,
    to_datetime=LAST_FILL_TIME,
    benchmark_price=ORDER_PRICE,
    fields=['SHARES_INCLUDE_EQUAL'],
    lower_price_limit=ORDER_LIMIT,
    trade_types=['Lit']    
)
```

<br>

#### <a name="Volatility">Volatility</a>

Intraday volatility using 5-minute bins.

```python
Volatility(
    fields=['REALIZED'],
    from_datetime=OPEN_TIME,
    to_datetime=CLOSE_TIME,
    bin_size=300
)
```

Intrade volatility using 3-minute bins, only Lit market.

```python
Volatility(
    fields=['REALIZED'],
    from_datetime=ARRIVAL_TIME,
    to_datetime=LAST_FILL_TIME,
    trade_types=['Lit'],
    bin_size=180
)
```

<br>

#### <a name="MarketParticipation">MarketParticipation</a>

Volume traded lower than POV 15% price (price achieved if one traded with 15% participation rate)

```python
MarketParticipation(
    from_datetime=ARRIVAL_TIME,
    to_datetime=LAST_FILL_TIME,
    trade_size=ORDER_SIZE,
    participation_rate=0.15,
    fields=['PVWAP', 'VOLUME_BELOW_PVWAP']
)
```

<br>

{% include links.html %}

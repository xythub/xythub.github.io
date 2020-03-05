---
title: Accessing data from R
keywords: R, sample, data access, xythub, xyt hub, xyt-hub, big xyt, big-xyt, documentation, API documentation, API, single API, tick data
summary: "This is the API documentation for xyt hub R package."
sidebar: xyt_sidebar
permalink: lib_r.html
folder: xyt
---

Installation
------------
In order to use xyt hub R Client please install `xythub` package:

```
R CMD INSTALL xythub_X.Y.Z.tar.gz
```

### Requirements

This library requires R 3.4 to run.

Library initialization
----------------------

To configure authentication settings invoke `configure` function:

```
library("xythub")
xythub.configure(USERNAME, password = PASSWORD)
```


Data representation
-------------------

Retrieved data is represented as S3 class instances, with following attributes:

| Attribute     | Description                                                  |
|---------------|--------------------------------------------------------------|
| data          | Data frame representation of the result.                     |
| message       | Deserialized protobuf message.                               |


Browsing data catalogue
-----------------------

### Data sources lookup

Listing of available sources:

```
sources <- xythub.lookupSources()
```

#### Output columns

| Column        | Type           | Description                     |
|---------------|----------------|---------------------------------|
| source        | character      | Source identifier.              |


### Exchanges lookup

Retrieving overview of exchanges on given data source.

Listing all available exchanges:

```
allExchanges <- xythub.lookupExchanges(source = "ACTIV")
```


#### Input parameters

| Parameter       | Type                | Required  | Description                                     |
|-----------------|---------------------|-----------|-------------------------------------------------|
| source          | character           | x         | Data source.                                    |
| exchangeFilters | character           |           | List of exchanges.                              |
| firstDay        | POSIXlt / character |           | First day that should be taken into account.    |
| lastDay         | POSIXlt / character |           | Last day that should be taken into account.     |


#### Output columns

| Column        | Type       | Description                                                          |
|---------------|------------|----------------------------------------------------------------------|
| source        | character  | Exchange identifier.                                                 |
| region        | character  | Region identifier.                                                   |
| exchange      | character  | Exchange identifier.                                                 |
| name          | character  | Full exchange name.                                                  |
| abbreviation	| character  | Exchange abbreviation.                                               |
| first_day     | POSIXlt    | First available day.                                                 |
| last_day      | POSIXlt    | Last available day.                                                  |
| feeds         | character  | List of included feeds.                                              |



### Symbols lookup

Looking up symbols on given data source by pattern:

```
symbolsByKeyword <- xythub.lookupSymbols(source = "ACTIV", pattern = "apple")
```

Looking up symbols on given data source by pattern and dates range:

```
symbolsByKeyword <- xythub.lookupSymbols(source = "ACTIV", pattern = "apple",
                                         firstDay = "2016-09-01", lastDay = "2016-09-02")
```

Looking symbols on given data source:

```
allSymbols <- xythub.lookupSymbols(source = "ACTIV")
```


#### Advanced searching

More advanced search limiting to specific exchanges on the data source and specific item type:

```
foundSymbols <- xythub.lookupSymbols(source = "ACTIV", pattern = "zal", exchangeFilters = c("XETRA", "BXTR"),
                                     firstDay = "2016-09-01", lastDay = "2016-09-02")
```

#### Input parameters

| Parameter            | Type                    | Required  | Description                                      |
|----------------------|-------------------------|-----------|--------------------------------------------------|
| source               | character               | x         | Data source.                                     |
| pattern              | character               |           | Lookup pattern.                                  |
| exchangeFilters      | character               |           | Narrow lookup to one or more exchange names.     |
| regionFilters        | character               |           | Narrow lookup to one or more region names.       |
| entityFilters        | character               |           | Narrow lookup to one or more entities.           |
| firstDay             | POSIXlt / character     |           | First day that should be taken into account.     |
| lastDay              | POSIXlt / character     |           | Last day that should be taken into account.      |
| pageLimit            | integer                 |           | Size of the paging window.                       |
| cursor               | character               |           | Paging cursor returned by the previous call.     |

#### Output columns

| Column           | Type       | Description                |
|------------------|------------|----------------------------|
| source           | character  | Source identifier.         |
| region           | character  | Region identifier.         |
| exchange         | character  | Exchange identifier.       |
| symbol           | character  | Symbol.                    |
| name             | character  | Full item name.            |
| isin             | character  | ISIN.                      |
| local_code       | character  | Exchange local code.       |
| currency         | character  | Item currency.             |
| market_segment   | character  | Market segment.            |
| entity_class     | character  | Entity class.              |
| item_type        | character  | Item type.                 |
| first_day        | POSIXlt    | First available day.       |
| last_day         | POSIXlt    | Last available day.        |


#### Pagination of results

By default, lookup returns up to 100 symbols matching the search criteria.
Using paging user can view more results matching criteria.

Size of the paging window is controlled by `pageLimit` parameter.
Service might throw an error if too large page size is requested.

In each response, service returns a `cursor` token, which denotes identifier
of the last item returned in lookup. Passing a returned `cursor` as
a parameter to subsequent searches, can be used to achieve paging over
entire search result.

For best performance: narrow search criteria using pattern, filters and dates.


Retrieving price data
---------------------

### Trades and/or quotes

Retrieves tick data (trades, quote, trades and quotes) for given symbol and filtering rules.

Retrieving all trades (including non-regular) with trade corrections applied:

```
tradesData <- xythub.getTickData(source = "ACTIV", symbol = "ZAL.XE", day = "2016-09-02",
                                 dataType = xythub.TICK_DATA_TYPE()$TRADES,
                                 flags = c(xythub.TICK_DATA_FLAGS()$INCLUDE_NON_REGULAR,
                                           xythub.TICK_DATA_FLAGS()$INCLUDE_TRADE_CONDITION_INFO))
```


Retrieving quotes:

```
quotesData <- xythub.getTickData(source = "ACTIV", symbol = "ZAL.XE", day = "2016-09-02",
                                 dataType = xythub.TICK_DATA_TYPE()$QUOTES)
```

Retrieving trades and quotes in one call:

```
tickData <- xythub.getTickData(source = "ACTIV", symbol = "ZAL.XE", day = "2016-09-02",
                               dataType = xythub.TICK_DATA_TYPE()$TRADES_AND_QUOTES)
```

#### Input parameters

| Parameter     | Type                   | Required  | Description                                                     |
|---------------|------------------------|-----------|-----------------------------------------------------------------|
| source        | character              | x         | Data source.                                                    |
| dataType      | xythub.TICK_DATA_TYPE  | x         | Specifies if output should contain trades, quotes or both.      |
| symbol        | character              | x         | Symbol.                                                         |
| day           | POSIXlt / character    | x         | Requested day.                                                  |
| flags         | xythub.TICK_DATA_FLAGS |           | List of flags.                                                  |

Available `xythub.TICK_DATA_TYPE()` values:

| Value                        | Description        |  
|------------------------------|--------------------|                                                                                                              
| TRADES                       | Trades only.       |
| QUOTES                       | Quotes only.       |
| TRADES_AND_QUOTES            | Trades and quotes. |

Available `xythub.TICK_DATA_FLAGS()`:

| Value                           | Description                                                                                                                                                                                                                                                          |
|---------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|                                                                                                                                                    |
| USE_MARKET_TS                   | Specifies if exchange timestamp is returned in the output. If flag USE_MARKET_TS is set, result contains exchange timestamp. Otherwise capture timestamp is returned.                                                                                                |
| APPLY_TRADE_CORRECTIONS         | Specifies if trade corrections should be applied to trades. If flag APPLY_TRADE_CORRECTIONS is set, prices and sizes of corrected trades are adjusted accordingly. Usually corrections apply to non-regular trades and are available for limited scope of exchanges. |
| INCLUDE_TRADE_CONDITION_INFO    | Specifies if trade conditions information is returned in the output. It contains mapping of trade conditions to human-readable descriptions, trade categories and markers of continuous session assignment. This flag is valid for TRADES and TRADES_AND_QUOTES.     |
| INCLUDE_NON_REGULAR             | Specifies if non-regular trades should be included in the output.                                                                                                                                                                                                    |
| INCLUDE_CONTINUOUS_SESSION_ONLY | Specifies if only continuous session messages should be included in the result set. This covers filtering by trade conditions assigned to continuous session for trades and instrument status filtering for quotes.                                                  |
| EXCLUDE_CANCELLED_TRADES 	      | Specifies if canceled trades are removed from the output. Functionality removes only canceled trades that are no corrections.                                                                                                                                        |

#### Output columns

| Column           | Type       | TRADES request      | QUOTES request      | TRADES_AND_QUOTES request                                  |
|------------------|------------|--------------------------------------------------------------------------------------------------------|
| symbol           | character  | Symbol.             | Symbol.             | Symbol.                                                    |
| time             | POSIXlt    | Timestamp of trade. | Timestamp of quote. | Timestamp of trade or quote.                               |
| trade            | numeric    | Trade price.        | -                   | Price of trade, empty in case of quote-only row.           |
| trade_size       | integer    | Trade size.         | -                   | Size of trade, empty in case of quote-only row.            |
| trade_condition  | character  | Trade conditions.   | -                   | Condition of trade, empty in case of quote-only row.       |
| bid              | numeric    | -                   | Bid price.          | Price of most recent bid as of trade time.                 |
| bid_size         | integer    | -                   | Bid size.           | Size of most recent bid as of trade time.                  |
| ask              | numeric    | -                   | Ask price.          | Price of most recent ask as of trade time.                 |
| ask_size         | integer    | -                   | Ask size.           | Size of most recent ask as of trade time.                  |

### Trades and/or quotes aggregated

Retrieves aggregated tick data (trades, quote, trades and quotes) for given symbol and filtering rules.
The result contains one row for each time-bin in given time range. Time-bins size is determined via `binSizeInSeconds` parameter.
In case there was no ticks in the market, the result still contains given bin with empty values.

Retrieving hourly aggregated trades:

```
trades <- xythub.getTickAggregated(source = "ACTIV", symbol = "CL/17U.NXG", day = "2017-08-16",
                                   binSizeInSeconds = 3600, # hourly
                                   dataType = xythub.TICK_DATA_TYPE()$TRADES,
                                   flags = c(xythub.TICK_DATA_FLAGS()$INCLUDE_NON_REGULAR,
                                             xythub.TICK_DATA_FLAGS()$USE_MARKET_TS))
```

Retrieving quotes aggregated in 15 minute intervals:

```
quotes <- xythub.getTickAggregated(source = "ACTIV", symbol = "CL/17U.NXG", day = "2017-08-16",
                                   binSizeInSeconds = 900, # 15 minutes
                                   dataType = xythub.TICK_DATA_TYPE()$QUOTES)
```

Retrieving trades and quotes in one minute bins:

```
trades_and_quotes <- xythub.getTickAggregated(source = "ACTIV", symbol = "CL/17U.NXG", day = "2017-08-16"
                                              dataType = xythub.TICK_DATA_TYPE()$TRADES_AND_QUOTES)
```


#### Input parameters

| Parameter        | Type                   | Required  | Description                                                  |
|------------------|------------------------|-----------|--------------------------------------------------------------|
| source           | character              | x         | Data source.                                                 |
| dataType         | xythub.TICK_DATA_TYPE  | x         | Specifies if output should contain trades, quotes or both.   |
| symbol           | character              | x         | List of symbols.                                             |
| day              | POSIXlt / character    | x         | Requested day.                                               |
| binSizeInSeconds | integer                |           | Size of the time bins expressed in seconds.                  |
| flags            | xythub.TICK_DATA_FLAGS |           | List of flags.                                               |

Available Type and Flag values - as described [here](lib_r.html#input-parameters-2).

#### Output columns

| Column           | Type       | TRADES request                     | QUOTES request                       | TRADES_AND_QUOTES request                 |
|------------------|------------|------------------------------------|--------------------------------------|-------------------------------------------|
| symbol           | character  | Symbol.                            | Symbol.                              | Symbol.                                   |
| time             | POSIXlt    | Bin closing time.                  | Bin closing time.                    | Bin closing time.                         |
| open             | numeric    | Open price for given bin.          | -                                    | Open price for given bin.                 |
| high             | numeric    | High price for given bin.          | -                                    | High price for given bin.                 |
| low              | numeric    | Low price for given bin.           | -                                    | Low price for given bin.                  |
| close            | numeric    | Close price for given bin.         | -                                    | Close price for given bin.                |
| trade_count      | integer    | Trade count for given bin.         | -                                    | Trade count for given bin.                |
| volume           | integer    | Total trades volume for given bin. | -                                    | Total trades volume for given bin.        |
| trade_condition  | character  | Trade conditions for last trade.   | -                                    | Trade conditions for last trade.          |
| bid              | numeric    | -                                  | Price of the last bid as of bin end. | Price of the last bid as of bin end.      |
| bid_size         | integer    | -                                  | Size of the last bid as of bin end.  | Size of the last bid as of bin end.       |
| ask              | numeric    | -                                  | Price of the last ask as of bin end. | Price of the last ask as of bin end.      |
| ask_size         | integer    | -                                  | Size of the last ask as of bin end.  | Size of the last ask as of bin end.       |


### Trades and/or quotes snapshot

Retrieves snapshot of tick data (trades, quote, trades and quotes) for given symbols and filtering rules.

Retrieving trade snapshot:

```
trades <- xythub.getTickSnapshot(source = "ACTIV", symbols = "CL/17U.NXG", day = "2017-08-16", time = "12:00:00",
                                 dataType = xythub.TICK_DATA_TYPE()$TRADES,
                                 flags = c(xythub.TICK_DATA_FLAGS()$INCLUDE_NON_REGULAR,
                                           xythub.TICK_DATA_FLAGS()$USE_MARKET_TS))
```

Retrieving quote snapshot:

```
quotes <- xythub.getTickSnapshot(source = "ACTIV", symbols = "CL/17U.NXG", day = "2017-08-16", time = "09:15:00",
                                 dataType = xythub.TICK_DATA_TYPE()$QUOTES)
```

Retrieving trade and quote snapshot:


```
trades_and_quotes <- xythub.getTickSnapshot(source = "ACTIV", symbols = "CL/17U.NXG", day = "2017-08-16",
                                            time = "10:03:00",
                                            dataType = xythub.TICK_DATA_TYPE()$TRADES_AND_QUOTES,
                                            flags = c(xythub.TICK_DATA_FLAGS()$INCLUDE_NON_REGULAR,
                                                      xythub.TICK_DATA_FLAGS()$USE_MARKET_TS))
```

#### Input parameters

| Parameter           | Type                 | Required  | Description                                                     |
|---------------------|----------------------|-----------|-----------------------------------------------------------------|
| source              | character            | x         | Data source.                                                    |
| dataType            | character            | x         | Specifies if output should contain trades, quotes or both.      |
| symbols             | character            | x         | List of symbols.                                                |
| day                 | POSIXlt / character  | x         | Requested day.                                                  |
| time                | difftime / character | x         | Timestamp of the requested snapshot.                            |
| flags               | character            |           | List of flags.                                                  |

Available Type and Flag values - as described [here](lib_r.html#input-parameters-2).

#### Output columns

| Column           | Type       | TRADES request                                              | QUOTES request                                 | TRADES_AND_QUOTES request                                   |
|------------------|------------|-------------------------------------------------------------|------------------------------------------------|-------------------------------------------------------------|
| symbol           | character  | Symbol.                                                     | Symbol.                                        | Symbol.                                                     |
| time             | POSIXlt    | Timestamp.                                                  | Timestamp.                                     | Timestamp.                                                  |
| trade            | numeric    | Price of most recent trade as of requested time.            | -                                              | Price of most recent trade as of requested time.            |
| trade_size       | integer    | Size of most recent trade as of requested time.             | -                                              | Size of most recent trade as of requested time.             |
| trade_condition  | character  | Trade conditions of most recent trade as of requested time. | -                                              | Trade conditions of most recent trade as of requested time. |
| bid              | numeric    | -                                                           | Price of most recent bid as of requested time. | Price of most recent bid as of requested time.              |
| bid_size         | integer    | -                                                           | Size of most recent bid as of requested time.  | Size of most recent bid as of requested time.               |
| ask              | numeric    | -                                                           | Price of most recent ask as of requested time. | Price of most recent ask as of requested time.              |
| ask_size         | integer    | -                                                           | Size of most recent ask as of requested time.  | Size of most recent ask as of requested time.               |


### Orders

Retrieves orders (Level 3 data) for given symbols and filtering rules:

```
orderData <- xythub.getOrderData(source = "ACTIV", symbols = "DBK.XE", day = "2016-10-04")
```

#### Input parameters

| Parameter     | Type                | Required  | Description                                  |
|---------------|---------------------|-----------|----------------------------------------------|
| source        | character           | x         | Data source.                                 |
| symbols       | character           | x         | List of symbols.                             |
| day           | POSIXlt / character | x         | Requested day.                               |

#### Output columns

| Column           | Type             | Description                                              |
|------------------|------------------|----------------------------------------------------------|
| symbol           | character        | Symbol.                                                  |
| time             | POSIXlt          | Timestamp.                                               |
| action           | character        | Orderbook entry update action.                           |
| order_id         | character        | Order ID.                                                |
| order_side       | character        | Order side.                                              |
| order_price      | numeric          | Order price.                                             |
| order_size       | integer          | Order size.                                              |
| trade_id         | character        | Trade ID.                                                |
| order_type       | character        | Order type.                                              |


### Auction data

Retrieves auction for given symbols and filtering rules:

```
auctionData <- xythub.getAuctionData(source = "ACTIV", symbols = "DBK.XE", day = "2016-10-04")
```

#### Input parameters

| Parameter     | Type                | Required  | Description                                  |
|---------------|---------------------|-----------|----------------------------------------------|
| source        | character           | x         | Data source.                                 |
| symbols       | character           | x         | List of symbols.                             |
| day           | POSIXlt / character | x         | Requested day.                               |

#### Output columns

| Column                | Type      | Description                                                |
|-----------------------|-----------|------------------------------------------------------------|
| time                  | POSIXlt   | Time.                                                      |
| imbalance_buy_volume  | integer   | Imbalance buy volume.                                      |
| imbalance_sell_volume | integer   | Imbalance sell volume.                                     |
| imbalance_volume_time | POSIXlt   | Imbalance volume time.                                     |
| cross_type            | character | Cross type.                                                |
| uncrossing_time       | POSIXlt   | Uncrossing timestamp.                                      |
| uncrossing_condition  | character | Uncrossing condition.                                      |
| uncrossing_price      | numeric   | Uncrossing price.                                          |
| uncrossing_volume     | integer   | Uncrossing volume.                                         |
| uncrossing_type       | character | Uncrossing type.                                           |
| auction_time          | POSIXlt   | Auction time.                                              |
| auction_status        | integer   | Auction status.                                            |
| auction               | numeric   | Auction.                                                   |
| auction_volume        | integer   | Auction volume.                                            |
| auction_type          | character | Auction type.                                              |

### End of day

Retrieving end of day data:

```
eodData <- xythub.getEndOfDayData(source = "ACTIV", symbols = "DBK.XE",
                                  firstDay = "2016-10-04", lastDay = "2017-10-04")
```

#### Input parameters

| Parameter     | Type                | Required  | Description                                  |
|---------------|---------------------|-----------|-------------------------------------- -------|
| source        | character           | x         | Data source.                                 |
| symbols       | character           | x         | List of symbols.                             |
| firstDay      | POSIXlt / character | x         | First requested day.                         |
| lastDay       | POSIXlt / character | x         | Last requested day.                          |

#### Output columns

| Column           | Type       | Description                   |
|------------------|------------|-------------------------------|
| symbol           | character  | Symbol.                       |
| POSIXlt          | POSIXlt    | Date.                         |
| open             | numeric    | Official open price.          |
| high             | numeric    | Official high price.          |
| low              | numeric    | Official low price.           |
| close            | numeric    | Official close price.         |
| trade_count      | integer    | Total trade count.            |
| volume           | integer    | Total trades volume.          |

### Settlement prices

Retrieving settlement prices for given list of derivative symbols:

```
settlementData <- xythub.getSettlementPrices(source = "ACTIV", symbols = "DBK.XE",
                                             firstDay = "2016-10-04", lastDay = "2016-11-04")
```

#### Input parameters

| Parameter     | Type                | Required  | Description                                             |
|---------------|---------------------|-----------|---------------------------------------------------------|
| source        | character           | x         | Data source.                                            |
| symbols       | character           | x         | List of symbols.                                        |
| firstDay      | POSIXlt / character | x         | First requested day.                         |
| lastDay       | POSIXlt / character | x         | Last requested day.                          |


#### Output columns

| Column                     | Type      | Description                                                      |
|----------------------------|-----------|------------------------------------------------------------------|
| symbol                     | character | Symbol.                                                          |
| settlement_time            | POSIXlt   | Settlement timestamp.                                            |
| settlement                 | numeric   | Settlement.                                                      |
| settlement_status          | integer   | Settlement status.                                               |
| settlement_size            | integer   | Settlement size.                                                 |
| capture_time               | POSIXlt   | Capture timestamp (time of the update recording - UTC timezone). |


Retrieving reference data
-------------------------

### Reference data

Retrieves reference data for given symbols and date.
Note searching by pattern and retrieval of basing reference data is possible via lookupSymbols method.

```
referenceData <- xythub.getReferenceData(source = "ACTIV", symbols = "ZALd.BTE", day = "2016-09-01")
```

#### Input parameters

| Parameter     | Type                | Required  | Description                                |
|---------------|---------------------|-----------|--------------------------------------------|
| source        | character           | x         | Data source.                               |
| symbols       | character           | x         | List of symbols.                           |
| day           | POSIXlt / character | x         | Requested day.                             |

#### Output columns

| Column                    | Type      | Description                                          |
|---------------------------|-----------|------------------------------------------------------|
| symbol                    | character | Symbol.                                              |
| local_code                | character | Local code.                                          |
| isin                      | character | Isin.                                                |
| name                      | character | Name.                                                |
| country_code              | character | Country code.                                        |
| currency                  | character | Currency.                                            |
| entity_type               | character | Entity type.                                         |


Additional columns available for equities:

| Column                    | Type      | Description                                          |
|---------------------------|-----------|------------------------------------------------------|
| lot_size                  | integer   | Lot size.                                            |
| market_sector             | character | Market sector.                                       |
| market_segment            | character | Market segment.                                      |
| security_primary_exchange | character | Security primary exchange.                           |
| sedol_code                | character | Sedol code.                                          |

Additional columns available for futures:

| Column                               | Type      | Description                               |
|--------------------------------------|-----------|-------------------------------------------|
| expiration_date                      | POSIXlt   | Expiration date.                          |   
| contract_size                        | integer   | Contract size.                            |
| currency_multiplier                  | numeric   | Currency multiplier.                      |
| minimum_tick                         | numeric   | Minimum tick.                             |
| point_size                           | numeric   | point size.                               |
| underlying_isin                      | character | Underlying isin.                          |
| underlying_symbol                    | character | Underlying symbol.                        |
| underlying_currency                  | character | Underlying currency.                      |
| underlying_mic                       | character | Underlying mic.                           |

Additional columns available for future options:

| Column                               | Type      | Description                               |
|--------------------------------------|-----------|-------------------------------------------|
| expiration_date                      | POSIXlt   | Expiration date.                          |   
| contract_size                        | integer   | Contract size.                            |
| currency_multiplier                  | numeric   | Currency multiplier.                      |
| minimum_tick                         | numeric   | Minimum tick.                             |
| option_type                          | character | Option type.                              |
| strike_price                         | numeric   | Strike price.                             |
| exercise_style                       | character | Exercise style.                           |
| strike_price_multiplier              | numeric   | Strike price multiplier.                  |
| underlying_isin                      | character | Underlying isin.                          |
| future_symbol                        | character | Future symbol.                            |
| future_expiration_date               | POSIXlt   | Future expiration_date.                   |

Additional columns available for options:

| Column                               | Type      | Description                               |
|--------------------------------------|-----------|-------------------------------------------|
| expiration_date                      | character | Expiration date.                          |
| contract_size                        | integer   | Contract size.                            |
| currency_multiplier                  | numeric   | Option root currency multiplier.          |
| minimum_tick                         | numeric   | Option root minimum tick.                 |
| option_type                          | character | Option type.                              |
| strike_price                         | numeric   | Strike price.                             |
| exercise_style                       | character | Exercise style.                           |
| expiration_type                      | character | Expiration type.                          |
| contract_multiplier                  | numeric   | Contract multiplier.                      |
| generation_number                    | character | Generation number.                        |
| strike_price_denominator             | numeric   | Strike price denominator.                 |
| shares_per_contract                  | integer   | Shares per contract.                      |
| underlying_isin                      | character | Underlying isin.                          |
| underlying_symbol                    | character | Underlying symbol.                        |
| underlying_currency                  | character | Underlying currency.                      |
| underlying_mic                       | character | Underlying mic.                           |


### Instrument status

Retrieving instrument status:

```
statuses <- xythub.getInstrumentStatus(source = "ACTIV", symbols = "DBK.XE", day = "2016-09-01")
```

#### Input parameters

| Parameter     | Type                | Required  | Description                                |
|---------------|---------------------|-----------|--------------------------------------------|
| source        | character           | x         | Data source.                               |
| symbols       | character           | x         | List of symbols.                           |
| day           | POSIXlt / character | x         | Requested day.                             |

#### Output columns

| Column                         | Type       | Description                                    |
|--------------------------------|------------|------------------------------------------------|
| symbol                         | character  | Symbol.                                        |
| time                           | POSIXlt    | Timestamp.                                     |
| instrument_status              | character  | Instrument status.                             |
| instrument_status_description  | character  | Instrument status description.                 |
| continuous_session             | logical    | Is continuous session.                         |

### Tick rules

Retrieving tick rules:

```
tickRules <- xythub.getTickRules(source = "ACTIV", symbols = "DBK.XE", day = "2016-09-20")
```

#### Input parameters

| Parameter     | Type                | Required  | Description                                |
|---------------|---------------------|-----------|--------------------------------------------|
| source        | character           | x         | Data source.                               |
| symbols       | character           | x         | List of symbols.                           |
| day           | POSIXlt / character | x         | Requested day.                             |

#### Output columns

| Column                         | Type      | Description                                     |
|--------------------------------|-----------|-------------------------------------------------|
| symbol                         | character | Symbol.                                         |
| tick_rule_symbol               | character | Tick rule symbol.                               |
| lower_bound                    | numeric   | Lower bound.                                    |
| tick_size                      | numeric   | Tick size.                                      |

### Option and future chains

Retrieve option or future chain for given chain identifier (either option stem or underlying) and given exchange and day:

```
result <- xythub.getChain(source = "ACTIV", symbol = "CL", exchange = "OPRA_COMPOSITE",
                          chainType = xythub.CHAIN_TYPE()$OPTIONS, day = "2017-08-15")
```

#### Input parameters

| Parameter   | Type                | Required  | Description                                                             |
|-------------|---------------------|-----------|-------------------------------------------------------------------------|
| source      | character           | x         | Data source.                                                            |
| chainType   | xythub.CHAIN_TYPE   | x         | Specifies if output should contain options, futures or future options.  |
| symbol      | character           | x         | Stem or underlying symbol.                                              |
| exchange    | character           | x         | Exchange identifier.                                                    |
| day         | POSIXlt / character | x         | Requested day.                                                          |

#### Output columns

| Column           | Type       | Description                |
|------------------|------------|----------------------------|
| chain_symbol     | character  | Chain identifier.          |
| symbol           | character  | Chain member symbol.       |
| underlying       | character  | Underlying identifier.     |
| name             | character  | Chain name.                |

Available `xythub.CHAIN_TYPE()` values:

| Type                | Description        |  
|---------------------|--------------------|                                                   
| OPTIONS             | Options            |
| FUTURES             | Futures            |
| FUTURE_OPTIONS      | Future Options     |

Other data
----------

### Generic data request

Apart from the data requests described above, it is possible to query for other data sets via generic data request.
The input parameters, query logic and output content is specified by the given `request`.

#### Input parameters

| Parameter     | Type                   | Required  | Description                                                     |
|---------------|------------------------|-----------|-----------------------------------------------------------------|
| source        | character              | x         | Data source.                                                    |
| request       | character              | x         | Custom-defined request name.                                    |
| parameters    | c(xythub.toParameter)  | x         | Vector with input parameters required for given request.        |

#### Output columns

Output columns are dependent on the selected request.

Troubleshooting
---------------

### Proxies

HTTP proxy can be set while configuring library settings:

```
xythub.configure("your@username.com", "password", proxy = httr::use_proxy("proxy.mydomain.com", 8080))
```


License
-------

[Apache License Version 2.0](http://opensource.org/licenses/Apache-2.0)

{% include links.html %}

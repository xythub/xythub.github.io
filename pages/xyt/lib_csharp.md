---
title: Accessing data from C#
keywords: csharp, sample, data access, xythub, xyt hub, xyt-hub, big xyt, big-xyt, documentation, API documentation, API, single API, tick data
summary: "This is the API documentation for xyt hub C Sharp package."
sidebar: xyt_sidebar
permalink: lib_csharp.html
folder: xyt
---

Installation
------------
In order to use xyt hub C# Client add a reference in C#:
1. In `Solution Explorer`, right-click the project node and click `Add Reference`.
2. In the `Add Reference` dialog box, select the `Browse` tab and locate `xythub-csharp.dll` and then click `OK`.

### Requirements

This library requires .NET Framework 4.5 to run.

Creating client object
-----------------------

The API client support authentication via password:

```
XytHubClient client = new XytHubClient(USERNAME, password: PASSWORD);
```

or via API key:

```
XytHubClient client = new XytHubClient(USERNAME, apiKey: API_KEY_ID);
```


Browsing data catalogue
-----------------------

### Data sources lookup

Listing of available sources:

```
var sources = client.LookupSources();
```

#### Output columns

| Column        | Type           | Description                     |
|---------------|----------------|---------------------------------|
| source        | String         | Source identifier.              |


### Exchanges lookup

Listing all available exchanges:

```
var exchanges = client.LookupExchanges("ACTIV");
```


#### Input parameters

| Parameter       | Type           | Required  | Description                                       |
|-----------------|----------------|-----------|---------------------------------------------------|
| source          | String         | x         | Data source.                                      |
| exchangeFilters | List\<String\> |           | Pattern.                                          |
| firstDay        | DateTime?      |           | First day that should be taken into account.      |
| lastDay         | DateTime?      |           | Last day that should be taken into account.       |


#### Output columns

| Column        | Type       | Description                        |
|---------------|------------|------------------------------------|
| source        | String     | Source identifier.                 |
| region        | String     | Region identifier.                 |
| exchange      | String     | Exchange identifier.               |
| name          | String     | Full exchange name.                |
| abbreviation	| String     | Exchange abbreviation.             |
| first_day     | DateTime   | First available day.               |
| last_day      | DateTime   | Last available day.                |
| feeds         | String[]   | List of included feeds.            |


### Symbols lookup

Looking up symbols on given data source by pattern:

```
var symbols = client.LookupSymbols("SOURCE", "apple");
```

Looking up symbols on given data source by pattern and dates range:

```
var symbols = client.LookupSymbols("SOURCE", "apple",
                                   firstDay: new DateTime(2016, 9, 1),
                                   lastDay: new DateTime(2016, 9, 2));
```

Looking symbols on given data source:

```
var symbols = client.LookupSymbols("SOURCE")
```


#### Advanced searching

More advanced search limiting to specific exchanges on the data source and specific item type:

```
String pattern = "zal";
List<string> exchanges = new List<string>() { "XETRA", "BXTR" };

var symbols = client.LookupSymbols("SOURCE", pattern,
                                   firstDay: new DateTime(2016, 9, 1), lastDay: new DateTime(2016, 9, 2),
                                   exchangeFilters: exchanges);
```


#### Input parameters

| Parameter            | Type                | Required  | Description                                      |
|----------------------|---------------------|-----------|--------------------------------------------------|
| source               | String              | x         | Data source.                                     |
| pattern              | String              |           | Lookup pattern.                                  |
| exchangeFilters      | List\<String\>      |           | Narrow lookup to one or more exchange names.     |
| regionFilters        | List\<String\>      |           | Narrow lookup to one or more region names.       |
| entityFilters        | List\<String\>      |           | Narrow lookup to one or more entities.           |
| firstDay             | DateTime?           |           | First day that should be taken into account.     |
| lastDay              | DateTime?           |           | Last day that should be taken into account.      |
| pageLimit            | int?                |           | Size of the paging window.                       |
| cursor               | String              |           | Paging cursor returned by the previous call.     |

#### Output columns

| Column           | Type       | Description                |
|------------------|------------|----------------------------|
| source           | String     | Source identifier.         |
| region           | String     | Region identifier.         |
| exchange         | String     | Exchange identifier.       |
| symbol           | String     | Symbol.                    |
| name             | String     | Full item name.            |
| isin             | String     | ISIN.                      |
| local_code       | String     | Exchange local code.       |
| currency         | String     | Item currency.             |
| market_segment   | String     | Market segment.            |
| entity_class     | String     | Entity class.              |
| first_day        | DateTime   | First available day.       |
| last_day         | DateTime   | Last available day.        |


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

Retrieving all items matching the pattern 'post' and iterating over search
result using paging window of 100 elements:

```
bool continueLookup;
String cursor = null;
long rows = 0;
do
{
    LookupSymbols lookup = client.LookupSymbols("ACTIV", "post", pageLimit: 100, cursor: cursor);
    cursor = lookup.Cursor;
    rows += lookup.RowCount;
    continueLookup = !String.IsNullOrEmpty(cursor);

    Console.WriteLine("Rows: {0}/{1} Cursor: {2}", rows, lookup.TotalHits, cursor);
} while (continueLookup);
```


Retrieving price data
---------------------

### Trades and/or quotes

Retrieves tick data (trades, quote, trades and quotes) for given symbol and filtering rules.

Retrieving all trades (including non-regular) with trade corrections applied:

```
var flags = new List<TickDataFlag>() { TickDataFlag.IncludeNonRegular,
                                       TickDataFlag.IncludeTradeConditionInfo };

var trades = client.TickData("SOURCE", "ZAL.XE",
                             day: new DateTime(2016, 9, 2),
                             dataType: TickDataType.Trades,
                             fromTime: new TimeSpan(9, 0, 0), toTime: new TimeSpan(9, 2, 0), flags: flags);
```

Retrieving quotes:

```
var quotes = client.TickData("SOURCE", "ZAL.XE", new DateTime(2016, 9, 2),
                             day: new DateTime(2016, 9, 2),
                             dataType: TickDataType.Quotes,
                             fromTime: new TimeSpan(9, 0, 0), toTime: new TimeSpan(9, 2, 0));
```

Retrieving trades and quotes in one call:

```
var flags = new List<TickDataFlag>() { TickDataFlag.IncludeNonRegular,
                                       TickDataFlag.IncludeTradeConditionInfo };

var tradesAndQuotes = client.TickData("SOURCE", "ZAL.XE",
                                      day: new DateTime(2016, 9, 2),
                                      dataType: TickDataType.TradesAndQuotes,
                                      fromTime: new TimeSpan(9, 0, 0), toTime: new TimeSpan(9, 2, 0), flags: flags);
```

Retrieving trades and quotes from multiple exchanges. Please note inclusion of `ZALd.BTE` symbol in `symbols` variable which will result in retrieving quotes from two different exchanges (XETRA and BATS in this case). Symbol naming convention is source specific.

#### Input parameters

| Parameter     | Type            | Required  | Description                                                     |
|---------------|-----------------|-----------|-----------------------------------------------------------------|
| source        | String          | x         | Data source.                                                    |
| dataType      | Type            | x         | Specifies if output should contain trades, quotes or both.      |
| symbol        | String          | x         | Symbol.                                                         |
| day           | DateTime        | x         | Requested day.                                                  |
| fromTime      | TimeSpan?       |           | Start of the requested time range.                              |
| toTime        | TimeSpan?       |           | End of the requested time range.                                |
| flags         | List\<Flag\>    |           | List of flags.                                                  |

Available Type values:

| Type              | Description        |  
|-------------------|--------------------|                                                                                                              
| Trades            | Trades only.       |
| Quotes            | Quotes only.       |
| TradesAndQuotes   | Trades and quotes. |

Available Flag values:

| Flag                            | Description                                                                                                                                                                                                                                                          |
|---------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|                                                                                                                                                    |
| UseMarketTs                     | Specifies if exchange timestamp is returned in the output. If flag UseMarketTs is set, result contains exchange timestamp. Otherwise capture timestamp is returned.                                                                                                  |
| ApplyTradeCorrections           | Specifies if trade corrections should be applied to trades. If flag ApplyTradeCorrections is set, prices and sizes of corrected trades are adjusted accordingly. Usually corrections apply to non-regular trades and are available for limited scope of exchanges.   |
| IncludeTradeConditionInfo       | Specifies if trade conditions information is returned in the output. It contains mapping of trade conditions to human-readable descriptions, trade categories and markers of continuous session assignment. This flag is valid for Trades and TradesAndQuotes.       |
| IncludeNonRegular               | Specifies if non-regular trades should be included in the output.                                                                                                                                                                                                    |
| IncludeContinuousSessionOnly    | Specifies if only continuous session messages should be included in the result set. This covers filtering by trade conditions assigned to continuous session for trades and instrument status filtering for quotes.                                                  |
| ExcludeCancelledTrades 	        | Specifies if canceled trades are removed from the output. Functionality removes only canceled trades that are no corrections.                                                                                                                                        |

#### Output columns

| Column           | Type       | TRADES request    | QUOTES request   | TRADES_AND_QUOTES request                                  |
|------------------|------------|---------------------------------------------------------------------------------------------------|
| symbol           | String     | Symbol.           | Symbol.          | Symbol.                                                    |
| date             | DateTime   | Date.             | Date.            | Date.                                                      |
| time             | TimeSpan   | Time of trade.    | Time of quote.   | Time of trade.                                             |
| trade            | double     | Trade price.      | -                | Price of trade.                                            |
| trade_size       | long       | Trade size.       | -                | Size of trade.                                             |
| trade_condition  | String     | Trade conditions. | -                | Condition of trade.                                        |
| ask              | double     | -                 | Ask price.       | Price of most recent ask as of trade time.                 |
| ask_size         | long       | -                 | Ask size.        | Size of most recent ask as of trade time.                  |
| bid              | double     | -                 | Bid price.       | Price of most recent ask as of trade time.                 |
| bid_size         | long       | -                 | Bid size.        | Size of most recent ask as of trade time.                  |


### Trades and/or quotes aggregated

Retrieves aggregated tick data (trades, quote, trades and quotes) for given symbols and filtering rules.
The result contains one row per symbol for each time-bin in given time range. Time-bins size is determined via `binSizeInSeconds` parameter.
In case there was no ticks in the market, the result still contains given bin with empty values.

Retrieving hourly aggregated trades:

```
var flags = new List<TickDataFlag>() { TickDataFlag.IncludeNonRegular,
                                       TickDataFlag.UseMarketTs };

var tickAggregated = client.TickAggregated("SOURCE", "CL/17U.NXG",
                                           day: new DateTime(2017, 8, 16),
                                           binSize: 3600, // hourly
                                           dataType: TickDataType.Trades,
                                           flags: flags);
```

Retrieving quotes aggregated in 15 minute intervals:

```
var flags = new List<TickDataFlag>() {};

var tickAggregated = client.TickAggregated("SOURCE", "CL/17U.NXG",
                                           day: new DateTime(2017, 8, 16),
                                           binSize: 900, // 15 minutes
                                           dataType: TickDataType.Quotes,
                                           flags: flags);
```

Retrieving trades and quotes in one minute bins:

```
var tickAggregated = client.TickAggregated("SOURCE", "CL/17U.NXG",
                                           day: new DateTime(2017, 8, 16),
                                           dataType: TickDataType.TradesAndQuotes);
```


#### Input parameters

| Parameter           | Type            | Required  | Description                                                     |
|---------------------|-----------------|-----------|-----------------------------------------------------------------|
| source              | String          | x         | Data source.                                                    |
| dataType            | Type            | x         | Specifies if output should contain trades, quotes or both.      |
| symbol              | String          | x         | List of symbols.                                                |
| day                 | DateTime        | x         | Requested day.                                                  |
| fromTime            | TimeSpan?       |           | Start of the requested time range.                              |
| toTime              | TimeSpan?       |           | End of the requested time range.                                |
| binSize             | int?            |           | Size of the time bins expressed in seconds.                     |
| flags               | List\<Flag\>    |           | List of flags.                                                  |

Available Type and Flag values - as described [here](lib_csharp.html#input-parameters-2).

#### Output columns

| Column           | Type       | TRADES request                     | QUOTES request                       | TRADES_AND_QUOTES request                 |
|------------------|------------|------------------------------------|--------------------------------------|-------------------------------------------|
| symbol           | String     | Symbol.                            | Symbol.                              | Symbol.                                   |
| date             | DateTime   | Date.                              | Date.                                | Date.                                     |
| time             | TimeSpan   | Bin closing time.                  | Bin closing time.                    | Bin closing time.                         |
| open             | double     | Open price for given bin.          | -                                    | Open price for given bin.                 |
| high             | double     | High price for given bin.          | -                                    | High price for given bin.                 |
| low              | double     | Low price for given bin.           | -                                    | Low price for given bin.                  |
| close            | double     | Close price for given bin.         | -                                    | Close price for given bin.                |
| trade_count      | long       | Trade count for given bin.         | -                                    | Trade count for given bin.                |
| volume           | long       | Total trades volume for given bin. | -                                    | Total trades volume for given bin.        |
| trade_condition  | String     | Trade conditions for last trade.   | -                                    | Trade conditions for last trade.          |
| bid              | double     | -                                  | Price of the last bid as of bin end. | Price of the last bid as of bin end.      |
| bid_size         | long       | -                                  | Size of the last bid as of bin end.  | Size of the last bid as of bin end.       |
| ask              | double     | -                                  | Price of the last ask as of bin end. | Price of the last ask as of bin end.      |
| ask_size         | long       | -                                  | Size of the last ask as of bin end.  | Size of the last ask as of bin end.       |


### Trades and/or quotes snapshot

Retrieves snapshot of tick data (trades, quote, trades and quotes) for given symbols and filtering rules.

Retrieving trade snapshot at 12:00:00.00:

```
var symbols = new List<string>() { "CL/17U.NXG" };
var flags = new List<TickDataFlag>() { TickDataFlag.IncludeNonRegular,
                                       TickDataFlag.UseMarketTs };

var tickSnapshot = client.TickSnapshot("SOURCE", symbols,
                                       day: new DateTime(2017, 8, 16),
                                       dataType: TickDataType.Trades,
                                       time: new TimeSpan(12, 0, 0), flags: flags);
```

Retrieving quote snapshot at 09:15:00:

```
var symbols = new List<string>() { "CL/17U.NXG" };
var flags = new List<TickDataFlag>() { };

var tickSnapshot = client.TickSnapshot("SOURCE", symbols,
                                       day: new DateTime(2017, 8, 16),
                                       dataType: TickDataType.Quotes,
                                       time: new TimeSpan(9, 15, 0), flags: flags);
```

Retrieving trade and quote snapshot at 10:03:00:

```
var symbols = new List<string>() { "CL/17U.NXG" };
var flags = new List<TickDataFlag>() { TickDataFlag.IncludeNonRegular,
                                       TickDataFlag.UseMarketTs };

var tickSnapshot = client.TickSnapshot("SOURCE", symbols,
                                       day: new DateTime(2017, 8, 16),
                                       dataType: TickDataType.TradesAndQuotes,
                                       time: new TimeSpan(10, 3, 0), flags: flags);
```


#### Input parameters

| Parameter           | Type            | Required  | Description                                                     |
|---------------------|-----------------|-----------|-----------------------------------------------------------------|
| source              | String          | x         | Data source.                                                    |
| dataType            | Type            | x         | Specifies if output should contain trades, quotes or both.      |
| symbols             | List\<String\>  | x         | List of symbols.                                                |
| day                 | DateTime        | x         | Requested day.                                                  |
| time                | TimeSpan        | x         | Timestamp of the requested snapshot.                            |
| flags               | List\<Flag\>    |           | List of flags.                                                  |

Available Type and Flag values - as described [here](lib_csharp.html#input-parameters-2).

#### Output columns

| Column           | Type       | TRADES request                                              | QUOTES request                                 | TRADES_AND_QUOTES request                                   |                                                    
|------------------|------------|-------------------------------------------------------------|------------------------------------------------|-------------------------------------------------------------|
| symbol           | String     | Symbol.                                                     | Symbol.                                        | Symbol.                                                     |
| date             | DateTime   | Date.                                                       | Date.                                          | Date.                                                       |
| time             | TimeSpan   | Trade.                                                      | Time.                                          | Time.                                                       |
| trade            | double     | Price of most recent trade as of requested time.            | -                                              | Price of most recent trade as of requested time.            |
| trade_size       | long       | Size of most recent trade as of requested time.             | -                                              | Size of most recent trade as of requested time.             |
| trade_condition  | String     | Trade conditions of most recent trade as of requested time. | -                                              | Trade conditions of most recent trade as of requested time. |
| bid              | double     | -                                                           | Price of most recent bid as of requested time. | Price of most recent bid as of requested time.              |
| bid_size         | long       | -                                                           | Size of most recent bid as of requested time.  | Size of most recent bid as of requested time.               |
| ask              | double     | -                                                           | Price of most recent ask as of requested time. | Price of most recent ask as of requested time.              |
| ask_size         | long       | -                                                           | Size of most recent ask as of requested time.  | Size of most recent ask as of requested time.               |


### Orders

Retrieving orders:

```
var symbols = new List<string>() { "DBK.XE" };

var orders = client.OrderData("SOURCE", symbols,
                              day: new DateTime(2016, 11, 10),
                              fromTime: new TimeSpan(9, 0, 0),
                              toTime: new TimeSpan(9, 1, 30));
```

#### Input parameters

| Parameter     | Type            | Required  | Description                                   |
|---------------|-----------------|-----------|-----------------------------------------------|
| source        | String          | x         | Data source.                                  |
| symbols       | List\<String\>  | x         | List of symbols.                              |
| day           | DateTime        | x         | Requested day.                                |
| fromTime      | TimeSpan?       |           | Start of the requested time range.            |
| toTime        | TimeSpan?       |           | End of the requested time range.              |

#### Output columns

| Column           | Type       | Description                                                 |
|------------------|------------|-------------------------------------------------------------|
| symbol           | String     | Symbol.                                                     |
| order_date       | DateTime   | Date.                                                       |
| time             | TimeSpan   | Time of trade or quote.                                     |
| action           | String     | Orderbook entry update action.                              |
| order_id         | String     | Order ID.                                                   |
| order_side       | String     | Order side.                                                 |
| order_price      | double     | Order price.                                                |
| order_size       | long       | Order size.                                                 |
| order_time       | TimeSpan   | Order time.                                                 |
| trade_id         | String     | Trade ID.                                                   |
| order_type       | TimeSpan   | Order type.                                                 |


### Auction data

Retrieves auction for given symbols and filtering rules:

```
var symbols = new List<string>() { "DBK.XE" };

var orders = client.AuctionData("SOURCE", symbols,
                                day: new DateTime(2016, 11, 10));
```

#### Input parameters

| Parameter     | Type            | Required  | Description                                   |
|---------------|-----------------|-----------|-----------------------------------------------|
| source        | String          | x         | Data source.                                  |
| symbols       | List\<String\>  | x         | List of symbols.                              |
| day           | DateTime        | x         | Requested day.                                |

#### Output columns

| Column                | Type       | Description                                            |
|-----------------------|------------|--------------------------------------------------------|
| time                  | TimeSpan   | Time.                                                  |
| imbalance_buy_volume  | long       | Imbalance buy volume.                                  |
| imbalance_sell_volume | long       | Imbalance sell volume.                                 |
| imbalance_volume_time | TimeSpan   | Imbalance volume time.                                 |
| cross_type            | String     | Cross type.                                            |
| uncrossing_date       | DateTime   | Uncrossing date.                                       |
| uncrossing_time       | TimeSpan   | Uncrossing time.                                       |
| uncrossing_condition  | String     | Uncrossing condition.                                  |
| uncrossing_price      | double     | Uncrossing price.                                      |
| uncrossing_volume     | long       | Uncrossing volume.                                     |
| uncrossing_type       | String     | Uncrossing type.                                       |
| auction_date          | DateTime   | Auction date.                                          |
| auction_time          | TimeSpan   | Auction time.                                          |
| auction_status        | long       | Auction status.                                        |
| auction               | double     | Auction.                                               |
| auction_volume        | long       | Auction volume.                                        |
| auction_type          | String     | Auction type.                                          |


### End of day

Retrieving end of day data:

```
var symbols = new List<string>() { "DBK.XE" };

var eodData = client.EndOfDayData("SOURCE", symbols,
                                  firstDay: new DateTime(2016, 9, 2),
                                  lastDay: new DateTime(2017, 9, 2));
```

#### Input parameters

| Parameter     | Type            | Required  | Description                                   |
|---------------|-----------------|-----------|-----------------------------------------------|
| source        | String          | x         | Data source.                                  |
| symbols       | List\<String\>  | x         | List of symbols.                              |
| firstDay      | DateTime        | x         | First requested day.                          |
| lastDay       | DateTime        | x         | Last requested day.                           |

#### Output columns

| Column           | Type       | Description                   |
|------------------|------------|-------------------------------|
| symbol           | String     | Symbol.                       |
| date             | DateTime   | Date.                         |
| open             | double     | Official open price.          |
| high             | double     | Official high price.          |
| low              | double     | Official low price.           |
| close            | double     | Official close price.         |
| trade_count      | long       | Total trade count.            |
| volume           | long       | Total trades volume.          |


### Settlement prices

Retrieving settlement prices for given list of derivative symbols:

```
var symbols = new List<string>() { "DBK.XE" };

var prices = client.SettlementPrices("SOURCE", symbols,
                                     firstDay: new DateTime(2016, 9, 2),
                                     lastDay: new DateTime(2017, 9, 2));
```

#### Input parameters

| Parameter     | Type            | Required  | Description                                            |
|---------------|-----------------|-----------|--------------------------------------------------------|
| source        | String          | x         | Data source.                                           |
| symbols       | List\<String\>  | x         | List of symbols.                                       |
| firstDay      | DateTime        | x         | First requested day.                                   |
| lastDay       | DateTime        | x         | Last requested day.                                    |


#### Output columns

| Column                     | Type      | Description                                                 |
|----------------------------|-----------|-------------------------------------------------------------|
| symbol                     | String    | Symbol.                                                     |
| settlement_date            | DateTime  | Settlement date.                                            |
| settlement_time            | TimeSpan  | Settlement time.                                            |
| settlement                 | double    | Settlement.                                                 |
| settlement_status          | long      | Settlement status.                                          |
| settlement_size            | long      | Settlement size.                                            |
| capture_date               | DateTime  | Capture date (date of the update recording - UTC timezone). |
| capture_time               | TimeSpan  | Capture time (time of the update recording - UTC timezone). |


Retrieving reference data
-------------------------

### Reference data

Retrieves reference data for given symbols and date.
Note searching by pattern and retrieval of basing reference data is possible via `LookupSymbols` method.

```
var symbols = new List<string>() { "DBK.XE" };

var reference = client.ReferenceData("SOURCE", symbols, day: new DateTime(2016, 9, 1));
```

#### Input parameters

| Parameter      | Type            | Required  | Description                                         |
|----------------|-----------------|-----------|-----------------------------------------------------|
| source         | String          | x         | Data source.                                        |
| symbols        | List\<String\>  | x         | List of symbols.                                    |
| day            | DateTime        | x         | Requested day.                                      |
| instrumentType | String          | x         | Type of the instrument.                             |

#### Output columns

| Column                    | Type   | Description                                                  |
|---------------------------|--------|--------------------------------------------------------------|
| symbol                    | String | Symbol.                                                      |
| local_code                | String | Local code.                                                  |
| isin                      | String | Isin.                                                        |
| name                      | String | Name.                                                        |
| country_code              | String | Country code.                                                |
| currency                  | String | Currency.                                                    |
| entity_type               | String | Entity type.                                                 |


Additional columns available for equities:

| Column                    | Type   | Description                                                  |
|---------------------------|--------|--------------------------------------------------------------|
| lot_size                  | long   | Lot size.                                                    |
| market_sector             | String | Market sector.                                               |
| market_segment            | String | Market segment.                                              |
| security_primary_exchange | String | Security primary exchange.                                   |
| sedol_code                | String | Sedol code.                                                  |

Additional columns available for futures:

| Column                               | Type   | Description                                       |
|--------------------------------------|--------|---------------------------------------------------|
| expiration_date                      | Date   | Expiration date.                                  |   
| contract_size                        | long   | Contract size.                                    |
| currency_multiplier                  | double | Currency multiplier.                              |
| minimum_tick                         | double | Minimum tick.                                     |
| point_size                           | double | point size.                                       |
| underlying_isin                      | String | Underlying isin.                                  |
| underlying_symbol                    | String | Underlying symbol.                                |
| underlying_currency                  | String | Underlying currency.                              |
| underlying_mic                       | String | Underlying mic.                                   |

Additional columns available for future options:

| Column                           | Type       | Description                                       |
|----------------------------------|------------|---------------------------------------------------|
| expiration_date                  | DateTime   | Expiration date.                                  |   
| contract_size                    | long       | Contract size.                                    |
| currency_multiplier              | double     | Currency multiplier.                              |
| minimum_tick                     | double     | Minimum tick.                                     |
| option_type                      | String     | Option type.                                      |
| strike_price                     | double     | Strike price.                                     |
| exercise_style                   | String     | Exercise style.                                   |
| strike_price_multiplier          | double     | Strike price multiplier.                          |
| underlying_isin                  | String     | Underlying isin.                                  |
| future_symbol                    | String     | Future symbol.                                    |
| future_expiration_date           | DateTime   | Future expiration date.                           |

Additional columns available for options:

| Column                           | Type       | Description                                       |
|----------------------------------|------------|---------------------------------------------------|
| expiration_date                  | String     | Expiration date.                                  |
| contract_size                    | long       | Contract size.                                    |
| currency_multiplier              | double     | Option root currency multiplier.                  |
| minimum_tick                     | double     | Option root minimum tick.                         |
| option_type                      | String     | Option type.                                      |
| strike_price                     | double     | Strike price.                                     |
| exercise_style                   | String     | Exercise style.                                   |
| expiration_type                  | String     | Expiration type.                                  |
| contract_multiplier              | double     | Contract multiplier.                              |
| generation_number                | String     | Generation number.                                |
| strike_price_denominator         | double     | Strike price denominator.                         |
| shares_per_contract              | long       | Shares per contract.                              |
| underlying_isin                  | String     | Underlying isin.                                  |
| underlying_symbol                | String     | Underlying symbol.                                |
| underlying_currency              | String     | Underlying currency.                              |
| underlying_mic                   | String     | Underlying mic.                                   |

### Instrument status

Retrieving instrument status:

```
var symbols = new List<string>() { "DBKd.BTE", "DBK.XE" };

var statuses = client.InstrumentStatus("SOURCE", symbols, day: new DateTime(2016, 9, 2));
```

#### Input parameters

| Parameter     | Type            | Required  | Description                                         |
|---------------|-----------------|-----------|-----------------------------------------------------|
| source        | String          | x         | Data source.                                        |
| symbols       | List\<String\>  | x         | List of symbols.                                    |
| day           | DateTime        | x         | Requested day.                                      |

#### Output columns

| Column                         | Type      | Description                                            |
|--------------------------------|-----------|--------------------------------------------------------|
| symbol                         | String    | Symbol.                                                |
| date                           | DateTime  | Local code.                                            |
| time                           | TimeSpan  | Isin.                                                  |
| instrument_status              | String    | Instrument status.                                     |
| instrument_status_description  | String    | Instrument status description.                         |
| continuous_session             | bool      | Is continuous session.                                 |


### Tick rules

Retrieving tick rules:

```
var symbols = new List<string>() { "DBK.XE" };

var rules = client.TickRules("SOURCE", symbols, day: new DateTime(2016, 9, 2));
```

#### Input parameters

| Parameter     | Type            | Required  | Description                                         |
|---------------|-----------------|-----------|-----------------------------------------------------|
| source        | String          | x         | Data source.                                        |
| symbols       | List\<String\>  | x         | List of symbols.                                    |
| day           | DateTime        | x         | Requested day.                                      |

#### Output columns

| Column                         | Type   | Description                                             |
|--------------------------------|--------|---------------------------------------------------------|
| symbol                         | String | Symbol.                                                 |
| tick_rule_symbol               | String | Tick rule symbol.                                       |
| lower_bound                    | double | Lower bound.                                            |
| tick_size                      | double | Tick size.                                              |


### Option and future chains

Retrieves option or future chain for given chain identifier (either option stem or underlying) and given exchange and day.

Retrieving option chain:

```
var chain = client.Chain("SOURCE", "=DJX.W", "OPRA_COMPOSITE",
                         chainType: ChainType.Options, day: new DateTime(2016, 9, 2));
```

Retrieving future chain:

```
var chain = client.Chain("SOURCE", "CL", "NYMEX",
                         chainType: ChainType.Futures, day: new DateTime(2016, 9, 2));
```

Retrieving future option chain:

```
var chain = client.Chain("SOURCE", "CL", "NYMEX",
                         chainType: ChainType.FutureOptions, day: new DateTime(2016, 9, 2));
```


#### Input parameters

| Parameter     | Type          | Required  | Description                                                                |
|---------------|---------------|-----------|----------------------------------------------------------------------------|
| source        | String        | x         | Data source.                                                               |
| chainType     | ChainType     | x         | Specifies if output should contain options, futures or future options.     |
| symbol        | String        | x         | Stem or underlying symbol.                                                 |
| exchange      | String        | x         | Exchange identifier.                                                       |
| day           | DateTime      | x         | Requested day.                                                             |

#### Output columns

| Column           | Type       | Description                |
|------------------|------------|----------------------------|
| chain_symbol     | String     | Chain identifier.          |
| symbol           | String     | Chain member symbol.       |
| underlying       | String     | Underlying identifier.     |
| name             | String     | Chain name.                |

Available `ChainType` values:

| Type                | Description        |  
|---------------------|--------------------|
| Options             | Options.           |
| Futures             | Futures.           |
| FutureOptions       | Future options.    |


Other data
----------

### Generic data request

Apart from the data requests described above, it is possible to query for other data sets via generic data request.
The input parameters, query logic and output content is specified by the given `request`.

#### Input parameters

| Parameter    | Type                         | Required  | Description                                                     |
|--------------|------------------------------|-----------|-----------------------------------------------------------------|
| source       | String                       | x         | Data source.                                                    |
| request      | String                       | x         | Custom-defined request name.                                    |
| parameters   | Dictionary\<String, Object\> | x         | Dictionary with input parameters required for given request.    |

#### Output columns

Output columns are dependent on the selected request.

Troubleshooting
---------------

### Diagnostics

The library provides an utility function which collects diagnostic information about runtime environment and
verifies basic connectivity with the API service:

```
var client = new XytHubClient(USERNAME, PASSWORD, ...);
client.Check();
```

Diagnostic information is printed to the console and saved to a file (`xyt-csharp-VERSION-TIMESTAMP.info`).

### Proxies

HTTP proxy can be set while constructing client object:

```
IWebProxy proxy = new WebProxy("proxy.mydomain.com", 8080);
XytHubClient client = new XytHubClient("your@username.com", "password", proxy: proxy);
```

Development
-----------

### Protobuf generation

To generate C# wrappers from protobuf API declaration, execute:

`protoc --proto_path=api --csharp_out=xythub\api api\xyt\api_data.proto api\xyt\types.proto`


License
-------

[Apache License Version 2.0](http://opensource.org/licenses/Apache-2.0)

{% include links.html %}

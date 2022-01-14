---
title: Accessing data from Python
keywords: python, sample, data access, ETF LC, ETF Liquidity Cockpit, big xyt, big-xyt, documentation, API documentation, API, single API, tick data
summary: "This is the API documentation for LC ETF API Python package."
sidebar: xyt_sidebar
permalink: lib_etf_lc_api_python.html
folder: xyt
---

[www_etf_lc]: https://etf-lc.com/

big xyt ETF Liquidity Cockpit Python API
---------
Official webpage: [https://etf-lc.com/][www_etf_lc]{:target="_blank"}

### 0.0. Library imports


```python
import pandas as pd

from xythub import ApiSettings, XytClient, get
```

### 0.1. API credentials


```python
ApiSettings.user_id='YOUR_USER_NAME'
ApiSettings.user_password='YOUR_PASSWORD'
```

### 1.0. Market share statistics


```python
query_params = {
    'isins': ['IE00BJ5JMP33', 'LU1781541252'],
    'dateFrom': pd.Timestamp('2021-12-01'),
    'dateTo': pd.Timestamp('2021-12-04'),
}

df = get(source='ETF-LC', endpoint='lcGetMarketShare', **query_params).to_pandas()
# Limit the number of rows to 5
df.head(5)
```

#### Input parameters

| Parameter              | Type                   | Required  | Description                                      |
|------------------------|------------------------|-----------|--------------------------------------------------|
| isins                  | [string]               | True      | List of isins                                    |
| dateFrom               | pandas Timestamp       | True      | Start date                                       | 
| dateTo                 | pandas Timestamp       | True      | End date                                         |  

#### Results

<table border="1" class="dataframe">
<thead><tr><th title="Field #1">group</th>
<th title="Field #2">exchange</th>
<th title="Field #3">date</th>
<th title="Field #4">market_category</th>
<th title="Field #5">trade_category</th>
<th title="Field #6">traded_value_in_eur</th>
<th title="Field #7">traded_value_in_local_ccy</th>
<th title="Field #8">trade_count</th>
<th title="Field #9">trade_size</th>
<th title="Field #10">value_traded_above_lisin_eur</th>
<th title="Field #11">stock</th>
<th title="Field #12">local_code</th>
<th title="Field #13">isin</th>
<th title="Field #14">currency</th>
<th title="Field #15">threshold</th>
<th title="Field #16">capped</th>
<th title="Field #17">symbol</th>
<th title="Field #18">traded_value_in_eur_correction</th>
<th title="Field #19">adjusted_turnover_of_trades_above_listhreshold_regular_hours</th>
<th title="Field #20">adjusted_turnover_outside_regular_hours</th>
<th title="Field #21">adjusted_traded_value_in_eur</th>
<th title="Field #22">umbrella_marketing_name</th>
<th title="Field #23">etfticker</th>
</tr></thead>
<tbody><tr>
<td>LSE</td>
<td>LSE</td>
<td>2021-12-01</td>
<td>Off Exchange</td>
<td>Off-Exchange/SI</td>
<td align="right">101567.0476</td>
<td align="right">2446435.32</td>
<td align="right">1</td>
<td align="right">400</td>
<td align="right">0</td>
<td>ISHARES $ TREASURY BOND 0-1YR UCITS ETF - MXN HEDGED (ACC)</td>
<td>0A24</td>
<td>IE00BJ5JMP33</td>
<td>MXN</td>
<td align="right">24416000</td>
<td>0</td>
<td>0A24.L</td>
<td> </td>
<td>0</td>
<td align="right">101567.0476</td>
<td align="right">101567.0476</td>
<td>iShares</td>
<td>IB1MXX</td>
</tr>
<tr>
<td>LSE</td>
<td>LSE</td>
<td>2021-12-02</td>
<td>Off Exchange</td>
<td>Off-Exchange/SI</td>
<td align="right">1082172.889</td>
<td align="right">26184579.88</td>
<td align="right">4</td>
<td align="right">4280</td>
<td align="right">0</td>
<td>ISHARES $ TREASURY BOND 0-1YR UCITS ETF - MXN HEDGED (ACC)</td>
<td>0A24</td>
<td>IE00BJ5JMP33</td>
<td>MXN</td>
<td align="right">24416000</td>
<td>0</td>
<td>0A24.L</td>
<td> </td>
<td>0</td>
<td align="right">404278.5087</td>
<td align="right">1082172.889</td>
<td>iShares</td>
<td>IB1MXX</td>
</tr>
<tr>
<td>LSE</td>
<td>LSE</td>
<td>2021-12-03</td>
<td>Off Exchange</td>
<td>Off-Exchange/SI</td>
<td align="right">51094.67522</td>
<td align="right">1225459.8</td>
<td align="right">1</td>
<td align="right">200</td>
<td align="right">0</td>
<td>ISHARES $ TREASURY BOND 0-1YR UCITS ETF - MXN HEDGED (ACC)</td>
<td>0A24</td>
<td>IE00BJ5JMP33</td>
<td>MXN</td>
<td align="right">24416000</td>
<td>0</td>
<td>0A24.L</td>
<td> </td>
<td>0</td>
<td align="right">51094.67522</td>
<td align="right">51094.67522</td>
<td>iShares</td>
<td>IB1MXX</td>
</tr>
<tr>
<td>Cboe Europe</td>
<td>Cboe TDM</td>
<td>2021-12-01</td>
<td>Off Exchange</td>
<td>Off-Exchange/SI</td>
<td align="right">4056555.284</td>
<td align="right">4056555.284</td>
<td align="right">6</td>
<td align="right">280248</td>
<td align="right">3794126.711</td>
<td>LYXOR CORE MSCI JAPAN (DR) UCITS ETF - ACC</td>
<td>LCUJd</td>
<td>LU1781541252</td>
<td>EUR</td>
<td align="right">1000000</td>
<td>0</td>
<td>LCUJd.BXTR</td>
<td> </td>
<td>0</td>
<td align="right">3794126.711</td>
<td align="right">4056555.284</td>
<td>Lyxor ETF</td>
<td>LCJP</td>
</tr>
<tr>
<td>Cboe Europe</td>
<td>Cboe TDM</td>
<td>2021-12-02</td>
<td>Off Exchange</td>
<td>Off-Exchange/SI</td>
<td align="right">421886.808</td>
<td align="right">421886.808</td>
<td align="right">4</td>
<td align="right">29624</td>
<td align="right">0</td>
<td>LYXOR CORE MSCI JAPAN (DR) UCITS ETF - ACC</td>
<td>LCUJd</td>
<td>LU1781541252</td>
<td>EUR</td>
<td align="right">1000000</td>
<td>0</td>
<td>LCUJd.BXTR</td>
<td> </td>
<td>0</td>
<td align="right">0</td>
<td align="right">421886.808</td>
<td>Lyxor ETF</td>
<td>LCJP</td>
</tr>
</tbody></table>

### 2.0. Spread statistics


```python
query_params = {
    'isins': ['IE00BJ5JMP33', 'LU1781541252'],
    'dateFrom': pd.Timestamp('2021-12-01'),
    'dateTo': pd.Timestamp('2021-12-04'),
}

df = get(source='ETF-LC', endpoint='lcGetSpreads', **query_params).to_pandas()
# Limit the number of rows to 5
df.head(5)
```

#### Input parameters

| Parameter              | Type                   | Required  | Description                                      |
|------------------------|------------------------|-----------|--------------------------------------------------|
| isins                  | [string]               | True      | List of isins                                    |
| dateFrom               | pandas Timestamp       | True      | Start date                                       | 
| dateTo                 | pandas Timestamp       | True      | End date                                         |  
| currency               | [string]               | False      | EUR (default value) or USD                       |

#### Results

<table border="1" class="dataframe">
<thead><tr><th title="Field #1">group</th>
<th title="Field #2">exchange</th>
<th title="Field #3">date</th>
<th title="Field #4">stock</th>
<th title="Field #5">local_code</th>
<th title="Field #6">isin</th>
<th title="Field #7">currency</th>
<th title="Field #8">symbol</th>
<th title="Field #9">size</th>
<th title="Field #10">spread_ask</th>
<th title="Field #11">spread_bid</th>
<th title="Field #12">spread_both</th>
<th title="Field #13">fill_presence_ask</th>
<th title="Field #14">fill_presence_bid</th>
<th title="Field #15">fill_presence_both</th>
<th title="Field #16">levels_ask</th>
<th title="Field #17">levels_bid</th>
<th title="Field #18">levels_both</th>
<th title="Field #19">ob_presence_ask</th>
<th title="Field #20">ob_presence_bid</th>
<th title="Field #21">ob_presence_both</th>
</tr></thead>
<tbody><tr>
<td>Cboe Europe</td>
<td>Cboe CXE</td>
<td>2021-12-01</td>
<td>iShares Treasury Bond 01yr UCITS ETF MXN Hed Acc</td>
<td>IB1MXx</td>
<td>IE00BJ5JMP33</td>
<td>MXN</td>
<td>IB1MXx.CHIX</td>
<td align="right">0</td>
<td align="right">4.528241059</td>
<td align="right">4.534529142</td>
<td align="right">9.056482118</td>
<td align="right">0.9984619012</td>
<td align="right">0.9984619012</td>
<td align="right">0.9984619012</td>
<td>1</td>
<td>1</td>
<td>1</td>
<td align="right">0.9984619012</td>
<td align="right">0.9984619012</td>
<td align="right">0.9984619012</td>
</tr>
<tr>
<td>Cboe Europe</td>
<td>Cboe CXE</td>
<td>2021-12-01</td>
<td>iShares Treasury Bond 01yr UCITS ETF MXN Hed Acc</td>
<td>IB1MXx</td>
<td>IE00BJ5JMP33</td>
<td>MXN</td>
<td>IB1MXx.CHIX</td>
<td align="right">5000</td>
<td align="right">4.528241059</td>
<td align="right">4.534529142</td>
<td align="right">9.056482118</td>
<td align="right">0.9984619012</td>
<td align="right">0.9984619012</td>
<td align="right">0.9984619012</td>
<td>1</td>
<td>1</td>
<td>1</td>
<td align="right">0.9984619012</td>
<td align="right">0.9984619012</td>
<td align="right">0.9984619012</td>
</tr>
<tr>
<td>Cboe Europe</td>
<td>Cboe CXE</td>
<td>2021-12-01</td>
<td>iShares Treasury Bond 01yr UCITS ETF MXN Hed Acc</td>
<td>IB1MXx</td>
<td>IE00BJ5JMP33</td>
<td>MXN</td>
<td>IB1MXx.CHIX</td>
<td align="right">10000</td>
<td align="right">4.528241059</td>
<td align="right">4.534529142</td>
<td align="right">9.056482118</td>
<td align="right">0.9984619012</td>
<td align="right">0.9984619012</td>
<td align="right">0.9984619012</td>
<td>1</td>
<td>1</td>
<td>1</td>
<td align="right">0.9984619012</td>
<td align="right">0.9984619012</td>
<td align="right">0.9984619012</td>
</tr>
<tr>
<td>Cboe Europe</td>
<td>Cboe CXE</td>
<td>2021-12-01</td>
<td>iShares Treasury Bond 01yr UCITS ETF MXN Hed Acc</td>
<td>IB1MXx</td>
<td>IE00BJ5JMP33</td>
<td>MXN</td>
<td>IB1MXx.CHIX</td>
<td align="right">25000</td>
<td align="right">4.528241059</td>
<td align="right">4.534529142</td>
<td align="right">9.056482118</td>
<td align="right">0.9984619012</td>
<td align="right">0.9984619012</td>
<td align="right">0.9984619012</td>
<td>1</td>
<td>1</td>
<td>1</td>
<td align="right">0.9984619012</td>
<td align="right">0.9984619012</td>
<td align="right">0.9984619012</td>
</tr>
<tr>
<td>Cboe Europe</td>
<td>Cboe CXE</td>
<td>2021-12-01</td>
<td>iShares Treasury Bond 01yr UCITS ETF MXN Hed Acc</td>
<td>IB1MXx</td>
<td>IE00BJ5JMP33</td>
<td>MXN</td>
<td>IB1MXx.CHIX</td>
<td align="right">100000</td>
<td align="right">4.528241059</td>
<td align="right">4.534529142</td>
<td align="right">9.056482118</td>
<td align="right">0.9984619012</td>
<td align="right">0.9984619012</td>
<td align="right">0.9984619012</td>
<td>1</td>
<td>1</td>
<td>1</td>
<td align="right">0.9984619012</td>
<td align="right">0.9984619012</td>
<td align="right">0.9984619012</td>
</tr>
</tbody></table>


{% include links.html %}
---
title: Accessing data from R
keywords: python, sample, data access, Liquidity Cockpit, big xyt, big-xyt, documentation, API documentation, API, single API, tick data
summary: "This is the API documentation for LC API Python package."
sidebar: xyt_sidebar
permalink: lib_lc_api_r.html
folder: xyt
---

[www_lc]: https://liquidity-cockpit.com
[www_lc_doc]: https://liquidity-cockpit.com/app/methodology

big xyt Liquidity Cockpit R API
---------
Official webpage: [https://liquidity-cockpit.com][www_lc]{:target="_blank"}

Official Liquidity Cockpit Documentation: [https://liquidity-cockpit.com/app/methodology][www_lc_doc]{:target="_blank"}

### 0.0. Library imports


```r
library(xythub)

library(tidyverse)
library(glue)
```

### 0.1. API credentials


```r
credentials <- Sys.getenv(c("API_USERNAME", "API_PWD"))
if (is.null(xythub:::xythub.env$username)) xythub.configure(username = credentials["API_USERNAME"], password = credentials["API_PWD"])
```

### 1.0. Symbols lookup


```r
query_name = 'lcGetIsinList'
query_params = c(xythub.toParameter('dateFrom', xythub.toDateValue('2019-10-02')),
                 xythub.toParameter('dateTo', xythub.toDateValue('2019-10-05'))
                 )

lcGetIsinList = xythub::xythub.getData(source = "LC", endpoint = query_name, parameters = query_params)
lcGetIsinList$data %>% head()
```


<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>isin</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>AEDFXA1EN018</td>
    </tr>
    <tr>
      <th>1</th>
      <td>AN8068571086</td>
    </tr>
    <tr>
      <th>2</th>
      <td>ANN4327C1220</td>
    </tr>
    <tr>
      <th>3</th>
      <td>ANN4327C1303</td>
    </tr>
    <tr>
      <th>4</th>
      <td>AT000000STR1</td>
    </tr>
  </tbody>
</table>
</div>



### 1.1. Market share statistics


```r
query_name = 'lcGetMarketShare'
query_params = c(xythub.toParameter("isins", xythub.toStringValue('DE000A1EWWW0', forceList = TRUE)),
                 xythub.toParameter("dateFrom", xythub.toDateValue('2019-10-02')),
                 xythub.toParameter("dateTo", xythub.toDateValue('2019-10-24'))
                 )

lcGetMarketShare = xythub.getData(source = "LC", endpoint = query_name, parameters = query_params)
lcGetMarketShare$data %>% head()
```


<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>value_traded_above_lisin_eur</th>
      <th>exchange</th>
      <th>addressable_traded_value_in_eur</th>
      <th>currency</th>
      <th>turnover_of_trades_above_listhreshold_regular_hours</th>
      <th>trade_count</th>
      <th>threshold</th>
      <th>traded_value_in_local_ccy</th>
      <th>capped</th>
      <th>local_code</th>
      <th>trade_category</th>
      <th>trade_size</th>
      <th>group</th>
      <th>date</th>
      <th>stock</th>
      <th>market_category</th>
      <th>traded_value_in_eur</th>
      <th>symbol</th>
      <th>isin</th>
      <th>turnover_outside_regular_hours</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>802756.25</td>
      <td>Aquis Exchange</td>
      <td>822375.00</td>
      <td>EUR</td>
      <td>0.0</td>
      <td>4</td>
      <td>650000.0</td>
      <td>822375.00</td>
      <td>0</td>
      <td>ADSd</td>
      <td>Lit/Auction Closing</td>
      <td>3060</td>
      <td>Aquis Exchange</td>
      <td>2019-10-02</td>
      <td>Adidas AG, DE000A1EWWW0</td>
      <td>On Exchange</td>
      <td>822375.00</td>
      <td>ADSd.AQX</td>
      <td>DE000A1EWWW0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>0.00</td>
      <td>Aquis Exchange</td>
      <td>15713.90</td>
      <td>EUR</td>
      <td>0.0</td>
      <td>12</td>
      <td>650000.0</td>
      <td>15713.90</td>
      <td>0</td>
      <td>ADSd</td>
      <td>Lit/Auction Periodic</td>
      <td>58</td>
      <td>Aquis Exchange</td>
      <td>2019-10-02</td>
      <td>Adidas AG, DE000A1EWWW0</td>
      <td>On Exchange</td>
      <td>15713.90</td>
      <td>ADSd.AQX</td>
      <td>DE000A1EWWW0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>0.00</td>
      <td>Aquis Exchange</td>
      <td>11798710.40</td>
      <td>EUR</td>
      <td>0.0</td>
      <td>997</td>
      <td>650000.0</td>
      <td>11798710.40</td>
      <td>0</td>
      <td>ADSd</td>
      <td>Lit/Order Book</td>
      <td>43050</td>
      <td>Aquis Exchange</td>
      <td>2019-10-02</td>
      <td>Adidas AG, DE000A1EWWW0</td>
      <td>On Exchange</td>
      <td>11798710.40</td>
      <td>ADSd.AQX</td>
      <td>DE000A1EWWW0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>0.00</td>
      <td>Aquis Exchange</td>
      <td>889218.40</td>
      <td>EUR</td>
      <td>0.0</td>
      <td>16</td>
      <td>650000.0</td>
      <td>889218.40</td>
      <td>0</td>
      <td>ADSd</td>
      <td>Lit/Auction Closing</td>
      <td>3274</td>
      <td>Aquis Exchange</td>
      <td>2019-10-04</td>
      <td>Adidas AG, DE000A1EWWW0</td>
      <td>On Exchange</td>
      <td>889218.40</td>
      <td>ADSd.AQX</td>
      <td>DE000A1EWWW0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>0.00</td>
      <td>Aquis Exchange</td>
      <td>13259.15</td>
      <td>EUR</td>
      <td>0.0</td>
      <td>4</td>
      <td>650000.0</td>
      <td>13259.15</td>
      <td>0</td>
      <td>ADSd</td>
      <td>Lit/Auction Periodic</td>
      <td>49</td>
      <td>Aquis Exchange</td>
      <td>2019-10-04</td>
      <td>Adidas AG, DE000A1EWWW0</td>
      <td>On Exchange</td>
      <td>13259.15</td>
      <td>ADSd.AQX</td>
      <td>DE000A1EWWW0</td>
      <td>0.0</td>
    </tr>
  </tbody>
</table>
</div>



### 1.2. Market share systematic internalizer statistics


```r
query_name = 'lcGetSiMarketShare'
query_params = c(xythub.toParameter("isins", xythub.toStringValue('DE000A1EWWW0', forceList = TRUE)),
                 xythub.toParameter("dateFrom", xythub.toDateValue('2019-10-02')),
                 xythub.toParameter("dateTo", xythub.toDateValue('2019-10-24'))
                 )

lcGetSiMarketShare = xythub.getData(source = "LC", endpoint = query_name, parameters = query_params)
lcGetSiMarketShare$data %>% head()
```


<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>group</th>
      <th>date</th>
      <th>trade_description</th>
      <th>stock</th>
      <th>market_category</th>
      <th>traded_value_in_eur</th>
      <th>isin</th>
      <th>turnover_outside_regular_hours</th>
      <th>value_traded_above_lisin_eur</th>
      <th>exchange</th>
      <th>addressable_condition</th>
      <th>currency</th>
      <th>turnover_of_trades_above_listhreshold_regular_hours</th>
      <th>trade_count</th>
      <th>threshold</th>
      <th>capped</th>
      <th>local_code</th>
      <th>trade_category</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Cboe TDM</td>
      <td>2019-10-10</td>
      <td>The Trade Was Reported To Cboe (UK) On Time An...</td>
      <td>Adidas AG, DE000A1EWWW0</td>
      <td>Off Exchange</td>
      <td>7.178919e+05</td>
      <td>DE000A1EWWW0</td>
      <td>7.178919e+05</td>
      <td>7.178919e+05</td>
      <td>Cboe TDM</td>
      <td>True</td>
      <td>HUF</td>
      <td>0.0</td>
      <td>1</td>
      <td>208637000.0</td>
      <td>0</td>
      <td>ADIDAt</td>
      <td>Off-Exchange/SI</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Cboe TDM</td>
      <td>2019-10-11</td>
      <td>The Trade Was Reported To Cboe (UK) On Time An...</td>
      <td>Adidas AG, DE000A1EWWW0</td>
      <td>Off Exchange</td>
      <td>5.792859e+05</td>
      <td>DE000A1EWWW0</td>
      <td>0.000000e+00</td>
      <td>0.000000e+00</td>
      <td>Cboe TDM</td>
      <td>True</td>
      <td>HUF</td>
      <td>0.0</td>
      <td>1</td>
      <td>208637000.0</td>
      <td>0</td>
      <td>ADIDAt</td>
      <td>Off-Exchange/SI</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Cboe TDM</td>
      <td>2019-10-02</td>
      <td>The Trade Was Reported To Cboe (UK) On Time An...</td>
      <td>Adidas AG, DE000A1EWWW0</td>
      <td>Off Exchange</td>
      <td>5.382965e+06</td>
      <td>DE000A1EWWW0</td>
      <td>4.869758e+06</td>
      <td>2.951611e+06</td>
      <td>Cboe TDM</td>
      <td>False</td>
      <td>EUR</td>
      <td>0.0</td>
      <td>29</td>
      <td>650000.0</td>
      <td>0</td>
      <td>ADSd</td>
      <td>Off-Exchange/SI</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Cboe TDM</td>
      <td>2019-10-02</td>
      <td>The Trade Was Reported To Cboe (UK) On Time An...</td>
      <td>Adidas AG, DE000A1EWWW0</td>
      <td>Off Exchange</td>
      <td>2.695486e+06</td>
      <td>DE000A1EWWW0</td>
      <td>0.000000e+00</td>
      <td>0.000000e+00</td>
      <td>Cboe TDM</td>
      <td>True</td>
      <td>EUR</td>
      <td>0.0</td>
      <td>122</td>
      <td>650000.0</td>
      <td>0</td>
      <td>ADSd</td>
      <td>Off-Exchange/SI</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Cboe TDM</td>
      <td>2019-10-02</td>
      <td>The Trade Was Reported To Cboe (UK) On Time An...</td>
      <td>Adidas AG, DE000A1EWWW0</td>
      <td>Off Exchange</td>
      <td>3.567410e+05</td>
      <td>DE000A1EWWW0</td>
      <td>0.000000e+00</td>
      <td>0.000000e+00</td>
      <td>Cboe TDM</td>
      <td>True</td>
      <td>EUR</td>
      <td>0.0</td>
      <td>45</td>
      <td>650000.0</td>
      <td>0</td>
      <td>ADSd</td>
      <td>Off-Exchange/SI</td>
    </tr>
  </tbody>
</table>
</div>



### 1.3. Market share statistics alternative closing mechanisms


```r
query_name = 'lcGetClosingMechanismsMarketShare'
query_params = c(xythub.toParameter("isins", xythub.toStringValue('DE000A1EWWW0', forceList = TRUE)),
                 xythub.toParameter("dateFrom", xythub.toDateValue('2019-10-02')),
                 xythub.toParameter("dateTo", xythub.toDateValue('2019-10-24'))
                 )

lcGetClosingMechanismsMarketShare = xythub.getData(source = "LC", endpoint = query_name, parameters = query_params)
lcGetClosingMechanismsMarketShare$data %>% head()
```


<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>value_traded_above_lisin_eur</th>
      <th>exchange</th>
      <th>currency</th>
      <th>trade_count</th>
      <th>threshold</th>
      <th>capped</th>
      <th>local_code</th>
      <th>trade_size</th>
      <th>group</th>
      <th>date</th>
      <th>stock</th>
      <th>traded_value_in_eur</th>
      <th>symbol</th>
      <th>isin</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>802756.25</td>
      <td>Aquis MaC</td>
      <td>EUR</td>
      <td>4</td>
      <td>650000.0</td>
      <td>0</td>
      <td>ADSd</td>
      <td>3060</td>
      <td>Aquis Exchange</td>
      <td>2019-10-02</td>
      <td>Adidas AG, DE000A1EWWW0</td>
      <td>822375.00</td>
      <td>ADSd.AQX</td>
      <td>DE000A1EWWW0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>0.00</td>
      <td>Aquis MaC</td>
      <td>EUR</td>
      <td>16</td>
      <td>650000.0</td>
      <td>0</td>
      <td>ADSd</td>
      <td>3274</td>
      <td>Aquis Exchange</td>
      <td>2019-10-04</td>
      <td>Adidas AG, DE000A1EWWW0</td>
      <td>889218.40</td>
      <td>ADSd.AQX</td>
      <td>DE000A1EWWW0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>0.00</td>
      <td>Aquis MaC</td>
      <td>EUR</td>
      <td>4</td>
      <td>650000.0</td>
      <td>0</td>
      <td>ADSd</td>
      <td>2016</td>
      <td>Aquis Exchange</td>
      <td>2019-10-07</td>
      <td>Adidas AG, DE000A1EWWW0</td>
      <td>549057.60</td>
      <td>ADSd.AQX</td>
      <td>DE000A1EWWW0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>0.00</td>
      <td>Aquis MaC</td>
      <td>EUR</td>
      <td>2</td>
      <td>650000.0</td>
      <td>0</td>
      <td>ADSd</td>
      <td>151</td>
      <td>Aquis Exchange</td>
      <td>2019-10-08</td>
      <td>Adidas AG, DE000A1EWWW0</td>
      <td>40747.35</td>
      <td>ADSd.AQX</td>
      <td>DE000A1EWWW0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>0.00</td>
      <td>Aquis MaC</td>
      <td>EUR</td>
      <td>4</td>
      <td>650000.0</td>
      <td>0</td>
      <td>ADSd</td>
      <td>549</td>
      <td>Aquis Exchange</td>
      <td>2019-10-09</td>
      <td>Adidas AG, DE000A1EWWW0</td>
      <td>153033.75</td>
      <td>ADSd.AQX</td>
      <td>DE000A1EWWW0</td>
    </tr>
  </tbody>
</table>
</div>



### 1.4. Market share statistics grouped by index


```r
query_name = 'lcGetMarketShareMonthlyStatsByIndex'
query_params = c(xythub.toParameter("dateFrom", xythub.toDateValue('2019-10-02')),
                 xythub.toParameter("dateTo", xythub.toDateValue('2019-10-24'))
                 )

lcGetMarketShareMonthlyStatsByIndex = xythub.getData(source = "LC", endpoint = query_name, parameters = query_params)
lcGetMarketShareMonthlyStatsByIndex$data %>% head()
```


<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>adjusted</th>
      <th>onoff</th>
      <th>advt</th>
      <th>liquidity_category</th>
      <th>index</th>
      <th>adjusted_reg_hours_only</th>
      <th>month_of_date</th>
      <th>average_value_per_trade</th>
      <th>traded_value_in_eur</th>
      <th>trade_category</th>
      <th>adjusted_reg_hours_only_sub_lis</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>3.356388e+07</td>
      <td>ON</td>
      <td>2.237592e+06</td>
      <td>Dark MTF</td>
      <td>AT20</td>
      <td>3.356388e+07</td>
      <td>2019.10</td>
      <td>8.390969e+05</td>
      <td>3.356388e+07</td>
      <td>Dark/IOI Negotiation</td>
      <td>3.356388e+07</td>
    </tr>
    <tr>
      <th>1</th>
      <td>4.554001e+08</td>
      <td>ON</td>
      <td>1.980001e+07</td>
      <td>Dark MTF</td>
      <td>AT20</td>
      <td>4.554001e+08</td>
      <td>2019.10</td>
      <td>1.860599e+04</td>
      <td>4.554001e+08</td>
      <td>Dark/Order Book</td>
      <td>4.554001e+08</td>
    </tr>
    <tr>
      <th>2</th>
      <td>9.364334e+08</td>
      <td>ON</td>
      <td>4.071450e+07</td>
      <td>Lit Auction</td>
      <td>AT20</td>
      <td>9.364334e+08</td>
      <td>2019.10</td>
      <td>1.887971e+06</td>
      <td>9.364334e+08</td>
      <td>Lit/Auction Closing</td>
      <td>9.364334e+08</td>
    </tr>
    <tr>
      <th>3</th>
      <td>4.510096e+06</td>
      <td>ON</td>
      <td>1.960911e+05</td>
      <td>Lit Auction</td>
      <td>AT20</td>
      <td>4.510096e+06</td>
      <td>2019.10</td>
      <td>1.153477e+04</td>
      <td>4.510096e+06</td>
      <td>Lit/Auction Intraday</td>
      <td>4.510096e+06</td>
    </tr>
    <tr>
      <th>4</th>
      <td>1.810073e+07</td>
      <td>ON</td>
      <td>7.869881e+05</td>
      <td>Lit Auction</td>
      <td>AT20</td>
      <td>1.810073e+07</td>
      <td>2019.10</td>
      <td>3.309091e+04</td>
      <td>1.810073e+07</td>
      <td>Lit/Auction Opening</td>
      <td>1.810073e+07</td>
    </tr>
  </tbody>
</table>
</div>


### 1.5. Market share statistics grouped by exchange and trade category


```r
query_name = 'lcGetMarketShareByExchangeTradeCategory'
query_params = xythub.toParameter("day", xythub.toDateValue('2019-10-02'))

lcGetMarketShareByExchangeTradeCategory = xythub.getData(source = "LC", endpoint = query_name, parameters = query_params)
lcGetMarketShareByExchangeTradeCategory$data %>% head()
```


<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>adjusted</th>
      <th>value_traded_above_lisin_eur</th>
      <th>group</th>
      <th>exchange</th>
      <th>adjusted_reg_hours_only</th>
      <th>date</th>
      <th>traded_value_in_eur</th>
      <th>market_category</th>
      <th>trade_category</th>
      <th>adjusted_reg_hours_only_sub_lis</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>6.059862e+07</td>
      <td>7.402420e+06</td>
      <td>Aquis Exchange</td>
      <td>Aquis Exchange</td>
      <td>6.059862e+07</td>
      <td>2019-10-02</td>
      <td>6.059862e+07</td>
      <td>On Exchange</td>
      <td>Lit/Auction Closing</td>
      <td>6.059862e+07</td>
    </tr>
    <tr>
      <th>1</th>
      <td>3.116206e+07</td>
      <td>0.000000e+00</td>
      <td>Aquis Exchange</td>
      <td>Aquis Exchange</td>
      <td>3.116206e+07</td>
      <td>2019-10-02</td>
      <td>3.116206e+07</td>
      <td>On Exchange</td>
      <td>Lit/Auction Periodic</td>
      <td>3.116206e+07</td>
    </tr>
    <tr>
      <th>2</th>
      <td>1.465978e+09</td>
      <td>1.574655e+04</td>
      <td>Aquis Exchange</td>
      <td>Aquis Exchange</td>
      <td>1.465978e+09</td>
      <td>2019-10-02</td>
      <td>1.465978e+09</td>
      <td>On Exchange</td>
      <td>Lit/Order Book</td>
      <td>1.465978e+09</td>
    </tr>
    <tr>
      <th>3</th>
      <td>5.239423e+06</td>
      <td>4.389265e+06</td>
      <td>Athens Stock Exchange</td>
      <td>Athens Stock Exchange</td>
      <td>5.239423e+06</td>
      <td>2019-10-02</td>
      <td>5.239423e+06</td>
      <td>On Exchange</td>
      <td>Lit/Auction Closing</td>
      <td>5.239423e+06</td>
    </tr>
    <tr>
      <th>4</th>
      <td>4.399461e+05</td>
      <td>0.000000e+00</td>
      <td>Athens Stock Exchange</td>
      <td>Athens Stock Exchange</td>
      <td>4.399461e+05</td>
      <td>2019-10-02</td>
      <td>4.399461e+05</td>
      <td>On Exchange</td>
      <td>Lit/Auction Opening</td>
      <td>4.399461e+05</td>
    </tr>
  </tbody>
</table>
</div>



### 1.6. Market share statistics for specified index


```r
query_name = 'lcGetMarketShareForIndex'
query_params = c(xythub.toParameter("dateFrom", xythub.toDateValue('2019-10-02')),
                 xythub.toParameter("dateTo", xythub.toDateValue('2019-10-24')),
                 xythub.toParameter(name = "index", value = "UK100"),
                 xythub.toParameter(name = "mode", value = c("asof", "ISINCurrency"))
                 )

lcGetMarketShareForIndex = xythub.getData(source = "LC", endpoint = query_name, parameters = query_params)
lcGetMarketShareForIndex$data %>% head()
```


<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>value_traded_above_lisin_eur</th>
      <th>exchange</th>
      <th>addressable_traded_value_in_eur</th>
      <th>currency</th>
      <th>turnover_of_trades_above_listhreshold_regular_hours</th>
      <th>trade_count</th>
      <th>threshold</th>
      <th>traded_value_in_local_ccy</th>
      <th>capped</th>
      <th>local_code</th>
      <th>trade_category</th>
      <th>trade_size</th>
      <th>group</th>
      <th>date</th>
      <th>stock</th>
      <th>market_category</th>
      <th>traded_value_in_eur</th>
      <th>symbol</th>
      <th>isin</th>
      <th>turnover_outside_regular_hours</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0.0</td>
      <td>Aquis Exchange</td>
      <td>11142.857143</td>
      <td>GBX</td>
      <td>0.0</td>
      <td>2</td>
      <td>35781200.0</td>
      <td>991380.0</td>
      <td>0</td>
      <td>HSXl</td>
      <td>Lit/Auction Closing</td>
      <td>620</td>
      <td>Aquis Exchange</td>
      <td>2019-10-02</td>
      <td>Hiscox Ltd, BMG4593F1389</td>
      <td>On Exchange</td>
      <td>11142.857143</td>
      <td>HSXl.AQX</td>
      <td>BMG4593F1389</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>0.0</td>
      <td>Aquis Exchange</td>
      <td>6324.148589</td>
      <td>GBX</td>
      <td>0.0</td>
      <td>3</td>
      <td>35781200.0</td>
      <td>562659.5</td>
      <td>0</td>
      <td>HSXl</td>
      <td>Lit/Auction Periodic</td>
      <td>347</td>
      <td>Aquis Exchange</td>
      <td>2019-10-02</td>
      <td>Hiscox Ltd, BMG4593F1389</td>
      <td>On Exchange</td>
      <td>6324.148589</td>
      <td>HSXl.AQX</td>
      <td>BMG4593F1389</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>0.0</td>
      <td>Aquis Exchange</td>
      <td>434787.400247</td>
      <td>GBX</td>
      <td>0.0</td>
      <td>123</td>
      <td>35781200.0</td>
      <td>38683035.0</td>
      <td>0</td>
      <td>HSXl</td>
      <td>Lit/Order Book</td>
      <td>24013</td>
      <td>Aquis Exchange</td>
      <td>2019-10-02</td>
      <td>Hiscox Ltd, BMG4593F1389</td>
      <td>On Exchange</td>
      <td>434787.400247</td>
      <td>HSXl.AQX</td>
      <td>BMG4593F1389</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>0.0</td>
      <td>Aquis Exchange</td>
      <td>3221.410069</td>
      <td>GBX</td>
      <td>0.0</td>
      <td>1</td>
      <td>35781200.0</td>
      <td>286029.0</td>
      <td>0</td>
      <td>HSXl</td>
      <td>Lit/Auction Closing</td>
      <td>183</td>
      <td>Aquis Exchange</td>
      <td>2019-10-03</td>
      <td>Hiscox Ltd, BMG4593F1389</td>
      <td>On Exchange</td>
      <td>3221.410069</td>
      <td>HSXl.AQX</td>
      <td>BMG4593F1389</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>0.0</td>
      <td>Aquis Exchange</td>
      <td>586045.737133</td>
      <td>GBX</td>
      <td>0.0</td>
      <td>196</td>
      <td>35781200.0</td>
      <td>52035001.0</td>
      <td>0</td>
      <td>HSXl</td>
      <td>Lit/Order Book</td>
      <td>33092</td>
      <td>Aquis Exchange</td>
      <td>2019-10-03</td>
      <td>Hiscox Ltd, BMG4593F1389</td>
      <td>On Exchange</td>
      <td>586045.737133</td>
      <td>HSXl.AQX</td>
      <td>BMG4593F1389</td>
      <td>0.0</td>
    </tr>
  </tbody>
</table>
</div>



### 1.7. Effective spreads


```r
query_name = 'lcGetSpreads'
query_params = c(xythub.toParameter("isins", c('DE000A1EWWW0', 'NL0000235190')),
                 xythub.toParameter("dateFrom", xythub.toDateValue('2019-10-02')),
                 xythub.toParameter("dateTo", xythub.toDateValue('2019-10-24'))
                 )

lcGetSpreads = xythub.getData(source = "LC", endpoint = query_name, parameters = query_params)
lcGetSpreads$data %>% head()
```


<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>ob_presence_both</th>
      <th>levels_both</th>
      <th>exchange</th>
      <th>spread_ask</th>
      <th>spread_bid</th>
      <th>currency</th>
      <th>spread_both</th>
      <th>fill_presence_ask</th>
      <th>fill_presence_bid</th>
      <th>local_code</th>
      <th>...</th>
      <th>levels_ask</th>
      <th>levels_bid</th>
      <th>group</th>
      <th>date</th>
      <th>ob_presence_ask</th>
      <th>ob_presence_bid</th>
      <th>stock</th>
      <th>symbol</th>
      <th>fill_presence_both</th>
      <th>isin</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1.0</td>
      <td>1.000000</td>
      <td>Cboe BXE</td>
      <td>12.096535</td>
      <td>12.114261</td>
      <td>EUR</td>
      <td>24.193070</td>
      <td>1.000000</td>
      <td>1.000000</td>
      <td>SPRd</td>
      <td>...</td>
      <td>1.000000</td>
      <td>1.000000</td>
      <td>Cboe Europe</td>
      <td>2019-10-02</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>Axel Springer SE, DE0005501357</td>
      <td>SPRd.BTE</td>
      <td>1.000000</td>
      <td>DE0005501357</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1.0</td>
      <td>1.217306</td>
      <td>Cboe BXE</td>
      <td>12.495105</td>
      <td>12.480330</td>
      <td>EUR</td>
      <td>24.956474</td>
      <td>1.000000</td>
      <td>1.000000</td>
      <td>SPRd</td>
      <td>...</td>
      <td>1.133201</td>
      <td>1.086896</td>
      <td>Cboe Europe</td>
      <td>2019-10-02</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>Axel Springer SE, DE0005501357</td>
      <td>SPRd.BTE</td>
      <td>1.000000</td>
      <td>DE0005501357</td>
    </tr>
    <tr>
      <th>2</th>
      <td>1.0</td>
      <td>1.990829</td>
      <td>Cboe BXE</td>
      <td>13.924029</td>
      <td>15.294344</td>
      <td>EUR</td>
      <td>29.190150</td>
      <td>1.000000</td>
      <td>1.000000</td>
      <td>SPRd</td>
      <td>...</td>
      <td>1.595098</td>
      <td>1.798573</td>
      <td>Cboe Europe</td>
      <td>2019-10-02</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>Axel Springer SE, DE0005501357</td>
      <td>SPRd.BTE</td>
      <td>1.000000</td>
      <td>DE0005501357</td>
    </tr>
    <tr>
      <th>3</th>
      <td>1.0</td>
      <td>3.744360</td>
      <td>Cboe BXE</td>
      <td>21.834671</td>
      <td>25.364437</td>
      <td>EUR</td>
      <td>47.122158</td>
      <td>0.999989</td>
      <td>0.999993</td>
      <td>SPRd</td>
      <td>...</td>
      <td>3.172367</td>
      <td>3.230307</td>
      <td>Cboe Europe</td>
      <td>2019-10-02</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>Axel Springer SE, DE0005501357</td>
      <td>SPRd.BTE</td>
      <td>0.999989</td>
      <td>DE0005501357</td>
    </tr>
    <tr>
      <th>4</th>
      <td>1.0</td>
      <td>9.999789</td>
      <td>Cboe BXE</td>
      <td>69.995172</td>
      <td>65.966689</td>
      <td>EUR</td>
      <td>83.657747</td>
      <td>0.124274</td>
      <td>0.060892</td>
      <td>SPRd</td>
      <td>...</td>
      <td>9.358650</td>
      <td>9.005864</td>
      <td>Cboe Europe</td>
      <td>2019-10-02</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>Axel Springer SE, DE0005501357</td>
      <td>SPRd.BTE</td>
      <td>0.002815</td>
      <td>DE0005501357</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 21 columns</p>
</div>



### 1.8. Effective spreads statistics for specified index


```r
query_name = 'lcGetSpreadsForIndex'
query_params = c(xythub.toParameter("dateFrom", xythub.toDateValue('2019-10-02')),
                 xythub.toParameter("dateTo", xythub.toDateValue('2019-10-24')),
                 xythub.toParameter(name = "index", value = "UK100"),
                 xythub.toParameter(name = "mode", value = c("asof", "ISINCurrency"))
                 )

lcGetSpreadsForIndex = xythub.getData(source = "LC", endpoint = query_name, parameters = query_params)
lcGetSpreadsForIndex$data %>% head()
```


<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>ob_presence_both</th>
      <th>levels_both</th>
      <th>exchange</th>
      <th>spread_ask</th>
      <th>spread_bid</th>
      <th>currency</th>
      <th>spread_both</th>
      <th>fill_presence_ask</th>
      <th>fill_presence_bid</th>
      <th>local_code</th>
      <th>...</th>
      <th>levels_ask</th>
      <th>levels_bid</th>
      <th>group</th>
      <th>date</th>
      <th>ob_presence_ask</th>
      <th>ob_presence_bid</th>
      <th>stock</th>
      <th>symbol</th>
      <th>fill_presence_both</th>
      <th>isin</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0.999993</td>
      <td>1.000000</td>
      <td>Cboe BXE</td>
      <td>17.240114</td>
      <td>17.276987</td>
      <td>GBX</td>
      <td>34.480228</td>
      <td>0.999993</td>
      <td>0.999993</td>
      <td>HSXl</td>
      <td>...</td>
      <td>1.000000</td>
      <td>1.000000</td>
      <td>Cboe Europe</td>
      <td>2019-10-02</td>
      <td>0.999993</td>
      <td>0.999993</td>
      <td>Hiscox Ltd, BMG4593F1389</td>
      <td>HSXl.BTE</td>
      <td>0.999993</td>
      <td>BMG4593F1389</td>
    </tr>
    <tr>
      <th>1</th>
      <td>0.999993</td>
      <td>2.133036</td>
      <td>Cboe BXE</td>
      <td>21.010621</td>
      <td>20.496922</td>
      <td>GBX</td>
      <td>41.459124</td>
      <td>0.999993</td>
      <td>0.999993</td>
      <td>HSXl</td>
      <td>...</td>
      <td>1.742867</td>
      <td>1.855267</td>
      <td>Cboe Europe</td>
      <td>2019-10-02</td>
      <td>0.999993</td>
      <td>0.999993</td>
      <td>Hiscox Ltd, BMG4593F1389</td>
      <td>HSXl.BTE</td>
      <td>0.999993</td>
      <td>BMG4593F1389</td>
    </tr>
    <tr>
      <th>2</th>
      <td>0.999993</td>
      <td>3.498155</td>
      <td>Cboe BXE</td>
      <td>25.846012</td>
      <td>25.644454</td>
      <td>GBX</td>
      <td>51.312774</td>
      <td>0.989612</td>
      <td>0.996653</td>
      <td>HSXl</td>
      <td>...</td>
      <td>2.949744</td>
      <td>3.110383</td>
      <td>Cboe Europe</td>
      <td>2019-10-02</td>
      <td>0.999993</td>
      <td>0.999993</td>
      <td>Hiscox Ltd, BMG4593F1389</td>
      <td>HSXl.BTE</td>
      <td>0.989404</td>
      <td>BMG4593F1389</td>
    </tr>
    <tr>
      <th>3</th>
      <td>0.999993</td>
      <td>6.148026</td>
      <td>Cboe BXE</td>
      <td>41.597791</td>
      <td>46.288868</td>
      <td>GBX</td>
      <td>87.643867</td>
      <td>0.988228</td>
      <td>0.987626</td>
      <td>HSXl</td>
      <td>...</td>
      <td>5.501066</td>
      <td>5.702067</td>
      <td>Cboe Europe</td>
      <td>2019-10-02</td>
      <td>0.999993</td>
      <td>0.999993</td>
      <td>Hiscox Ltd, BMG4593F1389</td>
      <td>HSXl.BTE</td>
      <td>0.987616</td>
      <td>BMG4593F1389</td>
    </tr>
    <tr>
      <th>4</th>
      <td>0.999999</td>
      <td>1.000000</td>
      <td>Cboe BXE</td>
      <td>23.570847</td>
      <td>23.635222</td>
      <td>GBX</td>
      <td>47.141694</td>
      <td>0.999999</td>
      <td>0.999999</td>
      <td>HSXl</td>
      <td>...</td>
      <td>1.000000</td>
      <td>1.000000</td>
      <td>Cboe Europe</td>
      <td>2019-10-03</td>
      <td>0.999999</td>
      <td>0.999999</td>
      <td>Hiscox Ltd, BMG4593F1389</td>
      <td>HSXl.BTE</td>
      <td>0.999999</td>
      <td>BMG4593F1389</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 21 columns</p>
</div>



### 1.9.  EBB, EBO by ISIN and Currency


```r
query_name = 'lcGetBestPrices'
query_params = c(xythub.toParameter("isins", 'DE0005501357'),
                 xythub.toParameter("dateFrom", xythub.toDateValue('2019-10-02')),
                 xythub.toParameter("dateTo", xythub.toDateValue('2019-10-24'))
                 )

lcGetBestPrices = xythub.getData(source = "LC", endpoint = query_name, parameters = query_params)
lcGetBestPrices$data %>% head()
```


<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>exchange</th>
      <th>presence_ebb_exclusive</th>
      <th>currency</th>
      <th>presence_ebo_exclusive</th>
      <th>presence_ebo</th>
      <th>size</th>
      <th>group</th>
      <th>combined_ask_presence</th>
      <th>combined_bid_presence</th>
      <th>date</th>
      <th>stock</th>
      <th>symbol</th>
      <th>isin</th>
      <th>presence_ebb</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Xetra</td>
      <td>2.041062e-01</td>
      <td>EUR</td>
      <td>0.106465</td>
      <td>0.995672</td>
      <td>0</td>
      <td>Xetra</td>
      <td>0.995685</td>
      <td>0.995685</td>
      <td>2019-10-02</td>
      <td>Axel Springer SE</td>
      <td>SPR.XE</td>
      <td>DE0005501357</td>
      <td>0.987787</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Aquis Exchange</td>
      <td>1.969282e-07</td>
      <td>EUR</td>
      <td>0.000042</td>
      <td>0.696558</td>
      <td>0</td>
      <td>Aquis Exchange</td>
      <td>0.995685</td>
      <td>0.995685</td>
      <td>2019-10-02</td>
      <td>Axel Springer SE</td>
      <td>SPRd.AQX</td>
      <td>DE0005501357</td>
      <td>0.687827</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Cboe BXE</td>
      <td>6.498631e-06</td>
      <td>EUR</td>
      <td>0.000000</td>
      <td>0.733834</td>
      <td>0</td>
      <td>Cboe Europe</td>
      <td>0.995685</td>
      <td>0.995685</td>
      <td>2019-10-02</td>
      <td>Axel Springer SE</td>
      <td>SPRd.BTE</td>
      <td>DE0005501357</td>
      <td>0.248540</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Cboe CXE</td>
      <td>3.938564e-07</td>
      <td>EUR</td>
      <td>0.000002</td>
      <td>0.634172</td>
      <td>0</td>
      <td>Cboe Europe</td>
      <td>0.995685</td>
      <td>0.995685</td>
      <td>2019-10-02</td>
      <td>Axel Springer SE</td>
      <td>SPRd.CHIX</td>
      <td>DE0005501357</td>
      <td>0.738243</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Turquoise</td>
      <td>3.708815e-06</td>
      <td>EUR</td>
      <td>0.000000</td>
      <td>0.375783</td>
      <td>0</td>
      <td>LSE</td>
      <td>0.995685</td>
      <td>0.995685</td>
      <td>2019-10-02</td>
      <td>Axel Springer SE</td>
      <td>SPRd.TU</td>
      <td>DE0005501357</td>
      <td>0.705556</td>
    </tr>
  </tbody>
</table>
</div>



### 1.10. EBB, EBO by Primary Exchange + MTFs


```r
query_name = 'lcGetBestPricesPrimaryExchange'
query_params = c(xythub.toParameter("isins", 'DE0005501357'),
                 xythub.toParameter("dateFrom", xythub.toDateValue('2019-10-02')),
                 xythub.toParameter("dateTo", xythub.toDateValue('2019-10-24'))
                 )

lcGetBestPricesPrimaryExchange = xythub.getData(source = "LC", endpoint = query_name, parameters = query_params)
lcGetBestPricesPrimaryExchange$data %>% head()
```


<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>exchange</th>
      <th>presence_ebb_exclusive</th>
      <th>currency</th>
      <th>primary_exchange_code</th>
      <th>presence_ebo_exclusive</th>
      <th>presence_ebo</th>
      <th>size</th>
      <th>group</th>
      <th>combined_ask_presence</th>
      <th>combined_bid_presence</th>
      <th>date</th>
      <th>stock</th>
      <th>symbol</th>
      <th>isin</th>
      <th>presence_ebb</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Xetra</td>
      <td>2.041062e-01</td>
      <td>EUR</td>
      <td>D</td>
      <td>0.106465</td>
      <td>0.995672</td>
      <td>0</td>
      <td>Xetra</td>
      <td>0.995685</td>
      <td>0.995685</td>
      <td>2019-10-02</td>
      <td>Axel Springer SE</td>
      <td>SPR.XE</td>
      <td>DE0005501357</td>
      <td>0.987787</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Aquis Exchange</td>
      <td>1.969282e-07</td>
      <td>EUR</td>
      <td>D</td>
      <td>0.000042</td>
      <td>0.696558</td>
      <td>0</td>
      <td>Aquis Exchange</td>
      <td>0.995685</td>
      <td>0.995685</td>
      <td>2019-10-02</td>
      <td>Axel Springer SE</td>
      <td>SPRd.AQX</td>
      <td>DE0005501357</td>
      <td>0.687827</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Cboe BXE</td>
      <td>6.498631e-06</td>
      <td>EUR</td>
      <td>D</td>
      <td>0.000000</td>
      <td>0.733834</td>
      <td>0</td>
      <td>Cboe Europe</td>
      <td>0.995685</td>
      <td>0.995685</td>
      <td>2019-10-02</td>
      <td>Axel Springer SE</td>
      <td>SPRd.BTE</td>
      <td>DE0005501357</td>
      <td>0.248540</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Cboe CXE</td>
      <td>3.938564e-07</td>
      <td>EUR</td>
      <td>D</td>
      <td>0.000002</td>
      <td>0.634172</td>
      <td>0</td>
      <td>Cboe Europe</td>
      <td>0.995685</td>
      <td>0.995685</td>
      <td>2019-10-02</td>
      <td>Axel Springer SE</td>
      <td>SPRd.CHIX</td>
      <td>DE0005501357</td>
      <td>0.738243</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Turquoise</td>
      <td>3.708815e-06</td>
      <td>EUR</td>
      <td>D</td>
      <td>0.000000</td>
      <td>0.375783</td>
      <td>0</td>
      <td>LSE</td>
      <td>0.995685</td>
      <td>0.995685</td>
      <td>2019-10-02</td>
      <td>Axel Springer SE</td>
      <td>SPRd.TU</td>
      <td>DE0005501357</td>
      <td>0.705556</td>
    </tr>
  </tbody>
</table>
</div>


### 1.11 Price movement


```r
query_name = 'lcGetPriceMovement'
query_params = c(xythub.toParameter("isins", 'FI0009000681'),
                 xythub.toParameter("dateFrom", xythub.toDateValue('2020-05-10')),
                 xythub.toParameter("dateTo", xythub.toDateValue('2020-05-20'))
                 )

lcGetPriceMovement = xythub.getData(source = "LC", endpoint = query_name, parameters = query_params)
lcGetPriceMovement$data %>% head()
```


<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>stock</th>
      <th>symbol</th>
      <th>isin</th>
      <th>abs_wavg_price_impact_ebbo</th>
      <th>wavg_price_movement_prob_ebbo</th>
      <th>exchange</th>
      <th>index</th>
      <th>traded_value_ebbo</th>
      <th>traded_value_primary</th>
      <th>abs_wavg_price_impact_primary</th>
      <th>threshold</th>
      <th>trade_category</th>
      <th>group</th>
      <th>date</th>
      <th>wavg_price_movement_prob_primary</th>
      <th>offset</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Nokia Corp</td>
      <td>0HAF.L</td>
      <td>FI0009000681</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>LSE</td>
      <td>FI25</td>
      <td>3327.0</td>
      <td>3327.0</td>
      <td>0.0</td>
      <td>BelowLIS</td>
      <td>Off-Book (On-Exchange)</td>
      <td>LSE</td>
      <td>2020-05-11</td>
      <td>0.0</td>
      <td>-300000</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Nokia Corp</td>
      <td>0HAF.L</td>
      <td>FI0009000681</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>LSE</td>
      <td>FI25</td>
      <td>3327.0</td>
      <td>3327.0</td>
      <td>0.0</td>
      <td>BelowLIS</td>
      <td>Off-Book (On-Exchange)</td>
      <td>LSE</td>
      <td>2020-05-11</td>
      <td>0.0</td>
      <td>-60000</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Nokia Corp</td>
      <td>0HAF.L</td>
      <td>FI0009000681</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>LSE</td>
      <td>FI25</td>
      <td>3327.0</td>
      <td>3327.0</td>
      <td>0.0</td>
      <td>BelowLIS</td>
      <td>Off-Book (On-Exchange)</td>
      <td>LSE</td>
      <td>2020-05-11</td>
      <td>0.0</td>
      <td>-30000</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Nokia Corp</td>
      <td>0HAF.L</td>
      <td>FI0009000681</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>LSE</td>
      <td>FI25</td>
      <td>3327.0</td>
      <td>3327.0</td>
      <td>0.0</td>
      <td>BelowLIS</td>
      <td>Off-Book (On-Exchange)</td>
      <td>LSE</td>
      <td>2020-05-11</td>
      <td>0.0</td>
      <td>-15000</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Nokia Corp</td>
      <td>0HAF.L</td>
      <td>FI0009000681</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>LSE</td>
      <td>FI25</td>
      <td>3327.0</td>
      <td>3327.0</td>
      <td>0.0</td>
      <td>BelowLIS</td>
      <td>Off-Book (On-Exchange)</td>
      <td>LSE</td>
      <td>2020-05-11</td>
      <td>0.0</td>
      <td>-5000</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Nokia Corp</td>
      <td>0HAF.L</td>
      <td>FI0009000681</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>LSE</td>
      <td>FI25</td>
      <td>3327.0</td>
      <td>3327.0</td>
      <td>0.0</td>
      <td>BelowLIS</td>
      <td>Off-Book (On-Exchange)</td>
      <td>LSE</td>
      <td>2020-05-11</td>
      <td>0.0</td>
      <td>-1000</td>
    </tr>
    <tr>
      <th>6</th>
      <td>Nokia Corp</td>
      <td>0HAF.L</td>
      <td>FI0009000681</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>LSE</td>
      <td>FI25</td>
      <td>3327.0</td>
      <td>3327.0</td>
      <td>0.0</td>
      <td>BelowLIS</td>
      <td>Off-Book (On-Exchange)</td>
      <td>LSE</td>
      <td>2020-05-11</td>
      <td>0.0</td>
      <td>-500</td>
    </tr>
    <tr>
      <th>7</th>
      <td>Nokia Corp</td>
      <td>0HAF.L</td>
      <td>FI0009000681</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>LSE</td>
      <td>FI25</td>
      <td>3327.0</td>
      <td>3327.0</td>
      <td>0.0</td>
      <td>BelowLIS</td>
      <td>Off-Book (On-Exchange)</td>
      <td>LSE</td>
      <td>2020-05-11</td>
      <td>0.0</td>
      <td>-200</td>
    </tr>
    <tr>
      <th>8</th>
      <td>Nokia Corp</td>
      <td>0HAF.L</td>
      <td>FI0009000681</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>LSE</td>
      <td>FI25</td>
      <td>3327.0</td>
      <td>3327.0</td>
      <td>0.0</td>
      <td>BelowLIS</td>
      <td>Off-Book (On-Exchange)</td>
      <td>LSE</td>
      <td>2020-05-11</td>
      <td>0.0</td>
      <td>-100</td>
    </tr>
    <tr>
      <th>9</th>
      <td>Nokia Corp</td>
      <td>0HAF.L</td>
      <td>FI0009000681</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>LSE</td>
      <td>FI25</td>
      <td>3327.0</td>
      <td>3327.0</td>
      <td>0.0</td>
      <td>BelowLIS</td>
      <td>Off-Book (On-Exchange)</td>
      <td>LSE</td>
      <td>2020-05-11</td>
      <td>0.0</td>
      <td>-50</td>
    </tr>
    <tr>
      <th>10</th>
      <td>Nokia Corp</td>
      <td>0HAF.L</td>
      <td>FI0009000681</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>LSE</td>
      <td>FI25</td>
      <td>3327.0</td>
      <td>3327.0</td>
      <td>0.0</td>
      <td>BelowLIS</td>
      <td>Off-Book (On-Exchange)</td>
      <td>LSE</td>
      <td>2020-05-11</td>
      <td>0.0</td>
      <td>-25</td>
    </tr>
    <tr>
      <th>11</th>
      <td>Nokia Corp</td>
      <td>0HAF.L</td>
      <td>FI0009000681</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>LSE</td>
      <td>FI25</td>
      <td>3327.0</td>
      <td>3327.0</td>
      <td>0.0</td>
      <td>BelowLIS</td>
      <td>Off-Book (On-Exchange)</td>
      <td>LSE</td>
      <td>2020-05-11</td>
      <td>0.0</td>
      <td>-10</td>
    </tr>
    <tr>
      <th>12</th>
      <td>Nokia Corp</td>
      <td>0HAF.L</td>
      <td>FI0009000681</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>LSE</td>
      <td>FI25</td>
      <td>3327.0</td>
      <td>3327.0</td>
      <td>0.0</td>
      <td>BelowLIS</td>
      <td>Off-Book (On-Exchange)</td>
      <td>LSE</td>
      <td>2020-05-11</td>
      <td>0.0</td>
      <td>-5</td>
    </tr>
  </tbody>
</table>
</div>



### 1.12 Volume profiles


```r
query_name = 'lcGetVolumeProfile'
query_params = c(xythub.toParameter("isins", 'FI0009000681'),
                 xythub.toParameter("dateFrom", xythub.toDateValue('2020-05-10')),
                 xythub.toParameter("dateTo", xythub.toDateValue('2020-05-20'))
                 )

lcGetVolumeProfile = xythub.getData(source = "LC", endpoint = query_name, parameters = query_params)
lcGetVolumeProfile$data %>% head()
```


<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>group</th>
      <th>date</th>
      <th>stock</th>
      <th>market_category</th>
      <th>traded_value_in_eur</th>
      <th>symbol</th>
      <th>isin</th>
      <th>turnover_outside_regular_hours</th>
      <th>value_traded_above_lisin_eur</th>
      <th>exchange</th>
      <th>...</th>
      <th>currency</th>
      <th>trade_count</th>
      <th>turnover_of_trades_above_listhreshold_regular_hours</th>
      <th>threshold</th>
      <th>trade_time</th>
      <th>traded_value_in_local_ccy</th>
      <th>capped</th>
      <th>local_code</th>
      <th>trade_category</th>
      <th>trade_size</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Cboe Europe</td>
      <td>2020-05-11</td>
      <td>Nokia Corp</td>
      <td>On Exchange</td>
      <td>10495.5088</td>
      <td>NOKIAh.BTE</td>
      <td>FI0009000681</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>Cboe BXE</td>
      <td>...</td>
      <td>EUR</td>
      <td>1</td>
      <td>0.0</td>
      <td>650000.0</td>
      <td>01:00:00</td>
      <td>10495.5088</td>
      <td>0</td>
      <td>NOKIAh</td>
      <td>Off-Book (On-Exchange)</td>
      <td>3182</td>
    </tr>
    <tr>
      <th>1</th>
      <td>NordicOmx</td>
      <td>2020-05-11</td>
      <td>Nokia Corp</td>
      <td>Off Exchange</td>
      <td>11883.3000</td>
      <td>NOKIA.HE</td>
      <td>FI0009000681</td>
      <td>11883.3</td>
      <td>0.0</td>
      <td>NordicOmx</td>
      <td>...</td>
      <td>EUR</td>
      <td>2</td>
      <td>0.0</td>
      <td>650000.0</td>
      <td>07:05:00</td>
      <td>11883.3000</td>
      <td>0</td>
      <td>NOKIA</td>
      <td>Off-Exchange/OTC</td>
      <td>3575</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Sigma X MTF</td>
      <td>2020-05-11</td>
      <td>Nokia Corp</td>
      <td>On Exchange</td>
      <td>8016.2187</td>
      <td>NOKIAh.SGMX</td>
      <td>FI0009000681</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>Sigma X MTF</td>
      <td>...</td>
      <td>EUR</td>
      <td>1</td>
      <td>0.0</td>
      <td>650000.0</td>
      <td>07:10:00</td>
      <td>8016.2187</td>
      <td>0</td>
      <td>NOKIAh</td>
      <td>Lit/Auction Periodic</td>
      <td>2401</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Cboe Europe</td>
      <td>2020-05-11</td>
      <td>Nokia Corp</td>
      <td>On Exchange</td>
      <td>31047.2271</td>
      <td>NOKIAh.BTE</td>
      <td>FI0009000681</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>Cboe BXE</td>
      <td>...</td>
      <td>EUR</td>
      <td>5</td>
      <td>0.0</td>
      <td>650000.0</td>
      <td>08:00:00</td>
      <td>31047.2271</td>
      <td>0</td>
      <td>NOKIAh</td>
      <td>Lit/Auction Periodic</td>
      <td>9338</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Cboe Europe</td>
      <td>2020-05-11</td>
      <td>Nokia Corp</td>
      <td>On Exchange</td>
      <td>26081.3380</td>
      <td>NOKIAh.BTE</td>
      <td>FI0009000681</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>Cboe BXE</td>
      <td>...</td>
      <td>EUR</td>
      <td>4</td>
      <td>0.0</td>
      <td>650000.0</td>
      <td>08:00:00</td>
      <td>26081.3380</td>
      <td>0</td>
      <td>NOKIAh</td>
      <td>Dark/Order Book</td>
      <td>7825</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 21 columns</p>
</div>



### 1.13 Volume profiles for specified index


```r
query_name = 'lcGetVolumeProfileForIndex'
query_params = c(xythub.toParameter("dateFrom", xythub.toDateValue('2020-05-10')),
                 xythub.toParameter("dateTo", xythub.toDateValue('2020-05-11')),
                 xythub.toParameter(name = "index", value = "UK100"),
                 xythub.toParameter(name = "mode", value = c("asof", "ISINCurrency"))
                 )

lcGetVolumeProfileForIndex = xythub.getData(source = "LC", endpoint = query_name, parameters = query_params)
lcGetVolumeProfileForIndex$data %>% head()
```


<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>stock</th>
      <th>market_category</th>
      <th>traded_value_in_eur</th>
      <th>symbol</th>
      <th>isin</th>
      <th>turnover_outside_regular_hours</th>
      <th>value_traded_above_lisin_eur</th>
      <th>exchange</th>
      <th>addressable_traded_value_in_eur</th>
      <th>currency</th>
      <th>...</th>
      <th>trade_count</th>
      <th>threshold</th>
      <th>trade_time</th>
      <th>traded_value_in_local_ccy</th>
      <th>capped</th>
      <th>local_code</th>
      <th>trade_category</th>
      <th>trade_size</th>
      <th>group</th>
      <th>date</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Coca Cola HBC AG</td>
      <td>On Exchange</td>
      <td>765.758161</td>
      <td>CCHl.BTE</td>
      <td>CH0198251305</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>Cboe BXE</td>
      <td>765.758161</td>
      <td>GBX</td>
      <td>...</td>
      <td>1</td>
      <td>42540000.0</td>
      <td>01:00:00</td>
      <td>67290.9984</td>
      <td>0</td>
      <td>CCHl</td>
      <td>Off-Book (On-Exchange)</td>
      <td>36</td>
      <td>Cboe Europe</td>
      <td>2020-05-11</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Coca Cola HBC AG</td>
      <td>On Exchange</td>
      <td>11238.179232</td>
      <td>CCHl.CHIX</td>
      <td>CH0198251305</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>Cboe CXE</td>
      <td>11238.179232</td>
      <td>GBX</td>
      <td>...</td>
      <td>2</td>
      <td>42540000.0</td>
      <td>08:00:00</td>
      <td>987555.0000</td>
      <td>0</td>
      <td>CCHl</td>
      <td>Lit/Order Book</td>
      <td>515</td>
      <td>Cboe Europe</td>
      <td>2020-05-11</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Coca Cola HBC AG</td>
      <td>Off Exchange</td>
      <td>1185.695590</td>
      <td>CCHl.BXTR</td>
      <td>CH0198251305</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>Cboe TDM</td>
      <td>1185.695590</td>
      <td>GBX</td>
      <td>...</td>
      <td>1</td>
      <td>42540000.0</td>
      <td>08:00:00</td>
      <td>104193.0000</td>
      <td>0</td>
      <td>CCHl</td>
      <td>Off-Exchange/SI</td>
      <td>54</td>
      <td>Cboe TDM</td>
      <td>2020-05-11</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Coca Cola HBC AG</td>
      <td>On Exchange</td>
      <td>2035.152205</td>
      <td>CCHl.AQX</td>
      <td>CH0198251305</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>Aquis Exchange</td>
      <td>2035.152205</td>
      <td>GBX</td>
      <td>...</td>
      <td>1</td>
      <td>42540000.0</td>
      <td>08:00:00</td>
      <td>178839.0000</td>
      <td>0</td>
      <td>CCHl</td>
      <td>Lit/Order Book</td>
      <td>93</td>
      <td>Aquis Exchange</td>
      <td>2020-05-11</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Coca Cola HBC AG</td>
      <td>Off Exchange</td>
      <td>1097.758179</td>
      <td>CCH.L</td>
      <td>CH0198251305</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>LSE</td>
      <td>1097.758179</td>
      <td>GBX</td>
      <td>...</td>
      <td>11</td>
      <td>42540000.0</td>
      <td>08:00:00</td>
      <td>96465.5000</td>
      <td>0</td>
      <td>CCH</td>
      <td>Off-Exchange/OTC</td>
      <td>50</td>
      <td>LSE</td>
      <td>2020-05-11</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 21 columns</p>
</div>



### 1.14 Spread capacity


```r
query_name = 'lcGetSpreadsCapacity'
query_params = c(xythub.toParameter("isins", 'FI0009000681'),
                 xythub.toParameter("dateFrom", xythub.toDateValue('2020-05-10')),
                 xythub.toParameter("dateTo", xythub.toDateValue('2020-05-20'))
                 )

lcGetSpreadsCapacity = xythub.getData(source = "LC", endpoint = query_name, parameters = query_params)
lcGetSpreadsCapacity$data %>% head()
```


<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>group</th>
      <th>bid_value</th>
      <th>ask_value</th>
      <th>bps</th>
      <th>date</th>
      <th>ob_presence_ask</th>
      <th>ob_presence_bid</th>
      <th>stock</th>
      <th>symbol</th>
      <th>isin</th>
      <th>bid_size</th>
      <th>ask_size</th>
      <th>spread_presence_bid</th>
      <th>spread_presence_ask</th>
      <th>exchange</th>
      <th>currency</th>
      <th>local_code</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Cboe Europe</td>
      <td>3673.206656</td>
      <td>4351.787397</td>
      <td>0</td>
      <td>2020-05-11</td>
      <td>0.999999</td>
      <td>0.999999</td>
      <td>Nokia Corp, FI0009000681</td>
      <td>NOKIAh.BTE</td>
      <td>FI0009000681</td>
      <td>1108.434401</td>
      <td>1311.849966</td>
      <td>0.999999</td>
      <td>0.999999</td>
      <td>Cboe BXE</td>
      <td>EUR</td>
      <td>NOKIAh</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Cboe Europe</td>
      <td>12614.172456</td>
      <td>10776.046678</td>
      <td>0</td>
      <td>2020-05-11</td>
      <td>0.999987</td>
      <td>0.999988</td>
      <td>Nokia Corp, FI0009000681</td>
      <td>NOKIAm.BTE</td>
      <td>FI0009000681</td>
      <td>3809.521764</td>
      <td>3244.436326</td>
      <td>0.999975</td>
      <td>0.999975</td>
      <td>Cboe BXE</td>
      <td>EUR</td>
      <td>NOKIAm</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Cboe Europe</td>
      <td>4846.146065</td>
      <td>6450.678880</td>
      <td>10</td>
      <td>2020-05-11</td>
      <td>0.999999</td>
      <td>0.999999</td>
      <td>Nokia Corp, FI0009000681</td>
      <td>NOKIAh.BTE</td>
      <td>FI0009000681</td>
      <td>1463.064035</td>
      <td>1945.327991</td>
      <td>0.881725</td>
      <td>0.881725</td>
      <td>Cboe BXE</td>
      <td>EUR</td>
      <td>NOKIAh</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Cboe Europe</td>
      <td>21882.427680</td>
      <td>25980.451846</td>
      <td>25</td>
      <td>2020-05-11</td>
      <td>0.999999</td>
      <td>0.999999</td>
      <td>Nokia Corp, FI0009000681</td>
      <td>NOKIAh.BTE</td>
      <td>FI0009000681</td>
      <td>6603.265135</td>
      <td>7830.678886</td>
      <td>0.990054</td>
      <td>0.990054</td>
      <td>Cboe BXE</td>
      <td>EUR</td>
      <td>NOKIAh</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Cboe Europe</td>
      <td>9410.905243</td>
      <td>7605.484221</td>
      <td>25</td>
      <td>2020-05-11</td>
      <td>0.999987</td>
      <td>0.999988</td>
      <td>Nokia Corp, FI0009000681</td>
      <td>NOKIAm.BTE</td>
      <td>FI0009000681</td>
      <td>2840.318208</td>
      <td>2287.924193</td>
      <td>0.131529</td>
      <td>0.131529</td>
      <td>Cboe BXE</td>
      <td>EUR</td>
      <td>NOKIAm</td>
    </tr>
  </tbody>
</table>
</div>



### 1.15 EBBO Size contribution


```r
query_name = 'lcGetEbboSizeContribution'
query_params = c(xythub.toParameter("isins", 'FI0009000681'),
                 xythub.toParameter("dateFrom", xythub.toDateValue('2020-05-10')),
                 xythub.toParameter("dateTo", xythub.toDateValue('2020-05-20'))
                 )

lcGetEbboSizeContribution = xythub.getData(source = "LC", endpoint = query_name, parameters = query_params)
lcGetEbboSizeContribution$data %>% head()
```


<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>stock</th>
      <th>symbol</th>
      <th>isin</th>
      <th>size_contribution_ebbo_both</th>
      <th>presence_ebb</th>
      <th>twa_ebovalue</th>
      <th>exchange</th>
      <th>index</th>
      <th>size_contribution_ebb</th>
      <th>currency</th>
      <th>...</th>
      <th>size_contribution_ebbo_any_side</th>
      <th>primary_exchange_code</th>
      <th>presence_ebo</th>
      <th>presence_ebbo_both</th>
      <th>local_code</th>
      <th>combined_quote_presence</th>
      <th>twa_ebbvalue</th>
      <th>group</th>
      <th>size_contribution_ebo</th>
      <th>date</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Nokia Corp, FI0009000681</td>
      <td>NOKIA.HE</td>
      <td>FI0009000681</td>
      <td>0.514782</td>
      <td>0.835671</td>
      <td>14570.35642</td>
      <td>NordicOmx</td>
      <td>FI25</td>
      <td>0.536410</td>
      <td>EUR</td>
      <td>...</td>
      <td>0.481538</td>
      <td>H</td>
      <td>0.915177</td>
      <td>0.760185</td>
      <td>NOKIA</td>
      <td>0.999973</td>
      <td>18040.695813</td>
      <td>NordicOmx</td>
      <td>0.604039</td>
      <td>2020-05-11</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Nokia Corp, FI0009000681</td>
      <td>NOKIAh.AQX</td>
      <td>FI0009000681</td>
      <td>0.373753</td>
      <td>0.710271</td>
      <td>14570.35642</td>
      <td>Aquis Exchange</td>
      <td>FI25</td>
      <td>0.402656</td>
      <td>EUR</td>
      <td>...</td>
      <td>0.337863</td>
      <td>H</td>
      <td>0.677287</td>
      <td>0.431337</td>
      <td>NOKIAh</td>
      <td>0.999973</td>
      <td>18040.695813</td>
      <td>Aquis Exchange</td>
      <td>0.466299</td>
      <td>2020-05-11</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Nokia Corp, FI0009000681</td>
      <td>NOKIAh.BTE</td>
      <td>FI0009000681</td>
      <td>0.096149</td>
      <td>0.318471</td>
      <td>14570.35642</td>
      <td>Cboe BXE</td>
      <td>FI25</td>
      <td>0.116584</td>
      <td>EUR</td>
      <td>...</td>
      <td>0.070660</td>
      <td>H</td>
      <td>0.157997</td>
      <td>0.049779</td>
      <td>NOKIAh</td>
      <td>0.999973</td>
      <td>18040.695813</td>
      <td>Cboe Europe</td>
      <td>0.136368</td>
      <td>2020-05-11</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Nokia Corp, FI0009000681</td>
      <td>NOKIAh.CHIX</td>
      <td>FI0009000681</td>
      <td>0.211549</td>
      <td>0.675756</td>
      <td>14570.35642</td>
      <td>Cboe CXE</td>
      <td>FI25</td>
      <td>0.309743</td>
      <td>EUR</td>
      <td>...</td>
      <td>0.182713</td>
      <td>H</td>
      <td>0.425488</td>
      <td>0.271610</td>
      <td>NOKIAh</td>
      <td>0.999973</td>
      <td>18040.695813</td>
      <td>Cboe Europe</td>
      <td>0.232612</td>
      <td>2020-05-11</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Nokia Corp, FI0009000681</td>
      <td>NOKIAh.TU</td>
      <td>FI0009000681</td>
      <td>0.110936</td>
      <td>0.173654</td>
      <td>14570.35642</td>
      <td>Turquoise</td>
      <td>FI25</td>
      <td>0.111161</td>
      <td>EUR</td>
      <td>...</td>
      <td>0.073420</td>
      <td>H</td>
      <td>0.090484</td>
      <td>0.017105</td>
      <td>NOKIAh</td>
      <td>0.999973</td>
      <td>18040.695813</td>
      <td>LSE</td>
      <td>0.120023</td>
      <td>2020-05-11</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 21 columns</p>
</div>


### 2. Index analysis - DE30
---

DE30 Index analysis

### 2.0. Index compositions


```r
query_name = "getIndexCompositions"
query_params = xythub.toParameter(name="date", value=as.Date("2020-10-30"))

indexcomp = xythub.getData(source="LC", endpoint = query_name, parameters = query_params)$data
indices = str_c(indexcomp$index %>% unique(), collapse = ", ")
print(glue("Available indices: {indices}"))

de30_idx_comp = indexcomp %>% filter(index == 'DE30')
de30_idx_comp
```

    Available indices: AT20, BE20, CH20, CHM30, DE30, DEM50, DES50, DK25, EP50, EPAC, ES35, FI25, FR40, FRM20, IE20, IT40, NL25, NLM25, NO25, PT20, SE30, UK100, UK250



<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>isin</th>
      <th>cusip</th>
      <th>index</th>
      <th>name</th>
      <th>currency</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>90</th>
      <td>DE000A1EWWW0</td>
      <td>ADSd</td>
      <td>DE30</td>
      <td>ADIDAS AG</td>
      <td>EUR</td>
    </tr>
    <tr>
      <th>91</th>
      <td>DE0008404005</td>
      <td>ALVd</td>
      <td>DE30</td>
      <td>ALLIANZ SE</td>
      <td>EUR</td>
    </tr>
    <tr>
      <th>92</th>
      <td>DE000BASF111</td>
      <td>BASd</td>
      <td>DE30</td>
      <td>BASF SE</td>
      <td>EUR</td>
    </tr>
    <tr>
      <th>93</th>
      <td>DE000BAY0017</td>
      <td>BAYNd</td>
      <td>DE30</td>
      <td>BAYER AG</td>
      <td>EUR</td>
    </tr>
    <tr>
      <th>94</th>
      <td>DE0005200000</td>
      <td>BEId</td>
      <td>DE30</td>
      <td>BEIERSDORF AG</td>
      <td>EUR</td>
    </tr>
    <tr>
      <th>95</th>
      <td>DE0005190003</td>
      <td>BMWd</td>
      <td>DE30</td>
      <td>BAYERISCHE MOTOREN WERKE AG</td>
      <td>EUR</td>
    </tr>
    <tr>
      <th>96</th>
      <td>DE0005439004</td>
      <td>CONd</td>
      <td>DE30</td>
      <td>CONTINENTAL AG</td>
      <td>EUR</td>
    </tr>
    <tr>
      <th>97</th>
      <td>DE0007100000</td>
      <td>DAId</td>
      <td>DE30</td>
      <td>DAIMLER AG</td>
      <td>EUR</td>
    </tr>
    <tr>
      <th>98</th>
      <td>DE0005810055</td>
      <td>DB1d</td>
      <td>DE30</td>
      <td>DEUTSCHE BOERSE AG</td>
      <td>EUR</td>
    </tr>
    <tr>
      <th>99</th>
      <td>DE0005140008</td>
      <td>DBKd</td>
      <td>DE30</td>
      <td>DEUTSCHE BANK AG</td>
      <td>EUR</td>
    </tr>
    <tr>
      <th>100</th>
      <td>DE0005552004</td>
      <td>DPWd</td>
      <td>DE30</td>
      <td>DEUTSCHE POST AG</td>
      <td>EUR</td>
    </tr>
    <tr>
      <th>101</th>
      <td>DE0005557508</td>
      <td>DTEd</td>
      <td>DE30</td>
      <td>DEUTSCHE TELEKOM AG</td>
      <td>EUR</td>
    </tr>
    <tr>
      <th>102</th>
      <td>DE000A0HN5C6</td>
      <td>DWNId</td>
      <td>DE30</td>
      <td>DEUTSCHE WOHNEN SE</td>
      <td>EUR</td>
    </tr>
    <tr>
      <th>103</th>
      <td>DE000ENAG999</td>
      <td>EOANd</td>
      <td>DE30</td>
      <td>EON SE</td>
      <td>EUR</td>
    </tr>
    <tr>
      <th>104</th>
      <td>DE0005785802</td>
      <td>FMEd</td>
      <td>DE30</td>
      <td>FRESENIUS MEDICAL CARE AG &amp; CO KGAA</td>
      <td>EUR</td>
    </tr>
    <tr>
      <th>105</th>
      <td>DE0005785604</td>
      <td>FREd</td>
      <td>DE30</td>
      <td>FRESENIUS SE &amp; CO KGAA</td>
      <td>EUR</td>
    </tr>
    <tr>
      <th>106</th>
      <td>DE0006047004</td>
      <td>HEId</td>
      <td>DE30</td>
      <td>HEIDELBERGCEMENT AG</td>
      <td>EUR</td>
    </tr>
    <tr>
      <th>107</th>
      <td>DE0006048432</td>
      <td>HEN3d</td>
      <td>DE30</td>
      <td>HENKEL AG &amp; CO KGAA PREF</td>
      <td>EUR</td>
    </tr>
    <tr>
      <th>108</th>
      <td>DE0008402215</td>
      <td>HNR1d</td>
      <td>DE30</td>
      <td>HANNOVER RUCK SE</td>
      <td>EUR</td>
    </tr>
    <tr>
      <th>109</th>
      <td>DE0006231004</td>
      <td>IFXd</td>
      <td>DE30</td>
      <td>INFINEON TECHNOLOGIES AG</td>
      <td>EUR</td>
    </tr>
    <tr>
      <th>110</th>
      <td>DE000KBX1006</td>
      <td>KBXd</td>
      <td>DE30</td>
      <td>KNORR-BREMSE AG</td>
      <td>EUR</td>
    </tr>
    <tr>
      <th>111</th>
      <td>DE0006599905</td>
      <td>MRKd</td>
      <td>DE30</td>
      <td>MERCK KGAA</td>
      <td>EUR</td>
    </tr>
    <tr>
      <th>112</th>
      <td>DE000A0D9PT0</td>
      <td>MTXd</td>
      <td>DE30</td>
      <td>MTU AERO ENGINES AG</td>
      <td>EUR</td>
    </tr>
    <tr>
      <th>113</th>
      <td>DE0008430026</td>
      <td>MUV2d</td>
      <td>DE30</td>
      <td>MUNICH REINSURANCE COMPANY</td>
      <td>EUR</td>
    </tr>
    <tr>
      <th>114</th>
      <td>DE0007037129</td>
      <td>RWEd</td>
      <td>DE30</td>
      <td>RWE AG</td>
      <td>EUR</td>
    </tr>
    <tr>
      <th>115</th>
      <td>DE0007164600</td>
      <td>SAPd</td>
      <td>DE30</td>
      <td>SAP SE</td>
      <td>EUR</td>
    </tr>
    <tr>
      <th>116</th>
      <td>DE0007236101</td>
      <td>SIEd</td>
      <td>DE30</td>
      <td>SIEMENS AG</td>
      <td>EUR</td>
    </tr>
    <tr>
      <th>117</th>
      <td>DE000A1ML7J1</td>
      <td>VNAd</td>
      <td>DE30</td>
      <td>VONOVIA SE</td>
      <td>EUR</td>
    </tr>
    <tr>
      <th>118</th>
      <td>DE0007664039</td>
      <td>VOW3d</td>
      <td>DE30</td>
      <td>VOLKSWAGEN AG PREF</td>
      <td>EUR</td>
    </tr>
    <tr>
      <th>119</th>
      <td>DE0007472060</td>
      <td>WDId</td>
      <td>DE30</td>
      <td>WIRECARD AG</td>
      <td>EUR</td>
    </tr>
  </tbody>
</table>
</div>



### 2.1. Daily market statistics for DE30 Index


```r
query_name = "lcGetMarketShare"
query_params = c(xythub.toParameter("isins", de30_idx_comp$isin),
                 xythub.toParameter("dateFrom", xythub.toDateValue('2020-05-10')),
                 xythub.toParameter("dateTo", xythub.toDateValue('2020-05-20'))
                 )

de30_market_shares = xythub.getData(source="LC", endpoint = query_name, parameters = query_params)$data
de30_market_shares %>% head()
```


<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>value_traded_above_lisin_eur</th>
      <th>exchange</th>
      <th>addressable_traded_value_in_eur</th>
      <th>currency</th>
      <th>turnover_of_trades_above_listhreshold_regular_hours</th>
      <th>trade_count</th>
      <th>threshold</th>
      <th>traded_value_in_local_ccy</th>
      <th>capped</th>
      <th>local_code</th>
      <th>trade_category</th>
      <th>trade_size</th>
      <th>group</th>
      <th>date</th>
      <th>stock</th>
      <th>market_category</th>
      <th>traded_value_in_eur</th>
      <th>symbol</th>
      <th>isin</th>
      <th>turnover_outside_regular_hours</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0.0</td>
      <td>Aquis Exchange</td>
      <td>4.145326e+04</td>
      <td>EUR</td>
      <td>0.0</td>
      <td>12</td>
      <td>650000.0</td>
      <td>4.145326e+04</td>
      <td>0</td>
      <td>DBKd</td>
      <td>Lit/Auction Periodic</td>
      <td>6255</td>
      <td>Aquis Exchange</td>
      <td>2019-09-02</td>
      <td>Deutsche Bank AG, DE0005140008</td>
      <td>On Exchange</td>
      <td>4.145326e+04</td>
      <td>DBKd.AQX</td>
      <td>DE0005140008</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>0.0</td>
      <td>Aquis Exchange</td>
      <td>2.205208e+06</td>
      <td>EUR</td>
      <td>0.0</td>
      <td>504</td>
      <td>650000.0</td>
      <td>2.205208e+06</td>
      <td>0</td>
      <td>DBKd</td>
      <td>Lit/Order Book</td>
      <td>332580</td>
      <td>Aquis Exchange</td>
      <td>2019-09-02</td>
      <td>Deutsche Bank AG, DE0005140008</td>
      <td>On Exchange</td>
      <td>2.205208e+06</td>
      <td>DBKd.AQX</td>
      <td>DE0005140008</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>0.0</td>
      <td>Aquis Exchange</td>
      <td>4.608193e+04</td>
      <td>EUR</td>
      <td>0.0</td>
      <td>21</td>
      <td>650000.0</td>
      <td>4.608193e+04</td>
      <td>0</td>
      <td>DBKd</td>
      <td>Lit/Auction Periodic</td>
      <td>6944</td>
      <td>Aquis Exchange</td>
      <td>2019-09-03</td>
      <td>Deutsche Bank AG, DE0005140008</td>
      <td>On Exchange</td>
      <td>4.608193e+04</td>
      <td>DBKd.AQX</td>
      <td>DE0005140008</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>0.0</td>
      <td>Aquis Exchange</td>
      <td>4.270761e+06</td>
      <td>EUR</td>
      <td>0.0</td>
      <td>1106</td>
      <td>650000.0</td>
      <td>4.270761e+06</td>
      <td>0</td>
      <td>DBKd</td>
      <td>Lit/Order Book</td>
      <td>643907</td>
      <td>Aquis Exchange</td>
      <td>2019-09-03</td>
      <td>Deutsche Bank AG, DE0005140008</td>
      <td>On Exchange</td>
      <td>4.270761e+06</td>
      <td>DBKd.AQX</td>
      <td>DE0005140008</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>0.0</td>
      <td>Aquis Exchange</td>
      <td>6.164211e+04</td>
      <td>EUR</td>
      <td>0.0</td>
      <td>3</td>
      <td>650000.0</td>
      <td>6.164211e+04</td>
      <td>0</td>
      <td>DBKd</td>
      <td>Lit/Auction Closing</td>
      <td>9143</td>
      <td>Aquis Exchange</td>
      <td>2019-09-04</td>
      <td>Deutsche Bank AG, DE0005140008</td>
      <td>On Exchange</td>
      <td>6.164211e+04</td>
      <td>DBKd.AQX</td>
      <td>DE0005140008</td>
      <td>0.0</td>
    </tr>
  </tbody>
</table>
</div>


{% include links.html %}

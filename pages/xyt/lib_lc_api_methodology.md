---
title: LC API methodology
keywords: python, sample, data access, xythub, xyt hub, xyt-hub, big xyt, big-xyt, documentation, API documentation, API, single API, tick data, methodology
summary: "This is the description of methodology that is used to calculate columns avaiabe in xyt hub Python package."
sidebar: xyt_sidebar
permalink: lib_lc_api_methodology.html
folder: xyt
---

## Spread ask

Time-weighted average distance between hypothetical ask and mid expressed in basis points after simulation of trade execution with given notional value in €. Calculated with millisecond resolution in the situation when mid exists and there is sufficient depth in the orderbook.

## Spread bid

Time-weighted average distance between hypothetical bid and mid expressed in basis points after simulation of trade execution with given notional value in €. Calculated with millisecond resolution in the situation when mid exists and there is sufficient depth in the orderbook.

## Spread both

Time-weighted distance between hypothetical bid and ask expressed in basis points after simulation of trade execution with given notional value in € on both sides of orderbook simultaneously. Calculated with millisecond resolution in the situation when there is sufficient depth in the orderbook on both sides of orderbook simultaneously.

## Fill presence (ask/bid/both)

Time period when given simulated transaction value can be fully executed, which means there is sufficient orderbook (either bid/ask or both simultaneously) depth in relation to time period when orderbook exists (limited to continuous session only).

## Levels ask

Time-weighted average number of levels on ask side consumed by simulated trade execution with given notional value in €. Calculated with millisecond resolution in the situation when there is sufficient depth in the orderbook.

## Levels bid

Time-weighted average number of levels on bid side consumed by simulated trade execution with given notional value in €. Calculated with millisecond resolution in the situation when there is sufficient depth in the orderbook.

## Levels both

Time-weighted average number of levels on both orderbook sides simultaneously (taken as max in case of sides imbalance) consumed by simulated trade execution with given notional value in €. Calculated with millisecond resolution in the situation when there is sufficient depth in the orderbook.

## Ob peresence (ask/bid/both)

Time period when orderbook (either bid/ask or both simultaneously) is not empty in relation to continuous session time period. Measure is the same across all sizes for given instrument/day.

## Presence ebb (Presence on European Best Bid)

Time period (in millisecond resolution) that particular product/exchange has the bid not worse than corresponding products/exchanges in the relation to time period that combined bid across all exchanges (EBB) exists.

## Presence ebb exclusive

Time period (in millisecond resolution) that particular product/exchange has the best (highest) bid across corresponding products/exchanges (and there is no other exchange on this level) in the relation to time period that combined bid across all exchanges (EBB) exists.

## Presence ebo (Presence on European Best Offer)

Time period (in millisecond resolution) that particular product/exchange has ask not worse than corresponding products/exchanges in the relation to time period that combined ask (offer) across all exchanges (EBO) exist.

## Presence ebo exclusive

Time period (in millisecond resolution) that particular product/exchange has the best (lowest) ask across corresponding products/exchanges (and there is no other exchange on this level) in the relation to time period that combined ask across all exchanges (EBO) exists.

## Combined ask presence

Time period (in millisecond resolution) that combined ask price (EBO) exists in relation to regular trading time period.

## Combined bid presence

Time period (in millisecond resolution) that combined bid price (EBB) exists in relation to regular trading time period.

## Size

Value thresholds used to filter quotes among exchanges.
Quotes having below conditions fulfilled are set to invalid:
- $$(\text{Bid} \cdot \text{BidSize}) \leq \text{rate} \cdot \text{size}$$,
- $$(\text{Ask} \cdot \text{AskSize}) \leq \text{rate} \cdot \text{size}$$,

where size is given transaction value and rate ensures conversion to listing (instrument group) currency based on ECB eod rates.


{% include links.html %}

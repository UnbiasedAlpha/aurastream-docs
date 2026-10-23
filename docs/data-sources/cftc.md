# CFTC Commitments of Traders

Weekly positioning in US futures markets from the Commodity Futures Trading Commission: who holds each contract (hedgers, swap dealers, hedge funds, small traders), back to 2006.

## Source ID

```json
"source": "cftc"
```

## Credentials

**Not required.**

## Ticker Format

Common futures symbols, or any CFTC contract market code:

| Example | Market |
|---|---|
| `GC`, `SI`, `HG`, `PL`, `PA` | Gold, silver, copper, platinum, palladium |
| `CL`, `NG`, `RB`, `HO` | Crude oil, natural gas, gasoline, heating oil |
| `ZC`, `ZS`, `ZW` | Corn, soybeans, wheat |
| `KC`, `SB`, `CC` | Coffee, sugar, cocoa |
| `LE`, `HE` | Live cattle, lean hogs |
| `YM`, `RTY`, `VX` | Dow, Russell 2000, VIX futures |
| `ZT`, `ZF`, `ZN`, `ZB`, `SR3` | Treasury futures, SOFR |
| `6E`, `6J`, `6B`, `6A`, `6C`, `6S`, `6M`, `DX` | Currency futures, dollar index |
| `BTC`, `ETH` | CME crypto futures |
| `088691` | Any CFTC contract market code |

Any other symbol is sent to the CFTC as a contract market code.

## Timing

Positions are as of Tuesday and published Friday at 15:30 New York. By default each report is stamped at its publication time (in UTC), so it can be used in backtests without look-ahead. The original Tuesday date is in the `report_date` field. In holiday weeks the CFTC publishes a day or two later than this rule.

## Fields

Each row carries every numeric column of the chosen report (long, short and spread positions by trader group, changes, percentages of open interest, trader counts) under the CFTC's own column names, plus `report_date`. The `interval` parameter is ignored: the data is weekly.

## Parameters

| Parameter | Values | Default | Description |
|---|---|---|---|
| `report` | `"disaggregated"`, `"tff"`, `"legacy"` | `"tff"` for financial symbols, `"disaggregated"` otherwise | Disaggregated for commodities, Traders in Financial Futures for financials |
| `options` | `true`, `false` | `false` | Include options (futures-equivalent) |
| `as_of` | `"release"`, `"report"` | `"release"` | Stamp at Friday publication or at Tuesday position date |

## Example

```python
response = requests.post(URL, headers=HEADERS, json={
    "tickerlist": ["GC", "ZN"],
    "start": "2020-01-01",
    "end": "2025-01-01",
    "interval": "1w",
    "source": "cftc"
})
```

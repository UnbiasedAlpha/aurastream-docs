# CFTC Commitments of Traders

Weekly positioning in US futures markets from the Commodity Futures Trading Commission: how much hedgers, swap dealers, hedge funds and small traders hold in each contract. Back to 2006.

**Source:** `cftc` | **Credentials:** Not required

## Example

```json
{
    "tickerlist": [
        "GC",
        "ES"
    ],
    "start": "2020-01-01",
    "end": "2025-01-01",
    "interval": "1w",
    "source": "cftc"
}
```

## Parameters

| Parameter | Type | Required | Description |
|---|---|---|---|
| `report` | `str` | No | `"disaggregated"` (commodities), `"tff"` (financial futures) or `"legacy"`. Default: `"tff"` for financial symbols, `"disaggregated"` otherwise |
| `options` | `bool` | No | Include options, futures-equivalent (default: `false`) |
| `as_of` | `str` | No | `"release"` (default): stamped at Friday 15:30 New York publication. `"report"`: stamped at the Tuesday position date |

## Ticker Format

Futures symbols or any CFTC contract market code:

| Ticker | Market |
|---|---|
| `GC`, `SI`, `HG`, `PL`, `PA` | Gold, silver, copper, platinum, palladium |
| `CL`, `NG`, `RB`, `HO` | Crude oil, natural gas, gasoline, heating oil |
| `ZC`, `ZS`, `ZW`, `KC`, `SB`, `CC`, `LE`, `HE` | Grains, softs, livestock |
| `ES`, `NQ`, `YM`, `RTY`, `VX` | Equity index futures, VIX |
| `ZT`, `ZF`, `ZN`, `ZB`, `SR3` | Treasury futures, SOFR |
| `6E`, `6J`, `6B`, `6A`, `6C`, `6S`, `6M`, `DX` | Currency futures, dollar index |
| `BTC`, `ETH` | CME crypto futures |
| `088691` | Any CFTC contract market code |

## Notes

- Each row carries every position column of the report (up to ~180 fields) plus `report_date`.
- With the default `as_of="release"` the data can be used in backtests without look-ahead. In holiday weeks the CFTC publishes a day or two later than the Friday rule.

# BEA National Accounts

US personal consumption expenditures (PCE), the PCE price index (the Fed's inflation gauge), personal income and saving, and GDP, from the Bureau of Economic Analysis.

**Source:** `bea` | **Credentials:** Required ([free key](https://apps.bea.gov/API/signup/))

## Example

```json
{
    "tickerlist": ["pce", "pce_real", "core_pce_price", "saving_rate"],
    "start": "2023-01-01",
    "end": "2026-12-31",
    "interval": "1m",
    "source": "bea",
    "credentials": {
        "api_key": "YOUR_BEA_KEY"
    }
}
```

## Ticker Format

A name below, or any line of a BEA national accounts (NIPA) table as `<table>:<line>`, e.g. `T20805:3`. Table names and line numbers are in BEA's [interactive tables](https://apps.bea.gov/iTable/?reqid=19&step=4&isuri=1&nipa_table_list=1&categories=flatfiles).

| Ticker | Series | Frequency |
|---|---|---|
| `pce` | Personal consumption expenditures, current dollars, seasonally adjusted annual rate | Monthly |
| `pce_goods`, `pce_durables`, `pce_nondurables`, `pce_services` | PCE by type of product | Monthly |
| `pce_real` | Real PCE, chained dollars | Monthly |
| `pce_price`, `core_pce_price` | PCE price index; excluding food and energy | Monthly |
| `personal_income`, `disposable_income` | Personal income; disposable personal income | Monthly |
| `saving_rate` | Personal saving as a percent of disposable income | Monthly |
| `gdp` | GDP, current dollars, seasonally adjusted annual rate | Quarterly |
| `gdp_real_growth` | Real GDP, percent change at an annual rate | Quarterly |

## Parameters

| Parameter | Type | Required | Description |
|---|---|---|---|
| `frequency` | `str` | No | `"M"`, `"Q"` or `"A"`; default is the series' own (monthly, or quarterly for GDP). Use it for `<table>:<line>` tickers from quarterly or annual tables |

## Fields

One row per period, stamped at the first day of the month, quarter or year:

| Field | Description |
|---|---|
| `value` | Value as BEA publishes it |
| `unit` | BEA's unit, e.g. `Level` or `Percent` |
| `unit_mult` | Power of ten the value is expressed in: `6` = millions of dollars (PCE, income, GDP), `0` = as is (indexes, percentages) |
| `line` | BEA's description of the line |

## Notes

- Values are the latest estimates. BEA revises recent months with each release and older data in its annual updates, so these are not the figures first published. `econcalendar` (`bea`) gives the release times.
- PCE for a month is published at the end of the following month (Personal Income and Outlays release); GDP about a month after the quarter.
- BEA allows 100 requests a minute per key.
- This product uses the Bureau of Economic Analysis (BEA) Data API but is not endorsed or certified by BEA.

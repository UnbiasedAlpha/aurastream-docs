# US Treasury Fiscal Data

Government cash, debt and auction results from the US Treasury.

## Source ID

```json
"source": "treasury"
```

## Credentials

**Not required.**

## Ticker Format

| Ticker | Description |
|---|---|
| `tga` | Treasury General Account at the Fed (`tga_open`, `tga_close`, `deposits`, `withdrawals`), daily, USD millions. A falling balance adds liquidity to markets. |
| `debt_to_penny` | Total public debt outstanding, daily, USD |
| `auctions` | Auction results, one row per security (bid-to-cover, high yield, allotments). Filter by term, e.g. `auctions:10-Year` |
| `avg_interest_rates` | Average interest rate on outstanding debt by security type, monthly |
| any endpoint path | e.g. `v1/accounting/mts/mts_table_1`, from [Fiscal Data](https://fiscaldata.treasury.gov/api-documentation/), returned with its own columns. The endpoint must have a `record_date` field |

Daily Treasury Statement data for a date is published around 16:00 New York the next business day. Before April 2021 the statement reported the Federal Reserve Account instead of the TGA, so `tga` only has `tga_open` and `tga_close` for those dates.

Except for `tga`, rows that share a date (several auctions on one day, one row per security type in `avg_interest_rates`) are separated by a microsecond in the timestamp. The `interval` parameter is ignored.

## Example

```python
response = requests.post(URL, headers=HEADERS, json={
    "tickerlist": ["tga", "auctions:10-Year"],
    "start": "2022-01-01",
    "end": "2025-01-01",
    "interval": "1d",
    "source": "treasury"
})
```

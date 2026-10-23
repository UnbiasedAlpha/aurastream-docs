# US Treasury

Government cash, debt and auction results from the US Treasury Fiscal Data API.

**Source:** `treasury` | **Credentials:** Not required

## Example

```json
{
    "tickerlist": [
        "tga",
        "auctions:10-Year"
    ],
    "start": "2022-01-01",
    "end": "2025-01-01",
    "interval": "1d",
    "source": "treasury"
}
```

## Ticker Format

| Ticker | Description |
|---|---|
| `tga` | Treasury General Account at the Fed: `tga_open`, `tga_close`, `deposits`, `withdrawals` (USD millions, daily). A falling balance adds liquidity to markets |
| `debt_to_penny` | Total public debt outstanding (daily) |
| `auctions`, `auctions:10-Year` | Auction results, one row per security (bid-to-cover, high yield, allotments); optional term filter |
| `avg_interest_rates` | Average interest rate on outstanding debt (monthly) |
| endpoint path | Any [Fiscal Data](https://fiscaldata.treasury.gov/api-documentation/) endpoint, e.g. `v1/accounting/mts/mts_table_1` |

## Notes

- Daily Treasury Statement data for a date is published around 16:00 New York the next business day.
- Auctions on the same day are separated by a microsecond in the timestamp.

# FINRA Short Interest

Short positions and daily short-sale volume for US stocks, from the Financial Industry Regulatory Authority.

**Source:** `finra` | **Credentials:** Not required

## Example

```json
{
    "tickerlist": [
        "AAPL",
        "GME"
    ],
    "start": "2023-01-01",
    "end": "2024-01-01",
    "interval": "1d",
    "source": "finra",
    "dataset": "short_interest"
}
```

## Parameters

| Parameter | Type | Required | Description |
|---|---|---|---|
| `dataset` | `str` | No | `"short_interest"` (default, twice a month) or `"short_volume"` (daily) |

## Ticker Format

US stock symbols: `AAPL`, `TSLA`, `GME`

## Fields

- **`short_interest`**: `short_interest`, `previous_short_interest`, `change`, `change_pct`, `avg_daily_volume`, `days_to_cover`, stamped at the settlement date.
- **`short_volume`**: `short_volume`, `short_exempt_volume`, `total_volume`, `short_ratio`, from the consolidated Reg SHO file published each evening.

!!! warning "Publication lag"
    FINRA publishes short interest about 7 business days after the settlement date. Shift it before using it in a backtest.

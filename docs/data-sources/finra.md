# FINRA Short Interest

Short positions and daily short-sale volume for US stocks, from the Financial Industry Regulatory Authority.

## Source ID

```json
"source": "finra"
```

## Credentials

**Not required.**

## Ticker Format

US stock symbols, e.g. `AAPL`, `TSLA`, `GME`.

## Parameters

| Parameter | Values | Default | Description |
|---|---|---|---|
| `dataset` | `"short_interest"`, `"short_volume"` | `"short_interest"` | Twice-monthly short interest, or daily short-sale volume |

**`short_interest`** returns `short_interest`, `previous_short_interest`, `change`, `change_pct`, `avg_daily_volume` and `days_to_cover`, stamped at the settlement date.

!!! warning "Publication lag"
    FINRA publishes short interest about 7 business days after the settlement date. Shift it before using it in a backtest.

**`short_volume`** returns `short_volume`, `short_exempt_volume`, `total_volume` and `short_ratio` per day, from the consolidated Reg SHO file published each evening.

## Example

```python
response = requests.post(URL, headers=HEADERS, json={
    "tickerlist": ["AAPL", "GME"],
    "start": "2023-01-01",
    "end": "2024-01-01",
    "interval": "1d",
    "source": "finra",
    "dataset": "short_interest"
})
```

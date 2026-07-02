# Finnhub

Company financials and market data.

**Source:** `finnhub` | **Credentials:** Required

## Credentials

```json
{"credentials": {"key": "YOUR_FINNHUB_KEY"}}
```

## Example

```json
{
    "tickerlist": ["AAPL"],
    "start": "2021-01-01",
    "end": "2025-01-10",
    "interval": "1d",
    "source": "finnhub",
    "credentials": {"key": "YOUR_KEY"}
}
```

## Ticker Format

Standard US symbols: `AAPL`, `MSFT`, `GOOGL`

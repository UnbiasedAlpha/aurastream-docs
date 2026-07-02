# Polygon

US equities, options, forex, and crypto data.

**Source:** `polygon` | **Credentials:** Required

## Credentials

```json
{"credentials": {"key": "YOUR_POLYGON_KEY"}}
```

## Example

```json
{
    "tickerlist": ["AAPL", "MSFT"],
    "start": "2025-01-01",
    "end": "2025-06-01",
    "interval": "1d",
    "source": "polygon",
    "credentials": {"key": "YOUR_KEY"}
}
```

## Ticker Format

Standard US symbols: `AAPL`, `MSFT`, `GOOGL`

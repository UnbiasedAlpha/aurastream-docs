# FRED

Federal Reserve Economic Data — macroeconomic indicators.

**Source:** `fred` | **Credentials:** Required

## Credentials

```json
{"credentials": {"key": "YOUR_FRED_KEY"}}
```

## Example

```json
{
    "tickerlist": ["USACPIALLMINMEI"],
    "start": "2021-01-01",
    "end": "2025-01-10",
    "interval": "1d",
    "source": "fred",
    "credentials": {"key": "YOUR_KEY"}
}
```

## Ticker Format

FRED series IDs: `USACPIALLMINMEI` (CPI), `UNRATE` (Unemployment), `DFF` (Fed Funds Rate), `GDP`

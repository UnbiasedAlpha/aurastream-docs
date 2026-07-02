# Nasdaq Data Link

Economic and financial datasets.

**Source:** `nasdaq` | **Credentials:** Required

## Credentials

```json
{"credentials": {"key": "YOUR_NASDAQ_KEY"}}
```

## Example

```json
{
    "tickerlist": ["LBMA/GOLD"],
    "start": "2025-01-01",
    "end": "2025-06-01",
    "interval": "1d",
    "source": "nasdaq",
    "credentials": {"key": "YOUR_KEY"},
    "filters": {}
}
```

## Parameters

| Parameter | Type | Default | Description |
|---|---|---|---|
| `filters` | `dict` | `{}` | Additional data filters |

## Ticker Format

Nasdaq Data Link dataset codes: `LBMA/GOLD`, `FRED/GDP`

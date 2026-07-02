# TradeStation

US equities, futures, and options data.

**Source:** `tradestation` | **Credentials:** Required

## Credentials

```json
{
    "credentials": {
        "contact": "YOUR_CONTACT",
        "key": "YOUR_CLIENT_KEY",
        "secret": "YOUR_CLIENT_SECRET",
        "refresh_token": "YOUR_REFRESH_TOKEN",
        "account_id": "YOUR_ACCOUNT_ID"
    }
}
```

## Example

```json
{
    "tickerlist": ["AAPL"],
    "start": "2025-01-01",
    "end": "2025-06-01",
    "interval": "5",
    "source": "tradestation",
    "bartype": "Minute",
    "credentials": {
        "contact": "YOUR_CONTACT",
        "key": "YOUR_KEY",
        "secret": "YOUR_SECRET",
        "refresh_token": "YOUR_REFRESH_TOKEN",
        "account_id": "YOUR_ACCOUNT_ID"
    }
}
```

## Parameters

| Parameter | Type | Default | Description |
|---|---|---|---|
| `bartype` | `str` | `"Minute"` | Bar type (e.g., `"Minute"`, `"Daily"`) |

## Ticker Format

Standard US symbols: `AAPL`, `MSFT`, `ES`

!!! note
    The `interval` parameter is the bar size number (e.g., `"5"` for 5-minute bars when `bartype` is `"Minute"`).

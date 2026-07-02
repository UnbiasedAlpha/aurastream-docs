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
    "tickerlist": ["AAPL", "AMZN"],
    "start": "2023-01-01",
    "end": "2025-01-10",
    "interval": "1d",
    "source": "tradestation",
    "credentials": {
        "contact": "YOUR_CONTACT",
        "key": "YOUR_KEY",
        "secret": "YOUR_SECRET",
        "refresh_token": "YOUR_REFRESH_TOKEN",
        "account_id": "YOUR_ACCOUNT_ID"
    }
}
```

## Ticker Format

Standard US symbols: `AAPL`, `AMZN`, `MSFT`, `ES`

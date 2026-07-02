# City Index

CFD bars and tick data.

**Source:** `cityindex` | **Credentials:** Required

## Credentials

```json
{
    "credentials": {
        "username": "YOUR_USERNAME",
        "password": "YOUR_PASSWORD",
        "key": "YOUR_APP_KEY",
        "client_account_id": "YOUR_CLIENT_ACCOUNT_ID",
        "trading_account_id": "YOUR_TRADING_ACCOUNT_ID"
    }
}
```

## Examples

=== "Bars"

    ```json
    {
        "tickerlist": ["401204130"],
        "start": "2025-02-05",
        "end": "2025-02-10",
        "interval": "1d",
        "source": "cityindex",
        "credentials": {
            "username": "YOUR_USER",
            "password": "YOUR_PASS",
            "key": "YOUR_KEY",
            "client_account_id": "YOUR_CLIENT_ID",
            "trading_account_id": "YOUR_TRADING_ID"
        }
    }
    ```

=== "Tick"

    ```json
    {
        "tickerlist": ["401204130"],
        "start": "2025-05-12",
        "end": "2025-05-15",
        "interval": "1d",
        "source": "cityindex",
        "bartype": "tick",
        "credentials": {
            "username": "YOUR_USER",
            "password": "YOUR_PASS",
            "key": "YOUR_KEY",
            "client_account_id": "YOUR_CLIENT_ID",
            "trading_account_id": "YOUR_TRADING_ID"
        }
    }
    ```

## Parameters

| Parameter | Type | Default | Description |
|---|---|---|---|
| `bartype` | `str` | `"bar"` | Data type: `"bar"` for OHLCV bars, `"tick"` for tick data |
| `dtype` | `str` | `"MID"` | Price type: `"MID"`, `"BID"`, `"ASK"` |

## Ticker Format

City Index market IDs (numeric): `401204130`

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
        "tickerlist": ["401484347"],
        "start": "2025-01-01",
        "end": "2025-06-01",
        "interval": "1d",
        "source": "cityindex",
        "dtype": "bars",
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
        "tickerlist": ["401484347"],
        "start": "2025-06-01",
        "end": "2025-06-02",
        "interval": "1m",
        "source": "cityindex",
        "dtype": "tick",
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
| `dtype` | `str` | `"bars"` | Data type: `"bars"` or `"tick"` |

## Ticker Format

City Index market IDs (numeric): `401484347`

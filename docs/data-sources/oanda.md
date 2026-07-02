# Oanda

Forex, metals, CFDs, and order/position book data.

**Source:** `oanda` | **Credentials:** Required

## Credentials

```json
{
    "credentials": {
        "id": "YOUR_OANDA_ACCOUNT_ID",
        "token": "YOUR_OANDA_TOKEN",
        "environment": "practice"
    }
}
```

## Examples

=== "Bars"

    ```json
    {
        "tickerlist": ["EUR_USD", "GBP_USD"],
        "start": "2021-01-01",
        "end": "2025-01-10",
        "interval": "1d",
        "source": "oanda",
        "credentials": {
            "id": "YOUR_ACCOUNT_ID",
            "token": "YOUR_TOKEN",
            "environment": "practice"
        }
    }
    ```

=== "Order Book"

    ```json
    {
        "tickerlist": ["EUR_USD"],
        "start": "2025-01-01",
        "end": "2025-01-10",
        "interval": "1d",
        "source": "oanda",
        "dtype": "orderbook",
        "credentials": {
            "id": "YOUR_ACCOUNT_ID",
            "token": "YOUR_TOKEN",
            "environment": "practice"
        }
    }
    ```

=== "Position Book"

    ```json
    {
        "tickerlist": ["EUR_USD"],
        "start": "2025-01-01",
        "end": "2025-01-10",
        "interval": "1d",
        "source": "oanda",
        "dtype": "positionbook",
        "credentials": {
            "id": "YOUR_ACCOUNT_ID",
            "token": "YOUR_TOKEN",
            "environment": "practice"
        }
    }
    ```

## Parameters

| Parameter | Type | Default | Description |
|---|---|---|---|
| `dtype` | `str` | `"bars"` | Data type: `"bars"`, `"orderbook"`, `"positionbook"` |
| `price_type` | `str` | `"A"` | For bars: `"A"` (ask), `"B"` (bid), `"M"` (mid) |

## Ticker Format

Oanda instrument format with underscore: `EUR_USD`, `GBP_USD`, `XAU_USD`, `USD_JPY`

# CoinAPI

Crypto bars, trades, and order book data.

**Source:** `coinapi` | **Credentials:** Required

## Credentials

```json
{"credentials": {"key": "YOUR_COINAPI_KEY"}}
```

## Examples

=== "Bars"

    ```json
    {
        "tickerlist": ["BITSTAMP_SPOT_BTC_USD"],
        "start": "2025-01-01",
        "end": "2025-01-10",
        "interval": "1d",
        "source": "coinapi",
        "dtype": "bars",
        "credentials": {"key": "YOUR_KEY"}
    }
    ```

=== "Trades"

    ```json
    {
        "tickerlist": ["BITSTAMP_SPOT_BTC_USD"],
        "start": "2025-01-01",
        "end": "2025-01-10",
        "interval": "1d",
        "source": "coinapi",
        "dtype": "trades",
        "credentials": {"key": "YOUR_KEY"}
    }
    ```

=== "Order Book"

    ```json
    {
        "tickerlist": ["BITSTAMP_SPOT_BTC_USD"],
        "start": "2025-01-01",
        "end": "2025-01-10",
        "interval": "1d",
        "source": "coinapi",
        "dtype": "ob",
        "credentials": {"key": "YOUR_KEY"}
    }
    ```

## Parameters

| Parameter | Type | Default | Description |
|---|---|---|---|
| `dtype` | `str` | `"bars"` | Data type: `"bars"`, `"trades"`, `"ob"` (order book) |

## Ticker Format

CoinAPI symbol IDs: `BITSTAMP_SPOT_BTC_USD`, `COINBASE_SPOT_ETH_USD`

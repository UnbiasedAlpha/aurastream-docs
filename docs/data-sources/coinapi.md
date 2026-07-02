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
        "tickerlist": "KRAKENFTS_PERP_BTC_USD",
        "start": "2025-01-01",
        "end": "2025-02-01",
        "interval": "1d",
        "source": "coinapi",
        "credentials": {"key": "YOUR_KEY"}
    }
    ```

=== "Trades"

    ```json
    {
        "tickerlist": "KRAKENFTS_PERP_BTC_USD",
        "start": "2025-01-01",
        "end": "2025-01-02",
        "interval": "1d",
        "source": "coinapi",
        "dtype": "trades",
        "exchange": "KRAKENFTS",
        "instrument": "PERPETUAL",
        "credentials": {"key": "YOUR_KEY"}
    }
    ```

=== "Order Book"

    ```json
    {
        "tickerlist": "KRAKENFTS_PERP_BTC_USD",
        "start": "2025-01-01 00:00:00",
        "end": "2025-01-01 00:01:00",
        "interval": "1d",
        "source": "coinapi",
        "dtype": "orderbook",
        "exchange": "KRAKENFTS",
        "instrument": "PERPETUAL",
        "credentials": {"key": "YOUR_KEY"}
    }
    ```

## Parameters

| Parameter | Type | Default | Description |
|---|---|---|---|
| `dtype` | `str` | `"bars"` | Data type: `"bars"`, `"trades"`, `"orderbook"` |
| `exchange` | `str` | — | Exchange identifier for trades/orderbook (e.g., `"KRAKENFTS"`) |
| `instrument` | `str` | `"spot"` | Instrument type for trades/orderbook (e.g., `"PERPETUAL"`, `"spot"`) |

## Ticker Format

CoinAPI symbol IDs: `KRAKENFTS_PERP_BTC_USD`, `BITSTAMP_SPOT_BTC_USD`, `COINBASE_SPOT_ETH_USD`

!!! note
    `tickerlist` can be a single string or a list of strings for this source.

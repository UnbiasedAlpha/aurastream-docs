# Binance

Crypto spot and futures market data.

**Source:** `binance` | **Credentials:** Not required

## Examples

=== "Spot"

    ```json
    {
        "tickerlist": ["BTCUSDT", "ETHUSDT"],
        "start": "2025-01-01",
        "end": "2025-06-01",
        "interval": "1d",
        "source": "binance",
        "instrument": "spot"
    }
    ```

=== "Futures"

    ```json
    {
        "tickerlist": ["BTCUSDT", "ETHUSDT"],
        "start": "2025-01-01",
        "end": "2025-06-01",
        "interval": "1d",
        "source": "binance",
        "instrument": "futures"
    }
    ```

## Parameters

| Parameter | Type | Default | Description |
|---|---|---|---|
| `instrument` | `str` | `"spot"` | `"spot"` for spot markets, `"futures"` for futures |

## Ticker Format

Standard Binance pair symbols: `BTCUSDT`, `ETHUSDT`, `SOLUSDT`

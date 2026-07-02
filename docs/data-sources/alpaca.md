# Alpaca

US equities and crypto data — bars, trades, and quotes.

**Source:** `alpaca` | **Credentials:** Required

## Credentials

```json
{"credentials": {"key": "YOUR_ALPACA_KEY", "secret": "YOUR_ALPACA_SECRET"}}
```

## Examples

=== "Equity Bars"

    ```json
    {
        "tickerlist": ["AAPL", "TSLA"],
        "start": "2025-01-01",
        "end": "2025-06-01",
        "interval": "1d",
        "source": "alpaca",
        "dtype": "bars",
        "assetclass": "equity",
        "credentials": {"key": "YOUR_KEY", "secret": "YOUR_SECRET"}
    }
    ```

=== "Crypto Bars"

    ```json
    {
        "tickerlist": ["BTC/USD", "ETH/USD"],
        "start": "2025-01-01",
        "end": "2025-06-01",
        "interval": "1d",
        "source": "alpaca",
        "dtype": "bars",
        "assetclass": "crypto",
        "credentials": {"key": "YOUR_KEY", "secret": "YOUR_SECRET"}
    }
    ```

=== "Equity Trades"

    ```json
    {
        "tickerlist": ["AAPL"],
        "start": "2025-06-01",
        "end": "2025-06-02",
        "interval": "1m",
        "source": "alpaca",
        "dtype": "trades",
        "assetclass": "equity",
        "credentials": {"key": "YOUR_KEY", "secret": "YOUR_SECRET"}
    }
    ```

=== "Crypto Trades"

    ```json
    {
        "tickerlist": ["BTC/USD"],
        "start": "2025-06-01",
        "end": "2025-06-02",
        "interval": "1m",
        "source": "alpaca",
        "dtype": "trades",
        "assetclass": "crypto",
        "credentials": {"key": "YOUR_KEY", "secret": "YOUR_SECRET"}
    }
    ```

=== "Equity Quotes"

    ```json
    {
        "tickerlist": ["AAPL"],
        "start": "2025-06-01",
        "end": "2025-06-02",
        "interval": "1m",
        "source": "alpaca",
        "dtype": "quotes",
        "assetclass": "equity",
        "credentials": {"key": "YOUR_KEY", "secret": "YOUR_SECRET"}
    }
    ```

=== "Crypto Quotes"

    ```json
    {
        "tickerlist": ["BTC/USD"],
        "start": "2025-06-01",
        "end": "2025-06-02",
        "interval": "1m",
        "source": "alpaca",
        "dtype": "quotes",
        "assetclass": "crypto",
        "credentials": {"key": "YOUR_KEY", "secret": "YOUR_SECRET"}
    }
    ```

## Parameters

| Parameter | Type | Default | Description |
|---|---|---|---|
| `dtype` | `str` | `"bars"` | Data type: `"bars"`, `"trades"`, `"quotes"` |
| `assetclass` | `str` | `"equity"` | Asset class: `"equity"` or `"crypto"` |

## Ticker Format

- **Equities:** Standard US symbols: `AAPL`, `TSLA`, `MSFT`
- **Crypto:** Slash-separated pairs: `BTC/USD`, `ETH/USD`

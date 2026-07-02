# FirstRate

Historical data for stocks, ETFs, futures, FX, options, and crypto.

**Source:** `firstrate` | **Credentials:** Required

## Credentials

```json
{"credentials": {"key": "YOUR_FIRSTRATE_KEY"}}
```

## Examples

=== "Stock"

    ```json
    {
        "tickerlist": ["AAPL"],
        "start": "2025-01-01",
        "end": "2025-06-01",
        "interval": "1d",
        "source": "firstrate",
        "assetclass": "stock",
        "credentials": {"key": "YOUR_KEY"}
    }
    ```

=== "ETF"

    ```json
    {
        "tickerlist": ["SPY"],
        "start": "2025-01-01",
        "end": "2025-06-01",
        "interval": "1d",
        "source": "firstrate",
        "assetclass": "etf",
        "credentials": {"key": "YOUR_KEY"}
    }
    ```

=== "Futures (Continuous)"

    ```json
    {
        "tickerlist": ["ES"],
        "start": "2025-01-01",
        "end": "2025-06-01",
        "interval": "1d",
        "source": "firstrate",
        "assetclass": "futures",
        "continuous_contracts": true,
        "credentials": {"key": "YOUR_KEY"}
    }
    ```

=== "Futures (Non-Continuous)"

    ```json
    {
        "tickerlist": ["ESH25"],
        "start": "2025-01-01",
        "end": "2025-03-01",
        "interval": "1d",
        "source": "firstrate",
        "assetclass": "futures",
        "continuous_contracts": false,
        "credentials": {"key": "YOUR_KEY"}
    }
    ```

=== "FX"

    ```json
    {
        "tickerlist": ["EURUSD"],
        "start": "2025-01-01",
        "end": "2025-06-01",
        "interval": "1d",
        "source": "firstrate",
        "assetclass": "fx",
        "credentials": {"key": "YOUR_KEY"}
    }
    ```

=== "Options"

    ```json
    {
        "tickerlist": ["AAPL"],
        "start": "2025-01-01",
        "end": "2025-06-01",
        "interval": "1d",
        "source": "firstrate",
        "assetclass": "options",
        "credentials": {"key": "YOUR_KEY"}
    }
    ```

=== "Crypto"

    ```json
    {
        "tickerlist": ["BTCUSD"],
        "start": "2025-01-01",
        "end": "2025-06-01",
        "interval": "1d",
        "source": "firstrate",
        "assetclass": "crypto",
        "credentials": {"key": "YOUR_KEY"}
    }
    ```

## Parameters

| Parameter | Type | Default | Description |
|---|---|---|---|
| `assetclass` | `str` | — | Asset class: `"stock"`, `"etf"`, `"futures"`, `"fx"`, `"options"`, `"crypto"` |
| `continuous_contracts` | `bool` | `true` | For futures: use continuous contracts |

## Ticker Format

- **Stocks/ETFs:** Standard symbols: `AAPL`, `SPY`
- **Futures:** Root symbol for continuous (`ES`), contract code for specific (`ESH25`)
- **FX:** Concatenated pair: `EURUSD`, `GBPUSD`
- **Crypto:** Concatenated pair: `BTCUSD`, `ETHUSD`

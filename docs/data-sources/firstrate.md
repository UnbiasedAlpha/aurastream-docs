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
        "end": "2025-01-05",
        "interval": "1d",
        "source": "firstrate",
        "instrument": "stock",
        "credentials": {"key": "YOUR_KEY"}
    }
    ```

=== "ETF"

    ```json
    {
        "tickerlist": ["SPY"],
        "start": "2025-01-01",
        "end": "2025-02-01",
        "interval": "1d",
        "source": "firstrate",
        "instrument": "etf",
        "credentials": {"key": "YOUR_KEY"}
    }
    ```

=== "Futures (Continuous)"

    ```json
    {
        "tickerlist": ["ES"],
        "start": "2025-01-01",
        "end": "2025-02-01",
        "interval": "1d",
        "source": "firstrate",
        "instrument": "futures",
        "credentials": {"key": "YOUR_KEY"}
    }
    ```

=== "Futures (Non-Continuous)"

    ```json
    {
        "tickerlist": ["ES"],
        "start": "2025-01-01",
        "end": "2025-02-01",
        "interval": "1d",
        "source": "firstrate",
        "instrument": "futures",
        "continuous_contracts": false,
        "credentials": {"key": "YOUR_KEY"}
    }
    ```

=== "FX"

    ```json
    {
        "tickerlist": ["EURUSD"],
        "start": "2025-01-01",
        "end": "2025-02-01",
        "interval": "1d",
        "source": "firstrate",
        "instrument": "fx",
        "credentials": {"key": "YOUR_KEY"}
    }
    ```

=== "Options"

    ```json
    {
        "tickerlist": ["SPX"],
        "start": "2025-01-01",
        "end": "2025-01-02",
        "interval": "1d",
        "source": "firstrate",
        "instrument": "options",
        "credentials": {"key": "YOUR_KEY"}
    }
    ```

=== "Crypto"

    ```json
    {
        "tickerlist": ["BTC"],
        "start": "2025-01-01",
        "end": "2025-02-01",
        "interval": "1d",
        "source": "firstrate",
        "instrument": "crypto",
        "credentials": {"key": "YOUR_KEY"}
    }
    ```

## Parameters

| Parameter | Type | Default | Description |
|---|---|---|---|
| `instrument` | `str` | `"stock"` | Instrument type: `"stock"`, `"etf"`, `"futures"`, `"fx"`, `"options"`, `"crypto"` |
| `continuous_contracts` | `bool` | `true` | For futures: use continuous contracts |

## Ticker Format

- **Stocks/ETFs:** Standard symbols: `AAPL`, `SPY`
- **Futures:** Root symbol: `ES`, `NQ`, `ZW`
- **FX:** Concatenated pair: `EURUSD`, `GBPUSD`
- **Options:** Index/stock symbol: `SPX`, `AAPL`
- **Crypto:** Base symbol: `BTC`, `ETH`

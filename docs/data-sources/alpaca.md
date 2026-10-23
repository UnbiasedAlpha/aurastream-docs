# Alpaca

US equities and crypto market data from Alpaca Markets.

## Source ID

```json
"source": "alpaca"
```

## Credentials

```json
"credentials": {
    "key": "your-alpaca-api-key",
    "secret": "your-alpaca-secret-key"
}
```

Get your credentials at [Alpaca](https://alpaca.markets/).

## Ticker Format

US equity symbols, and slash-separated pairs for crypto (with `"assetclass": "crypto"`):

| Example | Instrument |
|---|---|
| `AAPL` | Apple Inc. |
| `MSFT` | Microsoft Corp. |
| `SPY` | S&P 500 ETF |
| `BTC/USD` | Bitcoin (crypto) |
| `ETH/USD` | Ethereum (crypto) |
| `BTC/USDT` | Bitcoin / Tether (crypto) |

## Supported Intervals

`1M`, `1h`, `1d`, `1w`, `1m`

Only single units are used: the number in front is ignored, so `15M` returns 1-minute bars. Resample on your side for other bar sizes.

Bar fields: `open`, `high`, `low`, `close`, `volume`, `trade_count`, `vwap`.

## Parameters

| Parameter | Values | Default | Description |
|---|---|---|---|
| `dtype` | `"bars"`, `"trades"`, `"quotes"` | `"bars"` | Data type |
| `assetclass` | `"equity"`, `"crypto"` | `"equity"` | Asset class |
| `feed` | `"iex"`, `"sip"` | `"iex"` | Stocks only. `"iex"` is IEX trades only (thin volume, and for most names no history before mid-2020). `"sip"` is the consolidated tape from 2016; historical SIP data works on any Alpaca account, only the last 15 minutes need a subscription, so AuraStream stops SIP requests 16 minutes before now |
| `adjustment` | `"raw"`, `"split"`, `"dividend"`, `"all"` | `"raw"` | Stock bars only: as traded, or adjusted for splits, dividends or both |

!!! info "Rate limits"
    Alpaca allows 200 API calls per minute. Aurastream retries rate-limited calls (HTTP 429) with exponential backoff; other errors, such as an unknown symbol, come back at once. Large ticker lists are batched (50 per request by default).

## Examples

### US equities (daily)

```python
response = requests.post(URL, headers=HEADERS, json={
    "tickerlist": ["AAPL", "MSFT", "GOOGL", "AMZN"],
    "start": "2025-01-01",
    "end": "2025-06-01",
    "interval": "1d",
    "source": "alpaca",
    "credentials": {
        "key": "YOUR_ALPACA_KEY",
        "secret": "YOUR_ALPACA_SECRET"
    }
})
```

### Consolidated tape, split and dividend adjusted

```python
response = requests.post(URL, headers=HEADERS, json={
    "tickerlist": ["AAPL"],
    "start": "2018-01-01",
    "end": "2025-01-01",
    "interval": "1d",
    "source": "alpaca",
    "feed": "sip",
    "adjustment": "all",
    "credentials": {
        "key": "YOUR_ALPACA_KEY",
        "secret": "YOUR_ALPACA_SECRET"
    }
})
```

### Trades

`"dtype": "trades"` returns every trade between `start` and `end` (`"quotes"` returns bid/ask quotes). Keep the range short: a liquid stock prints many thousands of trades a day.

```python
response = requests.post(URL, headers=HEADERS, json={
    "tickerlist": ["AAPL"],
    "start": "2025-01-09 14:30",
    "end": "2025-01-09 15:30",
    "interval": "1d",
    "source": "alpaca",
    "dtype": "trades",
    "credentials": {
        "key": "YOUR_ALPACA_KEY",
        "secret": "YOUR_ALPACA_SECRET"
    }
})
```

### Crypto via Alpaca

```python
response = requests.post(URL, headers=HEADERS, json={
    "tickerlist": ["BTC/USD", "ETH/USD"],
    "start": "2025-06-01",
    "end": "2025-06-30",
    "interval": "1h",
    "source": "alpaca",
    "assetclass": "crypto",
    "credentials": {
        "key": "YOUR_ALPACA_KEY",
        "secret": "YOUR_ALPACA_SECRET"
    }
})
```

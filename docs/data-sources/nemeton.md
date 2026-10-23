# Nemeton

Order book signals for crypto markets: depth, imbalance, spread and order-flow metrics computed from recorded order books.

Nemeton is a separate paid product from Unbiased Alpha ($250/month). Subscribing gives you a Nemeton API key, which you pass to Aurastream as the credential for this source.

## Source ID

```json
"source": "nemeton"
```

## Credentials

```json
"credentials": {
    "key": "your-nemeton-api-key"
}
```

The key is created when your Nemeton subscription starts and is removed when it ends. A missing or rejected key returns an error, not an empty result.

## Ticker Format

`{SYMBOL}_{EXCHANGE}_{INSTRUMENT}`

| Example | Market |
|---|---|
| `BTCUSDT_BINANCE_PERPETUAL` | Bitcoin / USDT perpetual on Binance |

## Supported Intervals

Minutes, hours and days (e.g. `1M`, `5M`, `15M`, `1h`, `4h`, `1d`). The interval is sent to Nemeton as a number of minutes.

Data starts on 2023-12-27. A request returns at most 10,000 bars: if the range is longer, `start` is moved forward so the response covers the 10,000 bars before `end`.

## Parameters

| Parameter | Values | Default | Description |
|---|---|---|---|
| `metric` | see list below | none (required) | Signal to return |
| `level` | integer | `1000` | Order book depth the metric is computed over |

Available metrics:

`cumulative-depth`, `depth-ratio`, `dollar-value-imbalance`, `dynamic-orderflow-imbalance`, `ivdepth`, `liquidity-consumption-rate`, `liquidity-imbalance`, `liquidity-spread-depth`, `maxdepth`, `mid`, `normalized-spread`, `orderbook-pressure`, `orderbook-slope`, `price-movement-potential`, `quantity-imbalance`, `quote-asymmetry`, `quote-concentration`, `quote-intensity`, `quote-skewness`, `spread`, `static-orderflow-imbalance`, `trade-imbalance`, `weighted-mid`

## Examples

### Daily cumulative depth

```python
response = requests.post(URL, headers=HEADERS, json={
    "tickerlist": ["BTCUSDT_BINANCE_PERPETUAL"],
    "start": "2026-01-01",
    "end": "2026-06-15 23:59",
    "interval": "1d",
    "source": "nemeton",
    "metric": "cumulative-depth",
    "level": 1,
    "credentials": {
        "key": "YOUR_NEMETON_KEY"
    }
})
```

### Hourly order book pressure

```python
response = requests.post(URL, headers=HEADERS, json={
    "tickerlist": ["BTCUSDT_BINANCE_PERPETUAL"],
    "start": "2026-09-01",
    "end": "2026-09-30",
    "interval": "1h",
    "source": "nemeton",
    "metric": "orderbook-pressure",
    "credentials": {
        "key": "YOUR_NEMETON_KEY"
    }
})
```

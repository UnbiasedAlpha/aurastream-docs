# KuCoin

OHLCV bars for KuCoin Futures contracts and KuCoin indices.

## Source ID

```json
"source": "kucoin"
```

## Credentials

**Not required.** Bars come from KuCoin Futures' public market data API.

## Ticker Format

KuCoin Futures symbols. Index symbols start with a dot:

| Example | Instrument |
|---|---|
| `XBTUSDTM` | Bitcoin / USDT perpetual |
| `ETHUSDTM` | Ether / USDT perpetual |
| `.KXBT` | KuCoin Bitcoin index |
| `.KETH` | KuCoin Ether index |

Spot pairs such as `BTC-USDT` are not supported. KuCoin uses `XBT` for Bitcoin in futures symbols.

## Supported Intervals

`1M`, `5M`, `15M`, `30M`, `1h`, `2h`, `4h`, `8h`, `12h`, `1d`, `1w`

## Fields

`Open`, `High`, `Low`, `Close`, `Volume`

## Parameters

No source-specific parameters.

## Examples

### Indices (daily)

```python
response = requests.post(URL, headers=HEADERS, json={
    "tickerlist": [".KXBT", ".KETH"],
    "start": "2025-01-01",
    "end": "2025-03-01",
    "interval": "1d",
    "source": "kucoin",
})
```

### Perpetual (hourly)

```python
response = requests.post(URL, headers=HEADERS, json={
    "tickerlist": ["XBTUSDTM"],
    "start": "2025-06-01",
    "end": "2025-06-07",
    "interval": "1h",
    "source": "kucoin",
})
```

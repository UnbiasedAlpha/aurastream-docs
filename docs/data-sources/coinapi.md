# CoinAPI

OHLCV bars for crypto spot and derivatives markets across hundreds of exchanges, from CoinAPI. You need your own CoinAPI key.

## Source ID

```json
"source": "coinapi"
```

## Credentials

```json
"credentials": {
    "key": "your-coinapi-key"
}
```

Get a key at [CoinAPI](https://www.coinapi.io/). Requests use your CoinAPI plan's credits.

## Ticker Format

CoinAPI symbol IDs, `EXCHANGE_TYPE_BASE_QUOTE`:

| Example | Instrument |
|---|---|
| `BITSTAMP_SPOT_BTC_USD` | Bitcoin / US dollar on Bitstamp |
| `COINBASE_SPOT_ETH_USD` | Ether / US dollar on Coinbase |
| `KRAKENFTS_PERP_BTC_USD` | Bitcoin perpetual on Kraken Futures |

## Supported Intervals

Seconds (`1s`), minutes (`1M`, `5M`, ...), hours (`1h`), days (`1d`), months (`1m`) and years (`1y`), in the multiples CoinAPI accepts. Bars are stamped at their close, in UTC.

## Fields

`price_open`, `price_high`, `price_low`, `price_close`, `volume_traded`, `trades_count`

## Parameters

No source-specific parameters for bars.

## Examples

```python
response = requests.post(URL, headers=HEADERS, json={
    "tickerlist": ["BITSTAMP_SPOT_BTC_USD", "COINBASE_SPOT_ETH_USD"],
    "start": "2025-01-01",
    "end": "2025-02-01",
    "interval": "1d",
    "source": "coinapi",
    "credentials": {"key": "your-coinapi-key"},
})
```

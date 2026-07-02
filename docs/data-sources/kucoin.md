# KuCoin

Crypto spot and index data.

**Source:** `kucoin` | **Credentials:** Not required

## Example

```json
{
    "tickerlist": [".KXBT", ".KETH"],
    "start": "2025-01-01",
    "end": "2025-01-10",
    "interval": "1d",
    "source": "kucoin"
}
```

## Ticker Format

KuCoin index tickers start with a dot: `.KXBT`, `.KETH`

Standard spot pairs: `BTC-USDT`, `ETH-USDT`

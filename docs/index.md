# Aurastream

**One endpoint. 37 data sources. Zero infrastructure.**

Aurastream is a unified financial data API. Instead of integrating with each exchange and data provider individually, make a single POST request to Aurastream and get OHLCV bars, trades, quotes, funding rates, economic indicators, and more.

---

## Why Aurastream?

| Without Aurastream | With Aurastream |
|---|---|
| Integrate each exchange API separately | One POST request for all sources |
| Handle each provider's rate limits, pagination, retries | Handled server-side |
| Run infrastructure to host data pipelines | We host it for you |
| Different response formats per provider | Consistent `{ticker: {timestamp: {OHLCV}}}` |

## Supported Data Sources

| Group | Sources |
|---|---|
| Crypto | Binance, Bybit, Kraken, Deribit, KuCoin, Hyperliquid, dYdX, Derive; with your own key: CoinAPI, CoinGecko; Nemeton order book signals |
| Equities, FX and commodities | Yahoo Finance; with your own account: Alpaca, Polygon, Oanda, FirstRate Data, TradeStation, City Index |
| Macro and energy | US Treasury, SDMX (BIS, ECB, IMF, OECD...), World Bank, US economic calendar; with a free key: FRED, EIA; with your own key: Nasdaq Data Link, Trading Economics |
| Company data and positioning | SEC EDGAR, CFTC; with your own key: Finnhub |
| Trade flows | UK, EU and Hong Kong trade; with a free key: US trade |
| Alternative data | DefiLlama, GDELT, Open-Meteo, TSA checkpoint numbers; with your own key: Visual Crossing |

See the [Data Sources overview](data-sources/overview.md) for each source's identifier and credentials.

## Quick Example

```python
import requests

URL = "https://api.aurastream.unbiased-alpha.com/timeseries"
HEADERS = {"x-api-key": "YOUR_AURASTREAM_API_KEY"}

response = requests.post(URL, headers=HEADERS, json={
    "tickerlist": ["BTCUSDT"],
    "start": "2025-01-01",
    "end": "2025-01-31",
    "interval": "1d",
    "source": "bybit",
})

data = response.json()
# {"BTCUSDT": {"2025-01-01 00:00:00": {"Open": ..., "High": ..., ...}, ...}}
```

## Get Started

1. [Sign up](https://aurastream.unbiased-alpha.com): new accounts start on the **Free** plan, 500 calls a month, no card needed
2. Follow the [Quickstart guide](getting-started/quickstart.md)
3. Explore the [API Reference](api-reference/timeseries.md)
4. Check [Plans & Limits](getting-started/plans-and-limits.md) when you need more calls

# Aurastream

**One endpoint. 26+ data sources. Zero infrastructure.**

Aurastream is a unified financial data API. Instead of integrating with each exchange and data provider individually, make a single POST request to Aurastream and get OHLCV bars, trades, quotes, funding rates, economic indicators, and more.

---

## Why Aurastream?

| Without Aurastream | With Aurastream |
|---|---|
| Integrate each exchange API separately | One POST request for all sources |
| Manage rate limits, pagination, retries | Handled server-side |
| Run infrastructure to host data pipelines | We host it for you |
| Different response formats per provider | Consistent `{ticker: {timestamp: {OHLCV}}}` |

## Supported Data Sources

### Crypto

| Source | Asset Classes | Credentials |
|---|---|---|
| [Binance](data-sources/binance.md) | Spot, Futures | No |
| [Bybit](data-sources/bybit.md) | Perpetuals | No |
| [Deribit](data-sources/deribit.md) | Derivatives | No |
| [dYdX](data-sources/dydx.md) | Perpetuals | No |
| [KuCoin](data-sources/kucoin.md) | Spot, Indices | No |
| [CoinGecko](data-sources/coingecko.md) | Market data | No |
| [CoinAPI](data-sources/coinapi.md) | Bars, Trades, Order book | Yes |
| [Hyperliquid](data-sources/hyperliquid.md) | Perpetuals | No |
| [Lyra](data-sources/lyra.md) | Options | No |

### Equities & Futures

| Source | Asset Classes | Credentials |
|---|---|---|
| [Yahoo Finance](data-sources/yahoo.md) | Equities, ETFs, Indices, Commodities | No |
| [Alpaca](data-sources/alpaca.md) | US Equities, Crypto (Bars, Trades, Quotes) | Yes |
| [Polygon](data-sources/polygon.md) | US Equities, Options, Forex, Crypto | Yes |
| [FirstRate](data-sources/firstrate.md) | Stocks, ETFs, Futures, FX, Options, Crypto | Yes |
| [TradeStation](data-sources/tradestation.md) | US Equities, Futures, Options | Yes |
| [Nasdaq Data Link](data-sources/nasdaq.md) | Economic datasets | Yes |

### Forex & CFDs

| Source | Asset Classes | Credentials |
|---|---|---|
| [Oanda](data-sources/oanda.md) | Forex, Metals, CFDs, Order/Position books | Yes |
| [Kraken](data-sources/kraken.md) | Crypto Futures | No |
| [City Index](data-sources/cityindex.md) | CFDs (Bars, Tick) | Yes |

### Macro & Economics

| Source | Asset Classes | Credentials |
|---|---|---|
| [FRED](data-sources/fred.md) | Economic indicators (CPI, GDP, Rates) | Yes |
| [Finnhub](data-sources/finnhub.md) | Company financials | Yes |
| [Trading Economics](data-sources/tradingeconomics.md) | Global economic indicators | Yes |
| [SDMX](data-sources/sdmx.md) | Central bank statistics (BIS, ECB) | No |

### Alternative Data

| Source | Asset Classes | Credentials |
|---|---|---|
| [Nemeton](data-sources/nemeton.md) | Crypto order book depth & imbalance | Yes |
| [Open-Meteo](data-sources/openmeteo.md) | Weather data | No |
| [Visual Crossing](data-sources/visualcrossing.md) | Historical weather | Yes |
| [Calendar](data-sources/calendar.md) | Earnings & economic events | No |

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

1. [Sign up](https://aurastream.unbiased-alpha.com) and get your API key
2. Follow the [Quickstart guide](getting-started/quickstart.md)
3. Explore the [API Reference](api-reference/timeseries.md)

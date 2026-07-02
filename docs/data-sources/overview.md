# Data Sources Overview

Aurastream supports 26+ data sources across multiple asset classes. Each source is accessed through the same `/timeseries` endpoint — just change the `source` parameter.

## All Sources

| Source | `source` value | Credentials | Asset Classes |
|---|---|---|---|
| [Binance](binance.md) | `binance` | No | Crypto Spot, Futures |
| [Bybit](bybit.md) | `bybit` | No | Crypto Perpetuals |
| [Deribit](deribit.md) | `deribit` | No | Crypto Derivatives |
| [dYdX](dydx.md) | `dydx` | No | Crypto Perpetuals |
| [KuCoin](kucoin.md) | `kucoin` | No | Crypto Spot, Indices |
| [CoinGecko](coingecko.md) | `coingecko` | No | Crypto Market Data |
| [CoinAPI](coinapi.md) | `coinapi` | Yes | Crypto Bars, Trades, Order Book |
| [Hyperliquid](hyperliquid.md) | `hyperliquid` | No | Crypto Perpetuals |
| [Lyra](lyra.md) | `lyra` | No | Crypto Options |
| [Yahoo Finance](yahoo.md) | `yahoo` | No | Equities, ETFs, Indices |
| [Alpaca](alpaca.md) | `alpaca` | Yes | US Equities, Crypto |
| [Polygon](polygon.md) | `polygon` | Yes | US Equities, Options, Forex |
| [FirstRate](firstrate.md) | `firstrate` | Yes | Stocks, ETFs, Futures, FX, Options, Crypto |
| [TradeStation](tradestation.md) | `tradestation` | Yes | US Equities, Futures, Options |
| [Nasdaq Data Link](nasdaq.md) | `nasdaq` | Yes | Economic Datasets |
| [Oanda](oanda.md) | `oanda` | Yes | Forex, Metals, CFDs |
| [Kraken](kraken.md) | `kraken` | No | Crypto Futures |
| [City Index](cityindex.md) | `cityindex` | Yes | CFDs |
| [FRED](fred.md) | `fred` | Yes | Economic Indicators |
| [Finnhub](finnhub.md) | `finnhub` | Yes | Company Financials |
| [Trading Economics](tradingeconomics.md) | `tradingeconomics` | Yes | Global Macro |
| [SDMX](sdmx.md) | `sdmx` | No | Central Bank Statistics |
| [Nemeton](nemeton.md) | `nemeton` | Yes | Crypto Order Book Metrics |
| [Open-Meteo](openmeteo.md) | `openmeteo` | No | Weather Data |
| [Visual Crossing](visualcrossing.md) | `visualcrossing` | Yes | Historical Weather |
| [Calendar](calendar.md) | `calendar` | No | Earnings & Economic Events |

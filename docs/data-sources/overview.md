# Data Sources Overview

Aurastream supports 38+ data sources across multiple asset classes. Each source is accessed through the same `/timeseries` endpoint — just change the `source` parameter.

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
| [US Treasury](treasury.md) | `treasury` | No | Government Cash (TGA), Debt, Auctions |
| [EIA](eia.md) | `eia` | Yes | Energy Inventories, Production, Prices |
| [World Bank Prices](worldbank.md) | `worldbank` | No | Monthly Commodity Prices |
| [UK Trade (HMRC)](hmrc.md) | `hmrc` | No | Trade Flows (gold, metals) |
| [EU Trade (Eurostat)](comext.md) | `comext` | No | Trade Flows (gold, metals) |
| [US Trade (Census)](uscensus.md) | `uscensus` | Yes | Trade Flows (gold, metals) |
| [Hong Kong Trade](hktrade.md) | `hktrade` | No | Trade Flows (gold, silver, PGMs to and from China) |
| [CFTC](cftc.md) | `cftc` | No | Futures Positioning |
| [FINRA](finra.md) | `finra` | No | Short Interest, Short Volume |
| [SEC EDGAR](edgar.md) | `edgar` | Contact | Insider Trades, Company Financials |
| [DefiLlama](defillama.md) | `defillama` | No | Stablecoins, DeFi TVL, DEX Volume |
| [GDELT](gdelt.md) | `gdelt` | No | News Coverage & Tone |

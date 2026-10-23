# Data Sources Overview

Aurastream serves historical data from 39 providers through a single API. Each source has its own ticker format, supported intervals, and credential requirements.

## Source Identifiers

Use these values in the `source` field of your request:

| `source` | Provider | Data | Credentials |
|---|---|---|---|
| **Crypto** | | | |
| `binance` | [Binance](binance.md) | Spot, USD-M perpetual futures, funding rates | Not required |
| `bybit` | [Bybit](bybit.md) | Perpetuals (linear and inverse) | Not required |
| `kraken` | [Kraken](kraken.md) | Futures, spot | Not required |
| `deribit` | [Deribit](deribit.md) | Futures, perpetuals, options | Not required |
| `kucoin` | [KuCoin](kucoin.md) | Futures contracts and indices | Not required |
| `nemeton` | [Nemeton](nemeton.md) | Order book signals (depth, imbalance, spread) | Nemeton subscription |
| `hyperliquid` | [Hyperliquid](hyperliquid.md) | Perpetuals | Not required |
| `dydx` | [dYdX](dydx.md) | v4 perpetuals | Not required |
| `lyra` | [Derive (Lyra)](lyra.md) | Options and perpetuals trades | Not required |
| `coinapi` | [CoinAPI](coinapi.md) | Bars from hundreds of crypto exchanges | Your CoinAPI key |
| `coingecko` | [CoinGecko](coingecko.md) | Coin prices, market caps, volumes | Your CoinGecko key (free Demo key works) |
| **Equities, FX and commodities** | | | |
| `yahoo` | [Yahoo Finance](yahoo.md) | Equities, ETFs, indices, futures, forex; earnings | Not required |
| `alpaca` | [Alpaca](alpaca.md) | US equities, crypto (bars, trades, quotes) | Your Alpaca account |
| `polygon` | [Polygon](polygon.md) | US equities, options, forex, crypto | Your Polygon key |
| `oanda` | [Oanda](oanda.md) | Forex, metals, CFDs; order and position books | Your Oanda account |
| `firstrate` | [FirstRate Data](firstrate.md) | Historical US stocks, ETFs, futures, FX, indices, crypto; option chains | Your FirstRate Data account |
| `tradestation` | [TradeStation](tradestation.md) | US stocks, ETFs, futures, indices | Your TradeStation account |
| `cityindex` | [City Index](cityindex.md) | CFDs: indices, FX, commodities, shares | Your City Index account |
| **Macro and energy** | | | |
| `fred` | [FRED](fred.md) | Economic indicators (CPI, GDP, rates) | Free FRED key |
| `eia` | [EIA](eia.md) | Energy inventories, production, prices | Free EIA key |
| `treasury` | [US Treasury](treasury.md) | Government cash (TGA), debt, auctions | Not required |
| `sdmx` | [SDMX](sdmx.md) | Central bank and statistics agency series (BIS, ECB, IMF, OECD...) | Not required |
| `worldbank` | [World Bank](worldbank.md) | Monthly commodity prices since 1960 | Not required |
| `nasdaq` | [Nasdaq Data Link](nasdaq.md) | Data tables | Your Nasdaq Data Link key |
| `tradingeconomics` | [Trading Economics](tradingeconomics.md) | Economic indicators by country | Your Trading Economics key |
| `bea` | [BEA National Accounts](bea.md) | Consumer spending (PCE), PCE inflation, income, saving, GDP | Free BEA key |
| `econcalendar` | [US Economic Calendar](econcalendar.md) | Release times: jobs, CPI, GDP, retail sales, FOMC | Contact email (for BLS) |
| **Company data and positioning** | | | |
| `finnhub` | [Finnhub](finnhub.md) | Company financials | Your Finnhub key |
| `edgar` | [SEC EDGAR](edgar.md) | Insider trades, company financials, earnings filings, fails-to-deliver, 13F holdings | Contact email |
| `cftc` | [CFTC](cftc.md) | Futures positioning (Commitments of Traders) | Not required |
| **Trade flows** | | | |
| `hmrc` | [UK Trade](hmrc.md) | UK imports and exports by product and country | Not required |
| `comext` | [EU Trade](comext.md) | EU imports and exports by product and country | Not required |
| `uscensus` | [US Census](uscensus.md) | US imports and exports by product and country; retail sales | Free Census key |
| `hktrade` | [Hong Kong Trade](hktrade.md) | Hong Kong gold, silver, PGM trade by country (China demand) | Not required |
| **Alternative data** | | | |
| `defillama` | [DefiLlama](defillama.md) | Stablecoin supply, DeFi TVL, DEX volume, fees | Not required |
| `gdelt` | [GDELT](gdelt.md) | News coverage and tone | Not required |
| `openmeteo` | [Open-Meteo](openmeteo.md) | Historical weather by location | Not required |
| `visualcrossing` | [Visual Crossing](visualcrossing.md) | Historical weather by location | Your Visual Crossing key |
| `tsa` | [TSA Checkpoint Numbers](tsa.md) | Daily US air travelers screened | Not required |

How to pass provider credentials is described in [Credentials](../getting-started/credentials.md).

## Choosing a Source

**Forex & Metals**: Use `oanda` with your own account for bid, ask and mid bars and Oanda's order and position books, or `yahoo` without an account.

**Crypto**: Use `binance`, `bybit` or `kraken` for exchange-native bars without credentials; `binance` also has funding rates, `deribit` covers options and `kucoin` its futures and indices. For on-chain perpetuals use `hyperliquid` or `dydx`; `coinapi` and `coingecko` cover many more exchanges and coins with your own key.

**US Equities**: Use `alpaca` or `polygon` with your own account for intraday bars, trades and quotes, or `yahoo` without an account.

**Futures History**: Use `firstrate` with your FirstRate Data account for long intraday histories of continuous or individual futures contracts.

**Economic Data**: Use `fred` for macro indicators across many countries, and `sdmx` for series published by central banks and statistics agencies. `econcalendar` gives the scheduled release times of the main US figures and FOMC decisions.

**Company Fundamentals**: Use `finnhub` for company metrics, and `edgar` for financials as reported to the SEC and the times earnings releases were filed.

**Positioning & Flows**: Use `cftc` for who holds each futures market, and `edgar` for insider purchases and sales, fails-to-deliver and institutional (13F) holdings.

**Energy & Liquidity**: Use `eia` for oil and gas inventories, and `treasury` for government cash and auctions, which drive dollar liquidity.

**Metals & Trade Flows**: Use `hmrc`, `comext`, `uscensus` and `hktrade` for gold, silver and base-metal shipments between countries (e.g. `gold:CH`), and `worldbank` for long monthly price histories.

**Crypto Fundamentals & News**: Use `defillama` for stablecoin supply and DeFi activity, and `gdelt` for news coverage and tone on any topic.

**Consumer Activity**: Use `uscensus` (`dataset: "retail"`) for monthly retail sales by store type, `bea` for personal consumption expenditures, income and saving, and `tsa` for daily US air travel. Consumer credit (Federal Reserve G.19) is in `fred` (`TOTALSL`, `REVOLSL`, `NONREVSL`). `econcalendar` gives the release times.

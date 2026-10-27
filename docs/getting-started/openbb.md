# OpenBB

The `openbb-aurastream` extension adds Aurastream data to the [OpenBB Platform](https://github.com/OpenBB-finance/OpenBB). It covers five datasets that no other OpenBB provider offers. Each one is a command under `obb.aurastream`, and the results come back as OpenBB objects (`.to_df()` and so on).

| Command | Data | Source page |
|---|---|---|
| `net_short` | EU net short positions of 0.5% or more, with holder names (AMF, AFM, CNMV) | [EU Net Short Positions](../data-sources/netshort.md) |
| `wasde` | USDA WASDE projections since July 2010, one row per report | [USDA WASDE](../data-sources/wasde.md) |
| `fred_vintages` | FRED series with every revision, stamped at the publication date | [FRED](../data-sources/fred.md) |
| `fails_to_deliver` | SEC fails-to-deliver for US stocks and ETFs | [SEC EDGAR](../data-sources/edgar.md) |
| `funding_rates` | Binance USD-M perpetual funding rates | [Binance](../data-sources/binance.md) |

The extension is open source (MIT): [github.com/UnbiasedAlpha/openbb-aurastream](https://github.com/UnbiasedAlpha/openbb-aurastream).

## 1. Install

Requires Python 3.10+.

```bash
pip install openbb openbb-aurastream
```

OpenBB rebuilds its Python interface the first time you import it after installing a new extension.

## 2. Set your keys

```python
from openbb import obb

obb.user.credentials.aurastream_api_key = "your-aurastream-key"
```

| Credential | Used for |
|---|---|
| `aurastream_api_key` | Required. Your Aurastream key from the [dashboard](https://aurastream.unbiased-alpha.com). |
| `aurastream_fred_api_key` | `fred_vintages`: your own free [FRED key](https://fred.stlouisfed.org/docs/api/api_key.html) |
| `aurastream_sec_user_agent` | `fails_to_deliver`: your name or company and a contact email, which the SEC asks of every automated request, e.g. `Jane Doe jane@example.com` |

To keep the keys between sessions, put them in `~/.openbb_platform/user_settings.json` under `credentials`.

## 3. Use it

```python
from openbb import obb

# Who is short Ubisoft, and by how much
shorts = obb.aurastream.net_short("ubisoft", start_date="2024-01-01", country="FR").to_df()

# How the USDA's projection of US corn ending stocks changed, report by report
corn = obb.aurastream.wasde("corn:ending_stocks", start_date="2024-01-01").to_df()
corn["revision"] = corn["value"] - corn["previous"]

# CPI as first published vs. after revisions
cpi = obb.aurastream.fred_vintages("CPIAUCSL", start_date="2020-01-01").to_df()
first_print = cpi.groupby("reference_date")["value"].first()
latest = cpi.groupby("reference_date")["value"].last()

# Fails-to-deliver in GameStop
obb.aurastream.fails_to_deliver("GME", start_date="2024-01-01", end_date="2025-01-01").to_df()

# Funding rates of the Bitcoin and Ether perpetuals
obb.aurastream.funding_rates("BTCUSDT,ETHUSDT", start_date="2025-06-01").to_df()
```

Every command takes a symbol (several, comma-separated, where it makes sense), plus `start_date` and `end_date`. Without `start_date`, it returns the last year. Each command call is one call against your [allowance](plans-and-limits.md).

For the other Aurastream sources, call the [`/timeseries`](../api-reference/timeseries.md) endpoint directly, or use the [MCP server](mcp.md) from Claude or ChatGPT.

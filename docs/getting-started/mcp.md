# Claude & ChatGPT (MCP)

The Aurastream MCP server lets AI assistants that support the [Model Context Protocol](https://modelcontextprotocol.io) (Claude Desktop, Claude Code, ChatGPT and others) fetch data from Aurastream themselves. You ask in plain language; the assistant picks the source, calls the [`/timeseries`](../api-reference/timeseries.md) endpoint and works with the result.

## Tools

| Tool | Description |
|---|---|
| `list_sources` | Sources, asset classes, ticker formats, intervals, data types, and whether provider credentials are configured. Does not use your allowance. |
| `get_timeseries` | Calls `/timeseries` and returns a compact table per ticker: the most recent rows, 100 by default, up to 1000. |
| `get_latest` | The most recent N bars per ticker, without picking dates. |

22 of the sources in [Data Sources](../data-sources/overview.md) are available:

| Group | Sources |
|---|---|
| Market data | Binance, Bybit, Kraken, Yahoo Finance; with your own account: Oanda, Alpaca, Polygon, FirstRate |
| Macro and energy | US Treasury, World Bank; with a free key: FRED, EIA |
| Fundamentals | SEC EDGAR; with your own key: Finnhub |
| Positioning and flows | CFTC, FINRA, UK Trade, EU Trade, Hong Kong Trade; with a free key: US Trade |
| Crypto fundamentals and news | DefiLlama, GDELT |

The server is open source (MIT): [github.com/UnbiasedAlpha/aurastream-mcp](https://github.com/UnbiasedAlpha/aurastream-mcp).

## 1. Install

Requires Python 3.10+.

```bash
pip install aurastream-mcp
```

This installs the `aurastream-mcp` command. Check it with `aurastream-mcp --help`.

## 2. Configure your keys

The server reads every key from environment variables. Provider credentials are never typed into the chat and never pass through the model.

| Variable | Used for |
|---|---|
| `AURASTREAM_API_KEY` | Required. Your Aurastream key from the [dashboard](https://aurastream.unbiased-alpha.com). |
| `OANDA_ACCOUNT_ID`, `OANDA_TOKEN` | Oanda |
| `OANDA_ENVIRONMENT` | Oanda, optional: `practice` (default) or `live` |
| `ALPACA_API_KEY`, `ALPACA_SECRET_KEY` | Alpaca |
| `POLYGON_API_KEY` | Polygon |
| `FRED_API_KEY` | FRED |
| `FINNHUB_API_KEY` | Finnhub |
| `FIRSTRATE_USER_ID` | FirstRate: the user ID FirstRate Data gives you for API downloads |
| `EIA_API_KEY` | EIA |
| `CENSUS_API_KEY` | US Trade |
| `EDGAR_USER_AGENT` | SEC EDGAR: your name and contact email, e.g. `Your Company contact@yourcompany.com` |

The server turns these into the `credentials` object described in [Credentials](credentials.md). Set only the ones you need.

## 3. Connect your assistant

=== "Claude Desktop"

    Open **Settings > Developer > Edit Config** and add to `claude_desktop_config.json`:

    ```json
    {
        "mcpServers": {
            "aurastream": {
                "command": "aurastream-mcp",
                "env": {
                    "AURASTREAM_API_KEY": "YOUR_AURASTREAM_API_KEY"
                }
            }
        }
    }
    ```

    Restart Claude Desktop. If the command is not found, use its full path.

=== "Claude Code"

    ```bash
    claude mcp add aurastream -e AURASTREAM_API_KEY=YOUR_AURASTREAM_API_KEY -- aurastream-mcp
    ```

    Add `-e FRED_API_KEY=...` and similar for sources that need credentials.

=== "ChatGPT"

    ChatGPT connects to remote MCP servers over HTTPS. Run the server in HTTP mode:

    ```bash
    AURASTREAM_API_KEY=YOUR_AURASTREAM_API_KEY aurastream-mcp --http --port 8000 --allowed-host mcp.example.com
    ```

    Expose it at a public HTTPS address (a reverse proxy with TLS, or a tunnel such as Cloudflare Tunnel or ngrok) and add `https://mcp.example.com/mcp` as a connector in ChatGPT.

    !!! warning "HTTP mode is for single-user self-hosting"
        A client that sends an `x-api-key` or `Authorization: Bearer` header uses its own key. Without one, the server falls back to `AURASTREAM_API_KEY`, so anyone with the URL uses your allowance and your provider credentials. Keep the URL private, or start the server with `--require-client-key`.

## 4. Ask for data

Some prompts to try:

- "What sources does Aurastream have for forex?"
- "Get daily BTCUSDT and ETHUSDT from Binance for 2025 and compute their 30-day rolling correlation."
- "Show the last 20 hourly bars of EUR_USD from Oanda."
- "Show Binance BTCUSDT perpetual funding rates for June 2025."
- "How has managed-money positioning in gold futures changed since 2020?"
- "Show monthly UK gold exports to Switzerland since 2020."

## Limits

- Historical data, not a real-time feed.
- Each data request is one API call and counts against your monthly allowance (free plan: 500 calls per month, see [Plans & Limits](plans-and-limits.md)). When the allowance runs out the assistant is told so; it resets on the 1st.
- Responses are capped per ticker so they fit in the assistant's context. For long, fine-grained histories, call the [API](../api-reference/timeseries.md) directly.

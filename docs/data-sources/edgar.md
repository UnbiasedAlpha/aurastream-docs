# SEC EDGAR

Insider trades (Form 4), planned insider sales (Form 144), reported company financials (XBRL), earnings releases and all other 8-K events, fails-to-deliver, 5%+ ownership stakes (Schedules 13D and 13G), institutional holdings (Form 13F), and mutual fund and ETF holdings (Form N-PORT) and proxy votes (Form N-PX) from the US Securities and Exchange Commission.

**Source:** `edgar` | **Credentials:** Contact string required by the SEC

## Example

```json
{
    "tickerlist": [
        "AAPL"
    ],
    "start": "2024-01-01",
    "end": "2025-01-01",
    "interval": "1d",
    "source": "edgar",
    "dataset": "insider",
    "credentials": {
        "user_agent": "Your Company contact@yourcompany.com"
    }
}
```

## Parameters

| Parameter | Type | Required | Description |
|---|---|---|---|
| `dataset` | `str` | No | `"insider"` (default, Form 4 trades), `"financials"` (reported figures), `"earnings"` (earnings release filings), `"events"` (all 8-K filings), `"ftd"` (fails-to-deliver), `"13dg"` (5%+ stakes), `"144"` (planned insider sales), `"13f"` (institutional holdings), `"nport"` (fund holdings) or `"npx"` (fund proxy votes) |
| `concepts` | `list[str]` | No | XBRL concepts for `financials`, e.g. `["Revenues", "CostOfGoldProductsAndServices"]`. Default: revenue, costs, income, EPS, cash flow, capex, cash, debt, equity, shares |
| `period` | `str` | No | `"quarter"` (default) or `"annual"` for `financials` |
| `top` | `int` | No | `nport` only: keep the largest N positions of each filing (default: all) |
| `items` | `list[str]` | No | `events` only: 8-K item numbers to keep, e.g. `["5.02"]` (executive changes) or `["1.03"]` (bankruptcy). Default: all |
| `issuer` | `str` | No | `npx` only: the company whose votes to return, as a CUSIP, ISIN or part of its name (`"Tesla"`). Without it, each filing is summarised |

## Ticker Format

US stock symbols (`AAPL`) or SEC CIK numbers (`320193`, `1164727` for Newmont).

- `ftd`: a symbol (`GME`) or a 9-character CUSIP.
- `13dg`, `144`, `events`: the company the filings are about.
- `13f`: the manager's CIK, e.g. `1067983` for Berkshire Hathaway. Find a manager's CIK with [EDGAR company search](https://www.sec.gov/edgar/searchedgar/companysearch).
- `nport`, `npx`: a mutual fund or ETF ticker (`VTSAX`, `IVV`, `ARKK`, `SPY`), or the fund's SEC series id (`S000002848`).

## Fields

**`insider`**: one row per Form 4 filing, stamped when it became public (SEC acceptance time):

| Field | Description |
|---|---|
| `owner`, `officer_title` | Who filed |
| `is_director`, `is_officer`, `is_ten_percent_owner` | Relationship to the company (1/0) |
| `buy_shares`, `buy_value` | Open-market purchases (code P) |
| `sell_shares`, `sell_value` | Open-market sales (code S) |
| `other_shares_net` | Net shares from grants, option exercises, gifts |
| `plan_10b5_1` | 1 if the trades were under a pre-arranged 10b5-1 plan |
| `shares_owned_after`, `transaction_date`, `n_transactions` | Holdings afterwards, first trade date, number of trades |

**`financials`**: one row per filing date, with each concept's value and its calendar period (e.g. `Revenues`, `Revenues_period: "CY2024Q2"`). Each value comes from the filing that first reported it, and the row is stamped at that filing's date: later filings that repeat or restate the figure do not move it.

**`earnings`**: one row per 8-K filing that reports results of operations (item 2.02), stamped when it became public. This is when a company's earnings release reached the SEC, usually minutes after the press release:

| Field | Description |
|---|---|
| `event_date` | Date of the event the 8-K reports (usually the release date) |
| `items` | All 8-K items in the filing, e.g. `2.02,9.01` |
| `form`, `accession`, `url` | Filing type (`8-K` or `8-K/A`), SEC accession number, link to the document |

**`events`**: one row per 8-K (and amendments), stamped when it became public:

| Field | Description |
|---|---|
| `items`, `item_names` | 8-K item numbers (e.g. `5.02,9.01`) and their descriptions (e.g. "Departure or appointment of directors or officers") |
| `event_date` | Date of the event reported |
| `form`, `accession`, `url` | `8-K` or `8-K/A`, SEC accession number, link to the document |

**`144`**: one row per Form 144, the notice an insider or affiliate files before selling restricted or control shares (usually on the day of the sale), stamped when it became public:

| Field | Description |
|---|---|
| `seller`, `relationship` | Who is selling (e.g. `Officer; Director`) |
| `shares`, `market_value`, `shares_outstanding` | Shares to be sold, their market value in US dollars, and the company's shares outstanding |
| `approx_sale_date`, `exchange`, `broker` | Planned sale date, exchange and broker |
| `acquired_how`, `acquired_date` | How and when the seller got the shares (e.g. restricted stock units) |
| `plan_10b5_1_adopted` | Date the seller adopted a 10b5-1 trading plan, if the sale is under one |
| `form`, `accession` | `144` or `144/A`, SEC accession number |

**`13dg`**: one row per Schedule 13D or 13G filed about the company by an investor owning more than 5% of a class of its shares, stamped when it became public:

| Field | Description |
|---|---|
| `filer` | Reporting persons |
| `activist` | 1 for Schedule 13D (the holder may seek to influence or control the company), 0 for 13G (passive) |
| `percent_of_class`, `shares` | Percent of the class and shares beneficially owned (largest reporting person) |
| `reporting_persons` | List of `name`, `shares`, `percent_of_class`, `type` (structured filings only) |
| `purpose` | 13D Item 4, the purpose of the transaction (structured filings only, up to 3,000 characters) |
| `event_date` | Date of the event that required the filing (structured filings only) |
| `parsed_from` | `xml` for structured filings (from December 2024), `text` for older free-text filings |
| `form`, `accession`, `url` | e.g. `SCHEDULE 13D/A` or `SC 13G`, SEC accession number, link to the document |

**`ftd`**: fails-to-deliver, one row per settlement date on which the stock had fails outstanding at the NSCC (aggregated over all clearing members):

| Field | Description |
|---|---|
| `fails` | Total fails-to-deliver balance, in shares |
| `price` | Previous day's closing price |
| `symbol`, `cusip`, `description` | Security |
| `published` | When the SEC posted the half-month file holding the row (UTC): the earliest the row was public |

**`13f`**: one row per 13F-HR filing (and amendments), stamped when it became public:

| Field | Description |
|---|---|
| `report_period` | Quarter end the holdings refer to |
| `n_positions`, `total_value` | Number of positions and their total value |
| `holdings` | List of positions: `issuer`, `class`, `cusip`, `value`, `shares`, `share_type` (`SH` shares or `PRN` principal), `put_call`, `discretion` |
| `form`, `accession` | `13F-HR` or `13F-HR/A`, SEC accession number |

**`nport`**: one row per Form N-PORT filing (and amendments), stamped when it became public:

| Field | Description |
|---|---|
| `fund`, `series_id`, `report_period` | Fund name, SEC series id, month end the holdings refer to |
| `total_assets`, `net_assets` | In US dollars |
| `flow_sales`, `flow_redemptions`, `flow_reinvestment` | Money in (shares sold), out (redeemed) and dividends reinvested over the three months of the quarter, in US dollars |
| `returns_pct` | Total returns of the ticker's share class for the three months, in percent |
| `n_holdings` | Number of positions in the filing |
| `holdings` | List of positions, largest first: `name`, `title`, `cusip`, `isin`, `ticker` (when the fund reports one), `lei`, `balance`, `units` (`NS` shares, `PA` principal, `NC` contracts), `currency`, `value_usd`, `pct_net_assets` (percent), `payoff` (`Long`/`Short`), `asset_cat` (`EC` equity, `DBT` debt, ...), `issuer_cat`, `country` |
| `form`, `accession` | `NPORT-P` or `NPORT-P/A`, SEC accession number |

**`npx`**: one row per Form N-PX, the annual report of how a fund voted at shareholder meetings (July to June), stamped when it became public. With `issuer`, `votes` lists every vote at that company:

| Field | Description |
|---|---|
| `fund`, `period` | Registrant name and the end of the 12 months covered |
| `votes` | With `issuer`: list of `issuer`, `cusip`, `isin`, `meeting_date`, `proposal`, `category`, `proposed_by` (`ISSUER` or `SECURITY HOLDER`), `shares_voted`, `shares_on_loan`, `how_voted`, `management_recommendation`, `against_management` (1/0) |
| `n_votes`, `n_issuers` | Without `issuer`: number of votes and companies |
| `pct_for`, `pct_against`, `pct_abstain` | Without `issuer`: share of votes cast each way |
| `pct_against_management` | Without `issuer`: share of votes against the board's recommendation |
| `n_shareholder_proposals`, `pct_shareholder_proposals_supported` | Without `issuer`: shareholder proposals voted on, and the share the fund supported |

## Notes

- The SEC requires automated requests to identify the sender with a name and contact email; no registration is needed.
- Requests are throttled to the SEC limit of 10 per second, so long insider ranges for very active companies take a while.
- `ftd` data is published twice a month, for the first and second half of each month, about two weeks after the period ends. Rows are stamped at the settlement date, so recent dates appear late.
- `13f` filings are due 45 days after each quarter end. `value` is in US dollars for filings from 2023; earlier filings reported thousands of dollars. Large managers file thousands of positions: request a year or less at a time.
- `13dg`: filings before December 2024 are free text. The filer and percent of class are extracted from the text (found in nearly all filings tested); `shares` from free text is best effort, so check it against the document for important cases.
- `144`: electronic filings start in 2023; earlier notices were on paper and are not included.
- `npx`: structured filings start in August 2024 (votes from July 2023). A large index fund's year is about 18 MB, so summaries over several years take a while: request two years or less.
- `nport`: funds report every month, but only the last month of each fiscal quarter is made public, about 60 days after it ends. Data from late 2019. A fund with thousands of positions (a total market index fund) returns about 5 MB a year: use `top` or a shorter range to stay under the 10 MB response limit. Money market funds file a different form and are not included.
- All EDGAR content is free to reuse (SEC policy).

## OpenBB

Fails-to-deliver is also available in the [OpenBB Platform](../getting-started/openbb.md) with `pip install openbb-aurastream`:

```python
obb.aurastream.fails_to_deliver("GME")
```

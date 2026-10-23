# SEC EDGAR

Insider trades (Form 4), reported company financials (XBRL), earnings release filings, fails-to-deliver and institutional holdings (Form 13F) from the US Securities and Exchange Commission.

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
| `dataset` | `str` | No | `"insider"` (default, Form 4 trades), `"financials"` (reported figures), `"earnings"` (earnings release filings), `"ftd"` (fails-to-deliver) or `"13f"` (institutional holdings) |
| `concepts` | `list[str]` | No | XBRL concepts for `financials`, e.g. `["Revenues", "CostOfGoldProductsAndServices"]`. Default: revenue, costs, income, EPS, cash flow, capex, cash, debt, equity, shares |
| `period` | `str` | No | `"quarter"` (default) or `"annual"` for `financials` |

## Ticker Format

US stock symbols (`AAPL`) or SEC CIK numbers (`320193`, `1164727` for Newmont).

- `ftd`: a symbol (`GME`) or a 9-character CUSIP.
- `13f`: the manager's CIK, e.g. `1067983` for Berkshire Hathaway. Find a manager's CIK with [EDGAR company search](https://www.sec.gov/edgar/searchedgar/companysearch).

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

**`financials`**: one row per filing date, with each concept's value and its calendar period (e.g. `Revenues`, `Revenues_period: "CY2024Q2"`).

**`earnings`**: one row per 8-K filing that reports results of operations (item 2.02), stamped when it became public. This is when a company's earnings release reached the SEC, usually minutes after the press release:

| Field | Description |
|---|---|
| `event_date` | Date of the event the 8-K reports (usually the release date) |
| `items` | All 8-K items in the filing, e.g. `2.02,9.01` |
| `form`, `accession`, `url` | Filing type (`8-K` or `8-K/A`), SEC accession number, link to the document |

**`ftd`**: fails-to-deliver, one row per settlement date on which the stock had fails outstanding at the NSCC (aggregated over all clearing members):

| Field | Description |
|---|---|
| `fails` | Total fails-to-deliver balance, in shares |
| `price` | Previous day's closing price |
| `symbol`, `cusip`, `description` | Security |

**`13f`**: one row per 13F-HR filing (and amendments), stamped when it became public:

| Field | Description |
|---|---|
| `report_period` | Quarter end the holdings refer to |
| `n_positions`, `total_value` | Number of positions and their total value |
| `holdings` | List of positions: `issuer`, `class`, `cusip`, `value`, `shares`, `share_type` (`SH` shares or `PRN` principal), `put_call`, `discretion` |
| `form`, `accession` | `13F-HR` or `13F-HR/A`, SEC accession number |

## Notes

- The SEC requires automated requests to identify the sender with a name and contact email; no registration is needed.
- Requests are throttled to the SEC limit of 10 per second, so long insider ranges for very active companies take a while.
- `ftd` data is published twice a month, for the first and second half of each month, about two weeks after the period ends. Rows are stamped at the settlement date, so recent dates appear late.
- `13f` filings are due 45 days after each quarter end. `value` is in US dollars for filings from 2023; earlier filings reported thousands of dollars. Large managers file thousands of positions: request a year or less at a time.
- All EDGAR content is free to reuse (SEC policy).

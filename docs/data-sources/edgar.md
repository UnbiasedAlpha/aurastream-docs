# SEC EDGAR

Insider trades (Form 4) and reported company financials (XBRL) from the US Securities and Exchange Commission.

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
| `dataset` | `str` | No | `"insider"` (default, Form 4 trades) or `"financials"` (reported figures) |
| `concepts` | `list[str]` | No | XBRL concepts for `financials`, e.g. `["Revenues", "CostOfGoldProductsAndServices"]`. Default: revenue, costs, income, EPS, cash flow, capex, cash, debt, equity, shares |
| `period` | `str` | No | `"quarter"` (default) or `"annual"` for `financials` |

## Ticker Format

US stock symbols (`AAPL`) or SEC CIK numbers (`320193`, `1164727` for Newmont).

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

## Notes

- The SEC requires automated requests to identify the sender with a name and contact email; no registration is needed.
- Requests are throttled to the SEC limit of 10 per second, so long insider ranges for very active companies take a while.

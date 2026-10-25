# USAspending

US federal contract and grant spending by recipient, from USAspending.gov (US Treasury). Track how much the government pays a company each month, and its largest new contract awards.

**Source:** `usaspending` | **Credentials:** Not required

## Example

```json
{
    "tickerlist": ["palantir", "lockheed martin"],
    "start": "2024-01-01",
    "end": "2026-10-24",
    "interval": "1m",
    "source": "usaspending",
    "dataset": "monthly"
}
```

## Ticker Format

A recipient's name or part of it (e.g. `palantir`, `lockheed martin`), or its UEI. Every recipient whose name contains the text is included, so subsidiaries with the parent's name are counted together.

## Parameters

| Parameter | Type | Required | Description |
|---|---|---|---|
| `dataset` | `str` | No | `"monthly"` (default) or `"awards"` |
| `award_type` | `str` | No | `"contracts"` (default) or `"grants"` |
| `min_amount` | `float` | No | `awards` only: minimum award size in US dollars |

## Fields

**`monthly`**: one row per calendar month, stamped at the first of the month:

| Field | Description |
|---|---|
| `obligations` | Federal obligations to the recipient that month, in US dollars (net of de-obligations) |

**`awards`**: the largest awards (up to 100 per request) first obligated within the period, stamped at that date:

| Field | Description |
|---|---|
| `amount` | Total obligated on the award, in US dollars |
| `award_id`, `url` | Award identifier and its USAspending page |
| `recipient` | Recipient name |
| `agency`, `sub_agency` | Awarding agency |
| `description` | Award description |
| `start_date`, `end_date` | Period of performance |

## Notes

- Department of Defense contract data is published with a 90-day delay; other agencies report within days.
- History starts in October 2007.
- USAspending data is a US government work in the public domain.

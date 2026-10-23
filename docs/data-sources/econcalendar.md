# US Economic Calendar

Scheduled release times of US economic data, taken from the agencies' own published schedules: jobs, inflation, GDP, retail sales, housing, Fed decisions.

**Source:** `econcalendar` | **Credentials:** Contact string for `bls` (same as SEC EDGAR); none for the others

## Example

```json
{
    "tickerlist": ["bls", "bea", "census", "fomc"],
    "start": "2026-11-01",
    "end": "2026-12-31",
    "interval": "1d",
    "source": "econcalendar",
    "credentials": {
        "user_agent": "Your Company contact@yourcompany.com"
    }
}
```

## Ticker Format

Tickers are agencies. Leave `tickerlist` empty for all of them.

| Ticker | Releases | Coverage |
|---|---|---|
| `bls` | Bureau of Labor Statistics: Employment Situation, CPI, PPI, JOLTS, import prices, productivity, ... | Current and next year |
| `bea` | Bureau of Economic Analysis: GDP, Personal Income and Outlays (PCE), international trade, ... | Upcoming releases this year, and next year's once published |
| `census` | Census Bureau: retail sales, durable goods, housing starts, construction, inventories, ... | From 2025 |
| `fomc` | Federal Reserve FOMC statements | From 2021, and next year's meetings |

## Fields

One row per release, stamped at its scheduled time in **UTC** (the agencies publish in New York time; daylight saving is handled):

| Field | Description |
|---|---|
| `release` | Release name, e.g. `Consumer Price Index`, `GDP (Advance Estimate)` |
| `period` | Period the release covers, e.g. `September 2026`, where the agency gives it (BEA, Census); for `fomc`, the meeting dates |
| `projections` | `fomc` only: `true` when the meeting comes with the Summary of Economic Projections |

Releases at the same time (e.g. GDP and personal income at 8:30) are separated by a microsecond in the timestamp. The `interval` parameter is ignored.

## Notes

- The calendar lists scheduled times only, not the released values or forecasts. Use `fred` or the agencies' data for the figures.
- The agencies occasionally reschedule releases (e.g. during a government shutdown); the calendar reflects their latest published schedule.
- BLS refuses requests that do not identify the sender, so `bls` needs a name and contact email, like `edgar`.
- All schedules are US government works in the public domain.

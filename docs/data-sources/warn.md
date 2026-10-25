# WARN Layoff Notices

Notices of mass layoffs and plant closures that US employers must file with the state, usually 60 days ahead, under the Worker Adjustment and Retraining Notification (WARN) Act. An early, company-level signal of job cuts. Aurastream serves the states whose terms allow reuse: California and New Jersey.

**Source:** `warn` | **Credentials:** Not required

## Example

```json
{
    "tickerlist": ["all"],
    "start": "2026-01-01",
    "end": "2026-10-24",
    "interval": "1d",
    "source": "warn",
    "state": ["CA"]
}
```

## Ticker Format

- Part of a company name, e.g. `blue shield`, `walmart` (case-insensitive)
- `all` for every notice in the period

## Parameters

| Parameter | Type | Required | Description |
|---|---|---|---|
| `state` | `list[str]` | No | `"CA"`, `"NJ"` (default: both) |

## Fields

One row per notice (California files one per site), stamped at the notice date. Several notices on the same date are separated by a microsecond.

| Field | Description |
|---|---|
| `company` | Employer |
| `state` | `CA` or `NJ` |
| `employees` | Workers affected |
| `notice_type` | e.g. `Layoff Permanent`, `Closure Temporary` (California) |
| `effective_date` | When the layoff takes effect (New Jersey sometimes lists several dates as text) |
| `received_date` | When the state processed the notice (California) |
| `county`, `city`, `address` | Location (California: county and address; New Jersey: city) |
| `industry` | NAICS sector (California, current fiscal year) |

## Coverage

| State | History | Updates |
|---|---|---|
| California | July 2019 to June 2020, and July 2022 onwards | Tuesdays and Thursdays |
| New Jersey | 2004 | As notices are posted |

## Notes

- New Jersey gives only the month a notice was posted, so its rows are stamped at the first of that month.
- California's past fiscal years are published as PDF reports. The years between July 2020 and June 2022, and before July 2019, use layouts that can't be read reliably, so they are left out. In the July 2019 to June 2020 report the city is printed next to the company name, so it appears in `company`.
- Requests covering several California fiscal years take longer the first time (up to about 25 seconds for all years).

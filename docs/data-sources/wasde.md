# USDA WASDE

The US Department of Agriculture's monthly World Agricultural Supply and Demand Estimates: production, use, trade and stocks projections for corn, soybeans, wheat, cotton, rice and other crops, for the US and by country. Each row is one report, so the history shows how USDA's projection changed from month to month.

**Source:** `wasde` | **Credentials:** Not required

## Example

```json
{
    "tickerlist": ["corn:ending_stocks", "soybeans:production:Brazil"],
    "start": "2024-01-01",
    "end": "2026-10-24",
    "interval": "1m",
    "source": "wasde"
}
```

## Ticker Format

- `<commodity>:<attribute>` for the US balance sheet, e.g. `corn:ending_stocks`, `soybeans:avg_farm_price`
- `<commodity>:<attribute>:<region>` for the world tables, e.g. `corn:ending_stocks:World`, `wheat:production:Russia`

| | Commodities |
|---|---|
| US | `corn`, `feed_grains`, `sorghum`, `barley`, `oats`, `wheat`, `soybeans`, `soybean_oil`, `soybean_meal`, `rice`, `cotton`, `sugar` |
| World | `corn`, `coarse_grains`, `wheat`, `rice`, `soybeans`, `soybean_oil`, `soybean_meal`, `cotton` |

**Attributes** are the table lines in snake case, e.g. `area_planted`, `area_harvested`, `yield_per_harvested_acre`, `beginning_stocks`, `production`, `imports`, `exports`, `crushings`, `feed_and_residual`, `ethanol_and_by_products`, `ending_stocks`, `avg_farm_price`. World tables use `production`, `imports`, `domestic_feed`, `domestic_total`, `exports`, `ending_stocks`.

**Regions** are as printed in the report: `World`, `United States`, `China`, `Brazil`, `Argentina`, `European Union`, `Russia`, `Ukraine`, `India`, and others. An unknown region returns an error listing the valid ones.

## Fields

One row per report, stamped at its release (12:00 New York):

| Field | Description |
|---|---|
| `market_year` | The marketing year being projected, e.g. `2026/27` |
| `value` | This report's projection |
| `previous` | The previous month's projection, as printed in the same report |
| `prior_year`, `prior_year_value` | The year before and its estimate |
| `report` | Report month, e.g. `October 2026` |

## Units

US tables use US units: million bushels for grains and soybeans, million acres, bushels per acre, million hundredweight for rice, million pounds for soybean oil, thousand short tons for soybean meal, million 480-lb bales for cotton, dollars per bushel for farm prices. World tables are in million metric tons (cotton: million 480-lb bales).

## Notes

- History starts with the July 2010 report, the first published in machine-readable form.
- Reports come from the USDA National Agricultural Library's ESMIS archive. WASDE is a US government work in the public domain.
- A request covering many years fetches each report once and keeps it, so the first long request takes longer (about 13 seconds for five years).

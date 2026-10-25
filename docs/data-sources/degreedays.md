# NOAA Degree Days

Daily heating and cooling degree days for the contiguous US from NOAA's Climate Prediction Center. A degree day measures how far the day's mean temperature falls below (heating) or rises above (cooling) 65°F; weighted averages track heating and air-conditioning demand, which drives natural gas and power prices.

**Source:** `degreedays` | **Credentials:** Not required

## Example

```json
{
    "tickerlist": ["hdd:US", "cdd:TX"],
    "start": "2025-11-01",
    "end": "2026-03-31",
    "interval": "1d",
    "source": "degreedays",
    "weights": "population"
}
```

## Ticker Format

`hdd:<region>` (heating) or `cdd:<region>` (cooling), where region is:

- `US`: the contiguous US
- A census division: `new_england`, `middle_atlantic`, `east_north_central`, `west_north_central`, `south_atlantic`, `east_south_central`, `west_south_central`, `mountain`, `pacific` (or its number, 1 to 9)
- A state code, e.g. `TX`, `NY`

## Parameters

| Parameter | Type | Required | Description |
|---|---|---|---|
| `weights` | `str` | No | `"population"` (default). For heating degree days by division or US also `"gas"` (homes heated with utility gas, the usual natural gas demand measure), `"electricity"` or `"fuel_oil"`. States and cooling degree days are population weighted only |

## Fields

One row per day from 1981, stamped at 00:00 of that day:

| Field | Description |
|---|---|
| `value` | Degree days |

## Notes

- CPC publishes each day's figure the next morning.
- NOAA data is a US government work in the public domain.

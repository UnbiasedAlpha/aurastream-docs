# Open-Meteo

Weather data by geographic coordinates.

**Source:** `openmeteo` | **Credentials:** Not required

## Example

```json
{
    "tickerlist": [{"latitude": 0, "longitude": 0}],
    "start": "2025-01-01",
    "end": "2025-01-10",
    "interval": "1h",
    "source": "openmeteo",
    "variables": ["weather_code"]
}
```

## Parameters

| Parameter | Type | Required | Description |
|---|---|---|---|
| `variables` | `list[str]` | Yes | Weather variables to fetch (e.g., `["weather_code"]`, `["temperature_2m"]`) |
| `timezone` | `str` | No | Timezone (default: `"GMT"`) |

## Ticker Format

Latitude/longitude dictionaries: `{"latitude": 48.85, "longitude": 2.35}` (Paris)

## Intervals

`1h`, `1d` only

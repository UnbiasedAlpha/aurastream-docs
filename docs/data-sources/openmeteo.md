# Open-Meteo

Historical weather by location, from the Open-Meteo archive API.

## Source ID

```json
"source": "openmeteo"
```

## Credentials

**Not required.** If you have an Open-Meteo API key, you can pass it and it is sent with each request:

```json
"credentials": {
    "key": "your-open-meteo-key"
}
```

## Ticker Format

Each ticker is a location object with `latitude` and `longitude` in decimal degrees:

| Example | Location |
|---|---|
| `{"latitude": 48.85, "longitude": 2.35}` | Paris |
| `{"latitude": 40.71, "longitude": -74.01}` | New York |

Results are keyed as `latitude_longitude` (e.g. `48.85_2.35`).

## Supported Intervals

`1h`, `1d` only

## Parameters

| Parameter | Values | Default | Description |
|---|---|---|---|
| `variables` | list of Open-Meteo variable names | none (required) | Weather variables to fetch. Use hourly names with `1h` and daily names with `1d` |
| `timezone` | IANA time zone (e.g. `"Europe/Paris"`) or `"GMT"` | `"GMT"` | Time zone of the returned timestamps and of the daily aggregation |

Common variables:

- Hourly: `temperature_2m`, `precipitation`, `wind_speed_10m`, `cloud_cover`, `weather_code`
- Daily: `temperature_2m_max`, `temperature_2m_min`, `precipitation_sum`, `wind_speed_10m_max`, `weather_code`

See the [Open-Meteo historical weather docs](https://open-meteo.com/en/docs/historical-weather-api) for the full list.

## Examples

### Hourly temperature and precipitation

```python
response = requests.post(URL, headers=HEADERS, json={
    "tickerlist": [{"latitude": 48.85, "longitude": 2.35}],
    "start": "2025-01-01",
    "end": "2025-01-10",
    "interval": "1h",
    "source": "openmeteo",
    "variables": ["temperature_2m", "precipitation"],
})
```

### Daily data for two cities

```python
response = requests.post(URL, headers=HEADERS, json={
    "tickerlist": [
        {"latitude": 48.85, "longitude": 2.35},
        {"latitude": 40.71, "longitude": -74.01}
    ],
    "start": "2024-01-01",
    "end": "2024-12-31",
    "interval": "1d",
    "source": "openmeteo",
    "variables": ["temperature_2m_max", "precipitation_sum"],
})
```

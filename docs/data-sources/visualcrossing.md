# Visual Crossing

Historical weather by location from Visual Crossing. You need your own Visual Crossing key.

## Source ID

```json
"source": "visualcrossing"
```

## Credentials

```json
"credentials": {
    "key": "your-visualcrossing-key"
}
```

Get a key at [Visual Crossing](https://www.visualcrossing.com/weather-api).

## Ticker Format

Locations as latitude and longitude:

| Example | Location |
|---|---|
| `{"latitude": 40.71, "longitude": -74.01}` | New York |
| `{"latitude": 47.37, "longitude": 8.54}` | Zurich |

The response is keyed `"latitude_longitude"`, e.g. `"47.37_8.54"`.

## Supported Intervals

`1d` (one row per local calendar day, stamped at 00:00) and `1h` (stamped in UTC; whole local days are returned).

## Fields

Daily rows include `tempmax`, `tempmin`, `temp`, `feelslike`, `humidity`, `precip`, `snow`, `windspeed`, `windgust`, `pressure`, `cloudcover`, `solarradiation`, `conditions` and more; hourly rows the hourly equivalents.

## Parameters

| Parameter | Values | Default | Description |
|---|---|---|---|
| `unit_group` | `"us"`, `"metric"`, `"uk"`, `"base"` | `"us"` | Units: `"us"` is Fahrenheit, mph and inches |
| `variables` | list of field names | all | Only these fields |

## Examples

```python
response = requests.post(URL, headers=HEADERS, json={
    "tickerlist": [{"latitude": 47.37, "longitude": 8.54}],
    "start": "2025-01-01",
    "end": "2025-01-31",
    "interval": "1d",
    "source": "visualcrossing",
    "unit_group": "metric",
    "credentials": {"key": "your-visualcrossing-key"},
})
```

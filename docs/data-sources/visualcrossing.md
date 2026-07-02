# Visual Crossing

Historical weather data.

**Source:** `visualcrossing` | **Credentials:** Required

## Credentials

```json
{"credentials": {"key": "YOUR_VISUALCROSSING_KEY"}}
```

## Example

```json
{
    "tickerlist": [{"latitude": 0, "longitude": 0}],
    "start": "2025-01-01",
    "end": "2025-01-10",
    "interval": "1d",
    "source": "visualcrossing",
    "credentials": {"key": "YOUR_KEY"}
}
```

## Ticker Format

Latitude/longitude dictionaries: `{"latitude": 40.71, "longitude": -74.01}` (New York)

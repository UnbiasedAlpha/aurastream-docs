# Google Search Volume

How many times a month a keyword was searched on Google, from the Google Ads Keyword Planner. Unlike Google Trends, which scores interest from 0 to 100, these are absolute monthly searches, so keywords and countries can be compared directly.

**Source:** `googleads` | **Credentials:** Your Google Ads API access

## Example

```json
{
    "tickerlist": ["nvidia stock@US", "alpha vantage"],
    "start": "2023-01-01",
    "end": "2026-10-31",
    "interval": "1m",
    "source": "googleads",
    "credentials": {
        "developer_token": "your-developer-token",
        "client_id": "your-oauth-client-id",
        "client_secret": "your-oauth-client-secret",
        "refresh_token": "your-refresh-token",
        "customer_id": "1234567890"
    }
}
```

## Ticker Format

`<keyword>` or `<keyword>@<geo>`:

- `keyword`: any search phrase, e.g. `nvidia stock`. Google matches it case-insensitively and counts close variants (plurals, misspellings) as the same keyword.
- `geo`: a country code (`US`, `GB`, `DE`, `CH`, `FR`, `IT`, `ES`, `NL`, `JP`, `IN`, `CA`, `AU`, `BR` and others), a numeric [geo target id](https://developers.google.com/google-ads/api/data/geotargets), or `world`. Default: `world`.

## Parameters

| Parameter | Type | Required | Description |
|---|---|---|---|
| `language` | `str` | No | `"en"` (default), `"de"`, `"fr"`, `"es"`, `"it"`, `"ja"`, or a numeric language constant id. Applies to every ticker in the request |

The `interval` parameter is accepted but ignored: the data is monthly.

## Fields

One row per month for the last four years, stamped at 23:59 on the last day of the month:

| Field | Description |
|---|---|
| `value` | Google searches in that month |

## Notes

- Google publishes a month's figure a few days after the month ends.
- Figures are rounded into fixed buckets (e.g. 7.48M, 9.14M, 11.1M, 13.6M; 110, 170, 320, 480), so a series moves in steps rather than smoothly. They give the scale of interest; for finer week-to-week moves, combine them with Google Trends.
- Google gives no figures for keywords in restricted ad categories, such as cryptocurrencies (`bitcoin`, `ethereum`): those tickers return no data.
- Credentials: a Google Ads developer token whose Google Cloud project has **Basic** access or higher (Explorer access cannot use the Keyword Planner), an OAuth client and refresh token for a Google user with access to the Google Ads account `customer_id` (10 digits). If that account is reached through a manager account, add `login_customer_id`. See [Credentials](../getting-started/credentials.md#google-search-volume).

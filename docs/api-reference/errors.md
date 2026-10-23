# Errors

Errors come back as an HTTP status code with a JSON body containing a `message`.

| Status | Body (example) | Cause | Retry? |
|---|---|---|---|
| `400 Bad Request` | `{"message": "..."}` | Invalid parameters: unknown `source`, bad date format, unsupported `interval`, missing credentials for a source that needs them | No: fix the request |
| `403 Forbidden` | `{"message": "Forbidden"}` | Missing or invalid `x-api-key` header, the key was deleted (for example after cancelling a plan), or it is paused because a renewal payment failed (see [Billing](../getting-started/billing.md#if-a-payment-fails)) | No: check the key and your subscription in the dashboard |
| `429 Too Many Requests` | `{"message": "Limit Exceeded"}` | Monthly allowance for your plan is used up | After the monthly reset, or upgrade. See [Plans & Limits](../getting-started/plans-and-limits.md) |
| `429 Too Many Requests` | `{"message": "Too Many Requests"}` | Requests sent faster than your plan's rate | Yes, after a short pause |
| `500 Internal Server Error` | `{"message": "..."}` | The upstream data source failed or returned an error | Yes, with backoff |
| `504 Gateway Timeout` | `{"message": "Endpoint request timed out"}` | The request took too long, usually a very large range or many tickers | Yes, after splitting the request |

## Retrying safely

Retry only `429 Too Many Requests` (rate), `500` and `504`. Wait between attempts and increase the wait each time:

```python
import time
import requests

def fetch(payload, retries=4):
    for attempt in range(retries):
        r = requests.post(URL, headers=HEADERS, json=payload, timeout=130)
        if r.status_code == 200:
            return r.json()
        if r.status_code == 429 and r.json().get("message") == "Limit Exceeded":
            raise RuntimeError("Monthly allowance used up")
        if r.status_code in (429, 500, 504):
            time.sleep(2 ** attempt)
            continue
        r.raise_for_status()
    r.raise_for_status()
```

Don't retry `400` or `403`: the same request will fail again.

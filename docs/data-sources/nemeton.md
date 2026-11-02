# Nemeton

Order book signals for crypto markets: depth, imbalance, spread and order-flow metrics computed from recorded order books.

Nemeton is a separate paid product from Unbiased Alpha ($250/month). Subscribing gives you a Nemeton API key, which you pass to Aurastream as the credential for this source.

## Source ID

```json
"source": "nemeton"
```

## Credentials

```json
"credentials": {
    "key": "your-nemeton-api-key"
}
```

The key is created when your Nemeton subscription starts and is removed when it ends. A missing or rejected key returns an error, not an empty result.

## Ticker Format

`{SYMBOL}_{EXCHANGE}_{INSTRUMENT}`

| Example | Market |
|---|---|
| `BTCUSDT_BINANCE_PERPETUAL` | Bitcoin / USDT perpetual on Binance |

## Supported Intervals

Minutes, hours and days (e.g. `1M`, `5M`, `15M`, `1h`, `4h`, `1d`). The interval is sent to Nemeton as a number of minutes.

Data starts on 2023-12-27. A request returns at most 10,000 bars: if the range is longer, `start` is moved forward so the response covers the 10,000 bars before `end`.

## Parameters

| Parameter | Values | Default | Description |
|---|---|---|---|
| `metric` | see list below | none (required) | Signal to return |
| `level` | integer | `1` | Number of order book levels per side the metric uses (1 = best bid and ask, up to 1000) |

Order books are recorded every minute with 1000 levels per side; a bar's value comes from the books inside
that bar. Metrics marked *flow* compare each book with the one a minute earlier.

### Price and spread

| Metric | Fields | Description |
|---|---|---|
| `mid` | `Mid` | Midpoint between the bid and ask at `level` |
| `weighted-mid` | `WeightedMid`, `Microprice` | `WeightedMid`: average price of the top `level` levels, weighted by the size resting there. `Microprice`: the fair price implied by the best bid and ask sizes: it moves toward the ask when bids outweigh asks, and toward the bid when asks do |
| `spread` | `Spread` | Distance between the ask and bid at `level`, in price units |
| `normalized-spread` | `NormalizedSpread` | Spread at `level` relative to the size resting within it |
| `liquidity-spread-depth` | `SpreadDepth` | How much wider the book is at `level` than at the best quotes |
| `ivdepth` | `IVDepth` | Gap between the size-weighted bid and ask prices of the top `level` levels, relative to price: the cost of crossing that much of the book |

### Depth and shape

| Metric | Fields | Description |
|---|---|---|
| `cumulative-depth` | `CumulativeBidDepth`, `CumulativeAskDepth` | Size resting in the top `level` levels of each side, in the base asset |
| `maxdepth` | `BookReachBid`, `BookReachAsk` | How far from mid the recorded book extends on each side, in basis points: a thin market reaches far, a deep one stays close to the price |
| `quote-concentration` | `QuoteConcentration` | How much of the size in the top `level` levels sits close to mid, from 0 (spread out) to 1 (all at the touch). Distances in basis points, so values compare across coins |
| `quote-skewness` | `QuoteSkewness` | Skewness of where size sits relative to mid across the top `level` levels |
| `orderbook-slope` | `OBSlopeBid`, `OBSlopeAsk` | How quickly price moves away from the best quote per unit of size on each side: steep means thin |

### Imbalance

| Metric | Fields | Description |
|---|---|---|
| `liquidity-imbalance` | `LiquidityImbalance` | Bid size against ask size in the top `level` levels, from -1 (all asks) to 1 (all bids) |
| `orderbook-pressure` | `OrderbookPressure` | Bid against ask size where quotes close to mid count more than distant ones, from -1 to 1 |
| `quote-asymmetry` | `QuoteAsymmetry` | Whether the quotes behind the best sit closer together on the bid or the ask side, from -1 to 1; positive when bids are denser. Needs `level` 2 or more |
| `price-movement-potential` | `PriceMovementPotential` | Value resting on the ask side relative to the bid side, in the top `level` levels |

### Order flow (*flow*)

| Metric | Fields | Description |
|---|---|---|
| `dynamic-orderflow-imbalance` | `DynamicOrderFlowImbalance` | Net buying pressure from how the best `level` quotes moved and resized since the previous book |
| `static-orderflow-imbalance` | `StaticOrderFlowImbalance` | Net change in size at the top `level` levels, bid side against ask side |
| `dollar-value-imbalance` | `DollarValueImbalance` | Net change in value resting at the top `level` levels, bid side against ask side, relative to the value resting there |
| `quantity-imbalance` | `QuantityImbalance` | How far the top `level` levels moved in price since the previous book, weighted by their size |
| `liquidity-consumption-rate` | `LCRBid`, `LCRAsk` | Share of the previous book's size in the top `level` levels that has been taken or cancelled since |
| `quote-intensity` | `QuoteIntensity` | Number of the top `level` levels whose size changed since the previous book: how busy the quotes are |

### Trades

| Metric | Fields | Description |
|---|---|---|
| `trade-imbalance` | `TradeImbalance` | Share of traded volume initiated by buyers, from 0 to 1 |

### Crossed books

A book whose best bid is at or above its best ask holds price levels that no longer exist. For those books every
field is `null` and the response adds `"CrossedBook": true` (flow metrics also for the book after one). This
affects mainly BTC and ETH from February 2024 to March 2025.

## Examples

### Daily cumulative depth

```python
response = requests.post(URL, headers=HEADERS, json={
    "tickerlist": ["BTCUSDT_BINANCE_PERPETUAL"],
    "start": "2026-01-01",
    "end": "2026-06-15 23:59",
    "interval": "1d",
    "source": "nemeton",
    "metric": "cumulative-depth",
    "level": 1,
    "credentials": {
        "key": "YOUR_NEMETON_KEY"
    }
})
```

### Hourly order book pressure

```python
response = requests.post(URL, headers=HEADERS, json={
    "tickerlist": ["BTCUSDT_BINANCE_PERPETUAL"],
    "start": "2026-09-01",
    "end": "2026-09-30",
    "interval": "1h",
    "source": "nemeton",
    "metric": "orderbook-pressure",
    "credentials": {
        "key": "YOUR_NEMETON_KEY"
    }
})
```

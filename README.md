# btc-cent-forecast-01

Forecast Bitcoin's US-dollar price 4 hours ahead, from the last 72 hours of
1-minute closes, for the make.fun bounty `btc-cent-forecast-01`.

## Method

The forecast is the median of four candidate prices:

1. the checkpoint close (the last price in the case),
2. the median of the last 30 closes,
3. the median of the last 60 closes,
4. the last price pulled 10% of the way toward the median of the last 1440
   closes (24 hours).

The result is clamped to (0.00001, 9999999.99999) and rounded to 5 decimals.

## Why

Bitcoin's 4-hour move is close to a random walk: nothing beats plain
persistence (guessing the last price) by much, and trend-chasing or fitted
models that look good on one day lose badly on others. A robust average of
near-persistence signals keeps a small, consistent edge. On 249 checkpoints
cut every 4 hours from six weeks of Binance BTCUSDT 1-minute closes, this
method scored 40,342.9 cents MAE vs 40,862.1 for persistence (-1.27%),
better in up, down and flat regimes alike.

## Run

```sh
mkdir -p out && MAKEFUN_INPUT_DIR=input MAKEFUN_OUTPUT_DIR=out node main.ts
```

Node 22+, standard library only, no network, deterministic. One `.out` file
per `.in` case, each exactly `{"forecasts":[{"case":"<id>","price":<number>}]}`.

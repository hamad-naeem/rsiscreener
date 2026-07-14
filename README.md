<img src="assets/crate-open.svg" width="100%" alt="RSI Screener: live RSI for the entire Binance spot market.">

<img src="assets/wall.png" alt="The live market wall: 300+ pairs with RSI sparklines on the 4h timeframe" width="100%">

RSI Screener is a production SaaS that computes RSI for 300+ Binance spot pairs, crypto and tokenized stocks, across 15 timeframes, about 4,500 live series, and serves them as one grid that updates within seconds of each candle close. The source is private. This page documents the product and its design; every capture below is the logged-in product.

<br>

<p align="center">
  <a href="assets/extremes.png"><img src="assets/tour-01.png" width="32.8%" alt="Overbought filter: a handful of lit tiles against the dimmed wall"></a>
  <a href="assets/chart.png"><img src="assets/tour-02.png" width="32.8%" alt="BTC/USDT RSI chart with live reading"></a>
  <a href="assets/drawing.png"><img src="assets/tour-03.png" width="32.8%" alt="Two trendlines drawn on the chart, drawing toolbar active"></a>
</p>

<div align="center">
<sub>Filtering, search-to-chart, and drawing tools. Click a frame for the full-resolution capture.</sub>
</div>

<br>

## Design

A single always-on Next.js server runs a background market engine. The engine polls Binance's REST API, computes RSI with Wilder's smoothing (14-period, configurable per user), and recomputes each timeframe when its candle closes; a 4h close triggers only the 4h set. Results are held in memory as per-timeframe snapshots. Each snapshot is also scanned for trade setups (bullish and bearish divergence, RSI extreme exits, trend plus volume), which appear as badges on the grid.

Visitor requests read the snapshot; nothing a visitor does touches the exchange. Responses are small JSON payloads: symbol, RSI, price, sparkline. Filtering and ticker search run client-side against data already in the browser, and the grid renders progressively from whatever is warm. The server runs as one persistent process rather than serverless functions because the cache has to outlive the request.

The chart is built on TradingView's Lightweight Charts: synced price, volume, and RSI panes, live while open, with a drawing layer written for this product (trendlines, undo/redo, PNG export). Seven configurable indicators (two moving averages, Bollinger Bands, Supertrend, rolling VWAP, MACD, Stochastic RSI) are computed in the browser from candles already on hand, so changing a parameter redraws instantly and adds no server load.

Auth is JWT (HS256), with email OTP for registration and password reset. Subscription state is reconciled against the billing provider's API on read instead of trusting webhook delivery, so a dropped webhook cannot produce incorrect access in either direction.

All routes are rate-limited and validated. Per-user locks serialize the operations where a race could grant or revoke access incorrectly. Vitest covers the RSI math, webhook signature verification, and input validation.

<div align="center">

<samp>binance → engine → warm cache → grid → chart</samp>

</div>

<br>

<div align="center">

<samp>Next.js 16 · React · TypeScript · Tailwind v4 · shadcn/ui · Lightweight Charts · Supabase · Postgres · JWT · Paddle · Vitest · PWA</samp>

<sub>Built by <a href="https://github.com/hamad-naeem">Hamad Naeem</a>. This repository documents the product without exposing the private source.</sub>

</div>

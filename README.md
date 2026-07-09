<div align="center">

# RSI Screener

### Live RSI for the entire Binance spot market. The whole market at one glance.

<p>
  <img alt="Source private" src="https://img.shields.io/badge/source-private-241711?style=for-the-badge">
  <img alt="Next.js 16" src="https://img.shields.io/badge/Next.js%2016-000000?style=for-the-badge&logo=nextdotjs">
  <img alt="TypeScript" src="https://img.shields.io/badge/TypeScript-3178C6?style=for-the-badge&logo=typescript&logoColor=white">
  <img alt="Paddle" src="https://img.shields.io/badge/Paddle%20billing-ff8b5f?style=for-the-badge">
</p>

<img src="assets/hero-tool.png" alt="RSI Screener Terracotta dashboard" width="92%">

</div>

RSI Screener is a production SaaS that computes the Relative Strength Index for **300+ Binance spot pairs across 15 timeframes**, on the server, and streams it to a dense live scanner. You read where momentum is stretching across the whole market in a single glance, then drill into any pair for a full chart.

Everything shown here is the **real logged-in product**, recorded live on the Terracotta theme. The source code is private; this page documents the product and how it is built.

<br>

## See it work

### Isolate the extremes
Dim the entire wall down to only the oversold or overbought names in one tap. The handful of pairs that matter light up while everything else fades back.

<img src="assets/clip-filters.webp" alt="Filtering the grid to the oversold coins" width="100%">

### Search straight into the chart
Type a ticker, press enter, and jump from the market wall into a full RSI chart with price history and a live reading, without leaving the keyboard.

<img src="assets/clip-search.webp" alt="Searching a coin and opening its chart" width="100%">

### Mark the setup
A custom canvas charting engine, not a library. Draw trendlines, switch colours, undo, redo, clear, and export the chart to PNG.

<img src="assets/clip-draw.webp" alt="Drawing trendlines on the RSI chart" width="100%">

### Fully responsive on mobile
The same scanner folds down to a two-column wall on a phone, keeping the timeframe strip, search, filters, live status and full charting. It is an installable PWA, so it can live on the home screen and open like an app.

<div align="center"><img src="assets/clip-mobile.webp" alt="The mobile RSI scanner, scrolling" width="300"></div>

<br>

## How it is built

A single always-on **Next.js 16** server (App Router, React Server Components) with a background market engine feeding a live UI. The guiding idea is **centralization**: the server does the market work once, and every visitor reads a small pre-computed result.

### The market engine
If every browser called Binance directly, each user would hit rate limits and pages would crawl. Instead one server-side engine owns all market data:

- It polls Binance's public REST API for candlesticks (klines) across every tracked pair and **all 15 timeframes**.
- For each series it computes RSI with **Wilder's smoothing** (the standard 14-period calculation, and the period is configurable per user).
- Work is **aligned to candle closes**: when a 4h candle closes, only the 4h set recomputes. That keeps data fresh, staggers the load, and means the exchange is queried centrally rather than once per visitor.
- Results are held in a **warm in-memory cache** as compact snapshots, one per timeframe.

### Serving the scanner
- A page request reads the warm snapshot for the chosen timeframe and returns a **tiny JSON payload** (symbol, RSI, sparkline points, price). No exchange call happens per visitor, so it stays fast even on mobile data.
- The grid renders **progressively**: it paints whatever is already warm and fills the rest as it arrives, so there is never a spinner wall on 300 tiles.
- **Filtering and search run client-side** against the snapshot, so oversold/overbought and ticker search are instant and never refetch. The tiles dim in place instead of reflowing.

### The chart
A hand-built **canvas rendering engine** with no charting library: the RSI line and moving average, zoom and pan, and a drawing overlay for trendlines with undo/redo, a colour palette, and one-tap PNG export. It updates live while open.

### Accounts, data and billing
- **Supabase (Postgres)** stores users, subscriptions and synced settings, so your theme, RSI period and thresholds follow your account.
- **JWT (HS256)** sessions, with registration verified by **email OTP** and password reset by OTP.
- **Paddle** is the merchant of record (card and PayPal) with an inline one-page checkout.
- **Self-healing billing:** webhooks can be missed (a server restart, a dropped delivery). So subscription state is **reconciled against Paddle's live API on read** — if the database has drifted, it heals itself, and a paying customer is never locked out by a lost event.

### Delivery and hardening
- A **single always-on server**, not serverless: no cold starts, which suits a warm-cache engine that must stay hot.
- An installable **PWA** with a service-worker app-shell cache and a graceful offline screen.
- **Rate limiting**, per-user in-memory locks that close race conditions (for example, double-claiming the free month), and strict input validation on every route. A **Vitest** suite covers the RSI math, webhook signature verification, and validation.

### Data flow

```mermaid
flowchart LR
  B[Binance candles] --> E["Market engine<br/>RSI · Wilder smoothing"]
  E --> C[Warm snapshot cache]
  C --> G[Scanner grid]
  G --> Ch[Chart + drawing tools]
  S[("Supabase / Postgres")] -.-> A["Auth · JWT · settings"]
  A -.-> G
  P[Paddle billing] -.-> AC[Self-healing access] -.-> A
```

<br>

## Stack

<p>
  <img src="https://img.shields.io/badge/Next.js%2016-000000?style=flat&logo=nextdotjs&logoColor=white">
  <img src="https://img.shields.io/badge/React-20232A?style=flat&logo=react&logoColor=61DAFB">
  <img src="https://img.shields.io/badge/TypeScript-3178C6?style=flat&logo=typescript&logoColor=white">
  <img src="https://img.shields.io/badge/Tailwind%20CSS%20v4-38B2AC?style=flat&logo=tailwind-css&logoColor=white">
  <img src="https://img.shields.io/badge/shadcn/ui-000000?style=flat">
  <img src="https://img.shields.io/badge/Supabase-3ECF8E?style=flat&logo=supabase&logoColor=white">
  <img src="https://img.shields.io/badge/Postgres-4169E1?style=flat&logo=postgresql&logoColor=white">
  <img src="https://img.shields.io/badge/Paddle-ff8b5f?style=flat">
  <img src="https://img.shields.io/badge/JWT-000000?style=flat&logo=jsonwebtokens&logoColor=white">
  <img src="https://img.shields.io/badge/Vitest-6E9F18?style=flat&logo=vitest&logoColor=white">
  <img src="https://img.shields.io/badge/PWA-5A0FC8?style=flat&logo=pwa&logoColor=white">
</p>

Custom canvas charting engine · server-side market engine · warm-cache snapshots · email OTP auth · settings sync · installable PWA.

<br>

<div align="center">
<sub>This repository is a showcase. It documents the product and how it works, without exposing the private source.</sub>
</div>

<div align="center">

# RSI Screener

### Live RSI for the entire Binance spot market — the whole market at one glance.

<p>
  <img src="https://img.shields.io/badge/300+-Binance_pairs-c15f3c?style=for-the-badge&labelColor=241711">
  <img src="https://img.shields.io/badge/15-timeframes-c15f3c?style=for-the-badge&labelColor=241711">
  <img src="https://img.shields.io/badge/RSI-server_side_engine-c15f3c?style=for-the-badge&labelColor=241711">
  <img src="https://img.shields.io/badge/refresh-under_10s-c15f3c?style=for-the-badge&labelColor=241711">
</p>

<img src="assets/hero-tool.png" alt="RSI Screener Terracotta dashboard" width="94%">

</div>

<br>

> A production SaaS that computes RSI for **300+ Binance spot pairs across 15 timeframes**, on the server, and streams it to one dense live wall. Everything below is the **real logged-in product**, recorded live. The source code is private; this page shows the product and how it was built.

<br>

## Watch it work

**01 · Isolate the extremes**
Dim the entire wall to only the oversold or overbought names in one tap. The handful of pairs that matter light up while everything else fades back.

<img src="assets/clip-filters.webp" alt="Filtering the grid to the oversold coins" width="100%">

**02 · Search straight into the chart**
Type a ticker, press enter, and jump from the market wall into a full RSI chart with price history and a live reading, without leaving the keyboard.

<img src="assets/clip-search.webp" alt="Searching a coin and opening its chart" width="100%">

**03 · Mark the setup**
A custom canvas charting engine, not a library. Draw trendlines, switch colours, undo, redo, clear, and export the chart to PNG.

<img src="assets/clip-draw.webp" alt="Drawing trendlines on the RSI chart" width="100%">

<br>

## Engineering highlights

> **One connection for the whole market.**
> Instead of every browser calling the exchange, a single server computes RSI centrally and each visitor reads a small warm snapshot. It scales with a cache, not with the number of users.

> **A UI that is never blank.**
> The 300-tile wall paints whatever is already warm and fills the rest as it arrives. Filtering and search run client-side against the snapshot, so they are instant and never refetch.

> **Charts written from scratch.**
> A hand-built canvas engine renders the RSI line, moving average, zoom and pan, and a drawing overlay with trendlines, undo/redo and PNG export. No charting library.

> **Access that heals itself.**
> Subscription state reconciles against the billing provider on read, so a single missed webhook can never lock out a paying customer or leave a cancelled one with access.

<br>

## How it was built

A single always-on **Next.js 16** server (App Router, React Server Components) with a background market engine feeding a live UI. The guiding idea is **centralization**: the server does the market work once, and every visitor reads a small pre-computed result.

### The market engine
- Polls Binance's public REST API for candlesticks across every tracked pair and **all 15 timeframes**.
- Computes RSI with **Wilder's smoothing** (standard 14-period, configurable per user).
- Aligns work to **candle closes** — when a 4h candle closes, only the 4h set recomputes. That keeps data fresh, staggers load, and queries the exchange centrally instead of once per visitor.
- Holds results in a **warm in-memory cache** as compact snapshots, one per timeframe.

### Serving the scanner
- A request reads the warm snapshot for the chosen timeframe and returns a **tiny JSON payload** (symbol, RSI, sparkline, price) — no exchange call per visitor, fast even on mobile data.
- The grid renders **progressively** rather than blocking on a spinner across 300 tiles.
- **Filter and search are client-side**, so oversold/overbought and ticker search are instant and dim the tiles in place instead of reflowing.

### The chart
A hand-built **canvas rendering engine** with no charting library: RSI line and moving average, zoom and pan, and a drawing overlay for trendlines with undo/redo, a colour palette, and one-tap PNG export. It updates live while open.

### Accounts and data
- **Supabase (Postgres)** for users, subscriptions and synced settings, so theme, RSI period and thresholds follow the account.
- **JWT (HS256)** sessions, registration verified by **email OTP**, password reset by OTP.
- Subscriptions with a **self-healing access model**: billing state reconciles against the provider's live API on read, so the database can drift and still recover on its own.

### Delivery and hardening
- A **single always-on server**, not serverless — no cold starts, which suits a warm-cache engine that must stay hot.
- An installable **PWA** with a service-worker app-shell cache and a graceful offline screen.
- **Rate limiting**, per-user locks that close race conditions, and strict input validation on every route. A **Vitest** suite covers the RSI math, webhook signature verification and validation.

### Data flow

```mermaid
flowchart LR
  B[Binance candles] --> E["Market engine<br/>RSI · Wilder smoothing"]
  E --> C[Warm snapshot cache]
  C --> G[Scanner grid]
  G --> Ch[Chart + drawing tools]
  S[("Supabase / Postgres")] -.-> A["Auth · JWT · settings"]
  A -.-> G
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
  <img src="https://img.shields.io/badge/JWT-000000?style=flat&logo=jsonwebtokens&logoColor=white">
  <img src="https://img.shields.io/badge/Vitest-6E9F18?style=flat&logo=vitest&logoColor=white">
  <img src="https://img.shields.io/badge/PWA-5A0FC8?style=flat&logo=pwa&logoColor=white">
</p>

Custom canvas charting engine · server-side market engine · warm-cache snapshots · email OTP auth · settings sync · installable PWA.

<br>

<div align="center">
<sub>This repository is a showcase. It documents the product and how it works, without exposing the private source.</sub>
</div>

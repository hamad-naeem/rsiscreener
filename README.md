<img src="assets/title.svg" width="100%" alt="Inside the crate: RSI Screener, live RSI for the entire Binance spot market. 300+ pairs, 15 timeframes, refresh under 10 seconds, source private.">

<img src="assets/hero-tool.png" alt="RSI Screener dashboard: the live 300-tile market wall" width="100%">

You pressed the crate. This is what's inside: a production SaaS that computes RSI for **300+ Binance spot pairs across 15 timeframes** on the server and streams it to one dense live wall. Everything below is the real logged-in product, recorded live. The source is private; this page shows the product and how it's built.

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

## How it works

One always-on **Next.js 16** server (App Router, React Server Components) with a background market engine feeding a live UI. The guiding idea is **centralization**: the server does the market work once, and every visitor reads a small pre-computed result.

### The market engine
- Polls Binance's public REST API for candlesticks across every tracked pair and all 15 timeframes.
- Computes RSI with **Wilder's smoothing** (standard 14-period, configurable per user).
- Aligns work to **candle closes** — when a 4h candle closes, only the 4h set recomputes. Data stays fresh, load stays staggered, and the exchange is queried centrally instead of once per visitor.
- Holds results in a **warm in-memory cache** as compact snapshots, one per timeframe.

### Serving the scanner
- A request reads the warm snapshot and returns a tiny JSON payload (symbol, RSI, sparkline, price) — no exchange call per visitor, fast even on mobile data.
- The 300-tile wall paints whatever is already warm and fills the rest as it arrives — never a blank screen behind a spinner.
- **Filter and search run client-side** against the snapshot, so they are instant and dim tiles in place instead of refetching.

### The chart
A hand-built **canvas rendering engine** with no charting library: RSI line and moving average, zoom and pan, and a drawing overlay for trendlines with undo/redo, a colour palette, and one-tap PNG export. It updates live while open.

### Accounts and access
- **Supabase (Postgres)** for users, subscriptions and synced settings — theme, RSI period and thresholds follow the account.
- **JWT (HS256)** sessions; registration and password reset verified by **email OTP**.
- **Self-healing access**: subscription state reconciles against the billing provider's live API on read, so a single missed webhook can never lock out a paying customer or leave a cancelled one with access.

### Delivery and hardening
- A single always-on server, not serverless — no cold starts, which suits a warm-cache engine that must stay hot.
- Installable **PWA** with a service-worker app-shell cache and a graceful offline screen.
- **Rate limiting**, per-user locks that close race conditions, and strict input validation on every route. A **Vitest** suite covers the RSI math, webhook signature verification and validation.

### Data flow

```mermaid
%%{init: {"theme": "base", "themeVariables": {
  "background": "#151110",
  "primaryColor": "#241711",
  "primaryTextColor": "#e0855c",
  "primaryBorderColor": "#3a2b22",
  "lineColor": "#c15f3c",
  "secondaryColor": "#1a1512",
  "tertiaryColor": "#1a1512",
  "fontFamily": "monospace"
}}}%%
flowchart LR
  B[Binance candles] --> E["Market engine<br/>RSI · Wilder smoothing"]
  E --> C[Warm snapshot cache]
  C --> G[Scanner grid]
  G --> Ch[Chart + drawing tools]
  S[("Supabase / Postgres")] -.-> A["Auth · JWT · settings"]
  A -.-> G
```

<br>

<div align="center">

<samp>Next.js 16 · React · TypeScript · Tailwind v4 · shadcn/ui · Supabase · Postgres · JWT · Vitest · PWA</samp>

<sub>Built by <a href="https://github.com/hamad-naeem">Hamad Naeem</a> with AI-assisted development. This repository documents the product without exposing the private source.</sub>

</div>

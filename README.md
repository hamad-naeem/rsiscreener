<div align="center">

# RSI Screener

### Live RSI intelligence for the Binance spot market

Private production SaaS. Public product showcase.

<p>
  <a href="https://hamad12-cmd.github.io/rsiscreener/">
    <img alt="Showcase" src="https://img.shields.io/badge/visual_showcase-live-d46a47?style=for-the-badge">
  </a>
  <img alt="Source" src="https://img.shields.io/badge/source-private-241711?style=for-the-badge">
  <img alt="Stack" src="https://img.shields.io/badge/Next.js_16-TypeScript-000000?style=for-the-badge&logo=nextdotjs">
  <img alt="Payments" src="https://img.shields.io/badge/Paddle-billing-ff8b5f?style=for-the-badge">
</p>

<a href="https://hamad12-cmd.github.io/rsiscreener/"><strong>Open the full visual showcase</strong></a>
&nbsp;&nbsp;|&nbsp;&nbsp;
<a href="#demo-reel"><strong>Watch clips</strong></a>
&nbsp;&nbsp;|&nbsp;&nbsp;
<a href="#system-shape"><strong>Architecture</strong></a>

</div>

<br>

<p align="center">
  <a href="https://hamad12-cmd.github.io/rsiscreener/">
    <img src="assets/hero-tool.png" alt="RSI Screener Terracotta dashboard" width="100%">
  </a>
</p>

## Product Signal

RSI Screener is a logged-in market command center for traders who need to scan
hundreds of Binance spot pairs fast. The product turns RSI into a dense visual
wall: one tile per symbol, one mini-chart per pair, one toolbar for timeframe,
filters, search, and chart entry.

The public repository is intentionally source-free. It shows the product surface,
interaction flow, and architecture without exposing the private codebase.

<br>

## Demo Reel

### 01. Scan The Market Wall

Dense Terracotta grid. Every tile is a symbol. Every line is RSI movement.

<p align="center">
  <img src="assets/clip-grid-scroll.gif" alt="Scrolling through the RSI Screener market grid" width="100%">
</p>

### 02. Change Timeframe, Isolate Extremes

Move between RSI contexts, then dim everything except oversold or overbought
conditions.

<p align="center">
  <img src="assets/clip-timeframes-filters.gif" alt="Switching timeframes and RSI filters" width="100%">
</p>

### 03. Search Straight Into The Chart

Type a ticker, press enter, and jump from market wall to chart workflow.

<p align="center">
  <img src="assets/clip-search-chart.gif" alt="Searching BTC and opening the RSI chart" width="100%">
</p>

### 04. Mark The Setup

Draw trendlines, pick colors, undo, redo, clear, and export the chart as PNG.

<p align="center">
  <img src="assets/clip-chart-drawing.gif" alt="Drawing a trendline on the RSI chart" width="100%">
</p>

### 05. Mobile Scanner

The same scanner compressed into a phone layout with timeframe controls, search,
filters, live status, and a two-column grid.

<p align="center">
  <img src="assets/clip-mobile-scroll.gif" alt="Scrolling the mobile RSI Screener tool" width="390">
</p>

<br>

## Built Surface

<div>
  <p><strong>Market engine</strong><br>
  Server-side RSI computation for Binance spot candles, designed so browsers do not hammer the exchange.</p>

  <p><strong>Warm scanner grid</strong><br>
  Pre-computed snapshots feed a dense tile wall with timeframe switching, filters, search, and live status.</p>

  <p><strong>Chart workflow</strong><br>
  Full RSI chart modal with drawing tools, color palette, undo/redo, clear, and PNG export.</p>

  <p><strong>SaaS layer</strong><br>
  Auth, JWT sessions, user settings, protected access, Paddle billing, and responsive PWA shell.</p>
</div>

<br>

## System Shape

```mermaid
flowchart LR
  Binance[Binance candles] --> Engine[Market engine]
  Engine --> RSI[RSI computation]
  RSI --> Cache[Warm snapshot cache]
  Cache --> Grid[Logged-in scanner grid]
  Grid --> Chart[Chart + drawing tools]

  Supabase[(Supabase Postgres)] --> Auth[Auth + settings]
  Auth --> Grid

  Paddle[Paddle billing] --> Access[Subscription access]
  Access --> Auth
```

The core design choice is centralization: the server owns market polling and RSI
calculation, then the UI reads compact snapshots. That keeps the tool responsive
and avoids scaling Binance requests with visitor count.

<br>

## Stack

`Next.js 16` &middot; `React` &middot; `TypeScript` &middot; `Tailwind CSS v4` &middot; `Supabase`
&middot; `Paddle` &middot; `JWT` &middot; `Vitest` &middot; `PWA` &middot; `Playwright`

<br>

<div align="center">
  <strong>Want the polished version?</strong><br>
  The GitHub Pages showcase is the intended portfolio view:
  <br>
  <a href="https://hamad12-cmd.github.io/rsiscreener/">https://hamad12-cmd.github.io/rsiscreener/</a>
</div>

<div align="center">

# RSI Screener

### Live RSI for the entire Binance spot market. The whole market at one glance.

<p>
  <a href="https://hamad12-cmd.github.io/rsiscreener/">
    <img alt="Live showcase" src="https://img.shields.io/badge/%E2%96%B6%20live%20visual%20showcase-c15f3c?style=for-the-badge">
  </a>
  <img alt="Source private" src="https://img.shields.io/badge/source-private-241711?style=for-the-badge">
  <img alt="Next.js 16" src="https://img.shields.io/badge/Next.js%2016-000000?style=for-the-badge&logo=nextdotjs">
  <img alt="TypeScript" src="https://img.shields.io/badge/TypeScript-3178C6?style=for-the-badge&logo=typescript&logoColor=white">
</p>

<a href="https://hamad12-cmd.github.io/rsiscreener/"><img src="assets/hero-tool.png" alt="RSI Screener dashboard" width="90%"></a>

<sub>A production SaaS that computes RSI for 300+ Binance pairs across 15 timeframes, server-side, and streams it to a live scanner. Everything here is the real logged-in tool. The source stays private.</sub>

</div>

<br>

> **The polished version lives here → [hamad12-cmd.github.io/rsiscreener](https://hamad12-cmd.github.io/rsiscreener/)**
> A full video showcase of the product. The clips below are lightweight previews of the same thing.

<br>

## See it work

**Isolate the extremes** — dim the whole wall to only the oversold or overbought names in one tap.

<img src="assets/clip-filters.webp" alt="Filtering the grid to oversold coins" width="100%">

**Search straight into the chart** — type a ticker, press enter, land in a full RSI chart.

<img src="assets/clip-search.webp" alt="Searching a coin and opening its chart" width="100%">

**Mark the setup** — a custom canvas chart with trendlines, colours, undo/redo and PNG export.

<img src="assets/clip-draw.webp" alt="Drawing trendlines on the RSI chart" width="100%">

**On any device** — the same scanner folds down to a two-column wall on a phone, with the timeframe strip, search, filters and live status intact.

<img src="assets/clip-mobile.webp" alt="The mobile scanner" width="300">

## How it fits together

The core design choice is centralization: the server owns market polling and RSI math, and every visitor
reads a small warm snapshot. It scales with a cache, not with the number of users.

```mermaid
flowchart LR
  B[Binance candles] --> E[Market engine<br/>RSI · Wilder smoothing] --> C[Warm snapshot cache] --> G[Scanner grid] --> Ch[Chart + drawing tools]
  S[(Supabase / Postgres)] -.-> A[Auth · JWT · settings] -.-> G
  P[Paddle billing] -.-> AC[Self-healing access] -.-> A
```

A single always-on Next.js server, no serverless cold starts. Webhooks can be missed, so subscription state
reconciles against Paddle on read: a paying customer is never locked out by a dropped event.

## Built with

<p>
  <img src="https://img.shields.io/badge/Next.js%2016-000000?style=flat&logo=nextdotjs&logoColor=white">
  <img src="https://img.shields.io/badge/React-20232A?style=flat&logo=react&logoColor=61DAFB">
  <img src="https://img.shields.io/badge/TypeScript-3178C6?style=flat&logo=typescript&logoColor=white">
  <img src="https://img.shields.io/badge/Tailwind%20CSS%20v4-38B2AC?style=flat&logo=tailwind-css&logoColor=white">
  <img src="https://img.shields.io/badge/Supabase-3ECF8E?style=flat&logo=supabase&logoColor=white">
  <img src="https://img.shields.io/badge/Postgres-4169E1?style=flat&logo=postgresql&logoColor=white">
  <img src="https://img.shields.io/badge/Paddle-ff8b5f?style=flat">
  <img src="https://img.shields.io/badge/Vitest-6E9F18?style=flat&logo=vitest&logoColor=white">
  <img src="https://img.shields.io/badge/PWA-5A0FC8?style=flat&logo=pwa&logoColor=white">
</p>

Custom canvas charting engine · server-side market engine · warm-cache snapshots · email OTP auth · settings sync · installable PWA.

<br>

<div align="center">
<sub>This repository is a showcase. It documents the product and how it is built, without exposing the private source.</sub><br>
<sub>Built by Hamad · <a href="https://hamad12-cmd.github.io/rsiscreener/">view the full showcase</a></sub>
</div>

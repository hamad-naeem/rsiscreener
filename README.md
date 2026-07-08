# RSI Screener

Public showcase for a private production SaaS.

<p align="center">
  <img src="assets/readme-hero.png" alt="RSI Screener landing page" width="100%">
</p>

## What This Is

RSI Screener is a crypto market scanner for Binance spot pairs. It turns hundreds
of RSI readings into one visual grid, so a trader can scan the whole market
instead of opening chart after chart.

The source code is private. This repository shows the product, the workflow, and
the engineering shape behind it.

## Desktop Scanner

<p align="center">
  <img src="assets/desktop-flow.gif" alt="RSI Screener desktop scanner animation" width="100%">
</p>

<p align="center">
  <img src="assets/readme-desktop.png" alt="RSI Screener desktop demo" width="100%">
</p>

## Mobile

<p align="center">
  <img src="assets/readme-mobile.png" alt="RSI Screener mobile demo" width="360">
</p>

## Product Scope

- 300+ Binance spot pairs in one grid
- RSI mini-chart per symbol
- Oversold and overbought filters
- Sample and live demo modes
- Private full tool with 15 timeframes
- Chart modal with zoom, pan, drawing tools, undo/redo, and PNG export
- Email OTP auth, JWT sessions, user settings, Paddle billing, and protected access

## Architecture

```mermaid
flowchart LR
  Binance[Binance public candles] --> Engine[Server-side market engine]
  Engine --> Cache[Warm RSI cache]
  Cache --> Grid[Live screener grid]
  Grid --> Chart[Chart and drawing workflow]

  Supabase[(Supabase Postgres)] --> Auth[Email OTP and JWT sessions]
  Auth --> Grid

  Paddle[Paddle checkout] --> Webhooks[Webhook handlers]
  Webhooks --> Supabase
  Paddle --> Reconcile[Subscription reconciliation]
  Reconcile --> Supabase
```

The main backend decision is simple: browsers do not call Binance directly for
every tile. The server computes market state once, keeps it warm, and the UI
reads small snapshots.

## Stack

`Next.js 16` &middot; `React` &middot; `TypeScript` &middot; `Tailwind CSS v4` &middot; `Supabase`
&middot; `Paddle` &middot; `JWT` &middot; `Vitest` &middot; `PWA` &middot; `Playwright`

## Pricing Surface

<p align="center">
  <img src="assets/readme-pricing.png" alt="RSI Screener pricing page" width="100%">
</p>

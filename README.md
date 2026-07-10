<img src="assets/crate-open.svg" width="100%" alt="The crate from the profile, opened: glowing dots rise out. RSI Screener, live RSI for the entire Binance spot market. 300+ pairs, 15 timeframes, refresh under 10 seconds, source private.">

<img src="assets/wall.png" alt="RSI Screener: the live market wall, 300+ pairs with RSI sparklines on the 4h timeframe" width="100%">

<div align="center">
<sub>You pressed the crate. This is what's inside — the real logged-in product, captured live. Source private.</sub>
</div>

<br>

<p align="center">
  <a href="assets/extremes.png"><img src="assets/tour-01.png" width="32.8%" alt="Frame 01, isolate the extremes: overbought filter lights a handful of tiles while the wall dims back"></a>
  <a href="assets/chart.png"><img src="assets/tour-02.png" width="32.8%" alt="Frame 02, search into the chart: BTC/USDT RSI chart with live reading"></a>
  <a href="assets/drawing.png"><img src="assets/tour-03.png" width="32.8%" alt="Frame 03, mark the setup: two trendlines drawn with the toolbar active"></a>
</p>

<div align="center">
<sub>One tap isolates the extremes. Search lands straight in the chart. Trendlines are drawn on a hand-built canvas engine. Press a frame for full resolution.</sub>
</div>

<br>

## How it works

**The server does the market work once. Every visitor reads a warm snapshot.**

- A background engine polls Binance and recomputes RSI (Wilder's, 14-period) the moment each candle closes
- Results sit in an in-memory cache as compact per-timeframe snapshots; every response is tiny JSON
- The wall paints progressively, never a spinner; filter and search run client-side, so they're instant
- The chart is a hand-built canvas engine: zoom, pan, trendlines, undo/redo, PNG export, live while open
- JWT sessions with email OTP; subscription access self-heals against the billing API on every read
- Rate limits, per-user locks, strict validation; Vitest covers the RSI math and webhook signatures

<div align="center">

<samp>binance → engine → warm cache → wall → chart</samp>

</div>

<br>

<div align="center">

<samp>Next.js 16 · React · TypeScript · Tailwind v4 · shadcn/ui · Supabase · Postgres · JWT · Vitest · PWA</samp>

<sub>Built by <a href="https://github.com/hamad-naeem">Hamad Naeem</a> with AI-assisted development. This repository documents the product without exposing the private source.</sub>

</div>

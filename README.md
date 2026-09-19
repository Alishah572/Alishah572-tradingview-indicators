# SMC Order Flow Intelligence Engine

A modular **TradingView Pine Script v6** market-context engine combining Smart Money Concepts structure with bounded, non-repainting approximations of order-flow behavior.

> This script is a context engine, not a promise of profitability.

## Install

1. Open TradingView → **Pine Editor**.
2. Copy `pine/smc_order_flow_intelligence_engine.pine` into a new script.
3. Save and add to chart.
4. Configure modules from **MODULE ENABLE/DISABLE** and per-engine groups.
5. For alerts, use **Once Per Bar Close** (recommended).

---

## Data model honesty

The script explicitly separates data classes:

- **Direct TradingView data**: OHLCV, session/time states, VWAP input, `request.security` values.
- **Derived measurements**: pivots/structure, premium-discount/dealer range, profile POC approximation, regime scoring.
- **Heuristic/estimated measurements**: Estimated Order Flow, Estimated Delta/CVD, OHLCV Footprint Approximation, Liquidity Behavior Model.

### Important limitations

TradingView Pine does **not** provide full historical DOM, exchange aggressor flags, or true bid/ask tape reconstruction. Therefore:

- Delta/CVD is **Estimated Delta/CVD**, not exchange-true aggressor flow.
- Footprint module is an **OHLCV Footprint Approximation**.
- Liquidity behavior and confluence are model outputs, not probabilities.

---

## Non-repainting policy

The implementation is built to avoid repainting behavior in signal logic:

- No `lookahead_on` is used.
- Pivot events are consumed only after right-side confirmation (`ta.pivothigh/low`).
- HTF context uses **confirmed HTF bars** via `request.security(..., close[1]/ema[1], lookahead_off)`.
- Alerts can be gated to confirmed bar-close (`Require bar close confirmation`).

### Realtime note

Developing values on the current open bar can still change until close. This is expected and documented.

---

## Engine modules

All major modules can be independently toggled.

### 1) Data Engine
- ATR, bar anatomy, CLV, relative volume.

### 2) Market Structure Engine
- Major/minor confirmed pivots.
- HH/HL/LH/LL, BOS, CHoCH, MSS.

### 3) Liquidity Engine
- EQH/EQL style pool detection from confirmed swings with ATR tolerance.
- Bounded liquidity lines with aging cleanup.

### 4) Liquidity Sweep Engine
- Sweep confirmation when wick breaches pool and closes back through it.

### 5) Liquidity Behavior Model
- Sweep + delta/body confirmation labels as bullish/bearish reversal behavior.

### 6) Estimated Order Flow Engine
- Configurable pressure models from OHLCV (candle direction, CLV, body/range, wick pressure, composite).

### 7) Delta/CVD Engine + CVD Regime/Divergence
- Estimated buy/sell volume, estimated delta, cumulative delta with reset modes.
- CVD regime and pivot-based CVD divergence.

### 8) Footprint-Style OHLCV Approximation
- Body/wick volume partition approximation and imbalance ratio.

### 9) Absorption & Exhaustion
- Relative-volume and candle-anatomy heuristics for estimated absorption/exhaustion.

### 10) Displacement
- Wide body + high RVOL impulse detection.

### 11) FVG/IFVG Engine
- 3-candle fair-value gap detection and bounded zone lifecycle.
- IFVG event when invalidation criteria are met.

### 12) Order Blocks + Breaker Blocks
- OB zone derivation from pre-break opposing candles.
- Breaker state when OB is invalidated.

### 13) Premium/Discount + Dealer Range
- Premium/discount from latest confirmed major swing range.
- Dealer range midpoint from configurable rolling window.

### 14) VWAP Engine
- Session VWAP and optional deviation bands.
- VWAP reclaim/loss events.

### 15) Volume Profile Approximation
- Bounded rolling-bin profile with decay and estimated POC migration.

### 16) Session Engine
- Asia/London/New York session states and session bias.

### 17) SMT / Intermarket Divergence
- Confirmed pivot divergence vs comparison symbol.

### 18) Non-Repainting MTF Context
- Confirmed HTF close/EMA trend state.

### 19) Market Regime + Weighted Confluence
- Trend/range regime from EMA spread and ATR%.
- Weighted bullish/bearish confluence scoring (not probabilistic).

### 20) Entry/Event Sequence State
- Stage model (`SWEEP -> BUILD -> READY`) for long/short progression.

### 21) Dashboard, Visuals, Alerts, Performance Management
- Table dashboard with context summary.
- Event markers and optional zone rendering.
- Bounded arrays/caps for labels/lines/boxes with oldest-object deletion.

---

## Alerts included (implemented conditions only)

- Structure: BOS, CHoCH, MSS.
- Liquidity: sweep high/low, sweep behavior confirmations.
- Divergence: CVD divergence, SMT divergence.
- Imbalance: FVG, IFVG.
- Footprint heuristics: absorption, exhaustion.
- Displacement.
- OB + breaker events.
- VWAP reclaim/loss.
- Profile POC migration up/down.
- Confluence threshold (bull/bear).
- Setup stage change.

---

## Recommended presets

These are starting points; adjust to instrument/venue volatility.

### SCALPING
- Major/Minor: `7 / 2`
- Dealer range: `30`
- Profile lookback/bins: `60 / 18`
- Confluence threshold: `60`
- Keep sweeps, VWAP, CVD, displacement enabled.

### INTRADAY (default profile)
- Major/Minor: `10 / 3`
- Dealer range: `50`
- Profile lookback/bins: `120 / 24`
- Confluence threshold: `65`

### SWING
- Major/Minor: `20 / 5`
- Dealer range: `100`
- Profile lookback/bins: `240 / 30`
- Confluence threshold: `70`
- Use HTF context enabled and stricter sweep confirmation.

---

## Performance and object limits

To stay safe in Pine limits and avoid unbounded growth:

- Liquidity lines, zones, and labels are capped by inputs.
- Oldest objects are deleted once caps are reached.
- Profile loop complexity is bounded by `Profile bins`.
- No expensive full-history object recreation loops are used.

---

## Practical usage notes

- Use this as a **market-context engine** for directional bias, liquidity mapping, and event sequencing.
- Treat confluence as a weighted checklist score, **not probability**.
- Combine with risk controls, execution rules, and instrument-specific testing.

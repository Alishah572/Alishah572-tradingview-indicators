# SMC Order Flow Intelligence Engine

Phase 1 foundation for a modular TradingView Pine Script v6 market-context indicator.

## Install

1. Open TradingView and open the Pine Editor.
2. Copy `pine/smc_order_flow_intelligence_engine.pine` into a new script.
3. Save, add it to a chart, and configure the inputs.
4. For alerts, use **Once Per Bar Close** to align with the non-repainting policy.

## Current capabilities

- Confirmed major and minor pivot structure.
- HH, HL, LH, LL labels.
- Confirmed BOS, CHoCH, and MSS markers.
- OHLCV-derived estimated buy volume, sell volume, delta, delta percentage, and relative volume.
- CVD with selectable reset modes, moving average, momentum, slope, acceleration, and regime classification.
- Session VWAP and optional deviation bands.
- Premium/discount context from the latest confirmed major range.
- Configurable dashboard and candle-coloring modes.
- Phase 1 alerts for confirmed structure events, VWAP reclaim/loss, and setup-state changes.

## Data honesty

TradingView Pine generally does not expose a complete historical order book or exact exchange-level aggressor-side bid/ask volume. Accordingly, order flow, delta, CVD, and footprint-style concepts in this project are **estimated from OHLCV** and must not be interpreted as exact bid/ask measurements.

Confirmed pivots intentionally appear after their right-side confirmation bars. No `lookahead_on` requests or future data are used.

## Planned phases

1. Core foundation — implemented here.
2. Liquidity pools, equal highs/lows, and sweep behavior model.
3. Footprint approximation, imbalance, absorption, exhaustion, and displacement.
4. FVG, IFVG, order blocks, and breaker lifecycle management.
5. Volume profile approximation and expanded auction analysis.
6. Session statistics and SMT/intermarket divergence.
7. Confirmed multi-timeframe context.
8. Weighted confluence, event sequencing, signal states, and expanded dashboard/alerts.
9. Performance review and final compilation validation.

This indicator is a market-context tool, not a guarantee of profitability or a substitute for risk management.

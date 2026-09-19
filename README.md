# OHLCV Estimated CVD / Delta Pressure Analyzer

Focused TradingView Pine Script v6 indicator for readable **OHLCV-derived**:

- Estimated per-bar delta pressure
- Estimated average delta
- Estimated cumulative volume delta (CVD)
- Estimated CVD average

> This script uses OHLCV heuristics. It does **not** provide true exchange aggressor bid/ask volume, DOM, or exact footprint/tape data.

## Install

1. Open TradingView → **Pine Editor**.
2. Copy `pine/smc_order_flow_intelligence_engine.pine` into a new script.
3. Save and **Add to chart**.
4. Keep it in a separate oscillator pane (the script uses `overlay = false`).

## Pine limitation: multiple panes

A single Pine script cannot create multiple independent TradingView panes.

If you want pressure, delta average, and CVD in fully separate panes, duplicate this script (or split the logic into multiple scripts) and enable only the section you need in each copy.

## Inputs

### Data

- **Volume MA Length**
- **Delta Estimation Method**:
  - Candle Direction
  - CLV
  - Body/Range
  - Wick Pressure
  - Composite

### Delta

- **Average Delta Length**
- **CVD Reset Mode**: Never, Daily, Weekly, Session, Custom
- **Custom Reset Session**
- **CVD Average Length**

### Display

- Show/hide per-bar estimated pressure, estimated delta, average delta, CVD, CVD average, and zero line
- Histogram/column style toggles for pressure and delta
- Optional normalization for readability when CVD scale dominates

### Alerts

- Bar-close confirmation option
- Estimated positive/negative pressure
- Estimated delta crossing average
- Estimated CVD crossing average
- Estimated CVD crossing zero line

## Interpretation

- **Estimated Per-Bar Delta Pressure (OHLCV %)**: bounded oscillator derived from candle/volume behavior, not true order-book aggression.
- **Estimated Delta Per Bar**: estimated buy volume minus estimated sell volume from OHLCV pressure.
- **Estimated Average Delta**: moving average of estimated delta.
- **Estimated CVD**: cumulative sum of estimated delta (with chosen reset mode).
- **Estimated CVD Average**: moving average of estimated CVD.

## Non-repainting notes

- No lookahead requests are used.
- Values on the currently forming bar can change until bar close.
- Use bar-close-confirmed alerts if you want closed-bar signals.

## TradingView compile note

Pine compilation cannot be verified from this repository environment. Please compile in TradingView Pine Editor before production use.

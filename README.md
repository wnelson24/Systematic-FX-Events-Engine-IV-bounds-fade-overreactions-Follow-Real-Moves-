# Systematic-FX-Events-Engine-IV-bounds-fade-overreactions-Follow-Real-Moves-

# Systematic FX Event Engine – IV Expected Move Dislocation Strategy (EURUSD)

This project implements a **systematic short-horizon FX trading strategy** designed for **macro event windows** such as **CPI, NFP and FOMC**. The engine trades **EURUSD** using a **two-regime model**:
- **Fade regime** – exploits **stop-run overreactions** and liquidity grabs
- **Follow regime** – captures **genuine macro repricing momentum**

The boundary between the two is defined using **implied-volatility expected move logic**, a framework widely used on **FX options desks**.

---

## 🔥 Trading Logic – Core Intuition

Markets price in an **expected move** ahead of macro events via **ATM IV**. Price reactions outside this expected range are usually either:
✅ **Informed repricing** (real macro shock) → tends to continue  
❌ **Liquidity overreaction** (stop hunts, thin depth) → tends to revert  

This engine classifies each reaction into one of those buckets using IV boundaries.

---

## ⚙️ Strategy Architecture

| Component | Description |
|-----------|-------------|
| Signal Engine | Detects IV-boundary breach at 45s post-event |
| Regime Selection | Fade small mispricing, follow real surprise |
| Exit Logic | Time-based exit at 180s |
| Validation | 70/30 train-validation split (no leakage) |
| Optimisation | Grid search on train only |
| Metrics | Annualised Sharpe, Max DD, Rolling Sharpe |

---

## ✅ Assumptions (Transparent)

This project is built on **realistic FX microstructure behaviours**:

| Assumption | Justification |
|-------------|---------------|
| Markets overreact around macro events | Stop hunts + thin liquidity + dealer hedging |
| IV represents expected move | Standard FX options risk framework |
| Surprise size matters | Large surprises carry information → trend persistence |
| Noise overshoots cluster | Small surprises = more fading opportunities |
| Mean reversion vs momentum depends on surprise | Empirically observed across EURUSD |
| Realised IV ≈ proxy for ATM IV | Acceptable for synthetic research |
| Train on past → validate forward | Standard systematic workflow |

---

## 🧪 Synthetic Data Justification

This version uses **realistic synthetic EURUSD event reactions** because **1-minute FX event data is not freely accessible**. The synthetic generator is calibrated to mimic:

- Realistic **IV distribution** (8–12%)
- **Shock sizes** based on expected move scaling
- **Fat-tailed macro surprises**
- **Overreaction wicks** simulating orderbook sweeps
- **Mean reversion / drift dynamics**
- **Asymmetric USD response** to macro shocks

It’s synthetic, but **structurally accurate** and **useful for modelling research logic**.

---

## ✅ Results (Example Validation Run)

| Metric | Value |
|--------|--------|
| Validation Trades | 26 |
| Annualised Sharpe | **3.51** |
| Max Drawdown | **2.86 bps** |
| Rolling Sharpe (10 trades) | -0.12 → 1.22 |

---

## 🚧 Limitations

- No slippage, spread, or latency yet modelled  
- Synthetic-only version (real FX data version coming next)  
- No volatility regime filter  
- No position sizing logic yet  


So:
System activates when event realised vol exceeds IV.
If surprise is statistically small (according to standard normal dist of previous event surprises) -> mean-revert to IV bound
If surprise is statistically large (according to standard normal dist of previous event suprises) -> follow the move for 180s

If event realised vol does not exceed IV, then system is inactive.

## Systematic FX Alpha from State-Space Neural Networks

**Tech Stack:** Python, PyTorch, Pandas, NumPy, Matplotlib  
**Domain:** Quantitative Finance, Machine Learning, Systematic Trading  
**Docs:** [📄 2-Page Technical Summary (PDF)](/pdf/fx_trading_systematic.pdf) • [📦 Code Repo](#) • [📊 Tear-Sheet / Paper-Trade Log](#)

### One-liner
State-space model on hourly FX that produces **distributional** (Student-t) forecasts at 24h and 168h; z-score gating turns forecasts into a **USD-neutral, risk-controlled** cross-sectional strategy.

---

### At a glance (net of costs)
**In-Sample (2019–2024)**  
- **Sharpe:** 2.20 **CAGR:** 7.16% **Max DD:** −11.09% **Win:** 51.3%

**Out-of-Sample (Feb–Aug 2025)**  
- **Sharpe:** 1.75 **CAGR:** 15.87% **Max DD:** −3.85% **Win:** 52.7%

> Costs modeled with pair-specific spreads (~0.9–1.5 pips), half-spread at entry and exit.

---

### Why it works
Instead of point estimates, the model predicts **Student-t parameters** \((μ, s, ν)\) per horizon. We convert them to a **confidence score**:
`z = μ / s` (or use `σ` with `σ = s * sqrt(ν / (ν − 2))` for `ν > 2`)

We only act when confidence is high.  
- **168h** drives entries (top/bottom deciles by |z|).  
- **24h** refines timing/sizing (boost when aligned, haircut when opposed).  
- Signals aggregate into a **USD-neutral long/short book** to avoid USD drift.

---

### Architecture (deterministic state-space)
- **Encoder:** OHLCV + engineered features → latent \(e_t\)  
- **Recurrent core:** unroll 512h windows, \(h_t = f_\theta(h_{t-1}, e_t)\)  
- **Forecast head:** **decode from the final state** of each window to Student-t \((μ, s, ν)\) for **24h & 168h**  
- **Modes:** deterministic for stable, low-variance signals (stochastic ablations used in research)

*Universe:* EURUSD, GBPUSD, AUDUSD, NZDUSD, USDCAD, USDCHF, USDJPY (2018–2025).

---

### Risk & portfolio construction
- **Selection:** rank by \(|z_{168h}|\); long strongest positives, short strongest negatives  
- **Sizing:** volatility targeting + uncertainty scaling (bigger \(|z|\) → larger but capped)  
- **Exits:** dynamic **σ-scaled** TP/SL (σ implied from \(s, ν\))  
- **Controls:** per-pair & gross leverage caps; weekend flattening; drawdown brake (halve risk after −7% until 50% recovery)

---

### Validation
- **Ventile analysis:** 168h shows a clear monotonic lift vs. realized returns; 24h is noisier but adds timing value  
- **True OOS:** **Feb–Aug 2025** held out  
- **Costs on:** pair-specific spreads; turnover managed (avg hold ~2–4 days)

---

![Ventiles (24h/168h)](/images/z-score-ventiles.png)

---

### Implementation notes
- Temporal splits with strict anchoring (no look-ahead)  
- Deterministic state-space in PyTorch; Student-t likelihood for fat tails  
- Low-latency inference (<10 ms) suitable for intraday decisioning  
- Reproducible configs/seeds; per-symbol modeling with shared embeddings

---

### Contributions
1. **Uncertainty-aware trading:** σ-based sizing/exits from distributional forecasts  
2. **Multi-horizon fusion:** 168h entries + 24h timing without hard agreement rules  
3. **USD-neutral cross-section:** mitigates directional USD risk while harvesting spread  
4. **Deterministic state-space → portfolio:** clean link from latent state to deployable signals

---

### Files & docs
- **Technical Summary (PDF):** method, IS/OOS, risk, caveats  
- **Training & Backtest:** PyTorch world model + portfolio simulator with costs  
- **Analysis:** ventiles, thresholds, sensitivity (spreads, turnover, caps)

> *Backtested research; not investment advice. Paper-trading validation in progress — see tear-sheet for updates.*

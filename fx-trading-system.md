## Systematic FX Alpha from State-Space Neural Networks

**Tech Stack:** Python, PyTorch, Pandas, NumPy, Matplotlib  
**Domain:** Quantitative Finance, Machine Learning, Algorithmic Trading  
**Research Paper:** [📄 State-Space FX Alpha: Technical Summary (PDF)](/pdf/fx_trading_systematic.pdf)

### Executive Summary

I developed a sophisticated FX trading system that combines deterministic state-space neural networks with confidence-gated signal generation to achieve **Sharpe ratios exceeding 2.0** on in-sample data and **1.75 out-of-sample**. The system processes hourly FX data across 7 major currency pairs through a Dreamer-style world model to predict multi-horizon returns, then uses statistical confidence gating and advanced portfolio construction to generate consistent alpha.

### Key Innovation: Confidence-Weighted Signal Generation

The core breakthrough is using **distributional forecasts** rather than point estimates. For each currency pair and time horizon (24h, 168h), the model predicts not just expected returns (μ) but also uncertainty (σ), allowing us to compute confidence scores:

```
z-score = μ / σ
```

**Signal Logic:**
- **Entry decisions** based on 168h z-scores exceeding calibrated thresholds
- **24h horizon** acts as a timing/sizing filter (boost positions when aligned, reduce when opposed)
- **No requirement for both horizons to agree** - this allows more flexibility while maintaining signal quality

### Architecture: Deterministic State-Space Networks

**Model Components:**
- **Observation Encoder**: Transforms OHLCV + derived features into latent embeddings
- **RSSM (Recurrent State Space Model)**: Learns temporal market dynamics in latent space  
- **Distributional Head**: Predicts Student-t parameters (μ, σ, ν) for heteroscedastic returns
- **Deterministic Mode**: Direct latent mapping for cleaner trading signals (vs stochastic training mode)

The model processes 512-hour sequences across 7 FX majors (EURUSD, GBPUSD, AUDUSD, NZDUSD, USDCAD, USDCHF, USDJPY) using data from 2018-2025.

### Advanced Risk Management

**Portfolio Construction:**
- **USD-neutral cross-sectional selection**: Top-K strongest longs, bottom-K strongest shorts by |z_168h|
- **Volatility-aware position sizing**: Scale by predicted uncertainty and realized volatility
- **σ-scaled exits**: Dynamic take-profit/stop-loss based on model confidence rather than fixed percentages

**Risk Controls:**
- **Drawdown brake**: Halve exposure during >7% drawdowns until 50% recovery
- **Portfolio volatility targeting**: Scale to maintain ~30% annualized vol
- **Weekend risk management**: Flatten positions before gaps
- **Per-pair and gross leverage caps**

### Signal Validation: Ventile Analysis

I validated predictive power by sorting forecasts into ventiles (20 equal buckets) based on z-scores:
- **168h horizon**: Shows clear monotonic relationship between z-scores and realized returns
- **24h horizon**: Noisier but provides valuable timing information for position sizing

### Performance Results

**In-Sample (2019-2024):**
- Sharpe Ratio: **2.20**
- CAGR: **7.16%**  
- Max Drawdown: **-11.09%**
- Win Rate: **51.3%**

**Out-of-Sample (2025 YTD):**
- Sharpe Ratio: **1.75** 
- CAGR: **15.87%**
- Max Drawdown: **-3.85%**
- Win Rate: **52.7%**

*All results are net of realistic transaction costs (0.9-1.5 pip spreads)*

### Technical Implementation

**Data Pipeline:**
- Hourly FX data with comprehensive feature engineering (15+ derived features per symbol)
- Per-symbol standardization to prevent look-ahead bias
- Train/validation/test temporal splits with proper out-of-sample evaluation

**Model Training:**
- Deterministic state-space architecture for production consistency
- Student-t likelihood for robust tail modeling
- <10ms inference latency for real-time trading

**Cost Modeling:**
- Realistic spread-based transaction costs
- Half-spread charged at entry/exit
- Turnover optimization (average hold ~2-4 days)

### Research Contributions

1. **σ-based Risk Management**: Using model uncertainty for dynamic position sizing and exits
2. **Multi-horizon Signal Fusion**: Systematic combination of 24h/168h forecasts without requiring agreement
3. **Cross-sectional USD Neutrality**: Market-neutral FX momentum strategy
4. **Deterministic State-Space Trading**: First application to systematic FX alpha generation

### Files & Documentation

- **Research Paper**: 2-page professional summary with performance metrics and methodology
- **Training Pipeline**: PyTorch implementation with deterministic/stochastic modes
- **Backtesting Engine**: Full portfolio simulation with realistic costs and risk controls
- **Signal Analysis**: Comprehensive ventile studies and threshold calibration

This system demonstrates how modern deep learning can be combined with rigorous quantitative finance principles to generate sustainable alpha in competitive FX markets.

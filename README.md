# GeoCVaR — Climate Tail Risk & Spatial Systemic Risk Modeling

A Monte Carlo-based climate risk stress testing framework that quantifies extreme portfolio losses under correlated climate hazards, nonlinear damage dynamics, regime switching, and t-copula dependence — with CVaR-constrained optimization and climate risk pricing.


---

## Overview

Traditional climate risk models rely on linear vulnerability scores, independent hazard assumptions, and Gaussian dependence — all of which **systematically underestimate tail risk**. This project builds an institutional-grade climate risk engine that captures:

- **Formalized stochastic processes** — GBM-based hazard intensity with drift and regime-dependent volatility
- **Regime-switching** — Markov-chain Normal/Extreme climate states
- **Student-t copula** — joint tail dependence (replacing zero-tail-dependence Gaussian copulas)
- **Nonlinear damage functions** — convex polynomial/threshold damage curves
- **GPD tail fitting** — Generalized Pareto validation of extreme regime tails
- **CVaR-constrained optimization** — mean-variance optimization with climate tail risk budgets
- **Climate risk pricing** — risk premiums, mispricing detection, carbon-adjusted Sharpe ratios

---

## Key Results

| Model | CVaR (95%) | vs Baseline |
|-------|-----------|-------------|
| Linear Correlated (Baseline) | 1,008,818 | x1.00 |
| Nonlinear Damage | 1,156,565 | x1.15 |
| Gaussian Regime-Switching | Higher | x1.2+ |
| **t-Copula Regime-Switching (v=5)** | **Higher** | **x1.3+** |
| CVaR-Constrained Portfolio | Lower | Optimized |

**Key findings:**
- Gaussian copula has **zero tail dependence**; t-copula (v=5) produces up to **46% joint tail probability**
- ~47% of tail risk concentrated in just 5 assets
- Nonlinear damage produces ~15% CVaR uplift over linear models
- Multiple assets identified where **climate risk premium exceeds expected return** (mispriced risk)

---

---

### Running the Analysis

1. Place `Geo CVaR Input Data Contract.xlsx` in `data/` (see `data/README.md` for schema)
2. Run the notebook:
   ```bash
   jupyter notebook notebooks/GeoCVaR_Full_Analysis.ipynb
   ```

---

## Input Data Format

| Sheet | Key Columns |
|-------|-------------|
| **Asset Inventory** | `asset_id`, `country`, `region`, `primary_fuel`, `capacity_mw`, `asset_value_mln_usd`, `latitude`, `longitude` |
| **Vulnerability Rules** | `asset_type`, `primary_fuel`, `hazard_type`, `vulnerability_score` |
| **Scenario Settings** | `scenario`, `description`, `temp_increases_in_C` |

---

## Technical Details

### Damage Functions

| Hazard | Function | Cap |
|--------|----------|-----|
| Heat | 0.15 x max(0, I - 1.1)^3 | 0.80 |
| Flood | 0.20 x I^2 | 1.00 |
| Storm | 0.25 x I^3 | 1.00 |
| Drought | 0.20 x I^2 | 0.70 |

### Regime Parameters

| Parameter | Normal | Extreme |
|-----------|--------|---------|
| Volatility (sigma) | 0.25-0.35 | 0.75-0.90 |
| Hazard correlations | 0.15-0.55 | 0.50-0.80 |
| P(self-transition) | 0.88 | 0.45 |
| Stationary probability | 82.1% | 17.9% |

### t-Copula

| Parameter | Value |
|-----------|-------|
| Degrees of freedom (v) | 5 |
| Max tail dependence (lambda) | ~0.46 |
| Gaussian tail dependence | 0.000 |

---


## References

- Artzner, P. et al. (1999). *Coherent Measures of Risk*. Mathematical Finance.
- Rockafellar, R.T. & Uryasev, S. (2000). *Optimization of Conditional Value-at-Risk*. Journal of Risk.
- Embrechts, P., McNeil, A.J. & Straumann, D. (2002). *Correlation and Dependence in Risk Management*.
- Burke, M. et al. (2015). *Global non-linear effect of temperature on economic production*. Nature.
- Balkema, A.A. & de Haan, L. (1974). *Residual Life Time at Great Age*. Annals of Probability.

---

## License

MIT License — see [LICENSE](LICENSE).

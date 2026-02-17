# GeoCVaR-Climate-Risk
A Monte Carlo–based climate risk stress testing framework that quantifies extreme portfolio losses under correlated climate hazards and nonlinear damage dynamics.


## Overview

Traditional climate risk models rely on linear vulnerability scores and independent hazard assumptions — both of which **underestimate tail risk**. This project builds a systemic, tail-focused climate risk engine that captures:

- **Correlated hazard shocks** (heat + drought strike together)
- **Nonlinear/convex damage functions** (losses accelerate past thresholds)
- **Regime-switching** (normal vs crisis climate states)
- **Contagion cascades** (regional spillovers amplify losses)
- **Multi-period path dependence** (consecutive stress years compound)


## Methodology

### Phase 1: Foundation (Completed)
1. **Monte Carlo Simulation Engine** — 10,000-path lognormal loss generation per asset–hazard–scenario
2. **Correlated Hazard Modeling** — Cholesky-decomposed correlation structure across Heat, Flood, Drought, Storm
3. **Nonlinear Damage Functions** — Convex (polynomial/exponential) damage curves with hazard-specific thresholds
4. **Tail Risk Measurement** — VaR and CVaR at 95% confidence; tail amplification ratios
5. **Spatial Concentration Analysis** — Geographic clustering of tail losses; HHI concentration index

### Phase 2: Advanced Extensions (Completed)
6. **Regime-Switching** — Markov-style Normal/Crisis states with distinct volatility and correlation parameters
7. **Climate Contagion** — Matrix-based regional spillover modeling
8. **CVaR Optimisation** — Rockafellar-Uryasev LP formulation for tail-constrained portfolio allocation
9. **Multi-Period Simulation** — 10-year paths with Markov transitions and cumulative degradation
10. **Stress Testing Dashboard** — Consolidated institutional-grade summary metrics and visualisations


## Input Data Format

The framework expects an Excel file with three sheets:

| Sheet | Key Columns |
|-------|-------------|
| **Asset Inventory** | `asset_id`, `country`, `region`, `primary_fuel`, `capacity_mw`, `asset_value_mln_usd`, `latitude`, `longitude` |
| **Vulnerability Rules** | `asset_type`, `primary_fuel`, `hazard_type`, `vulnerability_score` |
| **Scenario Settings** | `scenario`, `description`, `temp_increases_in_C` |

---

## Technical Details

### Damage Functions

| Hazard | Functional Form | Cap |
|--------|----------------|-----|
| Heat | `0.15 × max(0, I − 1.1)³` | 0.80 |
| Flood | `0.20 × I²` | 1.00 |
| Storm | `0.25 × I³` | 1.00 |
| Drought | `0.20 × I²` | 0.70 |

### Correlation Structure

|  | Heat | Flood | Drought | Storm |
|--|------|-------|---------|-------|
| **Normal** | Baseline | 0.3 | 0.6 | 0.2 |
| **Crisis** | Elevated | 0.7 | 0.8 | 0.6 |

### Regime Parameters

| Parameter | Normal | Crisis |
|-----------|--------|--------|
| Volatility (σ) | 0.4 | 1.0 |
| Probability | 85% | 15% |
| Markov persistence | 90% | 40% |

---

## Visualisations Produced

- Loss distribution histograms with VaR/CVaR lines
- Scenario comparison overlays (Orderly / Current / Hot House)
- Asset-level CVaR bar charts
- Geospatial CVaR heatmaps (world map)
- Normal vs Crisis regime distributions
- Pre/post contagion regional bar charts
- CVaR-optimal portfolio weights
- Equal-weight vs optimal loss distribution overlay
- Multi-period fan chart (10-year cumulative)
- Terminal loss distribution
- Year-by-year CVaR evolution
- 4-panel institutional dashboard

---

## Future Roadmap

- [ ] Calibrate contagion matrix from trade/energy-grid dependency data
- [ ] Add sector-level contagion channels
- [ ] Integrate real climate projection data (CMIP6 / NGFS scenarios)
- [ ] Build interactive Streamlit/Dash dashboard
- [ ] Add regulatory reporting templates (TCFD, EU taxonomy)
- [ ] Backtesting framework against historical climate events

---

## References

- Rockafellar, R.T. & Uryasev, S. (2000). *Optimization of Conditional Value-at-Risk*. Journal of Risk.
- NGFS Climate Scenarios (2022). Network for Greening the Financial System.
- Burke, M. et al. (2015). *Global non-linear effect of temperature on economic production*. Nature.

---

## License

This project is licensed under the MIT License — see [LICENSE](LICENSE) for details.

---

## Contributing

Contributions are welcome. Please open an issue first to discuss proposed changes.

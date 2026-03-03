# Methodology

## 1. Monte Carlo Loss Simulation

Individual asset losses are modeled as lognormal random variables:

```
L = BaseLoss × exp(σZ − ½σ²)
```

where `Z ~ N(0,1)` and `σ` is the climate damage volatility parameter. This ensures:
- Losses are non-negative (right-skewed)
- The mean is preserved: E[L] = BaseLoss
- Fat tails emerge naturally from the exponential transformation

**Base loss** is computed as:
```
BaseLoss = AssetValue × VulnerabilityScore × ScenarioSeverity
```

## 2. Correlated Hazard Structure

Climate hazards are not independent. We impose a correlation structure using Cholesky decomposition:

1. Define a correlation matrix across hazards (Heat, Flood, Drought, Storm)
2. Decompose: `Σ = LLᵀ` (Cholesky)
3. Transform independent shocks: `Z_corr = Z_indep × Lᵀ`

This ensures joint extreme events occur with realistic frequency (e.g., heat + drought).

## 3. Nonlinear Damage Functions

Linear vulnerability scores (e.g., 0.4 for heat) are replaced with convex damage curves:

| Hazard | Function | Behavior |
|--------|----------|----------|
| Heat | `0.15 × max(0, I − 1.1)³` | Threshold + cubic acceleration |
| Flood | `0.20 × I²` | Quadratic |
| Storm | `0.25 × I³` | Cubic |
| Drought | `0.20 × I²` | Quadratic |

All are capped to prevent damage fractions > 1. These capture the empirical observation that climate damages accelerate past critical thresholds.

## 4. Tail Risk Metrics

- **VaR(α)**: The α-quantile of the loss distribution. "There is a (1−α) probability that losses exceed this level."
- **CVaR(α)**: The expected loss conditional on exceeding VaR. "If we're in the worst (1−α) scenarios, what's the average loss?"

CVaR is subadditive (unlike VaR), making it suitable for portfolio risk aggregation and optimisation.

## 5. Regime-Switching

Two climate states are modeled:

| Parameter | Normal | Crisis |
|-----------|--------|--------|
| Probability | 85% | 15% |
| Volatility (σ) | 0.4 | 1.0 |
| Hazard correlations | Baseline | Elevated (0.6–0.8) |

Each Monte Carlo path is assigned to one regime. This produces a mixture distribution with structurally fatter tails than any single-regime model.

For multi-period simulation, regime transitions follow a Markov chain:
```
P(Normal → Crisis) = 0.10
P(Crisis → Crisis) = 0.40  (crisis persistence)
```

## 6. Climate Contagion

Regional spillover is modeled via a contagion matrix `C`:

```
PostContagionLosses = C × PreContagionLosses
```

where `C[i,j]` is the fraction of region j's shock that spills into region i. The diagonal is 1 (own shock), off-diagonal entries represent spillover channels (supply chains, energy grids, financial linkages).

## 7. CVaR Optimisation

Portfolio weights are found by solving (Rockafellar & Uryasev, 2000):

```
min   ζ + 1/(N(1−α)) × Σⱼ uⱼ
s.t.  uⱼ ≥ w'Lⱼ − ζ    ∀j
      uⱼ ≥ 0             ∀j
      Σ wᵢ = 1
      wᵢ ≥ 0             ∀i
```

This is a linear program and can be solved efficiently even for large portfolios.

## 8. Multi-Period Dynamics

The simulation extends to T years with:
- **Markov regime transitions** at each time step
- **Cumulative degradation**: vulnerability increases by `δ × (cumulative_loss / portfolio_value)` per year, capturing infrastructure wear and reduced resilience
- **Path dependence**: unlike static stress tests, consecutive bad years compound losses nonlinearly

## 9. Concentration Metrics

- **Top-k share**: What fraction of tail risk comes from the top k assets?
- **HHI (Herfindahl–Hirschman Index)**: Sum of squared tail contribution weights. HHI → 0 means diversified; HHI > 0.2 means concentrated.

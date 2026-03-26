# Quant Finance Notebook — Implementation Handoff

> **For Claude on the user's computer:**
> This file is a complete, self-contained implementation brief.
> The user has no prior context in this session. Read this file fully before writing any code.
> Your only job is to implement the Jupyter notebook described below — nothing else.

---

## Context

The user has a **Quantitative Finance Formula Cheat Sheet** (image attached to their message or described below).
It covers 8 topic areas. They want a Jupyter notebook that:

- Explains every concept in **plain English** (assume zero finance background)
- Implements every formula in **Python with clear variable names**
- Shows a **concrete numeric worked example** for each formula with printed output explaining what the result means
- Is grouped into the same **8 sections** as the cheat sheet

---

## Output File

**Path:** `quant_finance_formulas.ipynb`
**Location:** root of this repository
**Format:** Jupyter Notebook (`.ipynb`)

---

## Cell Structure (repeat for every formula)

For each formula, create **3 cells in order**:

1. **Markdown cell** — concept name as heading + 2–4 sentence plain-English explanation of what it is and why it matters
2. **Code cell** — a clean Python function implementing the formula, with type hints and a one-line docstring
3. **Code cell** — a numeric example: call the function with realistic values, `print()` the result with a human-readable sentence explaining what the number means

---

## Dependencies

Use only these standard scientific Python packages — **no exotic installs**:

```
numpy
scipy
matplotlib
```

Add a **single setup cell at the very top** of the notebook:

```python
import numpy as np
import matplotlib.pyplot as plt
from scipy.stats import norm
from scipy.optimize import brentq
```

---

## Sections & Formulas

### 1. VOLATILITY

| Name | Formula |
|------|---------|
| Realized Volatility | `σ = sqrt( 1/(n-1) × Σ(rᵢ − r̄)² )` |
| Implied Volatility | Solve `C_mkt = C_BS(S, K, T, r, σ_impl)` for `σ_impl` using Brent's method (use the Black-Scholes formula below) |
| Forward Volatility | `σ²(T1,T2) = ( σ²(0,T2)·T2 − σ²(0,T1)·T1 ) / (T2 − T1)` |
| Cumulative Return | `R = Π(1 + rᵢ) − 1` |

---

### 2. RISK METRICS

| Name | Formula |
|------|---------|
| VaR (parametric) | `VaRα = μ − zα · σ` |
| Sharpe Ratio | `Sharpe = (E[R] − Rf) / σ` |
| Sortino Ratio | `Sortino = (E[R] − Rf) / σ_down`  where `σ_down` = std of **negative** returns only |
| RAROC | `RAROC = Expected Return / Economic Capital` |

---

### 3. OPTIONS & GREEKS

| Name | Formula |
|------|---------|
| Black-Scholes (call price) | `C = S·N(d1) − K·e^(−rT)·N(d2)` where `d1 = (ln(S/K) + (r + σ²/2)·T) / (σ·√T)` and `d2 = d1 − σ·√T` |
| Delta | `Δ = ∂C/∂S = N(d1)` |
| Gamma | `Γ = ∂²C/∂S² = N'(d1) / (S·σ·√T)` |
| Vega | `ν = ∂C/∂σ = S·N'(d1)·√T` |

> Note: `N(x)` = standard normal CDF, `N'(x)` = standard normal PDF

---

### 4. CREDIT RISK & EXECUTION

| Name | Formula |
|------|---------|
| LTV (Loan-to-Value) | `LTV = Loan / Collateral Value` |
| Expected Loss | `EL = PD × LGD × EAD` where PD = probability of default, LGD = loss given default, EAD = exposure at default |
| TWAP | `TWAP = (1/T) × Σ P(t)` (average price over equal time intervals) |
| VWAP | `VWAP = Σ(Pᵢ × Vᵢ) / ΣVᵢ` (volume-weighted average price) |

---

### 5. STOCHASTIC PROCESSES

| Name | Formula |
|------|---------|
| Brownian Motion | `dWt ~ N(0, dt)` — simulate a path of `n` steps |
| Ito Process | `dXt = μ·dt + σ·dWt` — simulate using Euler–Maruyama |
| Ito's Lemma | `df = (∂f/∂t + μ·∂f/∂x + σ²/2·∂²f/∂x²)dt + σ·∂f/∂x·dWt` — demonstrate with `f(x) = x²` |
| Mean-Reverting (OU) | `dXt = θ(μ − Xt)dt + σ·dWt` — simulate a path |
| Jump-Diffusion | `dSt = μ·St·dt + σ·St·dWt + J·dNt` — simulate using small time steps; `J` is jump size, `dNt` is Poisson increment |
| Quadratic Variation | `[X]t = lim Σ(X_{t+1} − Xt)²` — compute from a simulated path |

> For each stochastic process: also plot the simulated path using `matplotlib`.

---

### 6. DEPENDENCE & STATISTICS

| Name | Formula |
|------|---------|
| PCA | `Σ = V·Λ·Vᵀ` — eigendecomposition of a covariance matrix; show explained variance per component |
| Kalman Filter | `x_{t\|t} = x_{t\|t-1} + Kt·(zt − H·x_{t\|t-1})` — implement a simple 1D scalar version |
| Copula | `F(x,y) = C(FX(x), FY(y))` — demonstrate a Gaussian copula to generate correlated samples |
| CPPI | `Exposure = m · (Portfolio − Floor)` — simulate a CPPI strategy over time |
| APY | `APY = (1 + r/n)ⁿ − 1` |
| Capital Efficiency | `CE = Return / Capital Used` |

---

### 7. DeFi / AMM

| Name | Formula |
|------|---------|
| Impermanent Loss | `IL = 2·√P / (1 + P) − 1` where P = price ratio of asset vs entry price |
| Health Factor | `HF = (Collateral × Liquidation Threshold) / Debt` |
| Borrow Rate | `r = f(utilization)` — implement as a simple linear or kinked model |
| TVL | `TVL = Σ (quantity_i × price_i)` across all locked assets |

---

### 8. TRADING / EXECUTION

| Name | Formula |
|------|---------|
| Slippage | `Slippage = P_exec − P_expected` |
| Price Impact | `ΔP = λ · Q` where λ = market impact coefficient, Q = order size |

---

## Quality Requirements

- Every `print()` output must read as a **complete English sentence**, e.g.:
  `"The Sharpe ratio is 1.23, meaning the portfolio earns 1.23 units of return per unit of risk."`
- All plots must have **axis labels and a title**
- No cell should raise an error when run top-to-bottom in a freshly started kernel
- Do **not** use any packages outside numpy, scipy, and matplotlib
- Do **not** add any extra files — only `quant_finance_formulas.ipynb`

---

## Git Instructions

After completing the notebook:

```bash
git checkout -b claude/quant-finance-notebook-IyG45   # or switch to it if it exists
git add quant_finance_formulas.ipynb
git commit -m "Add quant finance formulas notebook with all 8 sections"
git push -u origin claude/quant-finance-notebook-IyG45
```

---

*End of brief. Implement the notebook now.*

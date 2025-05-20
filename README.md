# Options Pricing Model

This repository provides tools for evaluating the fair value of financial options—contracts that grant their holders the right (but not the obligation) to buy or sell an underlying asset at a set strike price before expiration. The project includes implementations of the Black-Scholes analytic formula, binomial tree models, and related utilities, as well as interactive web UIs built with Streamlit.

## Black-Scholes Model (1973)

Fisher Black, Myron Scholes, and Robert Merton introduced the Black-Scholes framework, which revolutionized option valuation by deriving closed-form solutions for European call and put prices. The formula inputs are:

- **S**: Current price of the underlying asset
- **K**: Strike price
- **T**: Time until expiration (in years)
- **r**: Continually compounded risk-free interest rate
- **σ**: Volatility of the underlying asset’s returns

The European call (C) and put (P) prices are:

```
C = S * N(d1) - K * e^{-rT} * N(d2)
P = K * e^{-rT} * N(-d2) - S * N(-d1)
```

where
```
d1 = [ln(S/K) + (r + σ²/2) T] / (σ√T)
d2 = d1 - σ√T
```

### Implementation

```python
from OptionsPricing import BlackScholesModel

bs = BlackScholesModel()
price = bs.premium(s=100, k=105, t=60/252, r=0.02, sigma=0.3, option_type='Call')
print(price)  # 3.9827...
```

A Streamlit app (`WebUI.py`) also lets you calculate and visualize results.

## Implied Volatility

Implied volatility is the σ that, when plugged into Black-Scholes, matches an observed market premium. This metric reflects trader expectations.

```python
from OptionsPricing import calc_implied_volatility

iv = calc_implied_volatility(s=10046, k=11000, t=27/252, r=0.02, premium=38, option_type='Call')
print(iv)  # 0.21707...
```

A companion Streamlit interface (`WebUI_impvol.py`) visualizes the process.

## Greeks

The “Greeks” measure how option prices react to changes in inputs:

- **Delta (Δ)**: ∂Price/∂S
- **Gamma (Γ)**: ∂Δ/∂S
- **Theta (Θ)**: ∂Price/∂T (time decay)
- **Vega (ν)**: ∂Price/∂σ
- **Rho (ρ)**: ∂Price/∂r

These sensitivities help traders understand and hedge their exposures.

## Project Layout

```
OptionsPricing/
├── BlackScholesModel.py
├── BinomialModel.py
├── ImpliedVolatility.py
├── WebUI.py
├── WebUI_impvol.py
├── requirements.txt
└── README_rewritten.md
```

## Installation

```bash
pip install -r requirements.txt
```

## References

- Black, F., & Scholes, M. (1973). The Pricing of Options and Corporate Liabilities.
- Cox, J., Ross, S., & Rubinstein, M. (1979). Option Pricing: A Simplified Approach.

# laplace-kinetics

A [Laplace](https://github.com/mlatinov/laplace) library of parametric nonlinear functions for pharmacokinetics, dose-response modeling, and growth/decay processes — ready to import into any `.laplace` model with namespaced calls (`kinetics::function_name(...)`).

Like all Laplace libraries, `kinetics` compiles down to plain, readable Stan functions. Nothing about how you use it hides what actually ends up in your `.stan` file.

## What's included

### Dose-response / pharmacology (`dose_response.laplacelib`)

Parametric functions for dose-response modeling, pharmacological effect and inhibition models, receptor binding and cooperativity, and biological assay calibration.

| Function | Description |
|---|---|
| `emax(E0, Emax, x, EC50)` | Parametric Emax dose-response: $E_0 + \frac{E_{\max}x}{EC_{50}+x}$ |
| `sigmoid_emax(E0, Emax, x, EC50, h)` | Sigmoid Emax / Hill: $E_0 + \frac{E_{\max}x^h}{EC_{50}^h+x^h}$ |
| `four_pl(d, a, x, b, c)` | Four-parameter logistic: $d + \frac{a-d}{1+(x/c)^b}$ |
| `imax(E0, Imax, x, IC50)` | Parametric Imax inhibitory response: $E_0 - \frac{I_{\max}x}{IC_{50}+x}$ |
| `inhibitory_emax(E0, Imax, x, IC50, h)` | Sigmoid inhibitory Emax: $E_0 - \frac{I_{\max}x^h}{IC_{50}^h+x^h}$ |
| `logistic_dose_response(L, U, x, k, xo)` | Logistic dose-response: $L + \frac{U-L}{1+\exp[-k(x-x_0)]}$ |
| `hill(x, n, K)` | Hill equation for binding/cooperativity: $\frac{x^n}{K^n+x^n}$ |

### Growth and decay (`growth_decay.laplacelib`)

Functions for biological/population growth, pharmacokinetic elimination, decay processes, organismal growth, and multi-phase exponential processes.

| Function | Description |
|---|---|
| `exponential_growth(A, k, x)` | Exponential growth: $Ae^{kx}$ |
| `exponential_decay(A, k, x)` | Exponential decay: $Ae^{-kx}$ |
| `logistic_growth(K, A, r, x)` | Logistic growth: $\frac{K}{1+Ae^{-rx}}$ |
| `gompertz(A, B, k, x)` | Gompertz growth: $Ae^{-Be^{-kx}}$ |
| `von_bertalanffy(Linf, B, k, t)` | Von Bertalanffy growth: $L_{\infty}(1-Be^{-kt})^3$ |
| `biexponential(A, B, k1, k2, t)` | Biexponential decay: $Ae^{-k_1t} + Be^{-k_2t}$ |

### Pharmacokinetics (`pharmacokinetics.laplacelib`)

Closed-form deterministic functions for one- and two-compartment PK models, IV bolus and infusion, first-order absorption, extravascular dosing, and Bateman kinetics.

| Function | Description |
|---|---|
| `one_comp_iv_bolus(D, V, k, t)` | One-compartment IV bolus: $\frac{D}{V}e^{-kt}$ |
| `one_comp_iv_infusion(R0, V, k, t)` | One-compartment IV infusion: $\frac{R_0}{Vk}(1-e^{-kt})$ |
| `one_comp_iv_post_infusion(R0, V, k, T, t)` | One-compartment IV post-infusion: $\frac{R_0}{Vk}(1-e^{-kT})e^{-k(t-T)}$ |
| `two_comp_iv_bolus(A, B, alpha, beta, t)` | Two-compartment IV bolus: $Ae^{-\alpha t} + Be^{-\beta t}$ |
| `bateman(F, D, V, ka, k, t)` | Bateman function (first-order absorption + elimination): $\frac{FDk_a}{V(k_a-k)}(e^{-kt}-e^{-k_at})$ |
| `first_order_absorption(D, ka, t)` | First-order absorption: $De^{-k_at}$ |

## Installation

`kinetics` is distributed as a git-hosted Laplace library — there's no published registry entry yet, so it's added by pointing `laplace` (or `cmdlaplacer`, if you're working from R) directly at the repository.

### Via the `laplace` CLI

From inside a Laplace project (a directory with its own `laplace.toml`):

```
laplace add kinetics --git https://github.com/mlatinov/laplace-kinetics --tag 0.1.0
```

### Via R (`cmdlaplacer`)

```r
library(cmdlaplacer)

laplace_install_git(
  "kinetics",
  "https://github.com/mlatinov/laplace-kinetics",
  tag = "0.1.0"
)
```

Either way, this pins the dependency in your project's `laplace.toml`/`laplace.lock` at tag `0.1.0`. Check the [releases](https://github.com/mlatinov/laplace-kinetics/tags) for newer tags as they become available.

## Usage

Import the library and call its functions with the `kinetics::` namespace prefix:

```
library {
    kinetics;
}

model {
    mu = kinetics::emax(E0, Emax, x, EC50);
    // ...
}
```

## License

See [LICENSE](./LICENSE).
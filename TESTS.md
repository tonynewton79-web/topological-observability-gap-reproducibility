# Test and Verification Guide

This document maps every executable check to the corresponding scientific statement.

## Pytest regression suite

The repository includes a standard `pytest` suite under `tests/`. It runs the four independent public reproductions and asserts their frozen release-level outputs:

```bash
pytest -q
```

The expected result is four passing test modules: exact symbolic certification, independent floating replay, single-ratio design replay, and correlated tomographic-bank replay. GitHub Actions runs this command automatically on every push and pull request.

## Test 1 — exact DESI DR2 certificate

Run:

```bash
python code/reproduce_exact.py
```

Governing reconstruction:

```math
u(z)=\sum_{n=0}^{3}a_nT_n(x(z)),\qquad x(z)=2z/2.33-1.
```

For the twelve DESI measurements linear in `u`, generalized least squares gives

```math
\hat a=(A^TC^{-1}A)^{-1}A^TC^{-1}y,
\qquad
S=(A^TC^{-1}A)^{-1}.
```

For a coefficient ellipsoid

```math
\delta a^TS^{-1}\delta a\le \Delta,
```

the largest possible derivative at fixed redshift is

```math
q(z)+\sqrt{\Delta\,V(z)},
\qquad
q(z)=u'(z),\quad V(z)=g(z)^TSg(z).
```

The exact sign certificate uses

```math
R_\Delta(z)=q(z)^2-\Delta V(z),
```

with `Delta=65/4`. Exact rational root counting verifies zero roots of both `q` and `R_Delta` on `[0,233/100]`, together with the required endpoint signs. This proves the derivative upper envelope remains negative over the full interval.

The same script checks the dark-energy topology polynomial

```math
G(z)=-2u(0)^2u'(z)-3\Omega_m(1+z)^2u(z)^3,
```

whose interior roots are the extrema of the normalized dark-energy density when the density stays positive. At `Omega_m=0.313` the five frozen histories reproduce counts

```text
best, A, B, C, D = 2, 1, 3, 4, 0.
```

Expected status:

```text
ALL_EXACT_CHECKS_PASS
```

## Test 2 — independent floating-point replay

Run:

```bash
python code/reproduce_numeric.py
```

This test deliberately uses ordinary NumPy/SciPy arithmetic rather than the exact symbolic route. It checks:

- cubic coefficient reconstruction;
- maximum derivative upper envelope;
- held-out `D_V/r_d` prediction;
- numerical witness root counts;
- the `chi^2_4` coverage of `Delta chi^2=16.25`.

Expected status:

```text
INDEPENDENT_FLOATING_REPLAY_PASS
```

The floating-point calculation is a regression/independence check. It is not the logical basis of the exact monotonicity theorem.

## Test 3 — single-ratio topology-resolution replay

Run:

```bash
python code/reproduce_single_ratio_design.py
```

For

```math
\beta(z_l,z_s)=\frac{\chi(z_s)-\chi(z_l)}{\chi(z_s)},
\qquad
R=\frac{\beta(z_l,z_{s1})}{\beta(z_l,z_{s2})},
```

define

```math
\delta_T=\left|\ln\frac{R_{K=0}}{R_{K=2}}\right|.
```

The local Gaussian resolution law is

```math
N_T=\frac{\delta_T}{\sqrt{\sigma_R^2+J_z^TC_zJ_z}}.
```

For zero residual ratio error and equal independent redshift errors, the replay verifies the hard calibration floors near

```text
3 sigma: sigma_z < 0.0021414
5 sigma: sigma_z < 0.0012849
```

at the frozen calibration-robust design.

Expected status:

```text
SINGLE_RATIO_DESIGN_REPLAY_PASS
```

## Test 4 — correlated tomographic bank

Run:

```bash
python code/reproduce_correlated_bank.py
```

For topology signal vector `d`, statistical covariance `C_stat`, nuisance Jacobian `B`, and nuisance prior covariance `Pi`, the marginalized separation is

```math
N_T^2=d^T(C_{\rm stat}+B\Pi B^T)^{-1}d.
```

The independent profiled form is

```math
N_T^2 = d^TC_{\rm stat}^{-1}d
-d^TC_{\rm stat}^{-1}B
(\Pi^{-1}+B^TC_{\rm stat}^{-1}B)^{-1}
B^TC_{\rm stat}^{-1}d.
```

The frozen five-lens/ten-source benchmark verifies approximately

```text
N_T = 3.063137195536
Delta chi_T^2 = 9.382809478675
Schur/Woodbury residual < 1e-10
N_T = 5.013450073397 when sigma_z = 0.001(1+z)
```

## Run everything

```bash
python code/run_all_tests.py
```

All four scripts must exit successfully.

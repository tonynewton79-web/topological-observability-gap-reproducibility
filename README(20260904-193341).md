# Topological Observability Gap in Precision Cosmology — Reproducibility Repository

Public numerical and exact-symbolic reproduction material for:

**Tony Newton, _The Topological Observability Gap in Precision Cosmology: Stable Expansion Histories, Non-Identifiable Dark-Energy Phase Structure, and Tomographic Closure Criteria_.**

This repository contains only the calculations needed to reproduce the public results reported in the paper. It does **not** contain any private research software, internal proof-search tooling, or unpublished general-purpose solver infrastructure.

## Main reproducible results

The repository checks four layers of the paper.

1. **Exact DESI DR2 observable-topology certificate**
   - reconstructs the cubic Chebyshev inverse-expansion history `u(z)=D_H(z)/r_d` from the public DESI DR2 BAO mean vector and covariance;
   - verifies by exact rational polynomial root counting that `u'(z)<0` on `0 <= z <= 2.33` throughout the declared `Delta chi^2 <= 16.25` coefficient ellipsoid;
   - verifies the explicit positive dark-energy-density witnesses with topology counts `K = 0,1,2,3,4` at `Omega_m=0.313`;
   - verifies the CPL density-derivative identity and finite-observable component-capacity construction.

2. **Independent floating-point replay**
   - independently reconstructs the cubic fit;
   - reproduces the held-out `D_V/r_d` prediction and `0.369 sigma` pull;
   - independently maximizes the derivative upper envelope over the declared ellipsoid;
   - checks the witness root counts using a different numerical root engine.

3. **Single-ratio topology-resolution replay**
   - recomputes the frozen geometric shear-ratio design;
   - verifies the `K=0/K=2` log-ratio signal and redshift Jacobian;
   - reproduces the equal-redshift-calibration hard floors of approximately `0.00214` for `3 sigma` and `0.001285` for `5 sigma` when residual ratio noise tends to zero.

4. **Correlated tomographic-bank replay**
   - recomputes the declared five-lens/ten-source, 45-ratio benchmark;
   - reproduces `N_T = 3.063137...` and `Delta chi_T^2 = 9.382809...`;
   - verifies the covariance and independent Schur/Woodbury forms agree to numerical precision;
   - reproduces `N_T = 5.013450...` when the mean-redshift calibration scale is changed to `0.001(1+z)`.

## Quick start

```bash
python -m venv .venv
source .venv/bin/activate        # Windows: .venv\Scripts\activate
python -m pip install -r requirements.txt
pytest -q
python code/run_all_tests.py
```

Expected final line:

```text
ALL PUBLIC REPRODUCTION TESTS PASS
```

For the meaning and scope of each test, see [`TESTS.md`](TESTS.md). For data provenance, mathematical claim boundaries, and reproduction notes, see [`REPRODUCIBILITY.md`](REPRODUCIBILITY.md).

## Repository layout

```text
.
├── README.md
├── TESTS.md
├── REPRODUCIBILITY.md
├── CITATION.cff
├── requirements.txt
├── code/
│   ├── reproduce_exact.py
│   ├── reproduce_numeric.py
│   ├── reproduce_single_ratio_design.py
│   ├── reproduce_correlated_bank.py
│   └── run_all_tests.py
├── data/
│   ├── desi_dr2_mean.txt
│   └── desi_dr2_cov.txt
├── results/
└── paper/
    └── Topological_Observability_Gap_in_Precision_Cosmology.pdf
```

## Scope

The exact DESI statements are certificate-level claims in the declared cubic reconstruction. The single-ratio and correlated-bank calculations are controlled survey-design benchmarks, **not mission-specific forecasts for Rubin/LSST, Euclid, Roman, DES, or DESI**. Mission-specific claims require each survey's full data vector, source distributions, covariance, systematics, and nuisance likelihood.

## Author

Tony Newton  
Newton Astro Labs  
London, UK  
tony.newton79@gmail.com

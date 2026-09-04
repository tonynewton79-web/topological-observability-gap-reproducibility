# Reproducibility, Data Provenance, and Claim Boundaries

## Purpose

This repository exposes the computations needed to reproduce the public numerical and exact-symbolic results in the accompanying paper. The intention is that a referee or independent researcher can reproduce the headline values without access to any private software stack.

## Public input data

The files in `data/` are the DESI DR2 BAO mean vector and covariance used by the paper. The calculation uses the twelve `D_H/r_d` and `D_M/r_d` entries that are linear functionals of

```math
u(z)=D_H(z)/r_d,
```

while the `D_V/r_d` datum at `z=0.295` is held out from the cubic fit and used as an external prediction check.

The repository stores the exact decimal values used in the paper so that the symbolic script can promote them directly to rational numbers.

## Exact versus numerical verification

Two independent verification paths are provided.

### Exact path

`code/reproduce_exact.py` uses SymPy rational arithmetic and exact polynomial root counting. The monotonicity and discrete topology-count statements in the paper rely on this path.

### Floating path

`code/reproduce_numeric.py` uses NumPy and SciPy. It reproduces the same reconstruction using floating-point linear algebra and a separate numerical root engine. This is an independent regression check, not a replacement for the exact certificate.

## Survey-design calculations

`code/reproduce_single_ratio_design.py` and `code/reproduce_correlated_bank.py` reproduce the frozen survey-design benchmarks reported in the paper.

They should be interpreted as **controlled proof-of-mechanism calculations**. They are not direct forecasts for Rubin/LSST, Euclid, Roman, DES, or DESI. A mission-specific analysis would need the actual survey redshift distributions, angular modes, shape noise, masks, galaxy bias, intrinsic alignment, magnification, shear-calibration model, cross-bin covariance, and nuisance likelihood.

## What is and is not claimed

The repository supports the following scoped statements:

- the cubic DESI DR2 inverse-expansion reconstruction has a certified zero-turn topology throughout the declared coefficient ellipsoid;
- after nonlinear matter subtraction, the frozen geometry-compatible histories realize several distinct dark-energy-density topology classes;
- CPL cannot represent more than one interior dark-energy-density extremum;
- current compressed lensing information does not by itself identify the latent topology;
- a sufficiently high-rank, sufficiently calibrated correlated shear-ratio bank can cross the fixed `K=0/K=2` identification threshold in the declared benchmark.

It does **not** claim that existing observations have already established the physical topology of dark energy, nor that the benchmark is a completed mission forecast.

## Environment

Tested with Python 3.11 and the dependencies in `requirements.txt`.

To reproduce:

```bash
python -m venv .venv
source .venv/bin/activate
python -m pip install -r requirements.txt
python code/run_all_tests.py
```

GitHub Actions runs the same command on pushes and pull requests.

## Output files

The exact, floating and single-ratio scripts write JSON summaries into `results/`. The bank script prints its frozen headline values and exits nonzero if any assertion fails.

## Computational verification statement

All computational algorithms, proof-search procedures, and certificate criteria were developed by the author(s). Large Language Models (LLMs) such as ChatGPT and Claude were utilized strictly as computational assistants for debugging code and cross-checking routines. No mathematical claims rely on LLM outputs; all results are grounded exclusively in independently reproducible, rigorously verified certificates (exact, symbolic, rational, interval, exhaustive, or controlled high-precision).

## Automated regression testing

A standard `pytest` suite is included in `tests/`. Run `pytest -q` after installing `requirements.txt`. The GitHub Actions workflow executes both `pytest -q` and `python code/run_all_tests.py`.

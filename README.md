# Blazar Cross-Correlation: MAGIC VHE vs. Effelsberg Radio

Analysis code of the Master's thesis *"Analyzing the Correlation between Gamma-Ray and Radio Emission of Blazars"* (Martin Bothen, TU Dortmund, 2026).

The analysis searches for correlated variability between the very-high-energy (VHE) γ-ray emission measured with MAGIC (2020–2025) and the radio emission measured with the Effelsberg 100-m telescope (TELAMON program, 7 mm and 14 mm) for Mrk 421, Mrk 501 and BL Lacertae. The correlation is quantified with the local cross-correlation function (LCCF), and its significance is estimated with simulated light curves (Emmanoulopoulos et al. 2013).

## Repository structure

```
├── emmanoulopoulos/        # modified light curve simulation package (L. Linhoff, MIT License)
├── envs/                   # package lists of the two environments
├── notebooks/              # one analysis notebook per source
└── README.md
```

## Workflow

Each source (Mrk 421, Mrk 501, BL Lac) is analyzed in one notebook in `notebooks/`. The notebook consists of two parts that require different environments, so the Jupyter kernel has to be switched in between (marked in the notebook):

| Part | Environment | Content |
|------|-------------|---------|
| 1. High-level analysis | Python 3.12.13 with Gammapy 2.0.1 (`envs/highlevel_requirements.txt`) | DL3 data selection and quality cuts, night-wise light curves, Bayesian Blocks, spectra |
| 2. Correlation and simulation | Python 3.8.20 (`envs/simulation_requirements.txt`) | LCCF, binning dependence, light curve simulation, significance bands |

Part 1 writes the night-wise MAGIC light curve to `table_lc_filtered.fits`, which is read in Part 2 together with the Effelsberg flux densities.

### Installation

```bash
# Part 1: high-level analysis
conda create -n highlevel python=3.12
conda activate highlevel
pip install -r envs/highlevel_requirements.txt

# Part 2: correlation and simulation
conda create -n sim_lc python=3.8
conda activate sim_lc
pip install -r envs/simulation_requirements.txt
```

Both environments have to be registered as Jupyter kernels, e.g. with `python -m ipykernel install --user --name <env-name>` in each environment.

## Data

The MAGIC DL3 data were produced with the autoMAGIC pipeline (MARS V3-2-0, point-like IRFs) and are not public. The radio flux densities are from the TELAMON program (Effelsberg 100-m telescope). The paths to the data have to be set in the notebooks.

## Light curve simulation

`emmanoulopoulos/` is a modified version of [lena-lin/emmanoulopoulos](https://github.com/lena-lin/emmanoulopoulos) (Copyright (c) 2021 lena-lin, MIT License, see `emmanoulopoulos/LICENSE.md`). Only the files required for this analysis are included. Tests and example scripts of the original repository are omitted. The simulation code in Part 2 of the notebooks is adapted from `scripts/simulate_lightcurves.py` of the original repository.

Changes (also documented at the top of the modified files):
- the light curves are normalized to their mean flux before the PSD/PDF fit instead of being scaled to a maximum of 10
- the red-noise factor of the Timmer–Koenig step is passed as a parameter (value 100) instead of being fixed

In the simulation, no Poisson noise is added (`poisson_noise=False`), since the light curves are given as fluxes and not as counts.

## Version

The results of the thesis were obtained with release `v1.0`.

## License

No license is granted for the code in this repository, except for the `emmanoulopoulos/` package, which retains its original MIT License (see `emmanoulopoulos/LICENSE.md`).

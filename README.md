## Getting the data files

The data files (`data/*.parquet`, `data/*.gz`) and the planetary ephemeris (`kernels/de440s.bsp`) are not in this
repository because they are too large for GitHub (~5 GB in total). There are two ways to get them.

### Option 1: download them

Download all the files from this Google Drive folder:

**https://drive.google.com/drive/folders/1AjEkNFcTgmMoKjl4mhf6MImUBFoZ2Rtn?usp=sharing**

Put the `.parquet` and `.gz` files in `data/` and `de440s.bsp` in `kernels/`. Then you can open any notebook
directly. These are the exact files used in the workshop.

### Option 2: build them by running the notebooks

**1. Download the raw inputs**

| File | Put it in | Source |
|---|---|---|
| `obs_sbn_X05_full.parquet` | `data/` | Rubin (X05) observations submitted to the MPC, [B612 mirror](https://b612.ai/rubin-mpc-downloads/) ([direct link](https://storage.googleapis.com/asteroid-institute-public/production/rubin/mpc/obs_sbn/full/parquet/obs_sbn_X05_full.parquet)) |
| `mpcorb_extended.dat.gz` | `data/` | MPC orbit catalogue, [Minor Planet Center](https://minorplanetcenter.net/Extended_Files/mpcorb_extended.dat.gz) |
| `de440s.bsp` | `kernels/` | Planetary ephemeris, [NAIF](https://naif.jpl.nasa.gov/pub/naif/generic_kernels/spk/planets/de440s.bsp) |

`kernels/naif0012.tls`, `kernels/gm_de440.tpc` and `data/medellin-vroc-v15_10_2026.csv` are already in the repository.

The Rubin and MPC files are updated regularly, so a fresh download will contain more observations than the
workshop files and the numbers in the notebooks will differ slightly. Use the Drive folder (Option 1) to reproduce
them exactly.

**2. Run the notebooks in this order**

Run each notebook from top to bottom; each one creates the files the next one needs.

| # | Notebook | Reads | Creates |
|---|---|---|---|
| 1 | `add_mpc_designation.ipynb` | `obs_sbn_X05_full.parquet`, `mpcorb_extended.dat.gz` | `obs_sbn_X05_full_mpc_desig.parquet` (Rubin observations + MPC designation) |
| 2 | `exploration_data.ipynb` | `obs_sbn_X05_full_mpc_desig.parquet`, `mpcorb_extended.dat.gz` | `obs_elements_state.parquet` (observations + MPC orbit + state vector at the MPC epoch) |
| 3 | `propagate_orbit.ipynb` | `obs_elements_state.parquet` | `obs_elements_state_at_obstime.parquet` (elements and state vector at each observation time)<br>`obs_elements_geometry_at_obstime.parquet` (+ distance from Earth and phase angle)<br>`obs_final_elements_geometry_H.parquet` (+ distance from the Sun and H) |
| 4 | `colors.ipynb` | `obs_final_elements_geometry_H.parquet`, `obs_sbn_X05_full_mpc_desig.parquet` | `colors.parquet` (H per band and colors per object) |
| 5 | `corner_plots.ipynb` | `colors.parquet`, `medellin-vroc-v15_10_2026.csv` | figures in `figures/` |

All files are read from and written to `data/`, relative to the notebook folder.

Notes:
- `propagate_orbit.ipynb` processes ~8 million observations in chunks; the full run takes several minutes.
  Its comparison with JPL Horizons needs an internet connection.
- You need ~5 GB of free disk space for all the files.
- Python packages used: `pandas`, `numpy`, `pyarrow`, `matplotlib`, `corner`, `plotly`, `spiceypy`, `astropy`, `astroquery`.

## Suggested papers:
- LSST predictions for many small body populations in [Kurlander et al (2025)](https://iopscience.iop.org/article/10.3847/1538-3881/add685)
- LSST Predictions [AAS Focus issue](https://iopscience.iop.org/collections/PSJ-250528-01)
- Analysis of Rubin First Look data by [Greenstreet et al (2026)](https://doi.org/10.3847/2041-8213/ae2a30)

## Suggested software: 
- [Sorcha](https://github.com/dirac-institute/sorcha)
- [Layup](https://github.com/Smithsonian/layup)

## Data access
- [B612 Foundation mirror of LSST data on the MPC](https://b612.ai/rubin-mpc-downloads/) 
- Individual objects in the [Minor Planet Center](https://www.minorplanetcenter.net)
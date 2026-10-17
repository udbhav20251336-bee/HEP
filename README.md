# HEP: Synthetic Solar Magnetogram and Active-Region Evolution

This project explores a synthetic model of solar magnetic activity. Its Jupyter
notebooks generate and evolve active regions on a rotating solar surface,
reconstruct magnetograms, and visualize how the magnetic field and flux change
over time.

The repository contains both the analysis notebooks and exported simulation
data and animations, so the existing visualizations can be inspected without
rerunning the full simulation.

## Project contents

| Path | Description |
| --- | --- |
| [`notebooks/03_differential rot_and_further_clean.ipynb`](notebooks/03_differential%20rot_and_further_clean.ipynb) | Main simulation notebook: active-region evolution, differential rotation, magnetogram reconstruction, and export of the clean dataset. |
| [`notebooks/04_magn_evolution_clean.ipynb`](notebooks/04_magn_evolution_clean.ipynb) | Loads the clean dataset and visualizes magnetogram evolution, field-strength statistics, region histories, and surface-area-weighted magnetic flux. |
| [`notebooks/defunct_test_04_stage3_timeevo.ipynb`](notebooks/defunct_test_04_stage3_timeevo.ipynb) | Earlier exploratory time-evolution analysis; retained for context, not the primary clean workflow. |
| [`clean_export_data/`](clean_export_data/) | Data exported by the clean simulation notebook. |
| [`old_export_data/`](old_export_data/) | Earlier/legacy simulation exports. |
| [`animations/`](animations/) | Rendered MP4 visualizations of synthetic magnetograms and magnetic-region evolution. |

The `defunct_*` notebooks are earlier experiments and are not required for the
main workflow.

## Model and results

The clean simulation notebook initializes **73 active regions** and advances
the model for **120 steps of 0.25 days** each, for a total modeled interval of
**30 days**. The differential-rotation profile is latitude-dependent and is
implemented in the form

```text
ω(λ) = A − B sin²(λ) − C sin⁴(λ)
```

with coefficients `A = 14.38`, `B = 2.97`, and `C = 0.55` in the clean
notebook. The notebooks use the resulting evolving region positions and
strengths to reconstruct synthetic magnetograms and animate the surface
evolution.

The magnetic-evolution notebook calculates and plots maximum field, mean
field, mean absolute field, region statistics, and positive, negative, net, and
unsigned flux. Its flux calculation weights each grid cell by the spherical
surface element, including the latitude-dependent cosine factor. The provided
MP4 files are ready-to-view visual outputs from the project.

These are exploratory simulation outputs, not observational measurements or a
validated solar-physics forecast. The notebooks note that a roughly 68-day
active-region decay timescale is longer than the 30-day clean run, so this run
does not cover a full decay interval. An earlier experiment also flags possible
field-strength overestimation where regions overlap; interpret peak-field
values with that limitation in mind.

## Data exports

The clean workflow writes the following files under `clean_export_data/`:

- `note3_clean_data.pkl` — frames, times, region history, and simulation
  parameters in a Python pickle.
- `note3_clean_frames.npy` — stacked magnetogram frames.
- `note3_clean_times.npy` — simulation times corresponding to the frames.
- `note3_clean_region_history.pkl` — active-region state at each time step.

Pickle files should only be loaded from a trusted source.

## Run the notebooks

Use Python with Jupyter, NumPy, Matplotlib, and IPython installed. FFmpeg is
needed only if you want to export the notebook animations as MP4 files.

From the project root, install the dependencies and start Jupyter in the
`notebooks/` directory (the analysis notebook resolves its data path relative
to that working directory):

```bash
python -m pip install jupyter numpy matplotlib ipython
cd notebooks
jupyter lab
```

Run `03_differential rot_and_further_clean.ipynb` first to generate the clean
exports. Then run `04_magn_evolution_clean.ipynb` to load those exports and
produce the magnetic-evolution visualizations.

## Notes

- Simulation data and rendered animations are committed for convenient
  inspection; rerunning a notebook may regenerate or replace outputs.
- `.venv/` and Jupyter checkpoint directories are excluded from version
  control.

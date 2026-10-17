# Experiment A4: numerical conditioning stress

Experiment A4 studies the floating-point behaviour of the row-wise Lewis-weight fixed-point iteration under increasingly ill-conditioned representations of the same problem. It corresponds to Figures 4 and 5 of the paper.

## Files

- `A4_reproduce.ipynb` — checks the released A4 results and regenerates the paper figures. It also contains an optional full rerun from the saved matrices.
- `A4_protocol_record.md` — the experimental design.
- `A4_EXPERIMENT_RECORD.md` — a short record of what happened in the final experiment.
- `data/` — the saved matrices, input diagnostics, raw trajectories, and summary tables.
- `figures/` — the two figures used in the paper
- `data/A4_environment_record.txt` — the recorded software environment for the original run.

## Reproducing the results

Open `A4_reproduce.ipynb` from this directory and run it from top to bottom. By default, the notebook checks the released data and writes regenerated summaries and figures to a new `reproduced/` directory; it does not modify anything in `data/` or `figures/`.

To rerun the 270 Lewis trajectories from the saved matrices, set `RERUN_TRAJECTORIES = True` in the configuration cell. A full rerun is written under `reproduced/raw/`. Because A4 deliberately probes floating-point behaviour under severe conditioning, exact stressed values can depend on the numerical linear-algebra environment; `data/A4_environment_record.txt` records the environment used for the released run.

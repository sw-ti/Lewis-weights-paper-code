# Experiment B public release

Experiment B studies the design geometry produced by finite-$p$ block Lewis weights on two real grouped sensing instances: the Intel Berkeley Research Lab sensor data and NOAA/NDBC Gulf buoy data. It compares the finite-$p$ design path with uniform and D-optimal allocation using nominal information and concentration metrics. The results of this experiment correspond to Figure 7 of our paper. 

## Contents

- `B_protocol_record.md` — the final scientific specification of Experiment B.
- `B_EXPERIMENT_RECORD.md` — the realized datasets and result summary.
- `B_reproduce.ipynb` — a notebook that checks the released arrays, reconstructs the plotted metrics, and reproduces the paper figure.
- `data/` — frozen processed Intel/Gulf inputs and the final B numerical results used by the figure.
- `figures/` — the exact PDF used in the paper and a PNG preview.
- `requirements.txt` — minimal Python dependencies for the public notebook.

## Reproducing the figure

From inside this `B/` directory, install the dependencies and execute the notebook:

```
python -m pip install -r requirements.txt
jupyter nbconvert --to notebook --execute --inplace B_reproduce.ipynb
```

You can also open `B_reproduce.ipynb` in JupyterLab and use **Run All**. The notebook writes any newly generated files under `reproduced/`; it never overwrites the released data or paper figure.

The public reproduction starts from the frozen processed inputs in `data/`. The raw Intel and NDBC source archives are not redistributed here; their preprocessing is documented in `B_protocol_record.md`.

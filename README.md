# Lewis weights paper: numerical experiments

This repository contains the code and released numerical results for **Computing Lewis Weights to High Precision by Fixed-Point Iteration** by Swati Padmanabhan (arXiv:2609.04338).

## Experiments

| Folder | Paper figures | Experiment |
| --- | --- | --- |
| `A0/` | 1 | Local contraction predictions and observed rates |
| `A1/` | 2 | Row-wise iteration counts on Gaussian matrices |
| `A2/` | 3 | Row-wise iteration counts on structured matrices |
| `A4/` | 4, 5 | Floating-point behaviour under ill-conditioning |
| `A3/` | 6 | Block Lewis-weight iteration counts |
| `B/` | 7 | Information and concentration of block Lewis designs |
| `C/` | 8 | Information preservation under block unavailability |

Each folder contains a README, a protocol record, an experiment record, a reproduction notebook, and the required data. Keep the folders together: the A2 comparison figure reads an A1 summary. C includes its own copies of the B inputs it needs.

## Environment and setup

The public notebooks were checked on Linux with Python **3.13.5**, NumPy **2.3.5**, SciPy **1.17.0**, pandas **2.2.3**, Matplotlib **3.10.8**, and JupyterLab **4.5.3**. This is a tested reproduction environment, not the original environment for every experiment. In particular, the original A4 environment is recorded in `A4/data/A4_environment_record.txt`.

Using Python 3.13, run the following from the repository root on Linux or macOS:

```bash
python3.13 -m venv .venv
source .venv/bin/activate
python -m pip install numpy==2.3.5 scipy==1.17.0 pandas==2.2.3 matplotlib==3.10.8 jupyterlab==4.5.3
```

The validation runs used one numerical-library thread. To use the same setting, set these variables before starting Jupyter:

```bash
export OPENBLAS_NUM_THREADS=1
export OMP_NUM_THREADS=1
export MKL_NUM_THREADS=1
export NUMEXPR_NUM_THREADS=1
```

Then open a notebook from its experiment directory, for example:

```bash
cd A0
jupyter lab A0_reproduce.ipynb
```

Run all cells in order from a fresh kernel. Replace `A0` with the desired experiment. Newly generated files go under that folder's `reproduced/` directory; the notebooks do not overwrite the released `data/` or `figures/` files. Do not commit the local `.venv/` directory.

## Saved results and fresh computations

A0 and A1 rerun their experiments by default. Set `RERUN_EXPERIMENT = False` in A0 or `RUN_FRESH = False` in A1 to use the released results instead. A2 and A3 rerun all 750 cases each.

A4, B, and C use shorter default workflows. To enable their complete numerical reruns, set:

| Notebook | Setting |
| --- | --- |
| `A4/A4_reproduce.ipynb` | `RERUN_TRAJECTORIES = True` |
| `B/B_reproduce.ipynb` | `RUN_FULL_RECOMPUTE = True` |
| `C/C_reproduce.ipynb` | `FULL_RECOMPUTE = True` |

C's default workflow already recomputes all Gulf primary cases, Intel cases with one or two unavailable blocks, and the complete secondary empirical comparison. Its complete primary rerun can take tens of minutes. A4's complete rerun is also substantially longer than its default checks. The folder READMEs describe the outputs and checks in detail.

## Scope of reproduction

B and C start from the released processed geometries and, for C, the released availability masks. The notebooks do not rebuild these inputs from the raw Intel/NDBC archives. The data-processing rules are documented in the protocol records; reproduction of the numerical experiments uses the frozen arrays supplied here.

A0 regenerates its matrices from recorded seeds because the original matrix arrays were not retained. A1–A4 include saved matrix banks.

Fresh floating-point trajectories need not match the original trajectories exactly, especially the low-residual A0 runs and the ill-conditioned A4 runs. Use the released data to reconstruct the paper's reported numerical results. Fresh reruns test the same experiments in the chosen numerical environment; PDF appearance can also vary with plotting and font versions.

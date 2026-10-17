# Experiment A3: block Lewis-weight iteration counts

A3 is the experiment corresponding to Figure 6 of our paper. It runs the  block Lewis fixed-point update on 50 Gaussian $100\times20$ matrices partitioned into 20 five-row blocks and studies how the number of updates needed to reach a fixed residual threshold varies with $p$. 

## Files

- `A3_protocol_record.md`: describes the scientific design.
- `A3_EXPERIMENT_RECORD.md`: describes what was run, the numerical implementation, final results, and the  relevant limitations.
- `A3_reproduce.ipynb`: a Jupyter notebook that reruns the experiment and regenerates Figure 6.
- `data/`: the released final set of matrices, raw results, residual trajectories, diagnostics, and summaries.
- `figures/`: Figure 6 used in the paper.

## Reproducibility notes

Use Python 3 with NumPy, pandas, SciPy, Matplotlib, and Jupyter. From the `A3/` directory, open `A3_reproduce.ipynb` and run all cells from a fresh kernel.

The notebook first verifies the released data, then regenerates the matrices, reruns all 750 cases, recomputes the summaries, compares the fresh results with the released results, and regenerates the figure. Fresh outputs are written only to `reproduced/`; they do not overwrite `data/` or `figures/`.

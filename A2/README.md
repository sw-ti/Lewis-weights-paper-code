# Experiment A2: row-wise Lewis weights for structured matrices 

A2 is the experiment that produces Figure 3 of our paper. Here, we test our algorithm to compute row-wise Lewis weights of matrices with two markedly different row scales, creating heterogeneous leverage scores while remaining well conditioned. See `A2_protocol_record.md` for the experimental design and `A2_EXPERIMENT_RECORD.md` for what was run and the final outcome.

## Files

* `A2_protocol_record.md`: our experimental design.
* `A2_EXPERIMENT_RECORD.md`: what was run and the final results.
* `A2_reproduce.ipynb`: notebook that reproduces the A2 computation and Figure 3.
* `data/`: released final A2 matrices, trajectories, diagnostics, and summaries.
* `figures/`: the final Figure 3 files.

## Reproducibility notes

Use Python with NumPy, pandas, Matplotlib, and Jupyter. Start Jupyter in this `A2/` directory and run `A2_reproduce.ipynb` from top to bottom.

The notebook regenerates the A2 matrices, reruns the experiment, recomputes the diagnostics and summaries, compares the fresh results with the released data, and regenerates the figure. Fresh outputs are written only to `reproduced/`; the released `data/` and `figures/` files are not modified.

Figure 3 also uses the released A1 summary at `../A1/data/A1_primary_summary.csv`. The A2 experiment itself can be rerun without A1; only the combined A1/A2 figure requires that file.

Floating-point trajectories can vary slightly across NumPy/BLAS/LAPACK environments. The notebook therefore compares the recomputed first-crossing iteration counts and their summaries (for each $p$) with the released results, rather than requiring the full trajectories to be byte-identical.


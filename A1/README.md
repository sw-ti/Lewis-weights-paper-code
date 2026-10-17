# Experiment A1

A1 is the experiment corresponding to Figure 2 of our paper. Here, we test row-wise Lewis weight iteration-count against $p$, on standard Gaussian matrices. 

## Files

* `A1_protocol_record.md`: the experimental design.
* `A1_EXPERIMENT_RECORD.md`: what was run and the final results.
* `A1_reproduce.ipynb`: notebook that reproduces the A1 computation and Figure 2.
* `data/`: the released matrix bank, final results and summaries, and the earlier results that motivated the modification to our originally planned numerical implementation of one of the subroutines.
* `figures/`: the final Figure 2 files.

## Reproducibility notes

Start Jupyter in this `A1/` directory and run `A1_reproduce.ipynb` from top to bottom. The notebook uses NumPy, pandas, Matplotlib, and standard Python/Jupyter functionality.

The notebook regenerates the matrix bank, reruns the experiment with the final leverage-score computation, recomputes the summaries, compares the fresh results with the released data, and regenerates the figure. Fresh outputs are written to `reproduced/`; the released `data/` and `figures/` files are not modified.

The original runs used `float64` arithmetic and recorded Python 3.8.10. The exact original NumPy and BLAS/LAPACK versions were not retained, so fresh floating-point trajectories need not be byte-identical across environments.


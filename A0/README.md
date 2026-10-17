# Experiment A0: local Lewis-weight dynamics

A0 is the experiment corresponding to Figure 1 of our paper. See `A0_protocol_record.md` for the experimental design and `A0_EXPERIMENT_RECORD.md` for the implementation and final outcome.

## Files

* `A0_protocol_record.md`: the experimental design and planned analysis.
* `A0_EXPERIMENT_RECORD.md`: implementation and final results.
* `A0_reproduce.ipynb`: notebook that reproduces the A0 computation and Figure 1.
* `data/`: the released final A0 results.
* `figures/`: the final Figure 1 files.

## Reproducibility notes

Start Jupyter in this directory and run `A0_reproduce.ipynb` from top to bottom.

The notebook can regenerate the Gaussian matrices from the recorded seeds, rerun all 420 (matrix, p) cases, recompute the predicted and observed contraction factors, and regenerate the figure. Fresh outputs are written only to `reproduced/`; the released `data/` and `figures/` files are not modified.

The original matrix arrays were not saved, so fresh reruns regenerate them from the recorded seeds. The exact original NumPy and BLAS/LAPACK versions were not retained, so floating-point trajectories need not be identical across environments.


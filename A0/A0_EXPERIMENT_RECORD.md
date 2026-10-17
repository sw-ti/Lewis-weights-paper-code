# Experiment A0 record

We ran A0 according to the design in `A0_protocol_record.md`.

## Final implementation

The numerical runs used `float64` arithmetic and a 5,000-iteration limit. All 420 runs reached the prescribed stopping threshold before this limit.

At the returned numerical point, we recomputed the weighted QR factorization and evaluated the finite-point Jacobian prediction used in the paper. The observed contraction factors were obtained from the three fitting windows specified in the protocol.

An earlier experiment used seed labels 100-119, while the final bank contains seed 117. The exact original matrix-generation procedure was not retained, so we cannot determine whether the pilot and final uses of seed 117 produced the same matrix.

## Final results

The predicted and observed contraction factors agree closely across the 420 (matrix, $p$) runs. In particular, over the primary window, the fit of $\log Res\_t$ against $t$ has $R^2\ge0.99$ for 418 of the 420 runs. The two exceptions are seed 571 at $p=61$ and $p=64$; in both cases the residual initially decays geometrically and later fluctuates near the lower end of the fitting window.

Over the earlier window $10^{-7}\le Res\_t\le10^{-4}$, all 420 runs have $R^2\ge0.99$. The final figure uses the primary-window estimate for every run, including these two exceptions.

## Figure

`figures/A0_main_agreement_figure.pdf` is the Figure 1 PDF used in the paper. It is generated from `data/A0_run_summary.csv`.

The left panel compares $\mu\_{\mathrm{pred}}$ and $\mu\_{\mathrm{obs}}$ for all 420 runs. The right panel shows their signed relative difference as a function of $p$.

`A0_reproduce.ipynb` regenerates the experiment and the figure. Because the exact original numerical-library versions were not retained, fresh floating-point trajectories need not be identical to the released ones.




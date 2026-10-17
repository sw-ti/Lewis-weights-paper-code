# Experiment A1 record

We ran A1 according to the design in `A1_protocol_record.md`. The final results come from the complete 750-case rerun using the rescaled-QR leverage-score evaluation.

## Final results

All 50 final matrices have rank 20. Their condition numbers range from 2.0496 to 2.8279.

All 750 final runs reached $R\_t<10^{-8}$ within the 10,000-iteration limit. The median iteration count increases from 18 at $p=3$ to 286 at $p=32$, 581 at $p=64$, and 1163.5 at $p=127.3$. The same qualitative dependence on $p$ appears at thresholds $10^{-4}$ and $10^{-6}$.

Across the tested Gaussian matrices, the iteration count grows approximately linearly with $p$ and varies little across matrices.

The earlier run contained 20 cases that did not reach $10^{-8}$ within 10,000 iterations. After changing the numerical evaluation of leverage scores and the complete rerun, all 750 cases reached the threshold. The earlier results remain in `data/` as part of the numerical history.

## Figure

`figures/A1_combined_panels.pdf` is the Figure 2 used in the paper. It was generated from `data/A1_primary_summary.csv` and `data/A1_tolerance_summary.csv`.

The left panel shows the median and empirical 20th/80th percentiles of $T\_{10^{-8}}$. The right panel shows the median iteration counts at thresholds $10^{-4}$, $10^{-6}$, and $10^{-8}$.

`A1_reproduce.ipynb` regenerates the same numerical display in `reproduced/`. Floating-point trajectories need not be byte-identical across numerical environments, so the notebook compares the resulting iteration counts and summary statistics with the released results.


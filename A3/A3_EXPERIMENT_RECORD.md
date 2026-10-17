# Experiment A3 record
 
We ran A3 according to the design in `A3_protocol_record.md`. The implementation used `scipy.linalg.solve_triangular` for the triangular solve in the QR-based block Lewis update.

## Final runs

All 50 stacked matrices have rank 20 and all 1,000 blocks have rank 5. The observed $\kappa\_2(A)$ values range from 2.120602 to 2.761191, with median 2.414731.

All 750 matrix/$p$ runs reached $R\_t<10^{-8}$. The observed $T\_{10^{-8}}$ values range from 14 to 1084. 

The median $T\_{10^{-8}}$ increases from 15 at $p=3$ to 932 at $p=127.3$. The high-$p$ distributions also become much wider across instances; at $p=127.3$, q20/median/q80 are 403.6/932/1040 and the full range is 165-1084. The tested block instances therefore show a steadily increasing, $\widetilde{O}(p)$-type dependence over this range. The curve has noticeable upward curvature at the largest $p$.

## Figure

`figures/A3_main_body_style.pdf` is the final PDF used in the paper. It was generated from `data/A3_primary_summary_v2.csv` and `data/A3_tolerance_summary.csv`. The left panel shows the median and empirical 20th/80th percentiles of $T_{10^{-8}}$; the right panel shows the median iteration counts at the three recorded thresholds.

`A3_reproduce.ipynb` regenerates the same numerical display in `reproduced/`. PDF bytes need not be identical across Matplotlib/font environments, so the notebook compares the numerical data underlying the figure directly.

## Released results

The released `data/` files are unchanged copies of the final experiment outputs, except for the explicitly corrected derived summary described next.

The first primary-summary file accidentally omitted the planned mean and sample standard deviation. We regenerated `A3_primary_summary_v2.csv` from the unchanged run data; every statistic shared with the first summary remained unchanged. The public release uses this corrected summary.


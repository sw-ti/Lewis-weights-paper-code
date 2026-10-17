# A2 experiment record

We ran A2 according to the design in `A2_protocol_record.md`.

## Final results

All 50 final matrices have rank 20. Their condition numbers $\kappa\_2(A)$ range from 1.1596 to 1.2145, with median 1.1837.

All 750 matrix/$p$ runs reached $\Res\_t<10^{-8}$ before the 10,000-iteration limit. The median iteration count increases from 19 at $p=3$ to 1229 at $p=127.3$. Variation across the 50 matrices is very small: at 14 of the 15 values of $p$, all 50 matrices have the same $10^{-8}$ iteration count; at $p=5.7$, the counts are 46 or 47.

The A1 comparison uses the released final A1 summary rather than rerunning A1. The two experiments use different matrix banks and are compared at the family-summary level, rather than instance by instance. For every tested $p\ge7.3$, the A2 median is about 5.5-6.9% larger than the A1 median.

## Geometry notes

The protocol specified the ratio of mean anchor leverage to mean cloud leverage. The implementation instead used the ratio of median anchor leverage to median cloud leverage for the geometry diagnostic shown in Figure 3.

This change affects only that descriptive geometry statistic; it does not enter matrix generation, the Lewis iteration, or the iteration-count analysis. Across the 50 matrices, the median is 16.7604 for the protocol's mean-based ratio and 16.7688 for the reported median-based ratio.

## Figure

`figures/A2_two_panel_scaling_plus_geometry_compact.pdf` is the Figure 3 used in the paper.

Its left panel uses the released A1 and A2 median and empirical 20th/80th percentile iteration-count summaries. Its right panel shows the implemented median anchor/cloud leverage ratio against $\kappa_2(A)$ for all 50 A2 matrices.

The underlying A2 values are stored in `data/`; the A1 comparison curve is read from `../A1/data/A1_primary_summary.csv`.

## Numerical interpretation

A2 measures the number of iterations needed by Res\_t to reach a fixed threshold in float64 arithmetic. A crossing of $\Res\_t<10^{-8}$ is not a separate certificate of coordinatewise error to the exact Lewis weights.


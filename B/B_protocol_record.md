# Experiment B: public protocol record

Experiment B studies how finite-$p$ block-Lewis allocations change with $p$ on two fixed real-data sensing geometries, moving from near-uniform toward D-optimal designs. The regularized D-optimal interpretation is known; our focus is the resulting finite-$p$ path on these two datasets. The results of this experiment correspond to Figure 7 of our paper. 

## Intel data construction

We use the Intel Berkeley Research Lab sensor dataset (`data.txt.gz` and `mote\_locs.txt`). The raw data contain date/time, epoch, mote ID, temperature, humidity, light, and voltage. We keep observations through 2004-03-18 with temperature in $[0,50]$ and humidity in $[0,100]$, and aggregate temperature, humidity, and light into five-minute bins using coordinatewise medians. Voltage is not used.

We restrict to sensors with official location metadata and observations before 2004-03-09. Mote 5 has no healthy observations because its raw reports lack humidity, and mote 55 has no location metadata. We also exclude mote 15, whose baseline coverage is only 592 of 2,869 bins, far below the next-lowest sensor. This leaves 52 sensors, IDs 1 through 54 except 5 and 15. We retain only bins with all three measurements for every sensor, without imputation.

The resulting 1,936 snapshots are split chronologically at 2004-03-06 into 1,198 training and 738 later snapshots. We apply `log1p` to light, fit feature means on training data, and use one pooled population standard deviation (`ddof=0`) per modality across sensors and training times.

We use the historical latent rank 30, selected during earlier model development after examining reconstruction on the 738 later snapshots. The rank-30 basis itself is fitted only on standardized training data, using the leading 30 right singular vectors without singular-value weighting. In sensor-major order, the three rows for sensor $i$ form $B\_i\in\mathbb{R}^{3\times30}$, with information block $C\_i=B\_i^\top B\_i$. Up to floating-point error, $\sum\_i C\_i=I\_{30}$. For reproducibility, we make the largest-magnitude entry of each basis column positive; this does not change the design quantities.

## Gulf / NOAA NDBC data construction

We use NOAA National Data Buoy Center standard-meteorological data from 2020–2023. The ten retained buoys, in experiment order, are `42026`, `42023`, `42035`, `42040`, `42019`, `42055`, `42022`, `42039`, `42036`, and `42002`. This network was inherited from earlier construction, which required at least 50% joint hourly pressure/wind coverage in each year after geographic/type and coverage screening.

We use pressure (`PRES`) and wind speed (`WSPD`) on a half-hour grid from 2020-01-01 00:00 through 2023-12-31 23:30. At each anchor, we take the nearest report within an inclusive $\pm5$ minute window containing both measurements, breaking ties by the earlier timestamp. Missing blocks remain missing; we do not interpolate.

For rank selection, we fit coordinatewise means and sample standard deviations (`ddof=1`) on 2020–2022. We estimate the pairwise correlation matrix, symmetrize it, clip negative eigenvalues to zero, reconstruct it, and rescale it to unit diagonal. We evaluate ranks 1 through 10 on 2023 by holding out each observed target buoy, fitting latent coordinates from the other observed blocks by unregularized least squares, and averaging the two-coordinate errors first within each target buoy and then equally across buoys. This selects rank 4.

With rank 4 fixed, we refit means, scales, the repaired correlation matrix, and the basis on all 2020–2023 data. The leading four eigenvectors, without eigenvalue weighting, give two-row blocks $B\_i\in\mathbb{R}^{2\times4}$.

We estimate a common effective covariance from matched :00 and :30 observations in the final standardized coordinates. For buoy $i$, let $\delta\_{ih}$ be the within-hour difference over $N\_i$ matched hours. We compute $S\_i=(2N\_i)^{-1}\sum\_h\delta\_{ih}\delta\_{ih}^\top$ and average the ten $S\_i$ equally to obtain $\Sigma\_{\mathrm{effective}}$. We then use $A\_i=\Sigma\_{\mathrm{effective}}^{-1/2}B\_i$ and $C\_i=A\_i^\top A\_i$. This covariance models short-timescale effective variation, not pure instrument noise.

## Frozen inputs

All final calculations use `data/B\_intel\_processed\_input\_frozen.npz` and `data/B\_gulf\_processed\_input\_frozen.npz`. The public notebook starts from these files rather than rebuilding them from raw archives.

## Finite-$p$ designs

For information blocks $C\_i=A\_i^\top A\_i$, let $w(p)$ denote the block-Lewis fixed point and $\rho=1-2/p$. Since the corresponding regularized design weights each $C\_i$ by $w\_i(p)^\rho$, we use the simplex allocation $\pi\_i(p)=w\_i(p)^\rho/\sum\_j w\_j(p)^\rho$.

The grid is $p\in\{2.2,2.5,3,4,6,8,12,16,24,32,49,64\}$. Uniform allocation is the exact $p=2$ endpoint, so no separate run is needed. The D-optimal endpoint is computed directly.

## Baselines and metrics

For $k$ blocks, the uniform allocation is $U\_i=1/k$. For any simplex allocation $\pi$, define $M(\pi)=\sum\_i\pi\_iC\_i$. The D-optimal allocation $D$ maximizes $\log\det M(\pi)$ over $\pi\ge0$ with $\sum\_i\pi\_i=1$, using the same blocks and budget as the finite-$p$ designs.

We compute the primary D-optimal solution using analytic-gradient SLSQP, initialized at uniform, with `ftol=1e-12` and a 10,000-iteration limit. We independently validate it using CVXPY/Clarabel. For validation only, we may scale all $C\_i$ by the same positive scalar $c=d/tr(M(U))$ to improve conditioning. This adds a constant to the log-det objective and does not change the optimizer. Final acceptance checks use the original matrices. We retain the direct D-optimal vector, including tiny coefficients, without thresholding.

We measure concentration using $N\_{\mathrm{eff}}(\pi)=1/\sum\_i\pi\_i^2$, which is an effective-support statistic rather than the number of nonzero blocks. We measure information using $L(\pi)=\log\det M(\pi)$ and the normalized gain $G(\pi)=[L(\pi)-L(U)]/[L(D)-L(U)]$. Thus $G$ measures the fraction of the uniform-to-D-optimal log-det gap recovered on that dataset, not an absolute fraction of physical information. Its values should not be interpreted as a common absolute information scale across datasets.

## Numerical block-Lewis computation

For each dataset and $p$, we initialize $w\_i^{(0)}=d rank(A\_i)/\sum\_j rank(A\_j)$, where $d$ is the latent dimension. Each update scales the rows of block $i$ by $w\_i^{\rho/2}$, computes a reduced QR factorization of the stacked matrix, solves $R^\top X^\top=A^\top$ by triangular solve, and sets $w\_i^+=w\_i^\rho\|X\_i\|\_F^2$.

We stop at the first update satisfying $\max\_i|\log(w\_i^+/w\_i)|\le10^{-8}$, with a cap of 10,000 updates, and form the allocation from the terminal raw weights. Development checks at $p\in\{2.2,16,64\}$ compare this tolerance with $10^{-10}$ and require negligible differences in allocation, $N\_{\mathrm{eff}}$, and $G$ at the reported precision.

This computation produces the design allocations; it is not the separate output-conversion step in the paper's coordinatewise high-precision theorem.

## Figure

For each dataset, we plot $G$ against $N\_{\mathrm{eff}}$ for all twelve finite-$p$ values and the uniform and D-optimal endpoints. All points are included, although only a subset may be labeled for readability.
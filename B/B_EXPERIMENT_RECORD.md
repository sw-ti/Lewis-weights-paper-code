# Experiment B: experiment record

## Inputs

The released Intel input has 52 sensor blocks, with three rows per block and latent dimension 30. The arrays `B\_blocks` and `C\_blocks` have shapes `(52, 3, 30)` and `(52, 30, 30)`. The complete-data construction produced 1,198 training snapshots and 738 later snapshots. The released processed file contains the final information matrices.

The released Gulf input has 10 buoy blocks, with two rows per block and latent dimension 4. The arrays `A\_blocks` and `C\_blocks` have shapes `(10, 2, 4)` and `(10, 4, 4)`. The half-hour grid contains 70,128 anchors over 2020–2023. The final pairwise correlation matrix required a small PSD repair: its smallest eigenvalue was approximately $-0.00507$, and the relative Frobenius change was approximately $7.12\times 10^{-4}$. The released file contains the repaired rank-4 basis, effective covariance, whitened blocks, and final information matrices.

## Numerical runs

The final sweep contains all 24 finite-$p$ cases, twelve per dataset, with no missing cases or support cleanup. Every run reached the $10^{-8}$ successive-iterate residual threshold within the 10,000-update cap. The released terminal allocations can be reconstructed from the released raw block weights up to floating-point rounding.

The D-optimal baselines were computed directly using SLSQP and stored in the released raw files. The first independent CVXPY/Clarabel validation for Intel did not meet the prescribed numerical tolerances. We repeated the validation with a common scaling of all information matrices and tighter solver tolerances. This scaling does not change the optimization problem, and the final direct and conic solutions passed the acceptance checks. We use the direct D-optimal solutions without thresholding small coefficients, unlike in the later block-unavailability experiment.

## Results

On both datasets, $N_{\mathrm{eff}}$ decreases and $G$ increases at every successive point of the evaluated finite-$p$ grid. The two datasets follow this trend at different rates.

## Figure used in the paper

The file `figures/B\_design\_geometry.pdf` is the source of Figure 7 in the paper. It is generated from `data/B\_final\_summary.csv` and `data/B\_final\_baselines.csv`. Both panels include all twelve finite-$p$ values and the uniform and D-optimal endpoints.

## Notes on timeline of experiment choices

The six values $p\in\{2.2,4,8,16,32,64\}$ had been explored before the final experiment. The six additional values $\{2.5,3,6,12,24,49\}$ were unseen when the final grid was fixed. The Intel rank-30 and Gulf rank-4 constructions were also inherited from earlier model development, rather than selected specifically for this experiment.
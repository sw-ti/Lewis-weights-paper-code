# Experiment A4 protocol

This experiment tests how the row-wise float64 Lewis-weight fixed-point implementation behaves as the input matrix becomes increasingly ill-conditioned, while its exact fixed-point dynamics remain unchanged.

The experiment utilizes the invariance of the exact Lewis map under invertible right transformations: replacing A by AC, for invertible C, leaves every Lewis update unchanged. We construct matrices related by such transformations in exact arithmetic and test how finite-precision computations behave as conditioning worsens. Since the actual inputs are saved float64 arrays, the experiment captures end-to-end rounding effects, including those introduced during matrix construction.

## Matrix family

We use m = 100, n = 20, and five final seeds: 5001 through 5005. Seed 5000 is reserved for development.

For each seed, we initialize `np.random.default_rng(seed)`, draw independent standard Gaussian matrices of sizes 100 × 20 and 20 × 20 (in that order), and take reduced QR factorizations. This gives a 100 × 20 matrix Q with orthonormal columns and a 20 × 20 orthogonal matrix V.

For each conditioning parameter s, we construct a diagonal matrix D_s with entries  2^(-e_l(s)), where e_l(s) = floor(s(l-1)/19 + 1/2) for l = 1,...,20. The conditioned matrix is computed as (Q @ D_s) @ V.T.

We use s = {0, 7, 13, 20, 27, 33, 40, 47, 53}. The ideal matrices have condition number 2^s; the realized condition numbers may differ because of floating-point rounding.

All 45 matrices are stored as `float64` arrays in data/A4_matrix_bank.npz. Final runs load these saved arrays rather than regenerate them. For each matrix, we record its singular values, realized spectral condition number, and SVD numerical-rank warning. Rank warnings are only diagnostic and do not result in excluding any matrix.

## Values of $p$ and numerical update 

We use p = {4, 7.3, 14.2, 32, 64, 127.3}. Therefore, overall, we have 5 bases × 9 conditioning levels × 6 values of p = 270 trajectories. Every trajectory starts from uniform weights w_i = n/m = 0.2.

At each iteration, we set r = 1 - 2/p, form the row-weighted matrix B = W^(r/2) A, and compute its reduced NumPy QR factorization B = QR.

We then compute X = AR^(-1) using X = np.linalg.solve(R.T, A.T).T and update each weight as w_i_new = w_i^r * ||X[i,:]||_2^2.

This is the same numerical implementation used in A1/A2. We do not use explicit matrix inverses, leverage scores extracted directly from the small rows of Q, etc.

## Residual and stopping rule

For each update we record $R\_t=\max\_i\left|\log\frac{w\_i^{(t+1)}}{w\_i^{(t)}}\right|$.

The run stops at the first strict crossing $R\_t<10^{-8}$. We also record the first crossings below $10^{-4}$ and $10^{-6}$ from the same trajectory. The iteration limit is 10,000 updates.

## Run status and benign reference check

A run that remains defined for 10,000 updates without reaching $10^{-8}$ is recorded as `cap_reached`. An arithmetic failure that prevents the computation from continuing is recorded as `hard_failure`.

When a conditioned run reaches the primary threshold, we perform one additional update at its terminal weights using the corresponding benign (s = 0) matrix. We compute the maximum absolute log-ratio between this reference update and the terminal weights. If it is below 1e-8, we assign `reference_agrees_stop`; otherwise, we assign that run the status of `reference_disagrees_stop`.

This reference check uses another `float64` computation, not an exact oracle. It checks consistency with the benign implementation rather than certifying proximity to the true Lewis weights.

## Effect of conditioning on the updates 

For each base and p, we save all update-input states from the benign (s=0) trajectory. At each saved state, we compute both the benign and conditioned float64 updates. We define E\_t(s) as the maximum absolute coordinatewise log-ratio between these two updates, and E\_{\max}(s)=\max\_t E\_t(s).

These measure discrepancies between floating-point implementations; the exact mathematical update maps coincide for this matrix family. The complete E\_t(s) sequence is saved for every base, s, and p.

## Numerical diagnostics

At every valid trajectory update we record:

- $\kappa\_2(B)$ for the weighted matrix $B=W^{r/2}A$;
- the relative mass defect $|\sum\_i w\_i^+-n|/n$;
- the QR reconstruction error $\lVert B-QR\rVert\_F/\lVert B\rVert\_F$;
- the scaled residual of the solve for $AR^{-1}$;
- the terminal weights and the complete residual trajectory.

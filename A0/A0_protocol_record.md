# Experiment A0 protocol record

Experiment A0 tests whether the local contraction factor mu_pred predicted by the Jacobian of the row-wise Lewis fixed-point iteration agrees with the contraction observed late in the numerical iteration on Gaussian matrices.

## Matrices and $p$-grid

We generate 30 matrices $A\in\mathbb{R}^{100\times20}$ with independent standard Gaussian entries and reuse each matrix across all values of $p$.

The original seed list included seed 21. We used seed 21 for a development walkthrough and therefore replaced it with seed 29 for the final experiment. The final seeds are:

```
29, 22, 24, 63, 89, 117, 143, 173, 176, 181,
207, 233, 271, 319, 364, 411, 478, 523, 571, 607,
611, 618, 674, 729, 803, 846, 917, 1009, 1183, 1427
```

The $p$-grid is `3, 4, 6, 8, 11, 12, 16, 17, 24, 32, 48, 54, 61, 64`.

This gives a total of $30\times14=420$ matrix/$p$ runs.

## Lewis iteration

Every run starts from $w\_i^{(0)}=n/m=0.2$. For $\gamma=1-2/p$, we form $B\_t=W\_t^{\gamma/2}A$ and replace the current weights by the row leverage scores of $B\_t$, $w\_i^{(t+1)}=\tau\_i(B\_t)$. We compute these leverage scores via a reduced QR factorization $B\_t=Q\_tR\_t$; this is valid because $\tau\_i(B\_t)=|(Q\_t)\_{i,:}|\_2^2$ for each row i.

We measure progress using the ``log progress ratio'' $Res\_t=\max\_i\left|\log\left(w\_i^{(t+1)}/w\_i^{(t)}\right)\right|$ and stop when $Res\_t<10^{-11}$. 

## Predicted and observed contraction

Let $w^\star$ denote the Lewis fixed point. The local Jacobian in log coordinates has the form $J=\gamma(I-K)$, giving the predicted contraction factor $\mu\_{\mathrm{pred}}=\left(1-\frac{2}{p}\right)\left(1-\lambda\_{\min}(K)\right)$. For the numerical experiment, we evaluate the corresponding quantity near the returned approximation to the fixed point. 

We estimate the observed factor $\mu\_{\mathrm{obs}}$ from the residual trajectory. If $Res\_t\approx C\mu\_{\mathrm{obs}}^t$, then $\log Res\_t$ is approximately linear in $t$. We therefore fit a straight line to $\log Res\_t$ versus $t$ and exponentiate its fitted slope.

The primary fitting window is $10^{-8}\le Res_t\le10^{-4}$. We also repeat the fit over $10^{-7}\le Res\_t\le10^{-4}$ and $10^{-8}\le Res\_t\le10^{-5}$ to assess sensitivity to the fitting range. 

## Comparison and recorded results

For every (matrix, $p$) pair, we compare $\mu\_{\mathrm{pred}}$ with the observed contraction factor from the primary window and retain the two sensitivity-window fits separately. We also record numerical diagnostics for the returned point, checking the accuracy of the returned fixed point and projection matrix, and the stability of the predicted contraction factor near the end of the run.

The main figure compares the observed primary-window contraction factors against the predicted across all 420 runs.


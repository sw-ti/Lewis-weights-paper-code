# Experiment A1 protocol 

Experiment A1 studies how the number of row-wise Lewis fixed-point updates needed to reach a fixed residual threshold varies with $p>2$ on a fixed set of Gaussian matrices.

## Matrices and $p$-grid used

We use 50 independent matrices $A\in\mathbb{R}^{100\times20}$ with i.i.d. standard Gaussian entries, reusing the same matrix at every value of $p$. We reserve seed `2000` for development. The final seeds are:

```
261, 397, 90, 556, 543, 461, 139, 172, 173, 346,
208, 636, 293, 100, 248, 130, 412, 608, 488, 204,
748, 314, 671, 592, 493, 576, 389, 440, 552, 596,
209, 822, 232, 260, 635, 160, 64, 219, 229, 231,
501, 722, 1389, 442, 413, 512, 559, 563, 882, 697
```

We generate each matrix with

```
np.random.default_rng(seed).standard_normal((100, 20))
```

and save the realized array. We choose the following values of $p$: 

`3, 4, 5.7, 7.3, 10.8, 14.2, 19.6, 27.9, 32, 38.4, 54.1, 64, 74.8, 98.6, 127.3`.

This gives $50\times15=750$ matrix/$p$ runs.

## Lewis iteration

Every run starts from the uniform initialization $w\_i^{(0)}=n/m=0.2$. For $r=1-2/p$, one Lewis update replaces the current weights by the row leverage scores of $W^{r/2}A$. Each (matrix, $p$) pair is run independently from the same initialization.

## Residual and iteration counts

We measure progress using $R\_t=\max\_i\left|\log(w\_i^{(t+1)}/w\_i^{(t)})\right|$. The main iteration count is $T\_{10^{-8}}$, the first update for which $R\_t<10^{-8}$. From the same trajectory we also record $T\_{10^{-4}}$ and $T\_{10^{-6}}$. Every run has a 10,000-iteration limit. We save the full residual trajectory and basic final-run diagnostics. For each $p$, the main summary is the median of $T\_{10^{-8}}$ across the 50 matrices, together with the empirical 20th and 80th percentiles. We also planned to retain the mean, standard deviation, minimum, and maximum. The threshold comparison reports the medians of $T\_{10^{-4}}$, $T\_{10^{-6}}$, and $T\_{10^{-8}}$.

The original implementation computed the leverage scores from a reduced QR factorization $B=QR$ using $w\_i^+=|Q\_{i,:}|\_2^2$ in float64 arithmetic.

In the first complete run, 20 of the 750 cases did not reach $10^{-8}$ within 10,000 iterations: one at $p=74.8$, four at $p=98.6$, and fifteen at $p=127.3$. Their behaviour was consistent with loss of relative accuracy when very small rows of $Q$ were extracted directly.

We therefore changed only the numerical evaluation of the same Lewis update. Since $Q=W^{r/2}AR^{-1}$, the leverage score can equivalently be written as $w\_i^+=w\_i^r|a\_i^\top R^{-1}|\_2^2$.

The modified implementation computes $X=AR^{-1}$ by solving $R^\top X^\top=A^\top$ and then sets $w\_i^+=w\_i^r|X\_{i,:}|\_2^2$.

The two computations are identical in exact arithmetic. The matrix bank, $p$-grid, initialization, residual thresholds, 10,000-iteration limit, and analysis remain unchanged.

After checking the modified computation on the numerical failures, we reran all 750 cases from scratch with this new implementation. The final A1 results come from this complete rerun. We retain the earlier  results to document the numerical issue that motivated the change.


# Experiment A2: Lewis weight computation on matrices with special geometry

Experiment A2 studies whether the qualitative dependence of the row-wise Lewis fixed-point iteration count on $p$, as seen in A1, persists on a structured (but well-conditioned) matrix construction with heterogeneous leverage scores.

## Structured matrix construction

For each matrix, we generate 80 independent Gaussian directions $g\_i\sim N(0,I\_{20})$ and normalize them to unit Euclidean norm, $\widetilde{g}\_i=g\_i/|g\_i|\_2$. Let $\widetilde{G}\in\mathbb{R}^{80\times20}$ contain these rows, and define $A=[I\_{20};0.25\widetilde{G}]\in\mathbb{R}^{100\times20}$.

Thus, rows 1-20 (from the identity) are the *anchor rows* and rows 21-100 are what we call the *cloud rows*. By design, the anchor rows have norm 1 and the cloud rows have norm $1/4$.

We use seed 3000 only for development checks. The 50 final matrices use seeds 3001-3050, and each matrix is reused across all values of $p$.

Our choices of $p$ are `3, 4, 5.7, 7.3, 10.8, 14.2, 19.6, 27.9, 32, 38.4, 54.1, 64, 74.8, 98.6, 127.3`. 

These choices give us a total of $50\times15=750$ matrix/$p$ runs.

## Lewis iteration

For every run, we initialize the weights uniformly with $w\_i^{(0)}=n/m=0.2$. For $r=1-2/p$, the row-wise Lewis update is $w\_i^{(t+1)}=\tau\_i(W\_t^{r/2}A)$, where $\tau\_i$ is the $i$th leverage score. We implement this update in float64 using the same rescaled-QR implementation as the final A1 experiment. Writing $B\_t=W\_t^{r/2}A=Q\_tR\_t$, we solve for $X\_t=AR\_t^{-1}$ and compute $w\_i^{(t+1)}=(w\_i^{(t)})^r|(X\_t)\_{i,:}|\_2^2$.

## Residual and iteration counts

We measure progress using $\Res\_t=\max\_i\left|\log(w\_i^{(t+1)}/w\_i^{(t)})\right|$. The main iteration count is $T\_{10^{-8}}$, the first update at which $\Res\_t<10^{-8}$. From the same trajectory we also record $T\_{10^{-4}}$ and $T\_{10^{-6}}$. Every run has a 10,000-iteration limit.

For each $p$, we summarize $T\_{10^{-8}}$ across the 50 matrices using the median and empirical 20th and 80th percentiles. We also planned to retain the mean, standard deviation, minimum, and maximum. The threshold comparison reports the medians of $T\_{10^{-4}}$, $T\_{10^{-6}}$, and $T\_{10^{-8}}$.

## Geometry notes

For each matrix, we record its numerical rank, spectral condition number $\kappa\_2(A)$ (ratio of largest to smallest singular values), row norms, and ordinary $p=2$ leverage scores. The quantity we intended to use to capture contrast in leverage scores is $\frac{\text{mean anchor leverage}}{\text{mean cloud leverage}}$ (and plotted in the second panel of our figure). 

The construction is well-conditioned by design. Since every row of $\widetilde{G}$ has unit norm, $A^\top A=I+\frac{1}{16}\widetilde{G}^\top\widetilde{G}$, and hence $\kappa\_2(A)\le\sqrt{6}$.

## Comparison with A1

We compare A2 with the released A1 Gaussian results using the same values of $p$, initialization, residual, numerical implementation, and stopping rule. For each $p$, we compare the median stopping time across the 50 A2 matrices with the corresponding median across the 50 A1 matrices; the individual matrices are not paired across experiments. The main figure plots the A1 and A2 median $T\_{10^{-8}}$ curves with their empirical 20th and 80th percentiles, and a separate panel showing the geometry of the rows of the matrices in A2. 


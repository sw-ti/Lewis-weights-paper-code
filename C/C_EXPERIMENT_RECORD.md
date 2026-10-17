# Experiment C — Experiment Record

We compare 14 designs on each dataset: uniform, twelve finite-$p$ designs, and D-optimal. We vary the number $f$ of unavailable blocks. Intel uses $f=1,\ldots,42$, with exhaustive enumeration for $f=1,2,3$ and 25,000 frozen masks for each $f=4,\ldots,42$. Gulf uses exact enumeration for $f=1,\ldots,8$. The secondary empirical comparison uses 2,361 eligible Intel anchors and 51,235 Gulf anchors.

## Reproducibility notes

An early Gulf structural audit used the unwhitened `B_blocks` rather than the correct whitened `A_blocks`. This was corrected before the final run and did not change $f_{\max}=8$.

Before the final run, we also set D-optimal weights below $10^{-9}$ to zero and renormalized. This affects structural rank loss but has little effect on nominal information. An independent solver confirms the retained support.


## Results

When few blocks are unavailable, larger-$p$, more concentrated finite-$p$ designs retain more mean information. As $f$ increases, the maximizing finite-$p$ design shifts from $p=64$ to $p=2.2$ on Intel and from $p=32$ to $p=4$ on Gulf. For five sampled values of $f$ on Intel, neighboring designs have close mean information relative to Monte Carlo uncertainty, so the exact transition points should not be overinterpreted.

This shift is not due to rank loss: uniform and all finite-$p$ designs remain full rank on the evaluated masks, yet their mean-information curves cross. The D-optimal design does lose rank when many blocks are unavailable, partly due to its sparse support.

In the secondary empirical comparison, conditional on $1\le f\le f_{\max}$, Intel is maximized by $p=49$ (with $p=64$ very close) and Gulf by $p=24$. Unlike the primary experiment, this comparison mixes different numbers and identities of unavailable blocks.

## Final figure and data

`data/C_exchangeable_summary.csv` contains the complete mean-information and rank-loss results. `figures/C_exchangeable_mu_rho_combined_balanced.pdf` is the final Figure 8 source.
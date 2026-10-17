# Experiment C — Protocol Record

Experiment C measures information preservation under whole-block unavailability. We fix each design allocation before availability is realized; mass assigned to unavailable blocks is lost, not reassigned. We compare surviving information across the uniform–finite-$p$–D-optimal family as the number of unavailable blocks increases. Whereas Experiment B studies nominal design geometry, Experiment C studies information under unavailability. This experiment's results correspond to Figure 8 of our paper. 

## 1. Frozen geometries and designs

We reuse the final frozen geometries and allocations from Experiment B, without refitting or preprocessing:

- **Intel:** 52 physical sensor blocks, latent dimension $r=30$. `data/B_intel_processed_input_frozen.npz` contains the final $C_i$ and three-row factors `B_blocks`, satisfying $F_i^\top F_i=C_i$.
- **Gulf:** 10 physical buoy blocks, latent dimension $r=4$. `data/B_gulf_processed_input_frozen.npz` contains the final whitened $C_i$ and two-row factors `A_blocks`, satisfying $F_i^\top F_i=C_i$.

The design family comprises uniform allocation, twelve finite-$p$ allocations with
$p\in\{2.2,2.5,3,4,6,8,12,16,24,32,49,64\}$, and a C-specific cleaned D-optimal endpoint. Uniform and finite-$p$ allocations are unchanged from `data/B_final_intel_raw.npz` and `data/B_final_gulf_raw.npz`. A-optimal design is excluded.

**D-optimal cleanup.** Starting from each frozen B direct-solver D-optimal vector, we set coefficients $\pi_i<10^{-9}$ to zero and renormalize once. This removes numerical solver dust from inactive coordinates, whose exact support matters for structural rank under block removal. An independent B solver confirms the same retained support, with discarded and retained coefficients separated by many orders of magnitude. The original B vectors remain unchanged; the C-specific cleaned vectors are released in `data/C_intel_Dopt_cleaned_frozen.npz` and `data/C_gulf_Dopt_cleaned_frozen.npz`.

## 2. Exact-severity unavailability and mask banks

We vary the number $f$ of unavailable physical blocks, selecting exactly $f$ blocks uniformly at random. This exchangeable model separates failure severity from block identity; it is a controlled mechanism experiment, not a stochastic hardware-failure model.

We evaluate every integer severity for which at least one surviving subset can span the latent space:

- **Intel:** $f=1,\ldots,42$.
- **Gulf:** $f=1,\ldots,8$.

Rank-deficient masks within these ranges are included. At greater severities, every surviving subset is structurally rank deficient.

**Mask construction:**

- **Gulf:** Enumerate all exact-$f$ unavailable subsets.
- **Intel:** For $N_f=\binom{52}{f}\le25{,}000$, enumerate all subsets; otherwise sample 25,000 distinct subsets uniformly without replacement. Thus $f=1,2,3$ are exhaustive, while $f=4,\ldots,42$ use 25,000 masks each.

Every design uses the same masks at each dataset/severity. The frozen banks, `data/C_intel_exchangeable_mask_bank_frozen.npz` and `data/C_gulf_exchangeable_mask_bank_frozen.npz`, are the source of truth. In their bit representation, bit $j=1$ denotes an unavailable block.

## 3. Information and structural rank

For allocation $\pi$ and available set $S$, define $M\_S(\pi)=\sum\_{i\in S}\pi\_i C\_i$ and 
$\Phi\_S(\pi)=\det(M_S(\pi))^{1/r}.$ 
The allocation is fixed before $S$ is observed, without renormalizing the surviving mass.

A block is active precisely when it is available and its saved coefficient satisfies $\pi\_i>0$.

**Structural rank test.** Stack the active block factors without weights. For singular values $s_1\ge s_2\ge\cdots$, declare rank loss if fewer than $r$ exceed $\tau=\epsilon\_{\mathrm{mach}}\max\{m\_{\mathrm{stack}},r\}s_1,$ where $m\_{\mathrm{stack}}$ is the stack's row count. This tests span/support, not determinant magnitude or conditioning.

If rank is deficient, set $\Phi_S(\pi)=0$. Otherwise, stack the active factors weighted by $\sqrt{\pi_i}$. From their $r$ positive singular values $\sigma_1,\ldots,\sigma_r$, compute $
\log\Phi_S(\pi)=\frac{2}{r}\sum_{j=1}^r\log\sigma_j$ and $\Phi_S(\pi)=\exp(\log\Phi_S(\pi)).$

Unusable weighted singular values in a structurally full-rank case are treated as an error, not rank loss. Intel uses `B_blocks`; Gulf uses the whitened `A_blocks`, never the unwhitened `B_blocks`.

## 4. Primary summaries

For each severity $f$ and design $\pi$, report:

- **Mean surviving information:** $\mu_f(\pi)$, the mean of $\Phi_S(\pi)$ over the exact-$f$ mask bank, with rank-deficient masks contributing zero.
- **Rank-loss probability:** $\rho_f(\pi)$, the fraction of masks declared structurally rank deficient.

These estimate expected information and rank-loss probability under the exchangeable exact-severity model, not worst-case robustness.

## 5. Secondary empirical-availability experiment

We also evaluate actual availability patterns on complete regular time grids, conditioning on $1\le f\le f_{\max}$ ($f_{\max}=42$ for Intel, $8$ for Gulf). Each eligible time anchor has equal weight, so repeated masks retain their empirical frequencies. All-available periods and severities beyond $f_{\max}$ are reported separately.

- **Intel:** Every five-minute anchor from 2004-03-09 00:00 through 2004-03-18 23:55. A sensor is available when all three B coordinates are finite after the B health filter and five-minute aggregation. The grid includes bins with no usable rows.
- **Gulf:** Every half-hour anchor from 2020-01-01 00:00 through 2023-12-31 23:30. A buoy is available when a single report contains both PRES and WSPD within $\pm5$ minutes of the nearest anchor. No 2024 data are used.

We report the frequency-weighted mean surviving information $\mu_{\mathrm{emp}}(\pi)$ and rank-loss frequency $\rho_{\mathrm{emp}}(\pi)$ over eligible anchors. This is a secondary operational check, since empirical masks mix failure severity and block identity.

## 6. Final figure

The paper figure contains four panels: Intel and Gulf mean surviving information $\mu_f$ (top row) and rank-loss probability $\rho_f$ (bottom row). It includes uniform allocation, all twelve finite-$p$ designs, the cleaned D-optimal endpoint, and every prescribed severity.

**Frozen figure:** `figures/C_exchangeable_mu_rho_combined_balanced.pdf`.
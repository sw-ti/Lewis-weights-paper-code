# Experiment A4: final experiment record

The final run used all 45 saved matrices and all six values of $p$, for 270 trajectories and 270 common-state map-discrepancy records. The released data contain every case; there were no hard computational failures.

## Experiment result

The realized conditioning ranges from $\kappa\_2(A)=1$ at $s=0$ to approximately $8.30\times10^{15}$--$9.25\times10^{15}$ at $s=53$. The intermediate levels relevant to the transition are approximately $1.34\times10^8$ at $s=27$ and $8.59\times10^9$ at $s=33$. Exact per-base values are in `data/A4_matrix_diagnostics.csv`.

At $s=0,7,13,20$, all 30 base/$p$ runs at each stress level ended with `reference_agrees_stop`. At $s=27$, all 30 trajectories reached their own $10^{-8}$ stopping threshold: 27 also passed the benign-reference check and 3 were `reference_disagrees_stop`. Their stopping times differed from the matched benign runs by at most about 1.76%. At each of $s=33,40,47,53$, all 30 runs reached the 10,000-update cap; none was a hard failure. The complete $p$-by-$s$ counts are in `data/A4_status_counts.csv`.

The corresponding all-five envelope reaches $s=27$ for $p\in\{4,7.3,14.2,32\}$ and $s=20$ for $p\in\{64,127.3\}$. The released values are in `data/A4_robustness_envelope.csv`.

The common-state discrepancy $E\_{\max}$ grows steadily with conditioning in the released run. Across all bases and values of $p$, it lies between about $5.0\times10^{-9}$ and $1.2\times10^{-8}$ at $s=27$, and between about $2.6\times10^{-7}$ and $6.3\times10^{-7}$ at $s=33$. Full per-run values are in `data/A4_summary_by_run.csv`, with per-$p$/stress summaries in `data/A4_map_error_summary.csv`.

At $s=33$, all 30 trajectories cross $10^{-6}$ but none reaches $10^{-8}$ within 10,000 updates. Their minimum residuals over the whole run are about $4.0\times10^{-8}$--$7.2\times10^{-8}$, while the median residual over the final 1,000 updates is about $1.4\times10^{-7}$--$1.9\times10^{-7}$. This is the residual-floor behaviour illustrated in Figure 5.

## Figures used in the paper

Figure 4 plots $E\_{\max}$ against the realized $\kappa\_2(A)$ for $p=4,32,127.3$, showing all five bases and all positive stress levels. It is generated from `data/A4_summary_by_run.csv`. The exact PDF used in the submitted paper is released as `figures/A4_Tp_vs_p_conditioning.pdf`; the historical filename is misleading because this final file contains the $E\_{\max}$ figure, not the earlier $T(p)$ display.

Figure 5 plots the complete residual trajectories at $s=33$ for $p=4,32,127.3$, with all five bases in each panel. It uses the 15 corresponding files in `data/raw/trajectories/` and is released as `figures/A4_posthoc_s33_residual_floor.pdf`. The choice to use the $s=33$ residual-floor display was made after inspecting the completed experiment, as an explanatory view of the already-released trajectories.

The original protocol called for the Figure-4 quantity to be displayed against controlled stress $s$ in six $p$-panels. The paper later uses realized $\kappa\_2(A)$ and three representative $p$-panels instead. This changes the presentation, not the underlying $E\_{\max}$ data.

## Numerical reproducibility note

The saved `float64` matrices are our inputs. They are rounded dense products, so at extreme stress they need not be exact right transforms of one another as stored. A4 therefore supports an end-to-end finite-precision interpretation, not a claim that QR/solve error is the only source of discrepancy.

The original run used the software stack recorded in `data/A4_environment_record.txt`. Independent reevaluation under a different NumPy/BLAS environment reproduced the qualitative deterioration with conditioning but not every stressed floating-point value bit-for-bit. The precise high-conditioning numbers should therefore be read as results for the recorded implementation and environment, not as a universal float64 threshold.

The public reproduction notebook fixes two unused failure-handling/display branches found during audit; neither branch was exercised by any released A4 case, so this does not change the released results.

# Experiment C: block unavailability

Experiment C studies how the design allocations from Experiment B preserve determinant information when entire sensor/buoy blocks are unavailable and the allocation already committed to those blocks is lost (i.e., we aren't allowed to reallocate to available blocks). This corresponds to Figure 8 of our paper. 

- `C_protocol_record.md` describes the planned experiment.
- `C_EXPERIMENT_RECORD.md` describes what was actually run, any corrections, results, and interpretation limits.
- `C_reproduce.ipynb` checks the released inputs/results and reproduces the scientific content of the paper figure.
- `data/` contains the frozen B-derived geometries/designs, frozen C mask banks, and released C result summaries.
- `figures/` contains the exact paper figure in PDF/SVG and a PNG preview.
- `requirements.txt` records the environment used to validate this public notebook.

To reproduce the package, create an environment from `requirements.txt`, start Jupyter in this `C/` directory, and run `C_reproduce.ipynb` from top to bottom. This reproduces Figure 8 from the released final summaries, recomputes all Gulf primary cases, the first two Intel severity classes, and all empirical unique-mask calculations directly from the released inputs. Set `FULL_RECOMPUTE = True` in the notebook to recompute the complete primary experiment from the frozen geometries, designs, and mask banks.


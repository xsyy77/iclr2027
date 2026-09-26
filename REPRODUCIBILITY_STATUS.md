# Reproducibility status and result families

**Verified in this package:** `python paper_checks/verify_bundle.py` passes 34/34 checks for the saved current DrugMechDB clean-condition Table 1 family, Stage-II AUROC/AUPRC, Stage-II utility-weight check, and Stage-III fixed/global/mean/shuffled controls. `python evaluation/evaluate_saved_results.py` recomputes saved detailed DrugMechDB and EpiKG aggregates. The source and data needed to inspect score definitions and benchmark splits are included.

**Requires path adaptation / fresh-run validation:** original recovered scripts under `code/original_recovered/` preserve historical `/mnt/data/...` input and output paths. The package includes relevant data and methods, but we have not asserted a clean-room fresh rerun of every table from these scripts in this release.

**Separate experimental families:** the 248-query clean-condition DrugMechDB table has Full Path Success 39.52% and Edge Recall 59.46%. The saved 1,240-row mixed-condition intervention in `results/dmdb/` has different aggregates; it must not be used as if it were the same test condition. Stage-III controls also fix the selected Bridge set and report lower Edge Recall; their figures answer a different question.

**Transfer:** `transfer/cwq/` and `transfer/biohopr/` contain source and saved outputs. CWQ uses target-agnostic ranked-answer metrics and a sampled training subset with full test evaluation; the saved CWQ Ours-TA run is not uniformly better than Fixed Bridge. BioHopR uses a held-out target-agnostic ranking protocol and external BioHopR/PrimeKG inputs. Neither experiment establishes that the complete DrugMechDB mechanistic result transfers unchanged. OpenBioLink has no verified runnable result in the recovered materials and is not presented as reproduced.

**Historical code:** `code/reconstructed_legacy/` is labeled as reconstructed compatibility code. Original recovered source is kept separately. Some historical results have surviving outputs without exact original generators; consult `audit/MISSING_SOURCE_BUT_OUTPUTS_EXIST.md`.

**External inputs:** CWQ, BioHopR, and PrimeKG benchmark files are not redistributed due to size and third-party provenance. Obtain them from their original providers, then supply paths as described in the root README. Exact versions/checksums for those external corpora were not preserved, so bit-for-bit replay is not guaranteed.

# Reproducibility status


The current download is [iclr2027_review_package_v3.zip](iclr2027_review_package_v3.zip). All paths below are relative to the extracted `iclr2027_review_package` directory.


**Numerical verification.** `python paper_checks/verify_bundle.py` passes 34/34 checks for saved DrugMechDB clean-condition results, Stage-II discrimination, utility-weight sensitivity, and Stage-III controls. These checks verify saved predictions and summaries; they do not retrain the models.


**Archived DrugMechDB selection scope.** `python portable/check_archived_bridge_selection.py` verifies that all 1,195 validation and 1,240 test query-condition groups have at most three distinct candidate Bridges. The v3 set-preserving adapter retains every candidate and its saved confidence within this reported budget. The original DrugMechDB `Red(h,H)`, `mu`, and complete manuscript gain selector were not recovered. The adapter raises an error for larger pools; it is not a general reconstruction. See `audit/DRUGMECHDB_SELECTOR_PROVENANCE.md` in the ZIP.


**Independent retrieval rerun.** `python portable/reproduce_dmdb.py` invokes the adapter and reruns archived DrugMechDB mixed-condition retrieval from the packaged raw graph, manifest, and saved Bridge/gate predictions. The regenerated overall, validation-grid, and bootstrap CSVs match the archived versions byte-for-byte (3/3). This is distinct from the 248-query clean-condition table and does not retrain the predictors.


**Stage-II controlled intervention.** `results/section5/stage2_selection_intervention_summary.csv` and `stage2_selection_bootstrap.csv` are included. Their recovered generator compares one-Bridge selections under a fixed gate. These outputs do not establish the missing three-Bridge redundancy-aware gain implementation.


**EpiKG limitation.** The archived EpiKG script uses process-randomized `hash(rel)` for sampling and split assignment, and the original Python hash seed was not recorded. A fresh run yielded test Bridge AUROC 0.8676 versus archived 0.8810. Thus archived EpiKG outputs are inspectable, but exact replay is unverified. Its layer-diversity selector is separate from the missing DrugMechDB selector.


**Other scope.** The 248-query clean-condition DrugMechDB table, 1,240-row mixed-condition intervention, and fixed-Bridge Stage-III controls are separate protocols. CWQ and BioHopR code and saved outputs are included; large third-party corpora are not redistributed. Saved CWQ Ours-TA does not uniformly outperform Fixed Bridge. OpenBioLink has no verified runnable result. Historical reconstructed scripts are labeled separately from recovered source. Consult the ZIP README, provenance audit, and `FILE_MANIFEST.csv` before a fresh run.

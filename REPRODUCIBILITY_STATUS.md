# Reproducibility status

**Numerical verification.** `python paper_checks/verify_bundle.py` passes 34/34 checks for saved DrugMechDB clean-condition results, Stage-II discrimination, utility-weight sensitivity, and Stage-III controls. These checks verify saved predictions and summaries; they do not retrain the models.

**Independent retrieval rerun.** `python portable/reproduce_dmdb.py` in the v2 ZIP reruns the archived final DrugMechDB mixed-condition retrieval from the packaged raw graph, manifest, and saved Bridge/gate predictions. The regenerated overall, validation-grid, and bootstrap CSVs match the archived versions byte-for-byte (3/3). This is not the separate 248-query clean-condition table and does not retrain the predictors.

**EpiKG limitation.** The archived EpiKG script uses process-randomized `hash(rel)` to seed sampling and split assignment. Its original Python hash seed was not recorded. A fresh run using the packaged graph and MCQ data yielded test Bridge AUROC 0.8676, versus 0.8810 in the archived output. Therefore the archived EpiKG metrics are inspectable, but exact replay is currently unverified.

**Other scope.** The 248-query clean-condition DrugMechDB table, 1,240-row mixed-condition intervention, and fixed-Bridge Stage-III controls are separate protocols and must not be pooled. CWQ and BioHopR sources and saved outputs are included; their large third-party corpora are not redistributed. The saved CWQ Ours-TA result does not uniformly outperform Fixed Bridge. OpenBioLink has no verified runnable result in the recovered materials. Historical reconstructed scripts are labeled separately from original recovered source. Consult the ZIP README and provenance audit before a fresh run.

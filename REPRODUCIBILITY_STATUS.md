# Reproducibility status — v5

Current manuscript and scientific responses are the root PDF/LaTeX/REBUTTAL.md and their matching copies in the v5 update. Use all four v4 volumes as the unchanged base, then extract v5 over the shared directory. CURRENT_VERSION.json identifies exact hashes. The earlier status below describes the frozen v4 base and its historical snapshots.

The v5 manifest includes the Clean constructor, all 3,682 ordered-list comparisons, the full automatic pipeline generator, per-query records, validation constant selection, paired intervals and independent implementation-verification scripts. Clean exact_verification.json and full-automatic verification.json are the completed checks; the full-automatic completion.json retains its earlier pending flag as an execution-stage record. Verification covers 4,090 paths and 32 intervals, without new model fitting. This is implementation verification on existing data, not independent validation.

The Clean rule is reference-informed and output-equivalent, not an authenticated historical writer. Full automatic gate and VAL-selected g0.45 yield identical per-query PS/ER/utility. Stable superiority over the simpler Proposal Top3 configuration is not established; neither is inferiority or equivalence established by the interval crossing zero. Separate positive usefulness and type-complementarity results retain their scope.

## Preserved v4 base status

# Reproducibility status — current v4 revision

Current artifacts: [v4 package](iclr2027_review_package_v4.zip), [revised paper](paper_REVISED.pdf), [version/checksums](CURRENT_VERSION.json). Download and extract all v4 ZIP volumes listed in README/CURRENT_VERSION into the same directory. The split only addresses the upload API size limit. This is a frozen evidence synchronization, not another experiment.

The archive includes the unchanged original v3 ZIP for byte-preservation checks, original package sources/data, and the exact frozen supplementary inputs and scripts. `verify_release.py` checks every manifest entry and the main reported values without training. The selector/path/gate verification commands are listed in README.

The complete selector is an explicit empty-start reconstruction of the original Gain, evaluated on239/248 oversized automatic pools. This closes operational execution, not historical-source identity. The main Clean pools still do not test filtering. TypeBonus's component interval is positive; whole-selector/Red independent benefits are not established.

The controller attribution is corrected to actual Table1 Full (ER59.46%,mean gate.36035). Revised manuscript Table5 replaces the original gate Table4.124,992 previously frozen control outcomes have been replayed. Strong constants are competitive and paired uncertainty is retained. The old ER38.05%/mean gate.10917 summary is a labeled correction record, not silently relabeled current evidence.

Figure3 and automatic retrieval both use14,029 normalized candidate records. The matching data and statistics are included. Complete-selector, matched path and gate controls have separate candidate/supervision scopes; small combined weighting/control gains are not isolated greedy/gate gains. Mechanism-record splits are not strict endpoint-pair isolation. Reused test diagnostics and failed development branches are labeled, not fresh confirmation.

The main manuscript retains the original three-stage method and limits unsupported claims. EpiKG replay's original hash-seed limitation and other transfer limitations remain as documented in the historical package.

## Historical v3 status

# Reproducibility status


The current download is [iclr2027_review_package_v3.zip](iclr2027_review_package_v3.zip). All paths below are relative to the extracted `iclr2027_review_package` directory.


**Numerical verification.** `python paper_checks/verify_bundle.py` passes 34/34 checks for saved DrugMechDB clean-condition results, Stage-II discrimination, utility-weight sensitivity, and Stage-III controls. These checks verify saved predictions and summaries; they do not retrain the models.


**Archived DrugMechDB selection scope.** `python portable/check_archived_bridge_selection.py` verifies that all 1,195 validation and 1,240 test query-condition groups have at most three distinct candidate Bridges. The v3 set-preserving adapter retains every candidate and its saved confidence within this reported budget. The original DrugMechDB `Red(h,H)`, `mu`, and complete manuscript gain selector were not recovered. The adapter raises an error for larger pools; it is not a general reconstruction. See `audit/DRUGMECHDB_SELECTOR_PROVENANCE.md` in the ZIP.


**Independent retrieval rerun.** `python portable/reproduce_dmdb.py` invokes the adapter and reruns archived DrugMechDB mixed-condition retrieval from the packaged raw graph, manifest, and saved Bridge/gate predictions. The regenerated overall, validation-grid, and bootstrap CSVs match the archived versions byte-for-byte (3/3). This is distinct from the 248-query clean-condition table and does not retrain the predictors.


**Stage-II controlled intervention.** `results/section5/stage2_selection_intervention_summary.csv` and `stage2_selection_bootstrap.csv` are included. Their recovered generator compares one-Bridge selections under a fixed gate. These outputs do not establish the missing three-Bridge redundancy-aware gain implementation.


**EpiKG limitation.** The archived EpiKG script uses process-randomized `hash(rel)` for sampling and split assignment, and the original Python hash seed was not recorded. A fresh run yielded test Bridge AUROC 0.8676 versus archived 0.8810. Thus archived EpiKG outputs are inspectable, but exact replay is unverified. Its layer-diversity selector is separate from the missing DrugMechDB selector.


**Other scope.** The 248-query clean-condition DrugMechDB table, 1,240-row mixed-condition intervention, and fixed-Bridge Stage-III controls are separate protocols. CWQ and BioHopR code and saved outputs are included; large third-party corpora are not redistributed. Saved CWQ Ours-TA does not uniformly outperform Fixed Bridge. OpenBioLink has no verified runnable result. Historical reconstructed scripts are labeled separately from recovered source. Consult the ZIP README, provenance audit, and `FILE_MANIFEST.csv` before a fresh run.

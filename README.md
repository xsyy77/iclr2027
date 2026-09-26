# Anonymous review artifacts

Code, saved data, and reproducibility checks for the accompanying ICLR submission. Download [the full review package](iclr2027_review_package.zip) and read [the reproducibility status](REPRODUCIBILITY_STATUS.md) before interpreting results. The ZIP contains source files in browsable directories after extraction; it is approximately 9.4 MB.

## Quick verification

```bash
unzip iclr2027_review_package.zip
cd iclr2027_review_package
python -m venv .venv
. .venv/bin/activate
pip install -r requirements.txt
python scripts/check_project.py
python paper_checks/verify_bundle.py
python evaluation/evaluate_saved_results.py
```

The bundled numerical check passes 34/34 tests for current DrugMechDB clean-condition results, Bridge-reliability discrimination, utility-weight sensitivity, and Stage-III controls. It verifies saved outputs; it is not a fresh training run.

## Package contents

- `methods/` and `code/original_recovered/`: current retrieval and reliability implementations, plus historically recovered experiment sources.
- `data/`, `checkpoints/`, `results/`: raw DrugMechDB/EpiKG resources, fixed splits, saved predictions, checkpoints, and experiment outputs.
- `paper_checks/`: claim-level numerical verification, sensitivity scripts, and figure scripts.
- `transfer/`: CWQ and BioHopR target-agnostic runner sources and saved results. Their large third-party benchmark corpora are not redistributed.
- `audit/` and `FILE_MANIFEST.csv`: source provenance, missing historical sources, and SHA-256 file hashes.

`code/reconstructed_legacy/` contains compatibility reconstructions, clearly separated from original recovered source. Some archived scripts retain historical `/mnt/data` paths; adapt those paths for a fresh run. Do not mix the 248-query clean-condition DrugMechDB table with the separate 1,240-row intervention or with Stage-III fixed-Bridge controls. OpenBioLink has no verified runnable result in this recovered package. Full limitations and transfer scope are in [REPRODUCIBILITY_STATUS.md](REPRODUCIBILITY_STATUS.md).

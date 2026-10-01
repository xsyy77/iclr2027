# Anonymous review artifacts

Code, saved inputs and outputs, and reproducibility checks for the accompanying ICLR submission. Download the [current v3 review package](iclr2027_review_package_v3.zip) and read [REPRODUCIBILITY_STATUS.md](REPRODUCIBILITY_STATUS.md) before interpreting results. The previous [v2 snapshot](iclr2027_review_package_v2.zip) remains available for comparison. The extracted v3 directory is named `iclr2027_review_package`.

## Quick verification

```bash
unzip iclr2027_review_package_v3.zip
cd iclr2027_review_package
python -m venv .venv
. .venv/bin/activate
pip install -r requirements.txt
python scripts/check_project.py
python paper_checks/verify_bundle.py
python evaluation/evaluate_saved_results.py
python portable/check_archived_bridge_selection.py
python portable/reproduce_dmdb.py
```

The saved-result check passes 34/34 claims. The archived-pool check confirms that all 1,195 validation and 1,240 test query-condition groups contain at most three distinct Bridges. With the included set-preserving adapter invoked, the fresh DrugMechDB mixed-condition retrieval reproduces three archived output CSVs byte-for-byte. This rerun uses saved Bridge and gate predictions; it is not a fresh model-training run.

## Package map and provenance

- `code/original_recovered/` and `methods/`: recovered experiment code and scoring definitions.
- `data/`, `checkpoints/`, and `results/`: packaged inputs, saved predictions, model files, and outputs.
- `paper_checks/`: checks of saved numerical claims and figure-generation materials.
- `transfer/`: CWQ and BioHopR code and saved outputs; large third-party corpora are not redistributed.
- `audit/` and `FILE_MANIFEST.csv`: provenance, limitations, and SHA-256 file checksums.

The DrugMechDB manuscript describes a redundancy-aware greedy Bridge gain, but the original `Red(h,H)`, `mu`, and complete selector source were not recovered. The new `portable/archived_bridge_selection.py` is explicitly a **set-preserving adapter for the archived validation/test candidate pools**, not a reconstruction of that general gain rule; it fails for pools above the three-Bridge budget. See `audit/DRUGMECHDB_SELECTOR_PROVENANCE.md` inside the ZIP. Historical compatibility reconstructions under `code/reconstructed_legacy/` are also labeled separately from recovered source.

Do not combine the 248-query clean-condition DrugMechDB table with the separate 1,240-row mixed-condition intervention or the Stage-III fixed-Bridge controls. EpiKG exact replay is limited by an unrecorded Python hash seed. OpenBioLink has no verified runnable result in this package. Further details are in [REPRODUCIBILITY_STATUS.md](REPRODUCIBILITY_STATUS.md).

# Code and reproducibility materials

This archive contains executable methods, data, checkpoints, frozen per-query outputs and verification scripts. It contains no manuscript, LaTeX project or reviewer-response draft.

Extract this archive, enter `iclr2027_review_revision/`, and run:

```
python verify_code_release.py
pip install -r package/iclr2027_review_package/requirements.txt
python audit_output/verify_clean_fixture_exact_20261117.py
python audit_output/verify_full_automatic_20261117.py
```

The first command checks every file hash and saved-result consistency without training or retrieval. The latter two verify the reference-informed Clean constructor and the frozen full automatic combination. See package/iclr2027_review_package/README.md for original execution instructions, audit_output/reproduction_environment_lock.txt for serialized-model dependencies, and each protocol JSON for its exact configuration.

Clean construction is an output-equivalent reconstruction, not authentication of its historical writer. It explicitly uses reference answers; automatic proposal does not. The existing TEST records were previously inspected, and the reconstruction was identified using saved inputs including TEST. Frozen automatic-pipeline verification does not fit or retune models. development_not_adopted/ preserves unsuccessful branches without substituting them into the released method. Older comments and status files inside package/ retain their historical context.

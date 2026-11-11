# Review artifacts

The current revision is [v4 frozen code and evidence](iclr2027_review_package_v4.zip), aligned with the [revised paper](paper_REVISED.pdf) (10 main pages,18 total). Read the [scientific reviewer responses](REBUTTAL.md), [REPRODUCIBILITY_STATUS.md](REPRODUCIBILITY_STATUS.md) and [CURRENT_VERSION.json](CURRENT_VERSION.json). The original [v3 package](iclr2027_review_package_v3.zip) is preserved unchanged.

## Download all v4 volumes

- [iclr2027_review_package_v4.zip](iclr2027_review_package_v4.zip)
- [iclr2027_review_package_v4_part2.zip](iclr2027_review_package_v4_part2.zip)
- [iclr2027_review_package_v4_part3.zip](iclr2027_review_package_v4_part3.zip)
- [iclr2027_review_package_v4_part4.zip](iclr2027_review_package_v4_part4.zip)

Extract every volume into the same directory before verification. The volumes contain disjoint files and preserve the original bytes.

## Verify the current revision

```
python -c "import zipfile; from pathlib import Path; [zipfile.ZipFile(p).extractall('.') for p in sorted(Path('.').glob('iclr2027_review_package_v4*.zip'))]"
cd iclr2027_review_revision
pip install -r package/iclr2027_review_package/requirements.txt
python verify_release.py
python audit_output/verify_stage2_original_greedy.py
python audit_output/verify_matched_path_scorer.py
python audit_output/verify_reconciled_gate_statistics.py
```

The package contains original data/code/checkpoints, the complete executable selector reconstruction, automatic-proposal evidence, main-configuration constant/shuffle gate controls, a matched compact path scorer, split/order audits, normalized Figure3 data, and the scientific eleven-question responses. Its manifest checks exact bytes. Private author notes and reviewer screenshots are excluded.

## Read the results in their stated scope

- Table1 uses supplied reference-aligned Clean candidates. Semantic+Fixed versus Utility+Fixed tests confidence weighting, not subset filtering.
- Revised Table4 tests the complete empty-start original-formula reconstruction on oversized automatic pools. TypeBonus has positive pointwise ablation evidence; stable complete-rule and Red advantages remain unestablished.
- Revised Table5/Figure4 use exactly Table1 Full candidates/confidences/predictions/budgets. Constants remain competitive; the paired gate-minus-mean utility interval includes zero. Historical `revised_table4.csv` file naming is documented inside the archive.
- Automatic proposal is evaluated separately and Figure3 uses the same14,029 candidate records. Coverage availability is distinct from final fixed-budget superiority.
- Matched path-control conclusions cover one compact numeric model and supplied candidate access. Split sensitivity is not a new blind holdout.
- Unsuccessful development refinements are recorded separately and not substituted into the main method. No new fitting, TEST tuning or method change was performed for this release.

The v4 README supersedes interpretation of the historical v3 README where the revision explicitly corrects configuration attribution or supplies a reconstructed operational selector. It does not claim authenticated recovery of missing historical code.

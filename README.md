# Review artifacts

The current paper is [paper_REVISED.pdf](paper_REVISED.pdf), with [LaTeX source](iclr2027_conference.tex) and the [complete source project](manuscript_source.zip) (10 main pages, 18 total). Read the [reviewer responses](REBUTTAL.md), [version and checksums](CURRENT_VERSION.json), and [reproducibility status](REPRODUCIBILITY_STATUS.md).

## Code, data and verification

Download all four base volumes: [v4 part 1](iclr2027_review_package_v4.zip), [part 2](iclr2027_review_package_v4_part2.zip), [part 3](iclr2027_review_package_v4_part3.zip), [part 4](iclr2027_review_package_v4_part4.zip). Extract them into the same directory. Then extract the [v5 code and evidence update](iclr2027_review_update_v5.zip) there, allowing the manuscript and rebuttal files to be replaced. All archives share `iclr2027_review_revision/` as their root. The original [v3 archive](iclr2027_review_package_v3.zip) and all v4 volumes are preserved unchanged.

From the extracted root:

```
python verify_v5.py
pip install -r package/iclr2027_review_package/requirements.txt
python audit_output/verify_clean_fixture_exact_20261117.py
python audit_output/verify_full_automatic_20261117.py
```

The first command checks exact file hashes and recorded results without fitting or retrieval. The later checks reproduce existing inputs and frozen outcomes, not new optimization. README_V5.md documents scope. The v4 manifest verifies the historical base before applying v5; verify_v5.py verifies the current manuscript and replies after the update.

## Evidence and scope

- Clean uses reference-informed node identities, including the intermediate answer, but supplies no complete path or relation sequence. The deterministic constructor matches all 3,682 ordered saved lists. Output-equivalent reconstruction does not authenticate the historical writer; its rule was identified using saved inputs including TEST.
- Automatic proposal uses endpoints, query intent and the fixed graph, excluding intermediate answers and query-specific reference membership. Candidate coverage availability increases from 100/248 to 152/248 before truncation. Fixed automatic Top3 PS/ER are 35.08/38.73% versus Graph Only 27.02/33.97%.
- Table4 tests complete greedy selection on oversized automatic pools. Usefulness estimation and the TypeBonus ablation have positive evidence in their matched settings; independent whole-selector and Red superiority remain unestablished.
- Table5 fixes the actual Table1 Full configuration for gate controls. Table6 reports the complete automatic combination: PS/ER/utility 33.47/41.84/0.35980. Its validation-selected constant reaches identical per-query PS/ER/utility. Full-minus-Proposal utility CI spans zero. This bounds gate/whole-pipeline claims without negating the separately supported component results.
- No refitting, TEST retuning, replacement method or adoption of rejected Bridge51 branches occurs in this synchronization. Previously inspected TEST results are not fresh independent confirmation. Pointwise intervals are not multiplicity-adjusted proof of every component.

Private author notes and reviewer screenshots are excluded. Historical v4 manuscript/rebuttal snapshots remain inside the unchanged base archives; the v5 overlay and root paper/replies are current.

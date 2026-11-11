# Rebuttal — Submission 36251

## Rebuttal to Reviewer 9P9q

Thank you for the detailed review and for recognizing the value of distinguishing candidate availability, downstream usefulness, and query-specific guidance. Your comments prompted us to clarify the technical contribution, compare against a learned path scorer, and examine the practical effects of selection and gating. We address your four questions below.

### Q1 / W1 — Contribution relative to adaptive retrieval, and a matched learned path scorer

**Our contribution is a retrieval-aware formulation of intermediate mechanistic guidance with separately testable coverage, utility, and influence decisions.** The technical problem is that a semantically plausible intermediate entity may be unavailable, unhelpful to the constrained retriever, or harmful at an inappropriate guidance strength. We make these distinctions operational: Stage I supplies graph-linked hypotheses; Stage II learns candidate usefulness from downstream retrieval benefit; Stage III controls their influence while every retrieved edge remains in the fixed graph. Standard logistic and greedy primitives implement this formulation; the contribution is the retrieval objective, graph-constrained use of intermediate guidance, and controlled evidence separating these decisions.

**The evidence connects this formulation to retrieval behavior.** In the controlled Clean setting, retrieval-aware reliability distinguishes useful candidates with AUROC/AUPRC **0.815/0.655**, versus lexical similarity **0.515/0.257**. In the separate automatic-candidate diagnostic, the original-formulation reliability model yields **0.870/0.108**, versus lexical **0.609/0.036**. These results support a usefulness signal beyond semantic plausibility and beyond supplied Clean candidates. Section 5.4 separately reports the resulting ER–PS tradeoff, so discrimination is not substituted for downstream performance.

The gate acts at a different point from Self-RAG's reflection tokens for passage retrieval and generation critique: it adjusts the strength of selected Bridge guidance during graph traversal. BTE-RAG provides relevant biomedical graph context for generated answers; we study intermediate path and evidence recovery under explicit graph-search budgets. The revision makes these task and control distinctions explicit rather than presenting interpolation or logistic regression themselves as new inventions.

**The requested learned-path comparison supports the guidance formulation under matched candidate access, inference budget, and supervision source.** The compact class-balanced logistic path scorer shares the graph, frozen Bridge confidences and permitted query context, with beam width 2, at most 8 hops, 14 visited nodes and 20 outgoing-edge considerations. It is trained only on TRAIN path prefixes using the same reference-derived utility, **M = 0.7 PS + 0.3 ER**. Prefix-benefit labels and the gate's query-level labels differ in granularity and count; no validation or test prefixes supply training labels.

| Supplied-Clean setting | PS (%) | ER (%) | Utility |
|:--|--:|--:|--:|
| Graph Only | 27.02 | 33.97 | 0.2910 |
| Learned path scorer | 35.48 | 56.93 | 0.4192 |
| Utility+Fixed | 39.52 | 58.94 | 0.4534 |
| Full | 39.52 | 59.46 | 0.4550 |

Full exceeds this path scorer by **4.03 PS points** (paired pointwise 95% CI **[1.57, 6.94]**) and **2.53 ER points [0.62, 4.46]**. Thus the supplied-Clean result is supported against the requested compact learned alternative, not only against Graph Only. This is one numeric linear path scorer; Utility+Fixed also has higher point estimates, so this comparison supports the guidance formulation rather than attributing its entire margin to the gate.

**We also quantify the smaller increment over fixed semantic guidance.** Full minus Semantic+Fixed gives ER **+0.90 points [0.23, 1.69]** and utility **+0.00552 [0.00118, 0.01275]**; PS is **+0.40 points [0.00, 1.24]**. These are combined confidence-weighting/control effects. The independent gate comparison is addressed in Q3. On frozen automatic Top3, gate-based guidance and the path scorer reach PS/ER **34.68/39.12%** and **33.06/39.82%**, with a utility-difference interval spanning zero. Both candidate regimes are retained, making the scope of the supported contribution explicit.

**Manuscript changes:** Sections 4.3–4.4 specify the usefulness and influence decisions; Sections 5.1–5.2 and Appendix B describe the matched path control and paired comparisons. Section 5.4 supplies the controlled and automatic usefulness evidence. The Introduction now frames the experiments as tests of the three decisions, RQ1 explicitly distinguishes confidence weighting with fixed candidate sets, and the Conclusion requires matched fixed-strength controls for query-specific influence. The original coverage–utility–influence method is preserved.

### Q2 / W2 — Why ER improves while PS decreases, and differences across tasks

**The task breakdown confirms an ER–PS tradeoff, and the usefulness objective explains why a positive label does not imply path completion.** ER increases in all three tasks, while PS is unchanged or lower. PS measures source–target connection; ER measures recovered directed reference relations. Under the fixed budget, improving intermediate evidence need not complete a path. We therefore evaluate usefulness discrimination and the two retrieval endpoints separately, rather than interpreting a positive usefulness label as a guarantee that both metrics improve.

The original matched single-Bridge intervention has the following breakdown:

| Task | PS: lexical → reliability (%) | ER: lexical → reliability (%) |
|:--|--:|--:|
| All | 29.92 → 29.19 | 38.19 → 39.92 |
| Gene | 31.87 → 31.38 | 36.46 → 38.82 |
| Metabolite | 25.71 → 25.71 | 33.47 → 36.78 |
| Process | 28.46 → 27.31 | 41.17 → 41.87 |

ER increases in all three tasks. PS is unchanged for Metabolite and lower for Gene and Process, with the largest reduction on Process. This supports usefulness discrimination and evidence recovery, not a uniform path-success improvement. A separate TRAIN-label diagnostic supports the objective distinction: among 1,313 positive automatic-candidate singleton labels, 973 improve ER without improving PS, while 340 improve PS. This diagnostic is consistent with the tradeoff; it does not prove its cause in every final selected set.

The intervention uses **248 queries under five candidate conditions**, giving 1,240 paired cases rather than 1,240 independent queries. Resampling keeps all tasks and variants from each mechanism record together. It tests K = 1 usefulness ranking; revised Table 4 separately tests the complete K = 3 selector.

**Manuscript changes:** Section 5.4 reports the task breakdown and label/metric distinction; Appendix B specifies the grouping and automatic-candidate diagnostic.

### Q3 / W2 — Paired uncertainty and cases where the gate helps

**We now test the gate controls on exactly the Table 1 Full configuration.** Revised Table 5 replaces the ambiguous original Table 4 attribution; its Full row matches all 248 Table 1 per-query outcomes. Candidates, confidences, gate features and search settings are fixed.

| Gate assignment | PS (%) | ER (%) | Utility | Mean gate |
|:--|--:|--:|--:|--:|
| Validation-selected constant | 39.11 | 59.74 | 0.4530 | 0.45000 |
| Validation-mean constant | 39.11 | 59.54 | 0.4524 | 0.35877 |
| Shuffled Full, 500-run mean | 39.24 | 59.06 | 0.4518 | 0.36035 |
| Full | 39.52 | 59.46 | 0.4550 | 0.36035 |

Full minus the validation-mean constant has utility difference **+0.00258**, paired 95% CI **[−0.00072, 0.00861]**, and ER difference **−0.0806 points [−0.2490, 0.0000]**. This quantifies the gate effect on the actual main configuration: the utility interval spans zero, and a reliable ER advantage over strong constants is not established.

The case analysis includes both directions. In **Tolbutamide/type-2-diabetes, Gene**, Full's gate 0.28795 versus constant 0.35877 changes the explored continuation toward an insulin-secretion path: **PS 0→1**, ER unchanged at **1/7**. In **Afamelanotide/erythropoietic-protoporphyria, Gene**, Full's gate 0.23878 still reaches the target but reduces **ER from 1.0 to 0.8**. Across the 248 queries there is one utility benefit, one harm and 246 unchanged cases. These are observed graph paths, not independent biological validation.

Distribution-preserving shuffles give raw one-sided randomization p = 0.0679/0.0220 for ER/utility; Holm adjustment across four endpoints gives **0.2036/0.0878**. Query–gate correspondence can change retrieval, but these controls do not establish a stable independent gate advantage over strong constants.

**Manuscript changes:** Section 5.5, Table 5, Figure 4 and Appendix B use the canonical configuration, paired intervals, tuning scopes, and positive/negative cases. The Conclusion retains the requirement for matched fixed-strength controls without asserting an independent gate advantage.

### Q4 / W2 — Reference-aligned versus automatically generated candidates

**Useful guidance survives automatic proposal: PS/ER reaches 35.08/38.73%, compared with Graph Only 27.02/33.97%.** We now separate this automatic evaluation from Table 1’s supplied-candidate diagnostic. All 692 archived Clean candidate records belong to their corresponding reference mechanisms, and all 248 query pools contain the benchmark intermediate answer. Table 1 therefore evaluates downstream guidance conditional on supplied candidate access; the automatic result below tests imperfect proposal.

The automatic proposer uses known endpoints and names, task intent and the fixed graph. It performs two forward expansion rounds from the source followed by two reverse rounds from the target, using a shared visited set, endpoint exclusion, biological-type filters and canonical-ID deduplication. A fixed lexical/adjacency/anti-hub/relation-quality rule ranks proposals. The intermediate answer and query-specific reference membership are not proposer inputs.

On automatic pools, useful-candidate coverage rises from **100/248 (40.32%)** for the strongest single view to **152/248 (61.29%)** for the union. Usefulness is assessed offline by the fixed-confidence singleton intervention. With at most three equally weighted proposals and fixed gate 0.3, automatic retrieval reaches **PS/ER 35.08/38.73%**, versus Graph Only **27.02/33.97%**. The paired 95% intervals for the gains are **[4.40, 12.11] PS points** and **[2.88, 6.84] ER points**.

Thus useful guidance survives imperfect automatic proposal. This does not turn the stronger Clean result into an automatic-pipeline result or establish uniform fixed-K multi-view superiority. The fixed aggregate graph includes held-out mechanism edges, so the evaluation is explicitly transductive.

**Manuscript changes:** Sections 4.2, 5.1–5.3, Figure 3 and Appendix B distinguish the candidate regimes and specify reference access.

---

## Rebuttal to Reviewer V6Sn

Thank you for highlighting the distinction between semantic plausibility and downstream usefulness. Your questions prompted us to clarify how candidates are constructed, how queries and their variants are split, and how the two Full configurations should be compared. We address your three questions below.

### Q1 — Are Table 1 candidates reference-aligned, and what can the proposer access?

**Yes—Table 1 uses reference-aligned Clean candidates; automatic construction is specified and evaluated separately.** All 692 archived test candidate records belong to the respective reference mechanisms, and all 248 query pools contain the benchmark intermediate answer. The saved candidate identities and downstream retrieval are verifiable. The automatic proposer below uses known endpoints, task intent and the fixed graph, without accessing the held-out intermediate answer or query-specific reference membership.

We separately evaluate an executable automatic proposer using known source/target metadata, task intent and the fixed graph. Two forward rounds from the source are followed by two reverse rounds from the target, with a shared visited set, endpoint exclusion, type filtering, canonical-ID deduplication and a fixed proposal-ranking rule. It does not access the intermediate answer or query-specific reference membership. Reference mechanisms enter supervised training labels and offline evaluation; the background graph itself remains transductive.

The automatic union supplies a useful candidate for **61.29%** of queries versus **40.32%** for the strongest single view. Fixed-guidance automatic Top3 reaches **35.08% PS / 38.73% ER**, versus **27.02% / 33.97%** for Graph Only, with paired gain intervals **[4.40, 12.11] / [2.88, 6.84]** points. Figure 3 now uses the same normalized pools as this retrieval evaluation: **14,029 query–candidate records across 248 queries**. We do not mix the former raw-endpoint plot into these results.

**Does usefulness learning extend to automatically proposed candidates?** On these 14,029 automatic candidates, the original-formulation reliability model gives the following singleton-usefulness diagnostic:

| Scorer | AUROC | AUPRC |
|:--|--:|--:|
| Lexical similarity | 0.609 | 0.036 |
| Retrieval-aware reliability | 0.870 | 0.108 |

The useful-candidate prevalence is 1.65%. These are point estimates with offline evaluation labels; they support a usefulness signal beyond supplied Clean candidates, not calibrated probabilities or guaranteed final-set improvement.

**We also make the remaining downstream limitation explicit.** On oversized automatic pools at the same fixed gate and budget, the complete reconstructed greedy selector obtains PS/ER **33.06/41.77%**, versus confidence-only **32.66/40.87%** and equally weighted proposal Top3 **35.08/38.73%** (revised Table 4). Greedy minus confidence-only utility is **+0.00554 [−0.01420, 0.02451]**. Thus automatic candidate access is supported, but the complete selector's stable advantage over strong proposal is not established. This supplemental reconstruction retains the original formula and uses previously inspected test queries; it does not authenticate the missing historical selector or constitute fresh independent confirmation.

**A component ablation also supports type complementarity within the reconstructed selector.** Complete greedy versus no-TypeBonus gives **+2.02 PS points [0.40, 4.02]** and **+0.01588 utility [0.00375, 0.03138]**, using paired pointwise 95% intervals. This isolates a contribution of the type term in the automatic-pool setting; it does not establish an independent Red benefit or complete-rule superiority to confidence-only.

**Manuscript changes:** Sections 4.2, 5.1–5.3, Figure 3 and Appendix B identify which inputs are reference-aligned and give the automatic construction protocol.

### Q2 — Train/validation/test splits and candidate/corruption variants

**Splitting is by curated mechanism-record ID, and all variants of a record stay together.**

| Split | Task queries | Mechanism records |
|:--|--:|--:|
| TRAIN | 1,354 | 1,120 |
| VAL | 239 | 202 |
| TEST | 248 | 218 |

No record ID or (task, record-ID) pair crosses splits. The audit checks all 25,710 Bridge-training rows, 25,710 Bridge-prediction rows, 9,205 gate-training rows and 9,205 gate-prediction rows against the manifest; candidate/corruption variants have no cross-split or assignment mismatches for the same record. Paired intervals use 5,000 mechanism-record bootstrap draws, retaining all task and condition variants together.

Record separation has a specific limit: different records can share a question or endpoint pair. Fifteen question-text groups and fifteen drug–disease-pair groups span splits, and nine test queries have an endpoint pair seen in TRAIN or VAL. We therefore describe this as **mechanism-record separation**, not strict unseen-pair or unseen-question generalization.

As a post-hoc sensitivity check, excluding those nine test queries leaves 239 queries without refitting. Full minus the matched path scorer retains positive PS/ER intervals **[0.85, 6.15] / [0.26, 4.02]** points. This qualifies the overlap concern for that comparison; it is a sensitivity subset, not a newly independent holdout.

**Manuscript changes:** Section 5.1 and Appendix B specify split counts, grouping, overlap and inference scope.

### Q3 — Table 1 versus original Table 4, and paired controller uncertainty

**The revised comparison uses one canonical Full configuration and supplies the requested paired intervals.** Full now matches between Tables 1 and 5 on every one of the 248 test queries: ER **59.46%**, mean gate **0.36035**. The original Table 4 entry, **38.05% / 0.10917**, is explicitly corrected and retained in the audit record. This removes the ambiguity about which current Full is used for controller attribution.

Revised **Table 5** uses exactly Table 1's supplied Clean nodes, learned confidences, query features, frozen predictions, original gate **g = 0.5 √p_rel** and search budgets. Its Full row matches Table 1 at all 248 queries. The controls change only the gate assignment.

| Assignment | PS (%) | ER (%) | Utility | Mean gate |
|:--|--:|--:|--:|--:|
| Validation-mean constant | 39.11 | 59.54 | 0.4524 | 0.35877 |
| Validation-selected constant | 39.11 | 59.74 | 0.4530 | 0.45000 |
| Full | 39.52 | 59.46 | 0.4550 | 0.36035 |

Full minus the validation-mean constant is **−0.0806 ER points [−0.2490, 0.0000]** and **+0.00258 utility [−0.00072, 0.00861]**, using paired mechanism-record bootstrap intervals. These controls do **not** support a stable independent gate advantage over strong constants.

Separately, the combined Table 1 Full-minus-Semantic+Fixed increment is ER **+0.90 points [0.23, 1.69]**, utility **+0.00552 [0.00118, 0.01275]**, and PS **+0.40 points [0.00, 1.24]**. We distinguish this combined increment from gate-only attribution. Per-query input, gate, control and statistical records have been prepared, and all **124,992** control outcomes have been replayed.

To assess the broader guidance formulation against a learned alternative, the requested compact learned-path control reaches **35.48% PS / 56.93% ER** on the same supplied-Clean inputs and inference budget. Full reaches **39.52% / 59.46%**, with paired differences **+4.03 PS points [1.57, 6.94]** and **+2.53 ER points [0.62, 4.46]**. The supervision source and utility are shared, while path-prefix and query-level label granularity differ. This is positive evidence against that specific control, conditional on supplied candidates; it does not isolate the gate or establish superiority to all learned path scorers.

**Manuscript changes:** Sections 5.2 and 5.5, revised Table 5, Figure 4 and Appendix B correct the configuration attribution and report uncertainty. Original Table 4 remains a labeled historical correction record. The Conclusion likewise requires matched fixed-strength controls rather than presuming query-specific control is superior.

---

## Rebuttal to Reviewer kKLG

Thank you for the careful review and for independently checking the released artifacts. Your comments prompted us to make candidate construction and selection explicit, reconcile the Full configurations, and test sensitivity to outgoing-edge order. We address these four concerns below.

### Q1 / W1.1 — Candidate construction and reference access

**Automatic candidate effectiveness is now evaluated directly using only known endpoints, task intent and the fixed graph.** We distinguish that evaluation from the archived supplied-Clean setting: all 692 saved Clean candidates belong to their corresponding reference mechanisms, and all 248 query pools contain the benchmark intermediate answer. Table 1 measures retrieval conditional on these supplied inputs; the automatic construction and retrieval evaluation below directly test Stage I without query-specific reference access.

**We directly test automatic candidate effectiveness separately.** The graph-native proposer receives known source/target IDs and names, task intent and the fixed graph. It performs two forward rounds from the source followed by two reverse rounds from the target, with a shared visited set, excludes endpoints, applies molecular/cellular/system type filters, deduplicates canonical IDs and uses a fixed proposal-ranking rule. Query-specific reference objects and intermediate answers are absent from its runtime inputs; removing those reference objects leaves its output unchanged.

On the same 248-query test manifest:

| Automatic-candidate evidence | Result |
|:--|:--|
| Useful-candidate coverage, strongest single view → union | 100/248 (40.32%) → 152/248 (61.29%) |
| PS, Graph Only → fixed automatic Top3 | 27.02% → 35.08% |
| ER, Graph Only → fixed automatic Top3 | 33.97% → 38.73% |
| Paired 95% CI of PS gain | [4.40, 12.11] percentage points |
| Paired 95% CI of ER gain | [2.88, 6.84] percentage points |

Useful-candidate coverage is measured offline by singleton utility improvement, before final-budget truncation. Automatic Top3 uses equal confidences and fixed gate 0.3, isolating candidate access from learned selection/control. These results establish useful automatically proposed guidance in this setting; they do not establish the entire automatic learned pipeline or reproduce the stronger supplied-Clean performance. The aggregate graph includes held-out mechanism edges, so we explicitly retain a transductive interpretation.

**Manuscript changes:** Sections 4.2, 5.1–5.3 and Appendix B specify reference access. Figure 3 now describes the same normalized automatic pools as the retrieval evaluation, totaling 14,029 candidate records.

### Q2 / W1.2 — Saved sets, the complete greedy rule, and confidence-only selection

**The complete rule is now executable and tested on genuinely oversized automatic pools.** Revised Table 4 evaluates 248 test queries, of which **239 pools exceed K=3**; the rule changes **101 selected sets** relative to confidence-only. Initial pools, candidate scores, selected IDs, stepwise traces, coefficients and executable definitions have been prepared in the audit. This directly supplies the requested subset-selection test.

**The archived at-most-three nodes are the supplied inputs passed to the main retriever, not a documented larger-pool selection trace.** Since those inputs fit within K, Table 1 Semantic+Fixed versus Utility+Fixed tests confidence weighting. Revised Figure 3 instead describes the larger automatic H0 pools used for the new selector comparison. We explicitly separate the two roles.

The operational selector preserves the original formula:

**Gain(h | H) = cₕ − μ Red(h, H) + β TypeBonus(h, H).**

Selection starts from the empty set. Red is maximum one-hop undirected-neighbor Jaccard overlap with a selected Bridge, zero for the empty set; TypeBonus indicates a raw graph type not yet selected. Ties use ascending node ID, and selection stops at K=3 or pool exhaustion. The original TF-IDF/graph-feature class-balanced logistic model is retained. TRAIN fitting and the bounded validation grid select **C=0.1, mu=0.5, beta=0.1**. These documented operational definitions constitute the completed reconstruction; they are not asserted to be authenticated historical coefficients or code.

**The component ablation provides positive evidence specifically for type complementarity.** All learned rows below share the same frozen candidate scores, fixed gate 0.3 and search budget. The no-Type and no-Red arms remove only the named term from the frozen rule; terms are not reselected using test outcomes.

| Automatic-pool selection | PS (%) | ER (%) | Utility |
|:--|--:|--:|--:|
| Proposal Top3, equal confidence | 35.08 | 38.73 | 0.3618 |
| Confidence-only, learned confidence | 32.66 | 40.87 | 0.3512 |
| Greedy without TypeBonus | 31.05 | 41.19 | 0.3409 |
| Greedy without Red | 33.06 | 41.80 | 0.3569 |
| Complete greedy | 33.06 | 41.77 | 0.3568 |

Complete greedy minus the no-Type arm gives **+2.02 PS points**, with paired pointwise 95% CI **[0.40, 4.02]**, and **+0.01588 utility [0.00375, 0.03138]**. This ablation supports the contribution of the original type-complementarity term under the specified automatic-pool reconstruction.

The comparisons also identify its scope. Complete greedy minus confidence-only utility is **+0.00554 [−0.01420, 0.02451]**; complete greedy minus no-Red utility is **−0.00009 [−0.01349, 0.01301]**. Thus the evidence supports TypeBonus in this ablation, while it does not establish an independent Red benefit or a stable complete-rule advantage over confidence-only. Against equal-confidence proposal Top3, ER is higher but PS and utility are lower. These are pointwise component intervals from existing, previously inspected test records, not multiplicity-adjusted proof of every term or fresh independent confirmation.

**Usefulness discrimination is supported separately from subset selection.** On the 14,029 automatic test candidates, useful-candidate prevalence is **1.65%**. The same original-formulation reliability model gives **AUROC/AUPRC 0.870/0.108**, versus lexical **0.609/0.036** and graph heuristic **0.346/0.012**. This establishes an informative ranking signal in the diagnostic; the downstream ablations above determine what that signal achieves after selection.

**Manuscript changes:** RQ1 and Section 4.3 distinguish confidence weighting with fixed candidates from subset filtering. Section 5.4 and Table 4 report the complete-selector comparison, and Appendix B specifies the reconstructed rule and validation coefficients. Appendix B now also reports the TypeBonus/Red ablations and their paired pointwise intervals, matching the supplementary evidence above.

### Q3 / W2.1–W2.2 — Reconciling Full and testing the actual main configuration

**The revised controls use exactly the Table 1 Full inputs and predictions, and all 248 Full outcomes now agree across Tables 1 and 5.** Selected candidate IDs, confidence scores, query features, graph and search settings are fixed; the control arms vary only gate assignment. The original **38.05% ER / 0.10917 mean-gate** summary is retained as a labeled correction record because its full historical generator was not verified. The canonical comparison below is executable and reproducible.

The canonical configuration uses the supplied Clean candidate IDs, learned confidences, original query features and frozen predictions, with ****g = 0.5 √p_rel****, beam 2, at most 8 hops, 14 visited nodes and 20 outgoing-edge considerations, in original adjacency order. Revised **Tables 1 and 5 have identical Full outcomes for every one of the 248 queries**: PS/ER **39.52/59.46%**, mean gate **0.36035**.

The selection protocols are now explicit. The controller's g_max = 0.5, γ = 0.5 comes from the original 42-combination validation grid over five candidate conditions, with a nearest-0.05 gate cache for selection and continuous gates at test replay. The global constant is selected on Clean validation utility; the mean constant uses validation gates only. These calibration scopes differ. All control arms subsequently fix selected nodes, confidences, graph and search budget; the 500 global shuffles permute the frozen test gate vector without parameter fitting.

| Gate assignment on Table 1 inputs | PS (%) | ER (%) | Utility | Mean gate |
|:--|--:|--:|--:|--:|
| Validation-selected constant | 39.11 | 59.74 | 0.4530 | 0.45000 |
| Validation-mean constant | 39.11 | 59.54 | 0.4524 | 0.35877 |
| Shuffled Full, 500-run mean | 39.24 | 59.06 | 0.4518 | 0.36035 |
| Full | 39.52 | 59.46 | 0.4550 | 0.36035 |

**The corrected controls support a narrower conclusion.** Full-minus-validation-mean utility is **+0.00258 [−0.00072, 0.00861]**, and ER is **−0.0806 points [−0.2490, 0.0000]**. Full-minus-validation-selected-constant utility is **+0.00199 [−0.00118, 0.00746]**. Neither establishes a stable utility advantage. Shuffles give raw ER/utility randomization p = 0.0679/0.0220, or **0.2036/0.0878** after Holm adjustment across four endpoints. We therefore correct the earlier independent-gate performance interpretation.

This does not remove all evidence for the learned guidance combination. On the supplied-Clean main comparison, Full-minus-Semantic+Fixed ER is **+0.90 points [0.23, 1.69]**, and utility is **+0.00552 [0.00118, 0.01275]**. These quantify the small combined weighting/control increment, not the greedy rule or gate alone, and not superiority to optimized constants. Most of the headline gain indeed comes from supplied Bridge guidance.

The canonical query-input records identify candidate IDs, confidence scores, feature inputs, gate probabilities and strengths. The generator, validation configuration, shuffle assignments and paired statistics have been prepared, and **124,992** control outcomes have been replayed. The correction is a reproducible replacement attribution experiment, not a claim that the historical generator was recovered.

**Manuscript changes:** Sections 5.1–5.2 and 5.5, revised Table 5, Figure 4 and Appendix B distinguish combined gains from gate-only effects and document the canonical configuration. The Introduction describes tests rather than presumed component gains, and the Conclusion requires matched fixed-strength controls.

### Q4 / W2.3 — Outgoing-edge order and robustness

**The mean incremental gains retain their sign across all 12 tested orders with models and predictions fixed.** We also specify the original order: outgoing adjacency lists follow graph insertion order, duplicate (neighbor, relation) pairs retain their first occurrence, and the 20-edge consideration budget is consumed in that order. The experiment below directly tests sensitivity to that implementation choice.

With models and confidence/gate predictions fixed, we evaluated the original order, a sorted order and ten seeded outgoing-edge permutations at the same budgets:

| Comparison across 12 orders | Range of mean PS difference (points) | Range of mean ER difference (points) |
|:--|--:|--:|
| Full − Graph Only | +9.68 to +13.71 | +22.91 to +25.49 |
| Full − Semantic+Fixed | +0.40 to +0.81 | +0.054 to +0.901 |

Thus both mean increments retain their sign across these orders, including the smaller combined learned-stage increment. These are ranges of paired point differences, not confidence intervals or significance under every order. Absolute performance remains order-sensitive; this experiment does not establish invariance, a gate-only benefit, or robustness to all budgets. We retain the original main order rather than choosing the best test permutation.

**Manuscript changes:** Section 5.5 and Appendix B report the order rule, tested orders, ranges and scope.

---

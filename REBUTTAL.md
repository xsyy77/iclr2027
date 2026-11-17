# Rebuttal — Submission 36251

**Rebuttal to Reviewer 9P9q**

Thank you for recognizing our problem, conceptual decomposition, and diagnostic experiments as strengths. Our answers follow.

**Q1 — Contribution**

**1. Contribution.** Our contribution is a mechanism-oriented perspective on scientific graph retrieval: intermediate hypotheses actively guide evidence recovery rather than serving solely as retrieval outcomes. Biomedical retrieval must recover evidence explaining a relationship, beyond connecting entities.

Three linked components address coverage, utility, and influence. Multi-view construction broadens candidate access across molecular, cellular, and system levels. Retrieval-aware reliability uses downstream-benefit supervision; selection accounts for redundancy and type complementarity. Query-specific control regulates their collective influence. Utility estimation cannot recover missing candidates; coverage cannot distinguish useful hypotheses from distractors; useful hypotheses do not justify uniformly strong guidance. BTE-RAG retrieves mechanistic context for answer generation; Self-RAG uses reflection tokens for passage retrieval and evidence/generation evaluation. We guide graph-valid path retrieval.

**2. Rationale and comparison.** Unlike the matched path scorer, our gate preserves structural and mechanistic scoring functions and learns their relative influence at query level. This introduces a stronger structural inductive bias, potentially reducing overfitting when mechanism-level supervision is limited.

The TRAIN-only logistic path scorer shares the graph, supervision source, utility, frozen Bridge confidences, and search budgets; input representations and supervision granularity differ. On Clean, Full/scorer PS/ER are 39.52/59.46% versus 35.48/56.93%; paired differences are +4.03 [1.57,6.94] and +2.53 [0.62,4.46] percentage points (95% CIs). On automatic Top3, they are 34.68/39.12% versus 33.06/39.82%, with the utility CI spanning zero. Clean results favor our implementation over this numeric linear scorer without isolating its source or establishing independent gate superiority.

**3. Table 1.** We agree that Bridge guidance accounts for most improvement. Under supplied Clean access, confidence weighting and query-specific control refine an already effective guidance strategy; their modest increments are not the primary gain.

**Q2 — ER–PS tradeoff**

The tradeoff reflects prioritizing path completion versus recovering mechanistically informative evidence. Connecting endpoints need not recover evidence explaining their relationship. Under fixed budgets, prioritizing such evidence may improve ER without increasing PS. Our utility jointly considers both: M=0.7 PS+0.3 ER. Among 1,313 positive automatic singleton TRAIN labels, 973 improve ER only and 340 improve PS.

The K=1 comparison covers 248 queries under five candidate conditions (1,240 query-condition cases):

| Task | PS lexical→reliability (%) | ER lexical→reliability (%) |
|:--|--:|--:|
| All | 29.92→29.19 | 38.19→39.92 |
| Gene | 31.87→31.38 | 36.46→38.82 |
| Metabolite | 25.71→25.71 | 33.47→36.78 |
| Process | 28.46→27.31 | 41.17→41.87 |

Mean ER increases by 0.69–3.31 points, with mean PS changes of −1.15 to 0 points. Metabolite has the largest ER gain with unchanged mean PS. This is consistent with recovering richer mechanistic evidence with small observed path-completion changes (§5.4, Appendix B).

**Q3 — Uncertainty and cases**

For revised Table 5’s actual Table 1 Full configuration, Full minus the validation-mean constant gives PS +0.40 [0.00,1.24] points, ER −0.0806 [−0.2490,0.0000] points, and utility +0.00258 [−0.00072,0.00861]. These paired 95% CIs use 5,000 mechanism-record bootstrap draws, retaining task variants together; they do not establish stable overall superiority.

As motivated in Q1, Bridge usefulness does not determine guidance strength. With Bridges/confidences fixed, Tolbutamide–type-2-diabetes Gene uses g=0.28795 instead of the validation-mean constant 0.35877, redirecting a potassium-channel-related path toward insulin secretion. PS changes 0→1; ER remains 1/7. This illustrates how separate influence control can improve completion (§5.5, Appendix B).

**Q4 — Automatic versus Clean candidates**

Gains survive automatic proposal. Automatic Top3 with fixed equal-confidence guidance improves PS/ER by 8.06/4.76 points over Graph Only—approximately 64.5%/18.7% of Clean Full gains, although guidance configurations differ.

Table 1 uses reference-aligned Clean Bridges to isolate guidance under controlled access; §5.3 separately evaluates automatic candidates constructed without held-out answers or query-specific reference membership.

Before budget truncation, useful-candidate coverage increases from 40.32% for the strongest view to 61.29% for the union. Automatic Top3 with equal confidence and g=0.3 reaches PS/ER 35.08/38.73% versus Graph Only 27.02/33.97%; paired gain CIs are [4.40,12.11]/[2.88,6.84] points.

Thank you again.

---

**Rebuttal to Reviewer V6Sn**

Thank you for recognizing the value of distinguishing semantic plausibility from downstream retrieval usefulness, and for your positive assessment of our controlled query-specific gating experiment. We address your questions below.

> **Q1 / W1 — Candidate construction and experimental settings.**

Yes, Table 1 uses reference-aligned Clean Bridges to isolate the effects of subsequent retrieval decisions under controlled candidate access. However, our framework does not require reference-aligned candidates and supports fully automatic Bridge generation. The diagnostic experiments in Section 3 start from reference-aligned Clean Bridges and introduce controlled corruptions; the controlled Stage-I view partition also uses reference-aligned candidates. Table 2 uses supplied Clean candidates and their corruption variants, and the revised Table 5 uses the Table 1 Clean configuration for gate controls. Section 5.3 evaluates automatically generated candidates.

Our automatic proposer performs bidirectional graph exploration (two forward rounds from the source and two reverse rounds from the target), constructs candidates across molecular, cellular, and system-level views, and ranks them using lexical relevance and local graph features. It uses only the known endpoints and their names, query intent, and the fixed knowledge graph, without using held-out intermediate answers or query-specific reference membership.

With automatically generated Top3 Bridges and fixed equal-confidence guidance (g=0.3), PS and ER improve by 8.06 and 4.76 percentage points over Graph Only, respectively. We have clarified the proposer and distinguished the two experimental settings in Sections 4.2 and 5.1–5.3.

> **Q2 — Data splits and candidate/corruption variants.**

No variants derived from the same mechanism record cross the training, validation, or test splits. DrugMechDB is split at the mechanism-record level, ensuring that all task, candidate, and corruption variants of each record remain in the same split.

To verify this, we conducted a data-split audit covering the released Bridge and gate training and prediction tables. The resulting split statistics are as follows:

| Split | Task queries | Mechanism records |
|:--|--:|--:|
| TRAIN | 1,354 | 1,120 |
| VAL | 239 | 202 |
| TEST | 248 | 218 |

The audit confirms zero split-assignment mismatches. Different mechanism records may share question texts or endpoint pairs, with nine test queries sharing endpoint pairs with TRAIN or VAL. We have clarified the splitting protocol and verification results in Section 5.1 and Appendix B.

> **Q3 — Experimental settings and paired controller uncertainty.**

Table 1 evaluates the overall framework under controlled candidate access, whereas Table 4 was intended to diagnose query-specific guidance control. However, the originally reported Table 4 Full configuration was not aligned with Table 1 Full. We have corrected this mismatch in revised Table 5.

To facilitate a direct comparison and assess the contribution of query-specific control, we have re-evaluated the gate controls using exactly the same candidate set, Bridge confidences, frozen predictions, and search settings as Table 1 Full.

Full minus the validation-mean constant gives an ER difference of −0.0806 percentage points (paired 95% CI [−0.2490, 0.0000]) and a retrieval-utility difference of +0.00258 (paired 95% CI [−0.00072, 0.00861]). These intervals do not establish a stable overall advantage over this constant. The matched results appear in revised Table 5, with paired intervals reported in Section 5.5 and Appendix B.

Thank you again for your helpful feedback on the experimental settings and evaluation protocol.

---

**Rebuttal to Reviewer kKLG**

Thank you for recognizing our Stage-II usefulness evidence and verifying the main retrieval results. Our answers follow.

**Q1 / W1.1 — Candidates and reference access**

Table 1 uses supplied reference-aligned Clean Bridges drawn from reference mechanisms, including intermediate answers, to isolate subsequent retrieval decisions under controlled candidate access. They are not automatic-proposer outputs; our framework also supports automatic generation.

The proposer explores two forward rounds from the source, then two reverse rounds from the target, sharing a visited set. It excludes endpoints, groups candidates into molecular/cellular/system views by entity type, and deduplicates canonical IDs. A fixed rule combines lexical relevance, endpoint adjacency, anti-hub weighting and relation quality. Inputs are known endpoints/names, query intent and the fixed graph, excluding held-out intermediate answers and query-specific reference membership.

Before budget truncation, useful-candidate coverage rises from 40.32% for the strongest view to 61.29% for the union. Automatic Top3 with equal confidence and g=0.3 improves PS/ER over Graph Only by 8.06/4.76 percentage points (paired 95% CIs [4.40,12.11]/[2.88,6.84]). Sections 4.2 and 5.1–5.3 clarify both settings.

**Q2 / W1.2 — Saved sets and greedy selection**

Saved ≤3-node sets are supplied retriever inputs, not complete-greedy outputs. Table 1 Semantic+Fixed versus Utility+Fixed tests confidence weighting; Figure 3 describes larger automatic pools.

Parts of the original selector implementation could not be recovered. We reimplemented the stated rule with explicit operational definitions and evaluated it on automatic pools; this is not recovery of the historical implementation.

Starting empty, greedy maximizes Gain=c−μRed+βTypeBonus until K=3 or pool exhaustion. Red is maximum one-hop undirected-neighbor Jaccard with selected nodes (zero when empty); TypeBonus indicates a new entity type; ties use ascending node ID. Validation fixes C=0.1, μ=0.5, β=0.1. Both strategies share confidence scores, K=3, g=0.3 and search budgets. Of 248 pools, 239 exceed K; greedy changes 101 subsets versus confidence-only.

| Selection | PS (%) | ER (%) |
|:--|--:|--:|
| Confidence-only | 32.66 | 40.87 |
| Complete greedy | 33.06 | 41.77 |

Both means improve numerically; utility differs by +0.00554 (paired 95% CI [−0.01420,0.02451]), without establishing stable superiority. Complete-selector results (§5.4, Table 4, Appendix B) use this implementation on previously inspected data. Its code, automatic pools, outputs and traces are released in the anonymous repository.

**Q3 / W2.1–W2.2 — Configurations and attribution**

Table 1 evaluates the overall framework; original Table 4 intended to diagnose guidance control, but its Full configuration (mean gate 0.10917) was not aligned with Table 1. Revised Table 5 corrects this.

Controls now share Table 1 Full's Clean candidates, confidences, query features, frozen predictions and search. All 248 Full outcomes match: PS/ER=39.52/59.46%, mean g=0.36035.

The gate is g=0.5√p_rel. A 42-setting grid over five validation candidate conditions selected g_max=0.5, γ=0.5 using utility, with nearest-0.05 cached gates for selection and continuous test gates. Clean validation utility selects constant 0.45; validation gates give mean constant 0.35877. Five hundred global shuffles preserve the test-gate distribution. Search fixes beam=2, hops≤8, nodes≤14, edge considerations≤20 and original adjacency order.

| Guidance | PS (%) | ER (%) | Utility |
|:--|--:|--:|--:|
| Validation-selected constant | 39.11 | 59.74 | 0.4530 |
| Validation-mean constant | 39.11 | 59.54 | 0.4524 |
| Shuffled (500-run mean) | 39.24 | 59.06 | 0.4518 |
| Full | 39.52 | 59.46 | 0.4550 |

Full has highest mean utility, without stable independent gate superiority. Full-minus-validation-mean paired 95% CIs are ER [−0.2490,0.0000] points and utility [−0.00072,0.00861].

For W2.2, Bridges supply most improvement; learned components refine their use. Full-minus-Semantic+Fixed ER is +0.90 points [0.23,1.69], a combined weighting/control increment, not gate alone. Results are in §5.2/5.5, Table 5 and Appendix B; code, generator, validation configurations, per-query records and assignments are in the anonymous repository.

**Q4 / W2.3 — Search-order robustness**

Outgoing edges follow graph insertion order; duplicate (neighbor,relation) pairs retain first occurrence. The 20-edge budget follows this order. With models, predictions and budgets fixed, mean gains remain positive across original, sorted and ten seeded random orders:

| Comparison | PS gain (pp) | ER gain (pp) |
|:--|--:|--:|
| Full–Graph Only | 9.68–13.71 | 22.91–25.49 |
| Full–Semantic+Fixed | 0.40–0.81 | 0.054–0.901 |

We retain the original order for main results (§5.5, Appendix B).

Thank you again for your feedback.

---

**Response to Reviewer kKLG — Follow-up**

> **W1. Clean candidate construction**

The distinction between controlled candidate access and automatic proposal is central to interpreting Table 1. It evaluates how supplied reference-aligned intermediate cues are used for evidence retrieval. The candidates provide node identities, including the intermediate answer, but not a complete connecting path or relation sequence. Thus, retrieval remains necessary, while the gains over Graph Only remain conditional on this additional information. Comparisons among the guided rows share these candidates and evaluate confidence weighting and influence control; they do not evaluate automatic proposal or nontrivial subset selection.

The explicit, executable construction procedure reproduces every saved Clean list and makes candidate construction fully specified. We build a global entity-type map by traversing the reference records in file order, with the last label retained for each canonical node ID. For each query, we traverse its reference mechanism in stored node order, exclude the known endpoints, and deduplicate IDs. We place the reference intermediate answer first, preferentially add nodes of previously unrepresented types, and fill remaining slots in record order to obtain up to three base candidates. Finally, we include the first eligible reference node with the answer's type if absent. This final inclusion produces four nodes for five TRAIN queries; all VAL and TEST pools have at most three.

The procedure exactly reproduces all 1,354 TRAIN, 239 VAL and 248 TEST lists, including order, in both the training-sample and prediction tables. The check covers all 3,682 saved lists across the two tables, with exact node and order agreement. It resolves the operational question of how to reproduce these inputs, although it does not authenticate the historical writer. This rule and verification scope can be stated explicitly in Appendix B. Automatic proposal remains separate and does not read query-specific intermediate answers or reference membership.

> **W2. Complete three-stage evaluation**

The component evaluations are deliberate controlled comparisons, each answering a distinct question: Section 5.3 tests whether automatically proposed Bridges improve retrieval; Table 2 tests downstream usefulness estimation; Table 4 compares complete greedy selection with confidence-only selection under fixed guidance; and Table 5 isolates query-specific control with candidates and confidences fixed. These comparisons are needed to distinguish candidate availability, usefulness estimation, selection and control. A complete-pipeline comparison complements them; it cannot replace their component-level attribution.

The complete-pipeline comparison evaluates automatic proposal followed by the Table 4 complete greedy selector and query-specific control. We retained the same automatic pools, learned confidence scores, selector parameters and search budgets. The existing gate model and mapping were frozen, with its original query features recomputed on the selected Bridges. A matched global constant was selected only on validation utility, yielding g=0.45.

| Automatic configuration | PS (%) | ER (%) | Utility |
|---|---:|---:|---:|
| Proposal Top3, equal confidence, g=0.3 |35.08|38.73|0.36176|
| Confidence-only, learned confidence, g=0.3 |32.66|40.87|0.35123|
| Complete greedy, learned confidence, g=0.3 |33.06|41.77|0.35677|
| Complete greedy, learned confidence, query-specific gate |33.47|41.84|0.35980|
| Same greedy selection and confidences, validation-selected constant |33.47|41.84|0.35980|

The complete pipeline recovers more reference edges but completes fewer paths than Proposal Top3 in point estimates. This is consistent with the distinction between endpoint completion and recovery of intermediate mechanistic evidence; it does not establish a superior tradeoff. Under the unchanged utility M=0.7PS+0.3ER, its difference from Proposal Top3 is −0.00197 (paired 95% CI [−0.04268,0.03855]). This directly addresses the request to evaluate the complete combination. The interval establishes neither superiority nor inferiority to Proposal Top3; it should not be interpreted as evidence of equivalence either. The component-level results remain relevant to assessing which decisions are supported.

> **W3. Incremental evidence for selection and control**

The evidence should be assessed at the level of the claim being tested. Intervals crossing zero in Tables 4 and 5 do not establish stable incremental gains over those controls. Equally, they do not negate the separately measured usefulness-estimation results or the positive matched term ablation below.

For Stage II, retrieval-aware reliability and the complete selection rule have different levels of support. Beyond the reliability results in Table 2, Appendix B reports a matched term ablation: complete greedy versus removal of TypeBonus improves PS by 2.02 percentage points (95% CI [0.40,4.02]) and utility by 0.01588 ([0.00375,0.03138]). This is direct positive evidence for type complementarity in the tested configuration. Its scope is specific: it does not establish an independent benefit of redundancy penalization or superiority to confidence-only selection, and the intervals are pointwise without multiplicity adjustment.

For Stage III, useful Bridges and appropriate guidance strength are distinct decisions: the gate changes their collective influence while preserving the selected nodes and confidence scores. The shuffled controls and cases examine whether this correspondence affects search, but do not by themselves establish superiority to a well-chosen constant. Likewise, the combined Clean gain from confidence weighting and gating is not a gate-only effect.

The complete automatic-pipeline comparison holds the greedy-selected nodes and confidences fixed. Query-specific control and the validation-selected constant achieve identical per-query PS, ER and utility; their paired utility difference and interval are both zero. This is a precise boundary on the gate claim in this setting: query variation is not shown to be necessary for these outcomes. It does not invalidate usefulness estimation or type complementarity, which are tested by different matched comparisons. The appropriate assessment therefore distinguishes the supported component results from the broader superiority claim that this experiment does not establish.

We ask that the assessment take into account both the exact candidate reconstruction and complete automatic-pipeline comparison, alongside the positive controlled evidence already reported. The remaining limitation concerns stable incremental superiority of complete selection and query-specific control; it should not be generalized to an absence of evidence for the framework as a whole.

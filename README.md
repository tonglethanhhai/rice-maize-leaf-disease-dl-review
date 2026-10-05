# Deep learning for rice and maize leaf disease recognition, 2016–2026: review data and evidence tables

Supplementary repository for the systematic review *[paper title, authors and DOI to be added on acceptance]*.  
**Zenodo (all versions):** [10.5281/zenodo.23151397](https://doi.org/10.5281/zenodo.23151397)  
**OSF Registration:** [https://osf.io/fwz4s](https://osf.io/fwz4s) (DOI: [10.17605/OSF.IO/FWZ4S](https://doi.org/10.17605/OSF.IO/FWZ4S))

It holds the full audit trail of the PRISMA 2020 selection and the evidence tables for **all 202 included studies**.
The paper narrates 38 representative studies in detail; every count, proportion and test statistic in the paper is
computed on all 202 studies and can be checked here.

> Tóm tắt tiếng Việt: Kho này chứa toàn bộ hồ sơ chọn lọc PRISMA 2020 và bảng bằng chứng của cả 202 nghiên cứu
> được nhận vào tổng quan. Bài báo chỉ tường thuật chi tiết 38 nghiên cứu tiêu biểu; mọi số đếm và kiểm định trong
> bài đều tính trên 202 nghiên cứu và có thể kiểm tra lại trên các tệp dữ liệu ở đây; mã phân tích không công bố, tác giả cung cấp khi có yêu cầu. Ghi chú tự do trong
> các tệp mã hóa và đánh giá toàn văn được viết bằng tiếng Việt.

## Checking the numbers

This repository holds data only. The analysis code is not published; it is available from the authors on
reasonable request. `number_checks.txt` lists the 57 checks that the authors' verification script runs against
the numbers reported in the paper, using exactly the files in this repository; all pass for this release.
`supplementary.pdf` is the supplementary material of the paper (search strings, protocol amendments, leakage-sign
rules, selection rule and statistical formulas) and `prisma2020_checklist.pdf` its PRISMA 2020 checklist.

## PRISMA 2020 flow

| Stage | Count | File |
|---|---|---|
| Records identified (Scopus 499, IEEE Xplore 41, ScienceDirect 774, MDPI 19) | 1,333 | `01_protocol_search/records_master.csv` |
| Duplicates removed (all matched on DOI) | 296 | `02_dedup_screening/duplicate_log.csv` |
| Records screened on title and abstract | 1,037 | `02_dedup_screening/screening_decisions.csv` |
| Excluded at title and abstract | 692 | same file, `exclusion_code` |
| Reports sought for retrieval | 345 | `03_fulltext/fulltext_manifest.csv` |
| Reports not retrieved | 5 | `03_fulltext/not_retrievable.csv` |
| Reports assessed in full text | 340 | `03_fulltext/fulltext_review_master.csv` |
| Excluded at full text (93 for train–test leakage, 19 for other invalid protocols) | 138 | `03_fulltext/excluded_fulltext.csv` |
| **Studies included** (rice 121, maize 77, both 4) | **202** | `04_included/coded_included.csv` |

## Repository layout

| Folder | Content |
|---|---|
| `01_protocol_search/` | Protocol with its 2026-09-26 amendments, exact search strings, search log, all 1,333 retrieved records (bibliographic fields only). |
| `02_dedup_screening/` | Duplicate log, the 1,037 unique reports, title/abstract decisions with exclusion codes and reasons, the log of protocol amendments A and B, round-two screening instructions, journal open-access verification. |
| `03_fulltext/` | Retrieval manifest with SHA-256 of each PDF, unretrievable reports, four-gate full-text assessment of every report with quoted evidence, assessment instructions, the 138 exclusions with reason codes. |
| `04_included/` | Structured coding of the 202 included studies, the codebook, BibTeX of the included studies, PRISMA counts, search-yield and inferential statistics as reported in the paper. |
| `05_evidence_tables/` | Evidence tables for all 202 studies: one CSV, one Markdown table per task (T1–T4), the 38 representative studies and the quota table. |
| `leakage_experiments/` | Near-duplicate measurements of six public datasets and the controlled split experiment: group files, per-run results of every protocol and seed, exact run commands. |
| `supplementary.pdf`, `prisma2020_checklist.pdf` | Supplementary material and PRISMA 2020 checklist of the paper. |

## Revision of 2026-10: leakage re-analysis, near-duplicate measurements and controlled experiment

Added after an internal critical review of the manuscript; every number below is among the checks in `number_checks.txt`.

| What | Files |
|---|---|
| Accuracy reported by the reports excluded for leakage (with page and quote; extracted for the original 94, of which REP-0229 was re-included on 2026-10-05, so analyses use the 93 still excluded) | `04_included/excluded_leak_metrics_94.csv` |
| Leakage re-analysis: field-level prevalence, Hodges–Lehmann shift, within-source van Elteren test, rank regressions, three-group trend, negative-binomial citation model | `04_included/leakage_reanalysis.json` |
| Same-source vs other-source metric pairs for the 17 studies with an external test (page and quote; REP-0037 added on 2026-10-05 as a row without a number) and the per-study gap | `04_included/external_delta_16.csv`, `04_included/external_delta_summary.json` |
| Near-duplicate measurements of six public datasets (perceptual hash + DINOv2 ViT-S/14; a pair counts when both agree) | `04_included/dedup_measurements.json`, `leakage_experiments/results/*/dup_criteria.json`, `images_verified.csv` |
| Protocol amendment of 2026-10-04: the blanket L3 rule for the Kaggle "Corn or Maize" set was withdrawn because it has ~0.5 % duplicates and no pre-augmented copies; 18 studies relabelled from their own protocol | `04_included/leakage_relabel_L3_2026-10-04.csv` |
| Review fixes of 2026-10-05 after an independent review: three reports without a held-out set (L5) excluded at gate 3; REP-0229 re-included as suspected leakage; L4 (augmented copies of test images inside the test set) kept as a flag only, so two L4-only studies became clean; REP-0037 coded as having an external test | `04_included/review_fixes_round6_2026-10-05.csv` |
| Provenance of the controlled experiment: exact commands, the lenient group file `images.csv` used by GROUP-SPLIT (SHA-256), and a check that re-creates the GROUP-SPLIT splits | `leakage_experiments/RUN_COMMANDS.md`, `leakage_experiments/results/*/images.csv` |
| Controlled experiment: same data and ResNet-50; only the split protocol changes (augment-then-split, random image split, near-duplicate group split; 3 seeds). AUG-SPLIT adds five offline copies per image before splitting, so it trains on about six times more items and its validation and test sets contain augmented copies; AUG-SPLIT vs GROUP-SPLIT measures the whole augment-then-split protocol, not leakage at equal training size on Sethy, PlantVillage maize (+ PlantDoc external test) and Kaggle "Corn or Maize" | `leakage_experiments/results/*/runs.csv`, `04_included/leakage_experiment_summary.json` |
| Matched pair at equal training budget (MATCH-CTRL vs MATCH-LEAK: same split, same training images and copies, same number of training items per epoch, untouched validation and test sets; MATCH-LEAK adds augmented copies of the test images to training). ResNet-50 and EfficientNet-B0 on four datasets (Sethy, PlantVillage maize, Kaggle Corn or Maize, Paddy Doctor), 5 seeds each | `leakage_experiments/results_matched*/*/runs.csv`, `04_included/leakage_experiment_matched_summary.json` |

Current leakage coding of the 202 studies: clean 116, suspect 52, unknown 34. The images themselves are not
redistributed; `images_verified.csv` lists file paths relative to each public dataset with the duplicate-group id.
`.zenodo.json` holds the metadata for archiving a release on Zenodo.

## Evidence tables

| Task | Studies | File |
|---|---|---|
| T1 Classification | 160 | [`05_evidence_tables/T1_classification.md`](05_evidence_tables/T1_classification.md) |
| T2 Detection and localisation | 35 | [`05_evidence_tables/T2_detection.md`](05_evidence_tables/T2_detection.md) |
| T3 Segmentation | 17 | [`05_evidence_tables/T3_segmentation.md`](05_evidence_tables/T3_segmentation.md) |
| T4 Generative models | 4 | [`05_evidence_tables/T4_generative.md`](05_evidence_tables/T4_generative.md) |

A study that addresses several tasks appears in each corresponding table, so the task counts sum to more than 202.
`evidence_included_studies.csv` holds one row per study with every coded field.

Reading the tables:

- **Metric** is the single headline number the study reports for its proposed model on its own test set. Datasets,
  splits and class sets differ, so values are not comparable across rows.
- **Leakage** codes the evaluation protocol: `clean` (independent split stated), `suspect` (at least one of the
  leakage signs L1-L6 defined in `04_included/CODEBOOK.md`, called AUG-FIRST, SUB-UNIT, PRE-AUG, TEST-AUG, NO-HOLDOUT and DUP-LEFT in the paper; the signs are listed in `leakage_criteria`; L4, augmented copies of a test image inside the test set, is recorded there as a flag but does not make a study `suspect`), `unknown`
  (not enough information). Labels use the reported procedure only, never the reported accuracy. The labels of 92
  at-risk studies were re-reviewed (AI-proposed with evidence excerpts, confirmed by one author, no double coding);
  see `04_included/leakage_review_92.csv`. Studies with evident leakage were
  excluded at full text (code `G3-LEAK`) and are listed in `03_fulltext/excluded_fulltext.csv`.
- **Val=Test** is `yes` when the study has no independent validation set, so the model is selected on the reported test set.
- **Rep.** marks the 38 representative studies narrated in the paper.

## Selection of the 38 representative studies

The rule was fixed before writing (supplementary material, section S5).

1. Studies are grouped by task and crop with a fixed quota per group (`05_evidence_tables/representative_quota.csv`).
2. Within a group only studies with a clean protocol are considered first.
3. Half of the quota goes to the studies with the highest citations per year, `cpy = citations / max(1, 2027 − year)`
   (Scopus counts at the search date). The other half goes to the newest clean study of each architecture family not
   yet represented in the group.
4. Ties go to the newer study. If clean studies run out, the next clean study by citations fills the slot, and a
   non-clean study is used only when the group has no clean study left. This happened only in T4, where all four
   studies were taken and two are coded `suspect`. After the leakage reviews, two selected classification studies were
   relabelled `suspect`; they were kept so the selection rule is unchanged, leaving 34 of 38 with a clean protocol.
5. A study is selected at most once. T4 is a single group covering rice and maize. The four studies on both crops are
   all T1 and belong to no crop-specific quota group, so none is selected.

Citation counts rank studies inside a group; they were never an eligibility criterion for the review.

## What is not included

- **Abstracts, keywords and raw database exports** are removed because publisher and database terms do not allow
  redistribution. Each record keeps its DOI and database link.
- **Full-text PDFs and extracted text** are not redistributed. The manifest lists the SHA-256 of every retrieved PDF so
  that a copy obtained from the publisher can be matched to the assessment.

## Use of generative AI in scientific writing

Title and abstract screening was AI-assisted. In addition, generative AI (Google Gemini 2.5 Flash) was used solely to assist with language editing, LaTeX formatting, minor script debugging and the re-review of the leakage labels of 92 studies (each label confirmed by an author). The AI tools were not involved in formulating scientific hypotheses, interpreting empirical data, or drawing conclusions; all scholarly ideas and final interpretations were performed entirely by the authors.

## Licence

Data, tables and documents: CC BY 4.0 (see `LICENSE`). The analysis code is not part of this repository.

## How to cite

See `CITATION.cff`. Please cite the review article rather than this repository alone.

# Round-2 title/abstract screening (human-style reading) — PRISMA 2020

Review topic: deep learning for **leaf disease** recognition in **rice (Oryza sativa)** and/or **maize/corn (Zea mays)**, 2016–2026, four vision tasks:
T1 whole-image classification, T2 bounding-box detection, T3 pixel segmentation, T4 generative augmentation (GAN/diffusion).

Round 1 was keyword-based and deliberately permissive. Your job: read each TITLE + ABSTRACT (+ keywords) carefully, as an expert screener, and decide whether the paper deserves full-text retrieval. Be decisive: exclude when the abstract makes ineligibility clear; forward as UNCLEAR only when the abstract genuinely cannot answer.

## Decisions and codes
- **INCLUDE** — primary empirical study; trains/fine-tunes a deep network; images (RGB or other imaging) of rice and/or maize LEAF disease (or leaf disease + pest/nutrient classes mixed in); reports a quantitative metric. Multi-crop studies (PlantVillage etc.) are INCLUDE only if the abstract suggests per-crop/per-class results may exist or the crop set is small and named (≤ ~5 crops incl. rice/maize); pure 38-class PlantVillage benchmarks with a single pooled accuracy → EXCLUDE X1.
- **EXCLUDE** with exactly one code:
  - X1 — crop not rice/maize, or multi-crop with no prospect of rice/maize-specific results (e.g., "38-class PlantVillage, 99.2% accuracy"), or rice/maize mentioned only in passing / as future work.
  - X2 — not leaf disease: panicle/ear/grain/seed/kernel disease, yield or biomass prediction, weed detection, insect counting with no disease class, nutrient deficiency only, growth stage, lodging, field-scale remote sensing of stress without disease labels, plant counting.
  - X3 — no deep network trained: classical ML on handcrafted features, spectral indices, statistics; a frozen CNN used only as feature extractor for SVM/RF also counts as X3 when the abstract says so.
  - X4 — no quantitative experiment: dataset descriptor only, system/app description without evaluation, position paper, protocol.
  - X5 — wrong publication type despite journal listing: review/survey, editorial, erratum, book chapter, conference paper reprinted.
  - X7 — duplicate/near-duplicate of another record in your batch (same authors, same data and model, different venue) — cite the other report_id in reason.
- **UNCLEAR** — abstract missing/too short, or eligibility hinges on something only the full text can show (e.g., "several crops including maize" with no detail).

## Output (STRICT)
Write a JSON array to the path given in your task; one object per input item, keys exactly:
{"report_id": "...", "decision": "INCLUDE"|"EXCLUDE"|"UNCLEAR", "code": ""|"X1"|"X2"|"X3"|"X4"|"X5"|"X7",
 "crop": "rice"|"maize"|"both"|"multi"|"other"|"unknown", "task_guess": "T1"|"T2"|"T3"|"T4"|"T1+T2"|...|"unknown",
 "reason": "<one sentence in Vietnamese, citing the abstract phrase that decides it>", "confidence": "high"|"medium"|"low"}
Do not skip any item. Keep report_id exactly as given.

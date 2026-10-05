# Full-text methodological review instructions (PRISMA 2020, Stage 05 → 06)

Topic: Deep learning for rice (Oryza sativa) and maize/corn (Zea mays) LEAF disease recognition, 2016–2026.

Reviewers examine each retrieved full-text PDF document carefully — especially Abstract, Materials/Dataset, Methods, Experimental Setup, Results. Do NOT rely on the title alone. Quote or paraphrase concrete evidence (section, numbers) in notes.

## Four quality gates (apply strictly, in order)

**Gate 1 – Crop & pathology**
- Must be leaf disease of rice or maize/corn.
- Multi-crop papers (e.g., PlantVillage 38-class, "Bangladeshi crops", cross-crop): INCLUDE only if the paper reports a SEPARATE metric table / per-class or per-crop result / confusion matrix that lets one read off rice- or maize-specific performance. If only one pooled number → EXCLUDE (reason code G1-POOLED).
- Non-rice/maize crop (pepper, citrus, sugarcane, pigeon pea…) with no rice/maize sub-result → EXCLUDE (G1-CROP).
- Not a leaf-disease image task (e.g., antibacterial peptide, salt content, cultivar selection from spectra, pest insect damage only, nutrient deficiency only) → EXCLUDE (G1-SCOPE). Note: insect-pest damage on leaves mixed with diseases is acceptable if disease classes are present; nutrient deficiencies mixed with diseases acceptable if disease classes are present.
- Non-RGB modalities (hyperspectral, multispectral UAV, thermal, spectra): acceptable only if it is still leaf disease on rice/maize AND a deep network is trained (Gate 2). Note modality in the "dataset" column.

**Gate 2 – Genuine deep learning**
- A deep neural network (CNN, ViT, YOLO, U-Net, GAN, DBN, capsule, BiLSTM on image features, etc.) must be TRAINED or FINE-TUNED (weights updated) on the task.
- Frozen pretrained CNN used ONLY as static feature extractor + SVM/RF/kNN/etc. with no network training → EXCLUDE (G2-FROZEN).
- Shallow MLP/BP network on handcrafted features (color/texture/GLCM/LBP) with no deep network → EXCLUDE (G2-SHALLOW). (A 1-D deep CNN on spectra counts as DL.)

**Gate 3 – Data leakage & scale (MOST IMPORTANT)**
- Look for the ORDER of augmentation vs. split. If augmentation (rotation, flip, crop, zoom, GAN synthesis…) was applied to the whole dataset BEFORE the train/val/test split, or if the paper reports the augmented total was then split (e.g., "after augmentation the 5,000 images were divided 80/20"), or test set clearly contains augmented copies → EXCLUDE (G3-LEAK). Also flag when the paper is silent on the order but numbers make it evident (split counts sum to the augmented total).
- If augmentation is applied only to the training split, or applied on-the-fly (Keras ImageDataGenerator on training generator, YOLO mosaic, torchvision transforms in train loader), or dataset used as-is → no leakage.
- If the paper is genuinely ambiguous (no numbers allowing inference), mark leakage = "Không rõ" and do NOT exclude on that ground alone; but note it.
- Dataset scale: if original (non-synthetic) images < 200 AND provenance (where/what cultivar/who collected) is unclear → EXCLUDE (G3-SMALL). If < 200 but source is a documented public dataset (e.g., UCI 120-image rice dataset by Prajapati) → do not exclude on scale alone, but note it. Report the original image count as precisely as possible.
- Other severe protocol faults (e.g., test set = training set, evaluation only on training data, no held-out test) → EXCLUDE (G3-PROTOCOL).

**Gate 4 – Vision task & metrics**
- Classify the actual task: T1 = whole-image classification; T2 = bounding-box detection (YOLO, Faster R-CNN, DETR, SSD…); T3 = pixel-level segmentation (U-Net, Mask R-CNN, DeepLab, SegFormer…); T4 = generative augmentation (GAN/diffusion) as the main contribution. Multi-task → list all (e.g., "T2+T3"). If title says "detection" but it is image classification → T1.
- Must report at least one quantitative metric (Accuracy, F1, mAP, IoU, Dice, SSIM/FID…). No numbers → EXCLUDE (G4-NOMETRIC).
- Non-vision tasks (image-text retrieval, IoT framework with no disease-model evaluation) → judge case-by-case; exclude if no vision model evaluated on rice/maize leaf disease (G4-TASK).

Other exclusion codes: OTHER-REVIEW (survey/review paper, no experiment), OTHER-DUP (duplicate), OTHER-UNREADABLE.

## Output format (STRICT)

Write your results to the file given in your task as a JSON array. One object per paper, keys exactly:

```
{
 "id": "ST-001",
 "file": "<original PDF filename>",
 "title": "<paper title>",
 "year": 2023,
 "crop": "Lúa" | "Ngô" | "Lúa+Ngô" | "Đa cây trồng (có tách lúa/ngô)" | "Đa cây trồng (gộp)" | "<khác>",
 "task": "T1" | "T2" | "T3" | "T4" | "T1+T3" ...,
 "task_note": "<short: e.g. title says detection but is classification>",
 "architecture": "<main model(s), e.g. Improved YOLOv5 (RDRM-YOLO); ResNet50 TL; ensemble MobileNetV2+EffNetB1>",
 "dataset": "<name/source; public or self-collected; location; modality if non-RGB>",
 "n_original": "<number of ORIGINAL images, as stated; e.g. 2,627 | 120 (UCI) | không rõ>",
 "n_augmented": "<number after augmentation, if stated, else '-'>",
 "classes": "<number & names of disease classes>",
 "split": "<e.g. 80/10/10 random; 5-fold CV; train 1,800/test 200>",
 "aug_order": "Tăng cường SAU khi chia (chỉ train) | Tăng cường TRƯỚC khi chia | On-the-fly | Không tăng cường | Không rõ",
 "leakage": "Không" | "CÓ" | "Không rõ" | "Nghi ngờ",
 "leakage_evidence": "<quote/paraphrase of sentence(s) + section supporting the leakage call>",
 "metrics": "<main numbers, e.g. Acc 97.5%, F1 0.96; mAP50 0.89; mIoU 0.82>",
 "decision": "NHẬN" | "LOẠI",
 "reason_code": "" | "G1-POOLED" | "G1-CROP" | "G1-SCOPE" | "G2-FROZEN" | "G2-SHALLOW" | "G3-LEAK" | "G3-SMALL" | "G3-PROTOCOL" | "G4-NOMETRIC" | "G4-TASK" | "OTHER-REVIEW" | "OTHER-UNREADABLE",
 "reason": "<Vietnamese, specific, 1–2 sentences; empty if NHẬN>",
 "notes": "<Vietnamese; caveats even for included papers, e.g. 'Nghi ngờ rò rỉ nhưng không đủ bằng chứng', 'tập test rất nhỏ', 'kết quả 100%'>"
}
```

Rules:
- Vietnamese for reason/notes/task_note; keep numbers and model names as in the paper.
- Be conservative but evidence-based: EXCLUDE for leakage only when the text (or the arithmetic of the reported counts) supports it. If the paper reports near-perfect accuracy (≥99.5%) AND augmentation is described without split order, set leakage = "Nghi ngờ" and keep decision NHẬN unless another gate fails; explain in notes.
- Multiple failing gates: give the FIRST failing gate as reason_code and mention others in reason.
- Do not skip any assigned paper. If the text file is empty/garbled, use OTHER-UNREADABLE.

## Addendum 2026-09-26 (rules added after the first 178-paper pass; apply uniformly)

- **Overlapping patches / tiles / video frames**: if images are cut into overlapping patches or extracted as consecutive video frames and the random split is done at patch/frame level (not at source-image or plant level), treat as G3-LEAK (near-duplicates straddle train/test). Non-overlapping patches split at source-image level → no leakage.
- **Multiple photos of the same leaf/plant** split randomly per image → leakage = "Nghi ngờ" (note it); exclude only if the paper itself confirms the same leaf appears in both sets.
- **Clean external/independent test set alongside a leaky main protocol**: still G3-LEAK for the main result, but write the external-test figure in `metrics` and say so in `notes` (the synthesis may use the external number only).
- **Kaggle sets that are already augmented copies of a tiny original set** (e.g. 4,000 images/class derived from the 120-image UCI rice set): leakage = "Nghi ngờ", n_original = the true source count if you can infer it; keep NHẬN unless the paper shows augmented copies in the test set.
- **Scope after protocol amendment A/B (2026-09-26)**: the study must be rice and/or maize only (multi-crop → G1-POOLED even if per-crop numbers exist) and RGB imaging only (hyperspectral/multispectral/UAV/thermal → G1-SCOPE).
- **Validation set used as test set** (no separate held-out test): allowed, but write "val = test" in `split` and mention in `notes`.
- **file / id fields**: `id` = the REP-xxxx report id given in the txt filename; `file` = the txt filename.

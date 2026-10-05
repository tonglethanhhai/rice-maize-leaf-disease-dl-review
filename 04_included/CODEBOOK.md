# Structured coding of included studies (for synthesis tables)

Input: JSON array of extraction records (one per included paper) with free-text fields written by full-text reviewers:
`report_id, title, year, crop, task, task_note, architecture, dataset, n_original, n_augmented, classes, split, aug_order, leakage, leakage_evidence, metrics, notes`.

Task: read EVERY record carefully and output ONE compact object per record with the fields below. Do not invent values; when the record does not say, use "unk". Numbers as plain integers/floats (no thousands separators). Keep report_id exactly.

```
{
 "report_id": "REP-0005",
 "tasks": ["T1"],                      // list of T1..T4 actually solved (from task field)
 "crop": "rice" | "maize" | "both",
 "diseases": "<short English list of disease classes, max 6 items, e.g. 'blast, brown spot, bacterial blight, healthy'>",
 "n_classes": 4,                       // integer or "unk"
 "data_source": "self" | "plantvillage" | "sethy_mendeley" | "kaggle_derived" | "other_public" | "mixed",
      // self = authors collected their own images; plantvillage = PlantVillage (incl. Kaggle copies of PV corn);
      // sethy_mendeley = Sethy et al. 2020 rice set (5,932) or its copies; kaggle_derived = other Kaggle/Roboflow/GitHub sets
      // whose provenance is a re-upload or augmentation of a small set (e.g. UCI 120-image rice set, 'riceleafs', bahribahri);
      // other_public = named public datasets with documented provenance (Paddy Doctor, CD&S, NLB, PlantDoc, AI Challenger,
      // BanglaRiceLeaf, Dhan-Shomadhan, IIITDMJ, RiceLeafDiseaseBD ...); mixed = self + public combined in the experiments
 "dataset_name": "<short name(s), e.g. 'PlantVillage-corn', 'Sethy', 'Paddy Doctor', 'self (VNUA, Vietnam)'>",
 "condition": "lab" | "field" | "mixed" | "unk",   // lab = plain background / controlled; field = in-situ canopy or natural background
 "n_original": 2167,                   // integer best estimate of ORIGINAL images used; "unk" if not stated
 "arch_family": "CNN-TL" | "CNN-custom" | "Lightweight-CNN" | "CNN-attention" | "ViT" | "CNN-ViT-hybrid" | "YOLO" | "Two-stage-detector" | "Other-detector" | "UNet-family" | "Transformer-seg" | "Instance-seg" | "GAN" | "Diffusion" | "Ensemble" | "Other",
      // choose the family of the PROPOSED / best model. CNN-TL = ImageNet-pretrained backbone fine-tuned (VGG/ResNet/EfficientNet/DenseNet/MobileNet...);
      // Lightweight-CNN = MobileNet/ShuffleNet/Ghost/custom tiny nets when lightness is the point; CNN-attention = CNN + SE/CBAM/ECA etc.
 "backbone": "<main backbone/model name, short, e.g. 'EfficientNet-B0', 'YOLOv8n', 'U-Net (EfficientNet-B7)', 'Swin-T'>",
 "params_M": 5.3,                      // parameters in millions if stated, else "unk"
 "metric_type": "Acc" | "F1" | "mAP50" | "mAP50-95" | "mIoU" | "Dice" | "FID" | "SSIM" | "other",
 "metric_value": 97.5,                 // the ONE headline number for the proposed model on the held-out set, in percent (IoU/Dice as percent too); FID raw
 "metric_note": "<≤12 words: e.g. 'val = test', 'training accuracy reported', 'external field set 92.4'>",
 "external_test": "yes" | "no",        // yes only if evaluated on an independent set from another source/season/location
 "val_is_test": "yes" | "no" | "unk",
 "cv": "yes" | "no",                   // k-fold cross-validation used for the headline number
 "leakage": "clean" | "suspect" | "unknown",   // map: Không -> clean; Nghi ngờ -> suspect; Không rõ -> unknown (CÓ never appears here)
 "edge_deploy": "yes" | "no",          // measured on an edge/mobile device (latency/FPS on device reported)
 "code_or_data_released": "yes" | "no" | "unk",
 "note": "<≤20 words Vietnamese caveat worth showing in a table footnote, or ''>"
}
```

Write the JSON array to the output path given in your task. Be consistent: the same dataset must get the same data_source label across records.

## Leakage review (added 2026-10-03)

`coded_included.csv` has two extra columns:

- `leakage_criteria`: for `suspect` studies, the leakage signs found (joined by `+`):
  - **L1** augmentation or image generation before the train/test split;
  - **L2** split at a unit smaller than the independent unit (patch, super-pixel, instance, video frame, or several
    images of the same leaf/plant split at random by image);
  - **L3** dataset already contains augmented copies or duplicates and is used without de-duplication;
  - **L4** test set contains augmented copies of the test images themselves (recorded as a flag only since 2026-10-05: it does
    not move training information into the test set, so on its own it does not make a study `suspect`);
  - **L5** no held-out set, or metrics computed on data used for training (reports with clear L5 are excluded at gate 3 since 2026-10-05);
  - **L6** duplicates across splits acknowledged by the study but not handled.
- Labels used in the paper for the six signs (the data keep L1-L6): L1 = AUG-FIRST, L2 = SUB-UNIT, L3 = PRE-AUG,
  L4 = TEST-AUG, L5 = NO-HOLDOUT, L6 = DUP-LEFT.
- `leakage_review`: non-empty for the 92 studies whose label was re-reviewed.

Labels use only the reported procedure, never the reported accuracy. For public datasets the rule is applied uniformly:
every study that splits the Sethy et al. rice set at random by image is L2; the former blanket L3 rule for the Kaggle
"Corn or Maize" set was withdrawn on 2026-10-04 after near-duplicate measurement (about 0.5 % duplicates, no pre-augmented copies).
`val_is_test` = yes means the study has no independent validation set, so the model is selected on the reported test set;
it is a separate flag and does not affect the leakage label.

Review procedure: an AI tool (Google Gemini 2.5) proposed a label with evidence excerpts from the full text; one author
checked and confirmed every label. There was no double coding for this step. The per-study record is
`leakage_review_92.csv` (columns `decision`, `criteria`, `reason_protocol`, `reviewer`, `batch`).

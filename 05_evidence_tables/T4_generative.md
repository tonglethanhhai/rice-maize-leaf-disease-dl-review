# T4: Generative (4 studies)

Protocol coding: clean 2, suspect 2, unknown 0. Rep. marks the 38 representative studies narrated in the paper. Metric is the headline number reported by the study itself on its own test set, so values are not comparable across rows. Field definitions are in `04_included/CODEBOOK.md`. Rows are sorted by crop, then newest first.

| ID | Study | Crop | Classes | Dataset | Cond. | N orig. | Family | Model | Metric | Val=Test | CV | Ext. test | Leakage | Edge | Code/data | Rep. |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| REP-0425 | Ali et al. (2026), *Discover Computing* [doi](https://doi.org/10.1007/s10791-025-09853-2) | rice | 5 | Kaggle rice-diseases-image-dataset + 40 Google BLB | field | 3393 | CNN-TL | InceptionDenseNet + Pix2Pix GAN + Otsu | Acc 86.98 | unk | no | no | clean | no | unk | yes |
| REP-0361 | Sharma & Khunteta (2024), *Edelweiss Applied Science and Technology* [doi](https://doi.org/10.55214/25768484.v8i6.2372) | rice | 4 | Sethy | field | 5932 | GAN | DCGAN + custom 5-conv CNN | Acc 99.75 | yes | no | no | suspect | no | unk | yes |
| REP-0318 | Zhang et al. (2023), *IEEE Access* [doi](https://doi.org/10.1109/ACCESS.2023.3251098) | rice | 3 | self (prior study + Hefei CAS) | unk | 2538 | GAN | WGAN-GP + Opt-Real-ESRGAN; ResNet18 classifier | Acc 91.65 | yes | no | no | clean | no | unk | yes |
| REP-0019 | Dong et al. (2024), *Agriculture (Switzerland)* [doi](https://doi.org/10.3390/agriculture14010074) | maize | 4 | Kaggle/OpenDataLab/PaddlePaddle corn 256×256 (PV-like) | lab | 2107 | YOLO | YOLOv5s-C3CBAM | mAP50 83.0 | unk | no | no | suspect | no | unk | yes |

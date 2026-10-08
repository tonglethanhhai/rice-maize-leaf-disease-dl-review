# Split manifests of the controlled experiment

These files list which images were in the training, validation and test sets of each of the 140 runs reported in Section 7 and Tables 14–15 of the manuscript, by dataset, protocol and seed.

They were regenerated on 2026-10-08 from the experiment's listing and splitting procedure, on CPU only and without training. Every manifest reproduces the `n_train`, `n_val` and `n_test` of the corresponding run in `runs.csv`: 35 of 35 runs for each of the four datasets (`verification.json`).

## Files

Each `<dataset>/` folder (`sethy`, `pvcorn`, `kaggle_cornmaize`, `paddydoctor`) contains:

| File | Content |
|---|---|
| `images.csv` | `path` (relative to the dataset root), `class`, `group` (the near-duplicate group used for splitting), and `sha256` of the image file. |
| `splits_image_level.csv` | `path`, plus two columns per seed `k`: `RAND-SPLIT_s<k>` (random image-level split) and `GROUP-SPLIT_s<k>` (split by near-duplicate groups). Each cell is `train`, `val` or `test`. |
| `splits_aug_split_s<k>.csv` | The AUG-SPLIT items: `path`, `copy` (0 = original image, 1–5 = offline augmented copy) and `split`. |

## How the matched protocols use these splits

- GROUP-SPLIT, MATCH-CTRL and MATCH-LEAK all use the `GROUP-SPLIT_s<k>` partition of the same seed.
- The validation and test sets of both matched arms contain only the original images marked `val` and `test`.
- The MATCH-CTRL training pool is the `train` images plus five augmented copies of each.
- The MATCH-LEAK training pool additionally contains five augmented copies of each `test` image.
- The augmentation parameters are a deterministic function of the seed, the image index and the copy index (Supplementary Material, Section S7).

## Source of the near-duplicate groups

- Sethy dataset, PlantVillage maize subset and Kaggle Corn or Maize: `results/<dataset>/images.csv` (broad near-duplicate criterion).
- Paddy Doctor: `results/paddydoctor/images_verified.csv` (pair criterion).

## Images

The images themselves are not redistributed. Obtain them from the original sources cited in the manuscript and check them against the `sha256` values.

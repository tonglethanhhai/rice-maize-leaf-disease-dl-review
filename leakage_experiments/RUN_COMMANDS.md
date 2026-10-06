# Commands used for the near-duplicate scan and the controlled split experiment

The scripts named below are not published (data-only repository); they are available from the authors on request.
The commands are listed so that the settings of every run are documented.

`DS` is the folder that holds the public datasets (here `C:/Users/Admin/Desktop/Dataset`). Run from `leakage_experiments/`.
Python 3.12, PyTorch with CUDA, one GPU; ResNet-50 (ImageNet weights), 224 px, AMP, batch 64.

Labels used in the paper and README, and the protocol ids written in `runs.csv`: AUG-SPLIT = `P1_augment_then_split`, RAND-SPLIT = `P2_random_image_split`, GROUP-SPLIT = `P3_group_split`, MATCH-CTRL = `P3m_offline_matched`, MATCH-LEAK = `P1m_leak_matched`.

## 1. Near-duplicate scan (writes `results/<name>/images.csv`, the lenient grouping)

    python dedup_scan.py --root "$DS/01_sethy_rice_mendeley" --name sethy --out results
    python dedup_scan.py --root "$DS/PlantVillage" --include "Corn_" --name pvcorn --out results
    python dedup_scan.py --root "$DS/03_kaggle_corn_or_maize/data" --name kaggle_cornmaize --out results
    python dup_criteria.py      # strict / verified / lenient groupings -> dup_criteria.json, images_verified.csv, images_strict.csv

## 2. Controlled split experiment (AUG-SPLIT, RAND-SPLIT, GROUP-SPLIT; seeds 0-4; writes `results/<name>/runs.csv`)

Seeds 0-2 were run first and seeds 3-4 added later with `--seeds 3 4` (finished runs are skipped).

    python leakage_experiment.py --root "$DS/Rice Leaf Disease Image Samples" --groups results/sethy/images.csv --name sethy --out results --seeds 0 1 2 3 4
    python leakage_experiment.py --root "$DS/PlantVillage" --include "Corn_" --groups results/pvcorn/images.csv --name pvcorn --out results --seeds 0 1 2 --external "$DS/PlantDoc" --external-map "Corn Gray leaf spot=Corn_(maize)___Cercospora_leaf_spot Gray_leaf_spot;Corn leaf blight=Corn_(maize)___Northern_Leaf_Blight;Corn rust leaf=Corn_(maize)___Common_rust_" --seeds 0 1 2 3 4
    python leakage_experiment.py --root "$DS/03_kaggle_corn_or_maize/data" --groups results/kaggle_cornmaize/images.csv --name kaggle_cornmaize --out results --seeds 0 1 2 3 4
    python leakage_experiment.py --root "$DS/04_paddy_doctor/train_images" --groups results/paddydoctor/images_verified.csv --name paddydoctor --cache-from results_matched/paddydoctor --out results --seeds 0 1 2 3 4

## 3. Group file used by GROUP-SPLIT

GROUP-SPLIT used the **lenient** grouping (`images.csv`, the upper bound of the near-duplicate measurement), not
`images_verified.csv`. Re-creating the GROUP-SPLIT split from each file with the same seeds reproduces the train/val/test sizes
in `runs.csv` for all fifteen GROUP-SPLIT runs on Sethy, PlantVillage maize and Kaggle Corn or Maize only with `images.csv`. Paddy Doctor is the exception: the lenient
grouping chains 72% of its images into one group, so its GROUP-SPLIT runs (and the matched pair) use the verified grouping `images_verified.csv`, which
reproduces all five Paddy Doctor GROUP-SPLIT runs. The authors' verification script checks both (see `number_checks.txt`).

| Dataset | Groups in `images.csv` | SHA-256 of `images.csv` |
|---|---|---|
| sethy | 1,093 | d6e0b69f4e5f9c5e016d8fab72af6cab4cefa1396fe1c8626f1dbd7c8549fbdc |
| pvcorn | 3,753 | c0f18d3bdc2c240cc1704dbc522450643486fc3778b28d7ff6a5d7d2d3d0124c |
| kaggle_cornmaize | 4,077 | a4320118db74e6a57cf97c7005618854f6e6a8e7e007a5976739cc2bcf0f0862 |
| paddydoctor (`images_verified.csv`) | 8,498 | 4c26f31b068f60d916d140f986d4ee52242263c6e6a56b3d067e4635ce82e7c5 |

## 4. Known limitation of the design

In AUG-SPLIT every image is augmented offline into five copies before the split, so the AUG-SPLIT training set has about six times
as many items as RAND-SPLIT/GROUP-SPLIT, and its validation and test sets also contain augmented copies. AUG-SPLIT vs GROUP-SPLIT therefore measures the
whole augment-then-split protocol as used by the excluded studies, not leakage alone at equal training size.

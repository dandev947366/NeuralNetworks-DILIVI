### 1. Notebook execution order

The notebooks are run in this order:

```text
1. Dan_preprocessing_local.ipynb
        ↓
2. Dan_E1_training_baseline_local.ipynb
        ↓
3. Dan_E2_training_local.ipynb
        ↓
4. Dan_E3_training_improvement_local.ipynb
```

### 2. Preprocessing notebook

```text
Dan_preprocessing_local.ipynb
```

Purpose:

- verify raw dataset structure,
- collect image metadata,
- check image integrity,
- detect exact duplicates using SHA-256,
- detect cross-split exact duplicates,
- detect near-duplicates using perceptual hashing,
- export near-duplicate pairs for manual review,
- build a final removal manifest,
- generate the clean dataset in `data/clean_tree/`.

Primary outputs:

```text
data/clean_tree/
outputs/dan/preprocessing/near_duplicate_review/
outputs/dan/preprocessing/near_duplicate_review.csv
outputs/dan/preprocessing/removal_manifest.csv
```

### 3. E1 baseline notebook

```text
Dan_E1_training_baseline_local.ipynb
```

Purpose:

- train the ResNet-18 baseline,
- establish the reference experiment,
- save baseline checkpoint and metrics,
- create the shared experiment registry entry,
- produce error analysis inputs for later experiments.

Primary outputs:

```text
outputs/dan/training_baseline/
outputs/dan/experiment_results.csv
```

### 4. E2 architecture comparison notebook

```text
Dan_E2_training_local.ipynb
```

Purpose:

- train ResNet-50,
- compare ResNet-50 against the E1 ResNet-18 baseline,
- compare Top-k behavior between E1 and E2,
- save architecture comparison artifacts.

Primary outputs:

```text
outputs/dan/training/
outputs/dan/experiment_results.csv
```

Important dependency:

```text
outputs/dan/training_baseline/resnet18_E1_topk.json
```

E2 needs the E1 Top-k file to generate the cross-model Top-k comparison plot and separation analysis.

### 5. E3 improvement notebook

```text
Dan_E3_training_improvement_local.ipynb
```

Purpose:

- select the best previous baseline from E1/E2 using validation Macro-F1,
- train one evidence-driven improvement,
- compare the improved model against the selected baseline,
- evaluate the final model once on the test set after validation-based decisions are frozen,
- update the shared experiment registry.

Primary outputs:

```text
outputs/dan/training/
outputs/dan/experiment_results.csv
```

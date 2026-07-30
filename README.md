# Topology-Aware Structural Parsing of Hand-Drawn Diagrams

Checkpoint-based reproduction materials for the manuscript:

**Topology-Aware Structural Parsing of Hand-Drawn Diagrams via Learning-Aligned Decoding**

This release is designed to reproduce the reported inference and evaluation
results from the two frozen model checkpoints.
## Repository contents

```text
.
├── README.md
├── LICENSE
├── CITATION.cff
├── requirements.txt
├── DATA_AND_MODEL_NOTICE.md
├── notebooks/
│   ├── 00_Convert_Original_Annotations.ipynb
│   ├── 01_Resize_Dataset_Images_to_1024.ipynb
│   ├── 02_Run_Two_Pass_Inference_and_Evaluation.ipynb
│   ├── 03_Run_Complete_Reproduction.ipynb
│   └── README.md
├── trained_models/
│   ├── README.md
│   ├── SHA256SUMS.txt
│   ├── fcinkml_best.pt
│   └── resnet_34.pth
├── converted_annotations/
│   ├── README.md
│   ├── fa/
│   ├── fca/
│   └── fcb/
└── sample_outputs/
    └── README.md
```

Original dataset images are not redistributed. The user obtains them from the
public source repository.

## Source data

Clone or download:

```text
https://github.com/bernhardschaefer/handwritten-diagram-datasets
```

The reproduction uses:

- `fa`: finite automata;
- `fca`: flowcharts from FC_A/OHFCD;
- `fcb`: flowcharts from FC_B.

Read `DATA_AND_MODEL_NOTICE.md` before redistributing any data-derived
artifact.

## Environment

Python 3.10 or newer is recommended. A CUDA-capable GPU is strongly recommended
for full-dataset inference.

```bash
python -m venv .venv
source .venv/bin/activate        # Linux/macOS
# .venv\Scripts\activate         # Windows

python -m pip install --upgrade pip
python -m pip install -r requirements.txt
```

PyTorch installation depends on the local CUDA version. When the generic
installation does not provide GPU support, install the matching PyTorch and
TorchVision build before installing the remaining requirements.

## Required checkpoints

Place the two frozen checkpoints under:

```text
trained_models/fcinkml_best.pt
trained_models/resnet_34.pth
```

Verify their SHA-256 hashes against `trained_models/SHA256SUMS.txt`.

## Recommended one-notebook workflow

Open:

```text
notebooks/03_Run_Complete_Reproduction.ipynb
```

Leave:

```python
PROJECT_ROOT = ""
```

blank when the notebook remains inside the repository. The runner automatically:

1. clones the public source-data repository;
2. locates the `fa`, `fca`, and `fcb` source folders;
3. uses the supplied converted annotations by default;
4. inserts all required paths into temporary notebook copies;
5. runs the image-resizing notebook;
6. loads the two frozen checkpoints;
7. runs inference and evaluation;
8. saves executed notebooks, outputs, and a reproduction manifest.

The default setting is:

```python
REGENERATE_ANNOTATIONS = False
```

Set it to `True` only when the annotation conversion should also be rerun.

The original publication notebooks remain unchanged and retain blank path
fields.

## Manual workflow

### Route A: use the supplied converted annotations

1. Download the original source images.
2. Place the frozen checkpoints under `trained_models/`.
3. Run `01_Resize_Dataset_Images_to_1024.ipynb`.
4. Run `02_Run_Two_Pass_Inference_and_Evaluation.ipynb`.

### Route B: regenerate the converted annotations

1. Download the original source repository.
2. Run `00_Convert_Original_Annotations.ipynb`.
3. Run `01_Resize_Dataset_Images_to_1024.ipynb`.
4. Run `02_Run_Two_Pass_Inference_and_Evaluation.ipynb`.

## Notebook roles

### 00 — Convert original annotations

Converts the source COCO-style annotations into one routed graph JSON file per
image. Node boxes, arrow endpoints, paths, and source-target assignments are
scaled to a 1024 × 1024 canvas.

This notebook does not copy or resize the source images.

### 01 — Resize source images

Uses the converted JSON files as the authoritative sample list, finds each
corresponding source image, resizes it physically to 1024 × 1024, and places it
beside the JSON annotation.

### 02 — Run inference and evaluation

Loads the frozen graph-evidence and plain ResNet34 checkpoints and runs:

```text
graph-evidence prediction
    → Pass I physical graph construction
    → Pass II graph-context finalization
    → ResNet34 node-region refinement
    → final graph JSON
    → evaluation
```

It produces the overall, per-image, per-family, and paper-table metrics.

### 03 — Run complete reproduction

Downloads the source repository, fills the notebook paths automatically, and
executes the required reproduction stages in isolated kernels.

## Reconstructed dataset layout

```text
converted_annotations/
└── <family>/
    └── <split>/
        ├── images/
        │   └── <image file>
        └── json/
            └── <image stem>.json
```

Every stored image and annotation coordinate uses a 1024 × 1024 canvas.

## Evaluation outputs

The inference notebook writes machine-readable graph predictions, per-image
metrics, family-wise metrics, paper-table rows, bounded debug examples, and a
reproduction manifest.

The evaluator reports:

- node precision, recall, and F1;
- Mask IoU on matched node regions;
- Soft-clDice on matched directed arrow centerlines;
- endpoint L2 distance;
- directed link precision, recall, and F1;
- normalized graph edit distance;
- loop-back recall;
- decision-branch recall.

The nGED implementation normalizes GED by:

```text
max(predicted nodes, ground-truth nodes)
+
max(predicted edges, ground-truth edges)
```

## Reproduction rules

- Use the supplied frozen checkpoints.
- Keep the distributed notebook path fields blank.
- Do not alter the held-out evaluation split.
- Do not redistribute the original source images.
- Generate final result tables through Notebook 02.
- Record the source-data commit and checkpoint hashes in the run manifest.

## Citation

Citation metadata are provided in `CITATION.cff`.

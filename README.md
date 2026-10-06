# Topology-Aware Structural Parsing of Hand-Drawn Diagrams

Reproduction and revision materials for:

**Topology-Aware Structural Parsing of Hand-Drawn Diagrams via Learning-Aligned Decoding**

This is the version-2 release of the public reproduction package associated with the manuscript. It preserves the original data-conversion, image-reconstruction, model-training, two-pass graph assembly, and evaluation implementation, and adds the experiments and metric clarifications introduced during peer review.

The previous release is associated with DOI `10.5281/zenodo.21698308`. This archive is intended to be published as a **new Zenodo version**, not as a replacement for the immutable v1 record.

## What is new in v2

Version 2 adds three publication notebooks:

1. `notebooks/01_Reproduce_Train_Infer_Evaluate.ipynb`  
   Consolidates the full original reproduction workflow: annotation conversion, 1024x1024 image reconstruction, optional ResNet34 node-model training, optional graph-evidence training, two-pass inference, and revised evaluation.
2. `notebooks/02_Parameter_Sensitivity.ipynb`  
   Reuses the frozen learned predictions and evaluates the deterministic assembler under the Reviewer-1 parameter-sensitivity protocol. Both the Stage-1 functional-group analysis and the Stage-2 individual-parameter drill-down are implemented.
3. `notebooks/03_Arrow_RCNN_Common_Protocol_Baseline.ipynb`  
   Implements the Arrow R-CNN common-protocol baseline used in the revised manuscript, including the common split, endpoint-order audit, training/resume logic, validation threshold selection, directed-link evaluation, and fixed-threshold check.

The public metric terminology is also corrected:

- **Matched Node Box IoU (mBox-IoU)** is the bounding-box IoU over one-to-one spatially matched predicted and ground-truth nodes. It is not a pixel-level node-mask IoU.
- **Normalized Structural Edit Error (nSEE)** is the normalized node/edge false-positive and false-negative structural error under the established node correspondence. It is not a general graph-edit-distance solver.
- Soft-clDice and Endpoint L2 are identified as **conditional geometry metrics** evaluated only on correctly matched directed links. Coverage is reported explicitly.

See `CHANGELOG.md` and `RELEASE_AUDIT.md` for the exact changes and verification checks.

## Repository layout

```text
.
├── README.md
├── CHANGELOG.md
├── RELEASE_AUDIT.md
├── LICENSE
├── CITATION.cff
├── requirements.txt
├── DATA_AND_MODEL_NOTICE.md
├── MANUSCRIPT_REPORTED_RESULTS.json
├── .zenodo.json
├── notebooks/
│   ├── README.md
│   ├── 01_Reproduce_Train_Infer_Evaluate.ipynb
│   ├── 02_Parameter_Sensitivity.ipynb
│   └── 03_Arrow_RCNN_Common_Protocol_Baseline.ipynb
├── logs/
│   ├── README.md
│   ├── 01_Main_Reproduction.log
│   ├── 02_Parameter_Sensitivity.log
│   └── 03_Arrow_RCNN_Common_Protocol_Baseline.log
├── trained_models/
│   ├── README.md
│   └── SHA256SUMS.txt
├── converted_annotations/
│   ├── README.md
│   ├── fa/
│   ├── fca/
│   └── fcb/
├── node_mask_dataset/
│   ├── README.md
│   ├── masks/
│   └── splits/
├── sample_outputs/
│   └── README.md
└── outputs/
    └── .gitkeep
```

Original source images are not redistributed. The two frozen proposed-method checkpoint binaries and the Arrow R-CNN baseline checkpoint are also not embedded in this archive; expected filenames and the hashes of the original proposed-method checkpoints are documented under `trained_models/`.

## Original dataset source

The handwritten diagram datasets are distributed by the original dataset authors at:

```text
https://github.com/bernhardschaefer/handwritten-diagram-datasets
```

The three families used by the proposed framework are:

- `fa`: finite automata;
- `fca`: flowcharts from FC_A/OHFCD;
- `fcb`: flowcharts from FC_B.

Read `DATA_AND_MODEL_NOTICE.md` before redistributing any data-derived artifact.

## Expected reconstructed dataset layout

```text
<DATASET_ROOT>/
└── <family>/
    └── <split>/
        ├── images/
        │   └── <image file>
        └── json/
            └── <image stem>.json
```

The release configuration uses:

```python
DATASET_FAMILIES = ("fa", "fca", "fcb")
TRAIN_SPLITS = ("train",)
VAL_SPLITS = ("val",)
TEST_SPLITS = ("test",)
USE_EXPLICIT_DATASET_SPLITS = True
```

All converted annotation coordinates use a 1024x1024 canvas. If only the supplied converted JSON files are present, Notebook 01 can reconstruct the image folders from the authorized original source data.

## Environment

Python 3.10 or newer is recommended. GPU acceleration is strongly recommended for model training and full-dataset inference.

```bash
python -m venv .venv
source .venv/bin/activate        # Linux/macOS
# .venv\Scripts\activate         # Windows
python -m pip install --upgrade pip
python -m pip install -r requirements.txt
```

PyTorch installation is CUDA-version dependent. If the generic installation does not provide GPU support, install the appropriate PyTorch/TorchVision build first and then install the remaining requirements.

## Path configuration

The notebooks are platform-neutral. They do not contain notebook-host-specific paths or storage guards.

The default project layout assumes the notebooks are run from the repository root. Paths can also be set through environment variables:

```text
IJDAR_PROJECT_ROOT
IJDAR_WORKING_ROOT
IJDAR_DATASET_ROOT
IJDAR_SOURCE_DATASETS_ROOT
IJDAR_GRAPH_CHECKPOINT
IJDAR_NODE_CHECKPOINT
IJDAR_NODE_MASK_ROOT
IJDAR_ARROW_RCNN_CHECKPOINT
IJDAR_OFFICIAL_ANNOTATION_ROOT
```

Each notebook has a single master-configuration cell near the top.

## Main reproduction workflow

Open:

```text
notebooks/01_Reproduce_Train_Infer_Evaluate.ipynb
```

The default policy is evaluation-first:

- existing compatible graph/node checkpoints are loaded when available;
- training does **not** start silently when a checkpoint is missing;
- model training must be explicitly enabled in the master configuration;
- existing prediction JSON can be re-evaluated without rerunning neural inference;
- original dataset reconstruction is opt-in.

The complete workflow available in this notebook is:

```text
source annotations/images
    -> graph-annotation conversion
    -> 1024x1024 image reconstruction
    -> optional ResNet34 node-model training
    -> optional graph-evidence model training
    -> Pass-I physical graph construction
    -> Pass-II graph-context finalization
    -> node refinement
    -> directed JSON graph
    -> mBox-IoU / nSEE / conditional-geometry evaluation
```

### Proposed-method checkpoint names

```text
trained_models/fcinkml_best.pt
trained_models/resnet_34.pth
```

Expected SHA-256 hashes for the frozen proposed-method checkpoints are recorded in `trained_models/SHA256SUMS.txt`.

## Revised metric definitions

### Matched Node Box IoU

After one-to-one spatial node matching, the evaluator averages bounding-box IoU over matched predicted/ground-truth node pairs. Unmatched nodes are reflected by Node F1 rather than being inserted into this conditional geometric average.

### nSEE

The normalized structural edit error is

```text
(node FP + node FN + edge FP + edge FN)
-------------------------------------------------------------
max(|V_pred|, |V_gt|) + max(|E_pred|, |E_gt|)
```

The directed edge errors are computed after spatial node correspondence. This quantity is intentionally named `nSEE` in v2 and should not be interpreted as an unconstrained graph-edit-distance optimization.

### Conditional connector geometry

A predicted connector enters Soft-clDice and endpoint-geometry evaluation only after its mapped source node, target node, and direction agree with a ground-truth directed relation.

The image-relative spatial tolerance is:

```text
tau = max(2, round(0.02 * max(width, height))) pixels
```

Coverage is exported for both matched links and valid endpoints. The revised manuscript also reports pooled matched-geometry diagnostics in addition to the macro-aggregated values used in the main tables.

## Manuscript-reported proposed-method metrics

The final manuscript reporting profile is stored machine-readably in `MANUSCRIPT_REPORTED_RESULTS.json`:

| Metric | Reported value |
|---|---:|
| Node F1 | 98.57% |
| Matched Node Box IoU (macro) | 82.71% |
| Soft-clDice (macro) | 96.72% |
| Endpoint L2 (macro) | 3.77 px |
| Link F1 | 92.49% |
| nSEE | 0.090 |
| Loop-back recall | 95.24% |
| Decision-branch recall | 94.04% |
| Matched-link coverage | 94.94% |
| Valid-endpoint coverage | 94.94% |
| Pooled matched-link Soft-clDice | 99.35% |
| Pooled endpoint L2 mean | 3.44 px |

The machine-readable file is the release reference for manuscript values. Computed outputs from a fresh run remain separate and should not be silently overwritten to match this file.

## Parameter-sensitivity experiment

Open:

```text
notebooks/02_Parameter_Sensitivity.ipynb
```

Set:

```python
SENSITIVITY_STAGE = "stage1"
```

for the five functional parameter groups and the coordinated all-relaxed/all-strict stress conditions, or:

```python
SENSITIVITY_STAGE = "stage2"
SENSITIVITY_STAGE2_GROUP = "G1"
```

for the individual evidence-admission parameter drill-down.

The notebook shares learned predictions across variants and changes only deterministic assembler decisions. The Stage-1 and Stage-2 manuscript values are stored in `MANUSCRIPT_REPORTED_RESULTS.json`; the supplied Stage-2 long-run console evidence is in `logs/02_Parameter_Sensitivity.log`.

## Arrow R-CNN common-protocol baseline

Open:

```text
notebooks/03_Arrow_RCNN_Common_Protocol_Baseline.ipynb
```

This is a **paper-faithful common-protocol reimplementation**, not a claim of exact reproduction of the historical Arrow R-CNN software. It retains the Faster R-CNN/ResNet101-FPN detector, arrow head-tail regression branch, optimization regime, and graph association logic, and evaluates the baseline on the same 752/188/450 common split and the same directed-link protocol.

The notebook can obtain official COCO annotations from the original dataset repository and can train to the 90,000-update paper profile or evaluate an existing compatible checkpoint.

The revised manuscript reports:

| Metric | Arrow R-CNN common protocol | Proposed framework |
|---|---:|---:|
| Node F1 | 99.95% | 98.57% |
| mBox-IoU (macro) | 97.15% | 82.71% |
| Link F1 | 97.58% | 92.49% |
| nSEE | 0.021 | 0.090 |
| Endpoint L2 (macro) | 5.35 px | 3.77 px |
| Loop-back recall | 95.24% | 95.24% |
| Decision-branch recall | 97.64% | 94.04% |

The detailed long-run log includes pooled geometry diagnostics as well; these are labeled separately and should not be confused with the macro values in the manuscript comparison table.

## Execution logs and long-running experiments

See `logs/README.md` before interpreting the logs.

The release includes saved evidence because some revision experiments are expensive to rerun:

- main 450-image reproduction/inference: **27.68 min** on two Tesla T4 GPUs in the saved run;
- Stage-2 sensitivity: **419.94 min (~7.0 h)** in the supplied run;
- Arrow R-CNN 90k/resume run: saved wall-clock trace of approximately **7.46 h**.

The log headers contain the manuscript-reported reference profile. Raw run-derived quantities later in a log are retained where scientifically relevant and are labeled according to their aggregation.

## CASIA-OHFC

No CASIA-OHFC experiment is included in this archive. Access to the dataset contents requires credentials from the dataset provider; those credentials were not obtained during the revision period despite following the official access procedure. The revised manuscript therefore does not claim cross-domain generalization from CASIA-OHFC.

## Reproducibility and release rules

- Do not regenerate the fixed paper split during evaluation.
- Use the supplied converted annotations and documented split configuration.
- Verify proposed-method checkpoints against `trained_models/SHA256SUMS.txt` when using the frozen checkpoints.
- Do not redistribute original third-party dataset images in this archive.
- Keep manuscript-reported reference values in `MANUSCRIPT_REPORTED_RESULTS.json` separate from freshly computed run outputs.
- Use `nSEE` and `mBox-IoU` terminology in new reports generated from v2.
- Treat pooled and macro conditional-geometry statistics as distinct quantities.

## Citation

Citation metadata are provided in `CITATION.cff`. When v2 is published on Zenodo, cite the version-specific DOI assigned to that release when exact reproducibility of this archive is required.

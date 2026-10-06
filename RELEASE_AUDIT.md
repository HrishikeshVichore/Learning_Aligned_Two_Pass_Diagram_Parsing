# Release audit - version 2.0.0

Baseline for the original implementation: the frozen v1 reproduction package supplied for the manuscript/Zenodo release associated with DOI `10.5281/zenodo.21698308`.

## Notebook integrity

- `01_Reproduce_Train_Infer_Evaluate.ipynb`: nbformat valid; 79 cells; code compile errors=0; saved outputs=0; saved execution counts=0; host-specific path/platform references=0.
- `02_Parameter_Sensitivity.ipynb`: nbformat valid; 81 cells; code compile errors=0; saved outputs=0; saved execution counts=0; host-specific path/platform references=0.
- `03_Arrow_RCNN_Common_Protocol_Baseline.ipynb`: nbformat valid; 26 cells; code compile errors=0; saved outputs=0; saved execution counts=0; host-specific path/platform references=0.

## Original v1 implementation coverage

All function/class definitions from the five original scientific stage notebooks are present in the consolidated v2 main notebook except the standalone CLI parser, which is intentionally replaced by notebook-native configuration:

- `00_Convert_Original_Annotations.ipynb`: 51 function/class definitions; missing in v2 main = none.
- `01_Resize_Dataset_Images_to_1024.ipynb`: 10 function/class definitions; missing in v2 main = none.
- `02_Train_ResNet34_Node_Segmentation.ipynb`: 25 function/class definitions; missing in v2 main = none.
- `03_Train_Graph_Evidence_Model.ipynb`: 103 function/class definitions; missing in v2 main = none.
- `04_Run_Two_Pass_Inference_and_Evaluation.ipynb`: 208 function/class definitions; missing in v2 main = ['parse_args'].

The omitted `parse_args` helper is interface code rather than scientific implementation; no model, conversion, training, assembly, or evaluation function/class from v1 is missing.

## Revision-source transformation audit

- main: revision-source definitions=409; publication definitions=406; intentionally removed definitions=['directory_size_bytes', 'storage_status', 'working_usage_gb'].
- sensitivity: revision-source definitions=429; publication definitions=426; intentionally removed definitions=['directory_size_bytes', 'storage_status', 'working_usage_gb'].
- baseline: revision-source definitions=93; publication definitions=91; intentionally removed definitions=['directory_size_bytes', 'storage_status'].
The only removed revision-source helpers are notebook-host storage-usage functions (`directory_size_bytes`, `storage_status`, and, where present, `working_usage_gb`).

## Derived artifact identity

- `converted_annotations`: v1 files=1393, v2 files=1393, identical path set=True, SHA-256 content differences=0 (README files excluded from this identity check).
- `node_mask_dataset`: v1 files=5633, v2 files=5633, identical path set=True, SHA-256 content differences=0 (README files excluded from this identity check).
- `LICENSE` byte-identical to v1: True.
- `DATA_AND_MODEL_NOTICE.md` byte-identical to v1: True.
- `trained_models/SHA256SUMS.txt` byte-identical to v1: True.

## Metric/terminology checks

- Public notebooks use `Matched Node Box IoU` / `mBox-IoU` terminology for the matched-node bounding-box measure.
- Public notebooks use `nSEE` for the normalized node/edge FP+FN structural error.
- Legacy public metric labels are absent from publication notebooks, logs, and current documentation (the changelog alone records the historical terminology correction).
- The stale alternative rerun headline is absent from publication text/log/notebook outputs; the manuscript reference file records 82.71% mBox-IoU.
- Macro manuscript geometry values and pooled conditional-geometry diagnostics are stored separately and labeled explicitly.

## Host-neutrality scan

- Host-specific path/platform hits in publication text/notebooks: none.

## Log evidence

- `01_Main_Reproduction.log`: no bracketed wall-clock timestamps parsed.
- `02_Parameter_Sensitivity.log`: maximum preserved timestamp 25239.8 s (7.01 h).
- `03_Arrow_RCNN_Common_Protocol_Baseline.log`: maximum preserved timestamp 26855.6 s (7.46 h).

`02_Parameter_Sensitivity.log` additionally preserves the supplied `Completed 450 samples in 419.94 minutes` line and the full saved Stage-2 parameter tables. The manuscript-reported Stage-1 group summary is labeled separately because no separate raw Stage-1 log was supplied in the revision ZIP.

## Release conclusion

The v2 package retains the original scientific implementation and derived artifacts, applies the corrected public metric definitions, contains code for each performed revision experiment, preserves long-run evidence, and removes notebook-host-specific storage/path machinery.

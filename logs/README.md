# Execution logs

This directory contains publication copies of the saved execution evidence used during the revision.

The logs were cleaned only to remove host-specific absolute paths, notebook-platform storage messages, debugger warnings, and notebook-export chatter. Scientific run traces, timings, validation/test outputs, and sensitivity/baseline results are retained.

The summary blocks labeled **MANUSCRIPT-REPORTED** are intentionally normalized to the final revised-paper reporting convention. They should not be interpreted as raw console output. In particular:

- `mBox-IoU` is the mean bounding-box IoU over one-to-one matched nodes;
- `nSEE` is the normalized node/edge FP+FN structural error, not a general GED solver;
- the paper's primary Soft-clDice and Endpoint L2 values are macro-aggregated values used consistently in the manuscript tables;
- pooled matched-geometry diagnostics are labeled separately.

Detailed run-derived values later in a log are preserved even when they use a different aggregation (for example, pooled Endpoint L2 in the Arrow R-CNN run).

## Files

- `01_Main_Reproduction.log` — 450-image two-pass inference/evaluation. Saved run time: **27.68 minutes** on two Tesla T4 GPUs.
- `02_Parameter_Sensitivity.log` — Stage-2 individual-parameter sensitivity run, with the manuscript-reported Stage-1 group summary prepended. Saved Stage-2 run time: approximately **7.0 hours**; the supplied log reports **419.94 minutes** for 450 samples.
- `03_Arrow_RCNN_Common_Protocol_Baseline.log` — Arrow R-CNN common-protocol training/resume, validation-threshold selection, and held-out test evaluation. Saved wall-clock trace reaches approximately **7.46 hours**. The supplied run resumed from optimizer step 57,800 and continued to 90,000 steps.

No CASIA-OHFC execution log is included because dataset credentials were not obtained during the revision period and no experiment was performed on that dataset.

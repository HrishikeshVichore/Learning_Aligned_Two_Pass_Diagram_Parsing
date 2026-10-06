# Changelog

## Version 2.0.0 - 2026-10-06

This release extends the original reproduction archive associated with DOI `10.5281/zenodo.21698308`.

### Reproduction implementation

- Consolidated the original annotation conversion, image reconstruction, node-model training, graph-evidence training, two-pass inference, and evaluation workflow into one platform-neutral publication notebook.
- Preserved all scientific function/class definitions from the original executable v1 notebooks. The only original helper not retained is the standalone CLI `parse_args` function; v2 uses a notebook-native master configuration instead.
- Retained the frozen graph-evidence architecture (`BASE_CH = 16`) and plain ResNet34 node-refinement branch used in the paper release.
- Removed notebook-host-specific filesystem paths, storage quotas, and platform metadata.

### Evaluation corrections

- Renamed the previously reported node-overlap quantity to **Matched Node Box IoU (mBox-IoU)** and made the bounding-box computation explicit.
- Replaced misleading `nGED` terminology with **Normalized Structural Edit Error (nSEE)**, defined directly from node/edge FP and FN counts under spatial node correspondence.
- Added explicit conditional-geometry coverage for matched links and valid endpoints.
- Added pooled Soft-clDice and endpoint-error diagnostics while retaining the macro values used by the manuscript tables.
- Added a machine-readable `MANUSCRIPT_REPORTED_RESULTS.json` containing the final revised-paper reference values.

### Reviewer-1 parameter sensitivity

- Added Stage-1 functional-group perturbation support for evidence admission, geometry/attachment, path tracing/recovery, conflict/duplicate resolution, and graph-context finalization/node refinement.
- Added coordinated all-relaxed/all-strict stress conditions.
- Added Stage-2 individual-parameter drill-down for the sensitive evidence-admission group.
- Included the supplied long-run Stage-2 execution evidence and manuscript-reported Stage-1 summary in `logs/02_Parameter_Sensitivity.log`.

### External baseline

- Added an Arrow R-CNN common-protocol reimplementation using the same common 752/188/450 split and directed-link evaluator.
- Added endpoint-order audit, validation-threshold selection, fixed-threshold check, family-wise evaluation, checkpointing/resume, and long-run execution evidence.
- The baseline is explicitly described as a common-protocol reimplementation, not an exact reproduction of the historical software.

### Logs

- Added cleaned publication logs for the main reproduction, sensitivity experiment, and Arrow R-CNN baseline.
- Host-specific paths, storage messages, debugger warnings, and notebook-export chatter were removed.
- Manuscript-reference summary blocks use the final revised-paper reporting convention; run-derived experimental tables/timings are retained.

### Unchanged from v1

- Derived converted graph annotations.
- Curated node-mask dataset.
- Original proposed-method checkpoint filenames and SHA-256 hashes.
- Third-party data licensing notice and Apache-2.0 code license.

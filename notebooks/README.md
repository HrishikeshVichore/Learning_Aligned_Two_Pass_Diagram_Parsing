# Publication notebooks

All notebooks are platform-neutral and use a master configuration cell rather than notebook-host-specific paths.

## 01_Reproduce_Train_Infer_Evaluate.ipynb

Complete proposed-method workflow: annotation conversion, image reconstruction, optional model training, two-pass inference, graph export, and revised evaluation. Training is opt-in; compatible checkpoints are used directly when supplied.

## 02_Parameter_Sensitivity.ipynb

Deterministic-assembler sensitivity experiment. Use `SENSITIVITY_STAGE="stage1"` for the five functional groups and coordinated stress conditions, or `SENSITIVITY_STAGE="stage2"` for the individual G1 parameter drill-down.

## 03_Arrow_RCNN_Common_Protocol_Baseline.ipynb

Arrow R-CNN common-protocol baseline. Uses the common split and the same directed-link/nSEE evaluation protocol as the proposed framework. Supports training/resume and evaluation from a compatible checkpoint.

Saved execution outputs are deliberately removed from the notebook files. Long-run evidence is provided under `../logs/`.

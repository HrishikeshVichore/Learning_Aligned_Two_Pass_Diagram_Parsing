# Reproduction outputs

`02_Run_Two_Pass_Inference_and_Evaluation.ipynb` writes its results here when
run through the master reproduction notebook.

Typical generated content includes:

```text
sample_outputs/final_run/
├── predictions/
├── metrics/
├── debug/
└── reproduction_manifest.json
```

The metric folder contains overall, per-image, per-family, and manuscript-table
outputs. Only results generated with the frozen release checkpoints and the
complete held-out test set should be treated as paper-reproduction results.

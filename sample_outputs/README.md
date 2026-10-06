# Sample outputs

The publication notebooks write generated predictions, metric CSV/JSON files, manifests, sensitivity tables, and optional debug visualizations under the configurable `outputs/` directory.

Generated run outputs are not frozen into this release because they can be reproduced from the notebooks and checkpoints. The machine-readable final manuscript reference profile is stored at the repository root in `MANUSCRIPT_REPORTED_RESULTS.json`, while saved console evidence from long-running revision experiments is retained under `logs/`.

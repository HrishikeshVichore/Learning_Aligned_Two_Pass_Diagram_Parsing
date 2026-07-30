# Reproduction notebooks

| Notebook | Purpose | Required in the default workflow |
|---|---|---:|
| `00_Convert_Original_Annotations.ipynb` | Regenerate the routed per-image graph annotations | Optional |
| `01_Resize_Dataset_Images_to_1024.ipynb` | Reconstruct the 1024 × 1024 image folders | Yes |
| `02_Run_Two_Pass_Inference_and_Evaluation.ipynb` | Run the frozen models, assembler, and evaluator | Yes |
| `03_Run_Complete_Reproduction.ipynb` | Download the source data, insert paths, and orchestrate reproduction | Recommended |

All user-specific paths remain blank in Notebooks 00--02.

The master runner creates temporary path-filled copies under
`workspace/prepared_notebooks/` and stores completed executions under
`workspace/executed_notebooks/`. It never modifies the distributed publication
notebooks.

By default, the master runner uses the supplied converted annotations. Set
`REGENERATE_ANNOTATIONS = True` only to rerun Notebook 00.

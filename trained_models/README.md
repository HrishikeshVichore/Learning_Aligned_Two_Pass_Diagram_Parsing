# Trained models

Checkpoint binaries are not embedded in this release archive. Place compatible checkpoints in this directory or set their paths in the relevant notebook master configuration.

## Proposed framework

### `fcinkml_best.pt`

- Role: graph-evidence model used before the two-pass assembler.
- Architecture: multi-head DiagramUNet.
- Required base width: `BASE_CH = 16`.
- Expected SHA-256 for the frozen paper checkpoint:

```text
3a3f902bfebfa18bb7fb2016c8780e95b679d6411549556a13fb7b094dce5a90
```

### `resnet_34.pth`

- Role: node refinement model.
- Architecture: plain ResNet34 U-Net.
- The frozen checkpoint does not contain GeLoVec parameters.
- Expected SHA-256 for the frozen paper checkpoint:

```text
f3acdd75f037fe4a1b4ea3514de583ebfff2b3d5bcea04df4215270e98efcb0b
```

The same values are stored in `SHA256SUMS.txt`.

## Arrow R-CNN common-protocol baseline

The baseline notebook uses the default filename:

```text
arrow_rcnn_common_final.pt
```

or can resume from:

```text
arrow_rcnn_common_last.pt
```

No Arrow R-CNN checkpoint is bundled here. The notebook contains the complete training/resume implementation used for the common-protocol experiment, and the saved long-run evidence is under `../logs/03_Arrow_RCNN_Common_Protocol_Baseline.log`.

Model checkpoints are data-derived artifacts. Read `../DATA_AND_MODEL_NOTICE.md` before redistribution.

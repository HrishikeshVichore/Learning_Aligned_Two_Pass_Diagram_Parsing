# Frozen model checkpoints

Place the two paper checkpoints in this directory.

## `fcinkml_best.pt`

- Role: graph-evidence prediction before structural assembly.
- Architecture: multi-head DiagramUNet.
- Required base width: `BASE_CH = 16`.

Expected SHA-256:

```text
3a3f902bfebfa18bb7fb2016c8780e95b679d6411549556a13fb7b094dce5a90
```

## `resnet_34.pth`

- Role: conservative node-region refinement.
- Architecture: plain ResNet34 U-Net.
- The checkpoint does not contain GeLoVec parameters.

Expected SHA-256:

```text
f3acdd75f037fe4a1b4ea3514de583ebfff2b3d5bcea04df4215270e98efcb0b
```

## Verification

Linux/macOS:

```bash
sha256sum fcinkml_best.pt resnet_34.pth
```

Windows PowerShell:

```powershell
Get-FileHash fcinkml_best.pt -Algorithm SHA256
Get-FileHash resnet_34.pth -Algorithm SHA256
```

Notebook 02 loads only the fields required for inference.

Model checkpoints are data-derived artifacts. Read
`../DATA_AND_MODEL_NOTICE.md` before redistribution.

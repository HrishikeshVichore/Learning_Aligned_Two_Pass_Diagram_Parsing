# Node-mask dataset

This folder contains the curated positive node-instance masks used to train the plain ResNet34 U-Net node model.

## Layout

```text
node_mask_dataset/
├── masks/
│   └── <image stem>/
│       ├── mask_0001.png
│       └── ...
└── splits/
    ├── train.txt
    ├── val.txt
    └── test.txt
```

The curated dataset contains:

- 528 training image IDs;
- 132 validation image IDs;
- 196 test image IDs;
- 856 image IDs in total;
- 5,629 positive instance masks.

The image files are not duplicated here. Obtain the original images from the authorized source data or reconstruct the 1024x1024 graph dataset using `notebooks/01_Reproduce_Train_Infer_Evaluate.ipynb`.

The v2 notebook unions the positive masks belonging to an image into the binary node target used for training. These masks are derived annotations; read `../DATA_AND_MODEL_NOTICE.md` before redistribution.

# Converted graph annotations

This folder contains the per-image graph JSON annotations used by the v2 reproduction notebook. They are the same derived annotation resource carried forward from the v1 reproduction package.

Original source images are not included.

## Layout

```text
converted_annotations/
└── <family>/
    └── <split>/
        └── json/
            └── <image stem>.json
```

After image reconstruction, the corresponding working dataset becomes:

```text
<family>/<split>/
├── images/
└── json/
```

## Per-image schema

```json
{
  "image": "example.png",
  "size": {"w": 1024, "h": 1024},
  "nodes": [
    {"id": "N1", "bbox": [100, 120, 180, 90]}
  ],
  "arrows": [
    {
      "id": 1,
      "tail_hint": {"x": 180, "y": 210},
      "head_hint": {"x": 510, "y": 330},
      "path": [{"x": 180, "y": 210}, {"x": 510, "y": 330}],
      "tail_node_id": "N1",
      "head_node_id": "N2",
      "control_points": []
    }
  ]
}
```

All bounding boxes, endpoint hints, paths, and reconstructed images use the same 1024x1024 coordinate system.

`notebooks/01_Reproduce_Train_Infer_Evaluate.ipynb` contains the original conversion and image-reconstruction implementation. These annotations are derived from third-party datasets and are not covered by the repository's source-code license. Read `../DATA_AND_MODEL_NOTICE.md`.

# Converted annotations

This folder stores the per-image graph JSON annotations produced by
`notebooks/00_Convert_Original_Annotations.ipynb`.

Original source images are not included here.

## Expected layout

```text
converted_annotations/
├── fa/
│   ├── train/json/
│   ├── val/json/       # only when available
│   └── test/json/
├── fca/
└── fcb/
```

After running `01_Resize_Dataset_Images_to_1024.ipynb`, each split also contains
an `images/` folder:

```text
<family>/<split>/
├── images/
└── json/
```

## Per-image annotation schema

```json
{
  "image": "example.png",
  "size": {"w": 1024, "h": 1024},
  "nodes": [
    {
      "id": "N1",
      "bbox": [100, 120, 180, 90]
    }
  ],
  "arrows": [
    {
      "id": 1,
      "tail_hint": {"x": 180, "y": 210},
      "head_hint": {"x": 510, "y": 330},
      "path": [
        {"x": 180, "y": 210},
        {"x": 510, "y": 330}
      ],
      "tail_node_id": "N1",
      "head_node_id": "N2",
      "control_points": []
    }
  ]
}
```

All bounding boxes, endpoint hints, paths, and stored images use the same
1024 × 1024 coordinate system.

The full release should contain the converted JSON files, not merely the small
illustrative sample used during repository development.

These annotations are derived from third-party datasets and are not covered by
the repository's source-code license. Read `../DATA_AND_MODEL_NOTICE.md`.

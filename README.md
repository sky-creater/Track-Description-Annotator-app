<h1 align="center">🎯 Track Description Annotator</h1>

<p align="center"><strong>A cross-platform desktop tool for text annotation on multi-object tracking data.</strong></p>

<p align="center">
  <img src="./docs/images/deep-light-mode.png" alt="Track Description Annotator dark and light mode preview" width="100%" />
</p>

<p align="center">
  <img src="https://img.shields.io/github/v/release/sky-creater/Track-Description-Annotator-app?color=blue&logo=github" alt="Release" />
  <img src="https://img.shields.io/badge/Channel-Official%20Releases-16a34a" alt="Channel" />
  <img src="https://img.shields.io/badge/Platform-Win%20%7C%20Mac%20%7C%20Linux-orange" alt="Platform" />
  <img src="https://img.shields.io/badge/Stack-Electron%20%7C%20React%20%7C%20Vite-4b5563" alt="Stack" />
</p>

<p align="center"><a href="./README_ZH.md">中文版本</a></p>

Track Description Annotator is designed for video multi-object tracking workflows. It helps annotate tracked objects with natural-language descriptions, review them frame by frame, label object relations, and export finalized results.

- Download releases: [GitHub Releases](https://github.com/sky-creater/Track-Description-Annotator-app/releases)
- Platforms: Windows / macOS / Linux
- Binary releases are published on this repository's Releases page.

---

## ✨ Core Features

- Import label folders in `.json` or `.jsonl` format and build indices by `sequence / track_id / frame`.
- Link an image folder and visualize both the current object box and related object box in the UI.
- Filter by sequence and Track ID to focus on a single trajectory.
- Lock fields and carry them forward to the next frame for faster continuous annotation.
- Export only sequences marked as `Submitted`.
- Package exported results as ZIP files for downstream processing or training workflows.

---

## 📥 Input Format

### 📄 Label Files

Two input formats are supported:

- `.json`: the file content is a JSON array
- `.jsonl`: the file content is newline-delimited JSON, one record per line

The core structure of a single record is:

```json
{
  "seq": "MOT17-02",
  "image_name": "000001.jpg",
  "frame_index": 1,
  "stride": 1,
  "object": {
    "track_id": 7,
    "category_name": "person",
    "bbox_xyxy": [100, 80, 220, 360],
    "description": {
      "appearance": "",
      "location": "",
      "event": "",
      "relations": {
        "environment": "",
        "relation_with_hint": {
          "related_track_id": -1,
          "related_category_name": null,
          "related_bbox_xyxy": null,
          "relation_type": "",
          "relation_description": ""
        }
      }
    },
    "uncertainty": []
  }
}
```

Key fields:

| Field | Type | Description |
| --- | --- | --- |
| `seq` | `string` | Sequence name |
| `image_name` | `string` | Image file name |
| `frame_index` | `number` | Frame index |
| `object.track_id` | `number` | Trajectory ID |
| `object.category_name` | `string` | Object category |
| `object.bbox_xyxy` | `number[4]` | Bounding box in `[x1, y1, x2, y2]` format |
| `object.description.appearance` | `string` | Appearance description |
| `object.description.location` | `string` | Location description |
| `object.description.event` | `string` | Action / event description |
| `object.description.relations.environment` | `string` | Environment description |
| `object.description.relations.relation_with_hint.*` | `object` | Related object and relation description |

### 🗂️ Image Folder

It is recommended to organize images by sequence:

```text
images/
  MOT17-02/
    000001.jpg
    000002.jpg
  MOT17-04/
    000001.jpg
```

The app first tries exact matching by `seq/image_name`. If that fails, it falls back to filename-based matching.

---

## 📤 Output Format

Only sequences marked as `Submitted` are included in export.

Export rules:

- Exported as a ZIP file: `aligned_labels_<timestamp>.zip`
- Grouped by original input file
- Output filenames automatically receive the `align_` prefix
- File contents are always **JSON Lines**

This means:

- Even if the input is a `.json` array, the exported content becomes line-based JSON
- If the original extension is `.json`, the exported file content may still actually be JSONL
- The runtime-only field `sourceFileName` is not written back to exported output

Example output:

```jsonl
{"seq":"MOT17-02","image_name":"000001.jpg","frame_index":1,"stride":1,"object":{"track_id":7,"category_name":"person","bbox_xyxy":[100,80,220,360],"description":{"appearance":"A person in a dark jacket.","location":"Walking near the center of the frame.","event":"Moving forward.","relations":{"environment":"Outdoor street scene.","relation_with_hint":{"related_track_id":12,"related_category_name":"person","related_bbox_xyxy":[260,90,360,355],"relation_type":"next_to","relation_description":"Walking beside another person."}}},"uncertainty":[]}}
```

---

## 🎬 Demo

Demo dataset download:

- Baidu Netdisk: [demo_dataset.zip](https://pan.baidu.com/s/19WOQ7pt966q4r1UTrtp4dg?pwd=tpwd)
- Extraction code: `tpwd`

Demo steps:

1. Click `Select Label Folder` and choose the label folder:
   `yourpath/demo_dataset/label/LaSOT`
2. Click `Images Linked` and choose the image folder:
   `yourpath/demo_dataset/Image/LaSOT`

Note:

- If the wrong folders are selected, the software may become unresponsive.
- If that happens, restart the application and load the correct folders again.

---

## ✅ Official Release Policy

Only release assets attached to this repository are official builds.

- Source code is maintained separately in a private repository.
- Modified or repackaged versions published elsewhere are not official unless explicitly stated by the copyright holder.
- Each official release may include `SHA256SUMS.txt` for checksum verification.

---

## 📄 Terms

- Download and usage terms: [TERMS.md](./TERMS.md)
- Branding and naming policy: [TRADEMARKS.md](./TRADEMARKS.md)

---

## ℹ️ Notes

- Windows, macOS, and Linux may show first-run security prompts for unsigned builds.
- Always verify the release notes and checksums before redistribution inside your own organization.

# ImageBind_ML_HCMUS_2026 Rubric Audit

Audit date: 2026-04-27

Scope: `01_esc50_audio.ipynb`, `02_flickr8k_text.ipynb`, `03_llvip_thermal.ipynb`, `04_nyu_depth_v2.ipynb`, `05_uci_har_motion.ipynb`.

Method: notebooks were parsed as `.ipynb` JSON in the local workspace because the current local shell does not expose Python/nbformat. Existing result JSON/TXT files under `ResultForSound1`, `ResultForText2`, `ResultForThermal3`, `ResultForDepth4`, and `ResultForMotion5` were inspected for metrics and reports.

| Notebook | Dataset | Modality | Train from scratch? | Test evaluation? | Metrics present? | Ablation present? | Missing items | Actions applied |
|---|---|---:|---:|---:|---|---|---|---|
| `01_esc50_audio.ipynb` | ESC-50 | Audio-Text | Yes | Yes, fold 5 | Top-1, Top-5, Audio->Text R@1/5/10, Text->Audio R@1/5/10, confusion matrix | Missing in notebook | No standardized rubric metadata/report; no ablation; prompt template benchmark not explicit; limitation about fold 5 should be explicit | Append rubric addon with metadata, final eval summary, temperature sweep from available embeddings/JSON fallback, prompt template benchmark status, recall bar, report |
| `02_flickr8k_text.ipynb` | Flickr8k | Image-Text | Yes | Yes | Image->Text R@1/5/10, Text->Image R@1/5/10, retrieval examples | Yes, temperature | Need standardized ablation JSON/PNG; recall bar; rubric report; limitation for 1 epoch | Append rubric addon with normalized summary, ablation summary, recall bar, retrieval examples status, report |
| `03_llvip_thermal.ipynb` | LLVIP | Visible-Thermal | Yes | Yes | Image->Thermal R@1/5/10, Thermal->Image R@1/5/10, optional binary accuracy/confusion if labels parsed | Yes, temperature | Need standardized rubric metadata/report; explicit note about retrieval pairs vs crop subset; normalized ablation and output paths | Append rubric addon with normalized eval/ablation, retrieval bar, optional confusion matrix export, report |
| `04_nyu_depth_v2.ipynb` | NYU Depth V2 | RGB-Depth | Yes | Yes | Image->Depth R@1/5/10, Depth->Image R@1/5/10, optional Top-1/Top-5 if image-level labels parsed | Yes, temperature | Need standardized ablation summary; optional second ablation status; recall bar; rubric report; retrieval examples fallback text | Append rubric addon with normalized eval/ablation, retrieval bar, second ablation status, report |
| `05_uci_har_motion.ipynb` | UCI HAR | Motion/IMU | Yes | Yes, official test split | Accuracy, macro-F1, per-class accuracy/report, confusion matrix | Yes, learning rate/hidden size | Must avoid claiming full ImageBind; add standardized ablation/report; add optional motion prototype retrieval proxy if embeddings/model available | Append rubric addon with normalized eval/ablation, confusion export, supervised prototype retrieval proxy, report |

## Global Rubric Gaps Before Patch

- Output naming is inconsistent across notebooks.
- Some notebooks have reports but not a unified rubric metadata cell.
- Some notebooks have ablation but not standardized `ablation_summary.json` and `ablation_summary.png`.
- ESC-50 lacks ablation in the original notebook.
- UCI HAR needs a stronger limitation statement because it has no image/video anchor and is not full ImageBind.
- Master project-level rubric report and submission checklist are missing.

## Patch Policy

- Existing cells are preserved.
- Addons are appended to the end of each notebook under `## ADDON: Rubric completion and missing components`.
- Optional sections use `try/except` and write `NOT_RUN` or `FAILED_WITH_REASON` instead of inventing metrics.
- New outputs are written under `/kaggle/working/imagebind_rubric_outputs/<notebook_stem>/` when run on Kaggle, with a local `./kaggle_working/...` fallback.

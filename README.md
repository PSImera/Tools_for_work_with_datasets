# Tools for working with datasets

> <a href="README-RU.md" target="_blank">**Это описание есть на русском языке**</a>

A collection of tools for building, labelling, and evaluating image datasets.  
Used during the development of [AI-Apex-Stat-Detector-Discord-Bot](https://github.com/PSImera/AI-Apex-Stat-Detector-Discord-Bot) — a bot that reads in-game statistics from Apex Legends screenshots via OCR.

---

## Pipeline

```
Screenshots
    │
    ├─► dataset_read_to_csv.py          # extract structured stats → CSV
    │
    ├─► EasyOCR_dataset_prepare.py      # crop text blocks + auto pre-labelling
    │
    ├─► EasyOCR_text_pre_markup.py      # auto pre-labelling with base EasyOCR (no custom model)
    │
    ├─► markup_text.py                  # manual text annotation (GUI)
    ├─► markup_category.py              # manual image classification (GUI)
    │
    ├─► EasyOCR_dataset_saving.ipynb    # save dataset in EasyOCR training format
    │
    └─► EasyOCR_models_test.py          # compare model accuracy
```

---

## Tools

### `dataset_read_to_csv.py`
Iterates over a folder of screenshots, extracts game statistics (kills, deaths, K/D, rank, etc.) via `stat_by_screen`, and appends results to a CSV. Skips already-processed files so the run can be interrupted and resumed.

### `EasyOCR_dataset_prepare.py`
1. Crops text blocks from images using the EasyOCR detector (`crop_text_from_img`).
2. Runs the blocks through the custom trained `apex_stats_detector` model and saves the predicted text as pre-labels (`pre_markup_blocks`).

### `EasyOCR_text_pre_markup.py`
Pre-labels text blocks with base EasyOCR (ru+en) without a custom model. Used at early stages before a custom model is available.

### `markup_text.py`
Tkinter app for manual text annotation. Displays an image, a text input field, and a Save button. Results are written to a CSV (`filename`, `words`). Already-annotated files are loaded on startup so work can be resumed.

### `markup_category.py`
Tkinter app for manual image classification. Supports category buttons and numeric hotkeys (1–9) for fast labelling. Skips files listed in `annotated_csv`, results are saved to `output_csv`.

### `EasyOCR_dataset_saving.ipynb`
Notebook for preparing and saving the dataset in the format expected by EasyOCR training.

### `role_model_training.ipynb`
Notebook for fine-tuning the recognition model.

### `EasyOCR_models_test.py`
Runs all `.pth` models from `EasyOCR_model/` against a test dataset and reports:
- **Accuracy** — exact string match
- **Character Accuracy** — per-character match


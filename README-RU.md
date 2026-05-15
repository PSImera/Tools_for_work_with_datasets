# Tools for working with datasets

> <a href="README.MD" target="_blank">**English version**</a>

Набор инструментов для сборки, разметки и проверки датасетов.  
Использовались при разработке [AI-Apex-Stat-Detector-Discord-Bot](https://github.com/PSImera/AI-Apex-Stat-Detector-Discord-Bot) — бота, который распознаёт игровую статистику из скриншотов Apex Legends через OCR.

---

## Пайплайн

```
Скриншоты
    │
    ├─► dataset_read_to_csv.py          # извлечь структурированную статистику → CSV
    │
    ├─► EasyOCR_dataset_prepare.py      # нарезать текстовые блоки + авто-предразметка
    │
    ├─► EasyOCR_text_pre_markup.py      # авто-предразметка базовым EasyOCR (без кастомной модели)
    │
    ├─► markup_text.py                  # ручная разметка текста в блоках (GUI)
    ├─► markup_category.py              # ручная разметка категорий изображений (GUI)
    │
    ├─► EasyOCR_dataset_saving.ipynb    # сохранение датасета в формат EasyOCR
    │
    └─► EasyOCR_models_test.py          # сравнить точность моделей
```

---

## Инструменты

### `dataset_read_to_csv.py`
Проходит по папке со скриншотами, извлекает игровую статистику (kills, deaths, K/D, rank и т.д.) через `stat_by_screen` и дописывает результат в CSV. Пропускает уже обработанные файлы — можно прерывать и продолжать.

### `EasyOCR_dataset_prepare.py`
1. Нарезает текстовые блоки из изображений с помощью EasyOCR-детектора (`crop_text_from_img`).
2. Прогоняет блоки через кастомную обученную модель `apex_stats_detector` и сохраняет предсказанный текст как предразметку (`pre_markup_blocks`).

### `EasyOCR_text_pre_markup.py`
Предразметка текстовых блоков базовым EasyOCR (ru+en) без кастомной модели. Используется на ранних этапах, когда кастомной модели ещё нет.

### `markup_text.py`
Tkinter-приложение для ручной разметки текста. Показывает изображение, поле для ввода текста и кнопку Save. Результат пишется в CSV (`filename`, `words`). Уже размеченные файлы подгружаются при старте — можно продолжать с места остановки.

### `markup_category.py`
Tkinter-приложение для ручной классификации изображений по категориям. Кнопки и цифровые хоткеи (1–9) для быстрой разметки. Пропускает файлы из `annotated_csv`, результат пишется в `output_csv`.

### `EasyOCR_dataset_saving.ipynb`
Ноутбук для подготовки и сохранения датасета в формат, ожидаемый EasyOCR при обучении.

### `role_model_training.ipynb`
Ноутбук для дообучения модели распознавания.

### `EasyOCR_models_test.py`
Прогоняет все `.pth`-модели из `EasyOCR_model/` по тестовому датасету и выводит метрики:
- **Accuracy** — точное совпадение строк
- **Character Accuracy** — посимвольное совпадение


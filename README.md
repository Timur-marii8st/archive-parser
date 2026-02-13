<div align="center">

[![ru](https://img.shields.io/badge/lang-ru-blue.svg)](#-archive-parser-ru)
[![en](https://img.shields.io/badge/lang-en-red.svg)](#-archive-parser-en)

</div>

---

<a id="ru"></a>

# 📚 Archive Parser (RU)

> Автоматизированная система создания архивных описей на основе OCR и искусственного интеллекта
> **UPDATE:** Добавил скрипт автоматизации создания описи одного документа

## 🎯 О проекте

Система автоматически обрабатывает фотографии архивных дел и создаёт структурированную опись в формате Word.
+ еще есть скрипт для обработки и создания описи ОДНОГО документа

**Процесс работы:**
1. 📸 Сканирование папок с фотографиями документов
2. 🔍 OCR распознавание текста с помощью DeepSeek-OCR-2
3. 🤖 Анализ и структурирование данных с помощью LLM
4. 📄 Генерация Word-документа с описью

## ✨ Возможности

- ✅ Распознавание рукописного и печатного текста
- ✅ Автоматическое определение типа документа
- ✅ Извлечение метаданных (даты, номера дел, количество листов)
- ✅ Автосохранение прогресса (защита от сбоев)
- ✅ Возможность продолжить обработку с места остановки
- ✅ Восстановление данных из логов Kaggle
- ✅ Генерация описи в формате ГОСТ

## 📋 Требования

### Аппаратные требования (все это можно найти на colab/kaggle)
- **GPU**: NVIDIA с поддержкой CUDA (минимум 16GB VRAM)
- **RAM**: 32GB+
- **Хранилище**: 50GB+ свободного места

### Программные требования
- Python 3.11+
- CUDA 11.8+
- PyTorch 2.6.0

## 🚀 Установка

### Загрузите ноутбук .ipynb на kaggle и запустите с x2 T4
### ЛИБО: Установите все перечисленные зависимости из файла и запустите все ячейки

## 📊 Формат выходных данных

### JSON структура записи

```json
{
  "num": 1,
  "index": "02-1193/2/2020",
  "title": "Судебное дело № 02-1193/2/2020 по иску о взыскании задолженности (Договор, Акт, Решение)",
  "date": "2020",
  "pages": "44",
  "storage": "5 лет",
  "note": ""
}
```

### Структура описи (Word)

| № п/п | Индекс дела | Заголовок дела | Дата дела | Срок хранения | Кол-во листов | Прим. |
|-------|-------------|----------------|-----------|---------------|---------------|-------|
| 1 | 02-1193/2/2020 | Судебное дело... | 2020 | 5 лет | 44 | |

## ⚙️ Конфигурация

### Основные параметры (main.py)

```python
# API настройки
API_KEY = "your-api-key"
BASE_URL = "https://openrouter.ai/api/v1"

# Модели
OCR_MODEL = "deepseek-ai/DeepSeek-OCR-2"
LLM_MODEL = "xiaomi/mimo-v2-flash"

# Пути
BATCH_FOLDERS = ["/path/to/photos"]
OUTPUT_FILE = "GOTOVAYA_OPIS.docx"
PROGRESS_FILE = "progress.json"

# OCR параметры
BASE_SIZE = 1024
IMAGE_SIZE = 768
CROP_MODE = True
```

### Настройка LLM промпта

```python
system_prompt = """
Ты — интеллектуальный архивариус...
"""
```

## 🔧 Решение проблем

### Ошибка "CUDA out of memory"

```python
# Добавьте перед обработкой каждого изображения
torch.cuda.empty_cache()
gc.collect()

# Уменьшите размер изображения
base_size = 768  # вместо 1024
image_size = 512  # вместо 768
```

### Ошибка "Package not found at 'template.docx'"

Скрипт создаёт Word программно, шаблон не нужен. Если ошибка возникает — используйте восстановление из JSON.

### Дубликаты в результатах

Логи Kaggle дублируют каждую строку. Используйте `parse_kaggle_log_fixed()`, который учитывает номера дел `[N/260]`.

### Пустые ячейки в Word

Убедитесь, что используете актуальную версию `create_opis_document()` с явным созданием `run` для каждой ячейки.

## 📈 Производительность

| Метрика | Значение |
|---------|----------|
| Скорость OCR | ~5-15 сек/изображение |
| Скорость LLM | ~1-3 сек/запрос |
| Среднее время на дело | ~30-60 сек |
| 260 дел | ~8-9 часов |

<p align="center">
  Сделано с ❤️ для автоматизации архивного дела
</p>

---
<br>

<a id="en"></a>

# 📚 Archive Parser (EN)

> Automated system for creating archive inventories based on OCR and Artificial Intelligence
> **UPDATE:** Added a script for automating single document inventory creation

## 🎯 About

The system automatically processes photos of archive cases/files and creates a structured inventory in Microsoft Word format.
+ there is also a script for processing and creating an inventory of a single document

**Workflow:**
1. 📸 Scanning folders containing document photos
2. 🔍 OCR text recognition using DeepSeek-OCR-2
3. 🤖 Data analysis and structuring using LLM
4. 📄 Generation of a Word document with the inventory

## ✨ Features

- ✅ Recognition of both handwritten and printed text
- ✅ Automatic document type detection
- ✅ Metadata extraction (dates, case numbers, page counts)
- ✅ Autosave progress (crash protection)
- ✅ Ability to resume processing from where it left off
- ✅ Data recovery from Kaggle logs
- ✅ Inventory generation in GOST format

## 📋 Requirements

### Hardware Requirements (available on Colab/Kaggle)
- **GPU**: NVIDIA with CUDA support (minimum 16GB VRAM)
- **RAM**: 32GB+
- **Storage**: 50GB+ free space

### Software Requirements
- Python 3.11+
- CUDA 11.8+
- PyTorch 2.6.0

## 🚀 Installation

### Upload the .ipynb notebook to Kaggle and run with x2 T4
### OR: Install all listed dependencies from the file and run all cells locally

## 📊 Output Format

### JSON Record Structure

```json
{
  "num": 1,
  "index": "02-1193/2/2020",
  "title": "Court case No. 02-1193/2/2020 on debt recovery claim (Contract, Act, Decision)",
  "date": "2020",
  "pages": "44",
  "storage": "5 years",
  "note": ""
}
```

### Inventory Structure (Word)

| No. | Case Index | Case Title | Case Date | Storage Period | Sheets | Note |
|-----|------------|------------|-----------|----------------|--------|------|
| 1 | 02-1193/2/2020 | Court case... | 2020 | 5 years | 44 | |

## ⚙️ Configuration

### Main Parameters (main.py)

```python
# API Settings
API_KEY = "your-api-key"
BASE_URL = "https://openrouter.ai/api/v1"

# Models
OCR_MODEL = "deepseek-ai/DeepSeek-OCR-2"
LLM_MODEL = "xiaomi/mimo-v2-flash"

# Paths
BATCH_FOLDERS = ["/path/to/photos"]
OUTPUT_FILE = "GOTOVAYA_OPIS.docx"
PROGRESS_FILE = "progress.json"

# OCR Parameters
BASE_SIZE = 1024
IMAGE_SIZE = 768
CROP_MODE = True
```

### LLM Prompt Setup

```python
system_prompt = """
You are an intelligent archivist...
"""
```

## 🔧 Troubleshooting

### "CUDA out of memory" Error

```python
# Add before processing each image
torch.cuda.empty_cache()
gc.collect()

# Reduce image size
base_size = 768  # instead of 1024
image_size = 512  # instead of 768
```

### "Package not found at 'template.docx'" Error

The script creates the Word document programmatically; a template is not needed. If this error occurs, use the JSON recovery method.

### Duplicates in Results

Kaggle logs often duplicate lines. Use `parse_kaggle_log_fixed()`, which accounts for case numbers like `[N/260]`.

### Empty Cells in Word

Ensure you are using the updated version of `create_opis_document()` which explicitly creates a `run` for each cell.

## 📈 Performance

| Metric | Value |
|--------|-------|
| OCR Speed | ~5-15 sec/image |
| LLM Speed | ~1-3 sec/request |
| Avg Time per Case | ~30-60 sec |
| 260 Cases | ~8-9 hours |

<p align="center">
  Made with ❤️ for archival automation
</p>

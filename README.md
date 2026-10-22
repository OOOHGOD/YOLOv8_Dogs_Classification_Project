**English** | [简体中文](README.zh-CN.md)

# YOLOv8 Dog-Breed Detection Demo

A coursework application for dog-breed recognition with a YOLOv8 detector, PyQt5 interface, and MySQL prediction history. Although the folder name uses “classification,” the checked-in workflow uses bounding-box labels and the detection API.

**Category:** Artificial Intelligence · Computer Vision  
**Stack:** Python · Ultralytics YOLOv8 · PyQt5 · PyMySQL · MySQL

## Repository guide

Application files are inside **`YOLOv8 _Dogs_classification/`**. The space in this directory name matters.

| File | Purpose |
| --- | --- |
| `dogs_windows.py` | Image upload, confidence controls, predictions, and history |
| `training_model.py` | Training, validation, and experimental export calls |
| `voc_to_yolo.py` | Annotation conversion and dataset preparation |
| `download_models.py` | Pretrained-model download helper |
| `dog_db.sql` | Database/table setup |
| `YOLO_Dataset/data.yaml` | Paths and configured 23 dog-breed labels |
| `trained_models/` | Saved experiment materials |

## Setup and launch

Create and activate a virtual environment, then run:

```bash
python -m pip install ultralytics pymysql PyQt5 requests torch torchvision
cd "YOLOv8 _Dogs_classification"
```

Choose a compatible PyTorch/torchvision build for your hardware. Install/start MySQL, import `dog_db.sql`, and configure `get_db_connection()` in `dogs_windows.py` for your database account.

The GUI expects `trained_models/dog_classification_yolov8n-200epoch/weights/best.pt`. Verify the checkpoint and label mapping, then run from the application directory:

```bash
python dogs_windows.py
```

## Reproduce training

1. Update the Colab-style dataset root in `YOLO_Dataset/data.yaml`.
2. Update the absolute data path in `training_model.py` and verify pretrained weights.
3. Review `classes=23`: this is a class-selection option in the detection API; the class count comes from dataset `nc`/`names`. Adapt it to your framework version.
4. Review the sample test-image path and export calls before running the whole script.

```bash
python training_model.py
```

The script currently selects CPU execution. Configure the device and converter paths for your environment.

## Limitations

This is a locally configured demo. Historical outputs do not establish reproduced accuracy. The former README's dataset attribution is not sufficient to establish provenance and redistribution rights for all images. Verify data/weight permissions separately; Ultralytics licensing is independent of the original-code license.

## Author and license

Author / maintainer: [https://github.com/OOOHGOD](https://github.com/OOOHGOD).

Eligible original contributions are under the [MIT License](LICENSE), permitting commercial use and modification with preservation of copyright and license notices in copies or substantial portions. [License scope](LICENSE_SCOPE.md) explains third-party exclusions and upstream terms.

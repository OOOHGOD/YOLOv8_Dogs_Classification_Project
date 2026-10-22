<div align="center">

# YOLOv8 犬种识别

结合 YOLOv8、PyQt5 与 MySQL 的犬种识别演示。

[English](README.md) · **简体中文**

![YOLOv8](https://img.shields.io/badge/YOLOv8-111827?style=flat-square) ![PyQt5](https://img.shields.io/badge/PyQt5-41CD52?style=flat-square) ![MySQL](https://img.shields.io/badge/MySQL-4479A1?style=flat-square) [![MIT License](https://img.shields.io/badge/License-MIT-2563EB?style=flat-square)](LICENSE)

</div>

<p align="center">
<a href="#项目概览">项目概览</a> · <a href="#快速开始">快速开始</a> · <a href="#训练前检查">训练前检查</a> · <a href="#目录与文件">目录与文件</a> · <a href="#作者与许可">作者与许可</a>
</p>

---

> **项目定位** · 课程实验与 GUI 演示


## 项目概览

结合 YOLOv8、PyQt5 与 MySQL 的犬种识别课程项目，支持图像检测、置信度控制和历史记录查看。

> **实现说明**：目录名使用 Classification，但当前代码采用 Bounding Box 标注与 YOLO Detection API；文档按实际代码说明。

## 目录与文件

应用代码位于 **`YOLOv8 _Dogs_classification/`**，目录名包含空格。

| 文件 | 用途 |
| --- | --- |
| `dogs_windows.py` | 图像上传、检测结果与历史记录 GUI |
| `training_model.py` | 训练、评估与实验性导出 |
| `voc_to_yolo.py` | VOC 标注转换与 Dataset 准备 |
| `download_models.py` | 预训练模型下载辅助脚本 |
| `dog_db.sql` | MySQL 建库与数据表初始化 |
| `YOLO_Dataset/data.yaml` | Dataset 路径与 23 个犬种 Label |
| `trained_models/` | 已保存的实验材料 |

## 快速开始

创建并激活虚拟环境，选择与本机硬件兼容的 PyTorch / torchvision 后安装依赖：

```bash
python -m pip install ultralytics pymysql PyQt5 requests torch torchvision
cd "YOLOv8 _Dogs_classification"
```

启动 MySQL，导入 `dog_db.sql`，并在 `dogs_windows.py` 的 `get_db_connection()` 中配置自己的数据库账号。

GUI 默认读取 `trained_models/dog_classification_yolov8n-200epoch/weights/best.pt`。确认 checkpoint 和 Label 对应关系后运行：

```bash
python dogs_windows.py
```

## 训练前检查

1. 将 `YOLO_Dataset/data.yaml` 中的 Colab 路径改为本机 Dataset 根目录。
2. 调整 `training_model.py` 的绝对路径，并确认预训练权重存在。
3. 检查 `classes=23`：在 Detection API 中它用于筛选类别；类别数由 Dataset 的 `nc` / `names` 定义。
4. 核对示例测试图片和 Export 调用是否适配当前 Ultralytics 版本。
5. 按本机配置调整 Device；当前训练脚本选择 CPU。

```bash
python training_model.py
```

转换标注前同样需要检查 `voc_to_yolo.py` 的路径。

## 复现与授权

项目需要本地配置，历史结果不等于新环境下重新验证的 Accuracy。旧文档中的大学 Dataset 归属说明不足以确认全部图片的来源与再分发权限；图片、权重和 Ultralytics 代码需分别核对授权。


## 历史资料

> 以下折叠内容保留原有课程 / 竞赛材料。旧路径、示例和展示指标以历史版本为背景；运行时请优先采用上方说明。

<details>
<summary>展开原有设计与实验说明</summary>

- **如果对你有帮助的话请在右上角↗️点个小小的start⭐谢谢😘💕**
- 基于 YOLOv8 的犬类品种检测项目，使用哈佛大学狗狗数据集完成训练与验证。

### 环境准备

在运行程序前，请确保以下环境正确配置：

- **操作系统**：Windows、Linux 或 macOS（推荐 Windows，含 PyQt5 GUI 界面）
- **Python 版本**：Python 3.8+
- **依赖库**：安装所需的 Python 第三方库
- **数据库**：MySQL 数据库服务已安装并运行
- **GPU 支持（可选）**：如需加速训练或推理，建议安装 CUDA 和 cuDNN，并确保 GPU 驱动正常

### 安装依赖库

在终端或命令行中运行以下命令安装依赖：

```bash
# 基础环境
pip install ultralytics pymysql PyQt5 requests torch torchvision

# GPU 环境（可选）
pip install torch torchvision torchaudio --index-url https://download.pytorch.org/whl/cu124
```

### 下载预训练模型

运行以下脚本下载预训练模型文件：

```bash
python download_models.py
```

该脚本会将模型文件下载到 `./pre_trained_model` 目录中。

### 准备数据集

#### 标注文件转换

将 PASCAL VOC 格式的 XML 文件转换为 YOLO 格式。运行 `voc_to_yolo.py` 脚本完成转换：

```bash
python voc_to_yolo.py
```

确保输入目录 (`./VOC_Dataset`) 包含 XML 文件，输出目录 (`./YOLO_Dataset`) 将生成对应的 YOLO 格式标签文件。

#### 数据集配置

编辑 `data.yaml` 文件，确保路径指向正确的训练集、验证集和测试集目录。例如：

```yaml
train: ./YOLO_Dataset/Train_YOLO/images
val: ./YOLO_Dataset/Validation_YOLO/images
test: ./YOLO_Dataset/Test_YOLO/images

nc: 23  # 类别数量
names: ['husky', 'beagle', 'chihuahua', ...]  # 类别名称
```

### 训练模型

运行 `training_model.py` 脚本以训练模型：

```bash
python training_model.py
```

如果使用 GPU，请将 `device='cpu'` 修改为 `device='cuda'`。训练完成后，模型权重将保存到 `./trained_models` 目录中。

### 配置 MySQL 数据库

#### 创建数据库

使用 MySQL 客户端工具（如 Navicat 或 MySQL Workbench），执行 `dog_db.sql` 文件以创建数据库和表：

```sql
SOURCE G:/Couresware/DeepLearning_project/YOLOv8 _Dogs_Classification_Project/YOLOv8 _Dogs_classification/dog_db.sql;
```

#### 修改数据库连接信息

打开 `dogs_windows.py` 文件，检查 `get_db_connection` 函数中的数据库连接参数，确保与本地 MySQL 配置一致：

```python
pymysql.connect(
    host='localhost',
    user='root',
    password='123456',
    database='dog_db',
    charset='utf8mb4',
    cursorclass=pymysql.cursors.DictCursor
)
```

### 启动 GUI 应用程序

运行 `dogs_windows.py` 脚本以启动狗品种识别系统的图形界面：

```bash
python dogs_windows.py
```

程序启动后，您可以上传图片进行检测，查看识别结果、历史记录等。

### 测试和验证

#### 测试模型

在训练完成后，使用 `model.val()` 和 `model.predict()` 方法对模型进行验证和测试。示例代码已在 `training_model.py` 中提供。

#### 导出模型

如果需要将模型部署到其他平台，可以导出为 ONNX 或 PyTorch 格式：

```python
model.export(format='onnx', file='./exported_models/dog_classifier.onnx')
model.export(format='pt', file='./exported_models/dog_classifier.pt')
```

</details>

## 作者与许可

作者 / 维护者：[OOOHGOD](https://github.com/OOOHGOD) · 原创贡献：[MIT License](LICENSE)

允许商用和修改；分发软件副本或实质部分时，须保留版权与许可声明。第三方代码、数据、模型和媒体素材保留各自权利，详见 [许可适用范围](LICENSE_SCOPE.md)。

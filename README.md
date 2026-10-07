**电力巡检无人机电力设备绝缘体缺陷检测数据集**
 YOLO txt格式｜3类目标检测｜1600张电力设备绝缘子图片


数据：
1. ​​1600张电力绝缘子图片​​
2. ​​txt格式标注​​，兼容YOLO等主流检测框架
3. ​​3类目标检测

数据集规格：
​​①标注格式​​：txt
​​②类别标签及中文意思​​：
0: broken —— 破损
1: insulator —— 绝缘子（正常）
2: pollution-flashover —— 污闪
​​③数据划分​​：
训练集：1120张（70%）
验证集：320张（20%）
测试集：160张（10%）
​​

![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/eb8b70ad5f17404195078052d75d83cf.png)


---

### ⚡ 绝缘子缺陷检测数据集信息表

| 信息类别             | 具体内容 |
|----------------------|--------|
| **数据集名称**         | 电力绝缘子缺陷检测数据集（Insulator Defect Detection Dataset） |
| **应用场景**           | 电网智能巡检、变电站/输电线路安全监测、预防性维护 |
| **总图像数量**         | 1600 张 |
| **图像类型**           | 高清电力设备特写图像（通常由无人机或巡检机器人拍摄，聚焦绝缘子串） |
| **标注格式**           | **YOLO 格式 `.txt`**<br>每行格式：`class_id center_x center_y width height`（坐标归一化至 [0,1]） |
| **兼容框架**           | YOLOv5 / YOLOv6 / YOLOv7 / YOLOv8 / Ultralytics 等主流目标检测框架 |
| **任务类型**           | 多类别目标检测（3 类） |
| **类别数量**           | 3 类 |
| **类别定义（ID → 中英文）** | |
| &nbsp;&nbsp;`0: broken`            | **破损**（绝缘子瓷件开裂、破碎、缺失） |
| &nbsp;&nbsp;`1: insulator`         | **绝缘子（正常）**（无缺陷的完整绝缘子） |
| &nbsp;&nbsp;`2: pollution-flashover` | **污闪**（表面污秽导致放电痕迹、碳化路径或闪络烧蚀） |
| **数据集划分**         | 按 7:2:1 比例划分 |
| - 训练集（train）      | 1120 张（70%） |
| - 验证集（val）        | 320 张（20%） |
| - 测试集（test）       | 160 张（10%） |
| **典型挑战**           | - 缺陷区域细小（如微裂纹）<br>- 正常与污闪外观相似<br>- 背景复杂（金具、导线、天空）<br>- 光照不均（强反光/阴影） |
| **推荐输入分辨率**     | **1280×1280**（高分辨率有助于捕捉细微缺陷） |
| **数据集亮点**         | ✅ 真实电力场景数据<br>✅ 包含正常与两类典型缺陷<br>✅ 标注规范，可直接用于 YOLO 训练<br>✅ 支持缺陷分类 + 定位一体化 |

---
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/0671333ab0e44e1cadcb2f959fb8c354.png)

### 📁 推荐目录结构

```bash
insulator_defect_dataset/
├── images/
│   ├── train/      # 1120 张 .jpg
│   ├── val/        # 320 张 .jpg
│   └── test/       # 160 张 .jpg
├── labels/
│   ├── train/      # 1120 个 .txt
│   ├── val/        # 320 个 .txt
│   └── test/       # 160 个 .txt
└── dataset.yaml
```

---

### 📄 `dataset.yaml` 示例（YOLOv8）

```yaml
# dataset.yaml
train: ./images/train
val: ./images/val
test: ./images/test

nc: 3
names: ['broken', 'insulator', 'pollution-flashover']
```

> ⚠️ 注意：类别顺序必须与标签文件中的 `class_id` 严格一致。

---

该数据集适用于构建 **智能电网视觉诊断系统**，可有效识别绝缘子健康状态，预防因破损或污闪引发的跳闸、断电等重大事故，提升电网运行可靠性。
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/7bcf751774f143ee8bf7fad23be2dfcb.png)
1
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/06ca08c9d1b54d30b70f553e0bdfab0a.png)
1
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/2ef5282997094b93a8a19a9c09b48570.png)
 **绝缘子缺陷检测数据集**（1600 张电力设备图像，3 类目标：`broken`、`insulator`、`pollution-flashover`，YOLO txt 格式）的完整 **YOLOv8 训练系统代码**，包含训练、验证、推理和部署全流程。

---

### ✅ 一、前提条件

- 数据已按以下结构组织：
  ```bash
  insulator_defect_dataset/
  ├── images/train/     # 1120 张 .jpg
  ├── images/val/       # 320 张 .jpg
  ├── images/test/      # 160 张 .jpg
  ├── labels/train/     # 对应 .txt
  ├── labels/val/
  ├── labels/test/
  └── dataset.yaml
  ```
- 已安装依赖：
  ```bash
  pip install ultralytics opencv-python torch torchvision
  ```

---

### 📄 二、`dataset.yaml` 配置文件

```yaml
# dataset.yaml
train: ./images/train
val: ./images/val
test: ./images/test

nc: 3
names: ['broken', 'insulator', 'pollution-flashover']
```

> ⚠️ 确保 `.txt` 标签中类别 ID 为 `0`、`1`、`2`，分别对应上述顺序。

---

### 🚀 三、YOLOv8 训练代码（`train.py`）

```python
# train.py
from ultralytics import YOLO
import torch

def main():
    # 自动选择设备
    device = 'cuda' if torch.cuda.is_available() else 'cpu'
    print(f"🚀 Using device: {device}")

    # 加载预训练模型（推荐 yolov8s，平衡速度与精度）
    model = YOLO('yolov8s.pt')  # 可选: yolov8n (更快), yolov8m (更准)

    # 开始训练
    results = model.train(
        data='dataset.yaml',                # 数据配置文件路径
        epochs=200,                         # 小样本建议 150~200 轮
        imgsz=1280,                         # 高分辨率捕捉细微缺陷（如裂纹、污闪痕迹）
        batch=8,                            # 根据 GPU 显存调整（1280 分辨率下 batch=8 较安全）
        name='insulator_defect_v8s',
        optimizer='AdamW',
        lr0=0.001,
        lrf=0.01,
        weight_decay=0.0005,
        warmup_epochs=3,
        # 数据增强（针对电力设备优化）
        hsv_h=0.015,                        # 色调（适应不同光照/天气）
        hsv_s=0.7,
        hsv_v=0.4,
        degrees=15.0,                       # 旋转（模拟无人机多角度拍摄）
        translate=0.1,
        scale=0.5,
        fliplr=0.5,                         # 左右翻转（绝缘子对称）
        mosaic=0.9,                         # Mosaic 增强（提升小缺陷泛化能力）
        mixup=0.2,
        copy_paste=0.4,                     # Copy-Paste（将缺陷粘贴到正常绝缘子上，极有效！）
        close_mosaic=10,                    # 最后10轮关闭 Mosaic
        device=device,
        workers=4,
        save=True,
        save_period=10,
        exist_ok=False,
        verbose=True
    )

if __name__ == '__main__':
    main()
```

> 💡 **为什么用 `copy_paste=0.4`？**  
> 绝缘子缺陷（尤其是 `broken` 和 `pollution-flashover`）样本可能较少，Copy-Paste 可合成更多缺陷样本，显著提升召回率。

---

### 🔍 四、推理与可视化（`detect.py`）

```python
# detect.py
from ultralytics import YOLO
import cv2

# 加载最佳模型
model = YOLO('runs/detect/insulator_defect_v8s/weights/best.pt')

def detect_image(image_path, conf_thres=0.3):
    """检测单张图像并显示结果"""
    results = model(image_path, conf=conf_thres)
    annotated = results[0].plot()  # 自动绘制框 + 标签（含类别名和置信度）
    cv2.imshow("Insulator Defect Detection", annotated)
    cv2.waitKey(0)
    cv2.destroyAllWindows()

def detect_folder(input_folder, output_folder="output"):
    """批量检测文件夹并保存结果"""
    model.predict(
        source=input_folder,
        conf=0.3,
        save=True,
        project=output_folder,
        name="results",
        exist_ok=True
    )
    print(f"✅ 检测结果已保存至: {output_folder}/results/")

# 示例使用
detect_image("test_insulator.jpg")
# detect_folder("test_images/")
```

---

### 📊 五、测试集评估（`evaluate.py`）

```python
# evaluate.py
from ultralytics import YOLO

model = YOLO('runs/detect/insulator_defect_v8s/weights/best.pt')
metrics = model.val(data='dataset.yaml', split='test')

print("="*60)
print("📊 测试集性能报告 (Test Set Performance):")
print(f"mAP@0.5 (Box):      {metrics.box.map50:.4f}")
print(f"mAP@0.5:0.95:       {metrics.box.map:.4f}")
print(f"Precision:          {metrics.box.mp:.4f}")
print(f"Recall:             {metrics.box.mr:.4f}")
print("="*60)

# 按类别输出 AP（分析哪类缺陷难检）
class_names = ['broken', 'insulator', 'pollution-flashover']
for i, name in enumerate(class_names):
    ap50 = metrics.box.ap50[i] if hasattr(metrics.box, 'ap50') else 0.0
    print(f"{name:<20}: AP@0.5 = {ap50:.4f}")

# 保存评估结果
with open("evaluation_results.txt", "w") as f:
    f.write(f"mAP@0.5: {metrics.box.map50:.4f}\n")
    f.write(f"mAP@0.5:0.95: {metrics.box.map:.4f}\n")
    for i, name in enumerate(class_names):
        ap50 = metrics.box.ap50[i] if hasattr(metrics.box, 'ap50') else 0.0
        f.write(f"{name}: AP@0.5 = {ap50:.4f}\n")
print("📝 评估结果已保存至 evaluation_results.txt")
```

---

### 🛠️ 六、关键优化建议（针对绝缘子缺陷）

| 挑战 | 解决方案 |
|------|--------|
| **缺陷细小（裂纹/污闪）** | ✅ 使用 `imgsz=1280`<br>✅ 启用 `copy_paste` 增强 |
| **正常 vs 污闪混淆** | ✅ 增强 `hsv_s`（饱和度）突出污秽颜色<br>✅ 使用更高精度模型（如 `yolov8m`） |
| **样本不平衡** | 检查各类别数量，若 `broken` < 300，可过采样或增加 `copy_paste` 概率 |
| **漏检破损绝缘子** | ✅ 降低推理阈值（`conf=0.2`）<br>✅ 后处理保留低分但高 IoU 的预测 |

---

### 📦 七、模型导出（用于部署）

```python
# export_model.py
from ultralytics import YOLO

model = YOLO('runs/detect/insulator_defect_v8s/weights/best.pt')

# 导出为 ONNX（用于 CPU/Web）
model.export(format='onnx', imgsz=1280, simplify=True)

# 导出为 TensorRT（用于 Jetson/NVIDIA GPU）
model.export(format='engine', imgsz=1280, half=True)  # FP16 加速
```

---

### ✅ 总结

该系统专为 **电力绝缘子智能巡检** 设计，具备：
- 高精度缺陷识别（mAP@0.5 通常 >85%）
- 强鲁棒性（应对复杂背景与光照）
- 快速部署能力（支持边缘设备）

> 📌 **典型应用流程**：  
> 无人机巡检 → 实时检测 → 自动标记缺陷类型 → 生成工单 → 运维人员精准维修

---

=

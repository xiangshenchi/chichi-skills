---
name: yolo-report-analyst
description: >
  自动分析 YOLO 目标检测模型训练结果并输出完整的专业文档报告。当用户上传或提供 YOLO 训练输出内容时必须激活此 skill——包括 results.csv、args.yaml、训练日志（.log/.md/.txt）、混淆矩阵、PR 曲线图、loss 曲线图等任意组合。触发场景包括但不限于：用户说"分析我的训练结果"、"帮我生成训练报告"、"看看这次训练效果怎么样"、"YOLO 训练完了帮我总结一下"、"给我输出文档"，或直接上传了 results.csv / args.yaml / 训练 zip 包 / 训练日志文件。即使用户只提供了部分文件，也应当使用此 skill 尽可能完整地分析。
---

# YOLO 训练结果分析 Skill

本 skill 将 YOLO 训练输出（日志、CSV、YAML、图片）转化为一份完整的专业分析报告，涵盖配置解析、性能评估、训练过程分析、问题诊断与改进建议。

---

## 一、数据收集清单

激活此 skill 后，首先盘点用户已提供了哪些文件，按优先级获取：

| 优先级 | 文件 | 用途 |
|--------|------|------|
| ★★★ | `results.csv` | 每个 epoch 的训练/验证 loss 和 mAP 指标 |
| ★★★ | 训练日志（.md/.log/.txt） | 完整训练过程、最终验证指标、per-class 结果 |
| ★★ | `args.yaml` | 超参数与训练配置 |
| ★★ | `confusion_matrix_normalized.png` | 类别混淆情况 |
| ★★ | `results.png` | 综合训练曲线图 |
| ★ | `BoxP_curve.png` / `BoxR_curve.png` / `BoxF1_curve.png` / `BoxPR_curve.png` | P/R/F1/PR 曲线 |
| ★ | `val_batch*_pred.jpg` / `val_batch*_labels.jpg` | 验证集预测可视化 |
| ★ | `train_batch*.jpg` | 训练数据样本 |
| ★ | `labels.jpg` | 标签分布 |

如果是 zip 文件，先解压后再分析。如果文件不完整，根据现有信息尽量输出，并在报告中注明缺失项。

---

## 二、数据解析步骤

### 2.1 解析 results.csv

使用 Python 解析 CSV，提取关键趋势：

```python
import csv, ast

# 读取所有行，关注这些字段：
# epoch, train/box_loss, train/cls_loss, train/dfl_loss,
# metrics/precision(B), metrics/recall(B), metrics/mAP50(B), metrics/mAP50-95(B),
# val/box_loss, val/cls_loss, val/dfl_loss

# 找到最佳 epoch（以 mAP50 最高为准）
# 计算收敛情况：最后 10 个 epoch 的 mAP50 方差
# 检测 loss 是否出现发散或震荡
```

需要提取的关键数值：
- 最佳 epoch 编号、对应 mAP50 和 mAP50-95
- 最终 epoch（last）的各指标
- 训练 loss 和验证 loss 的下降趋势（是否过拟合）
- Precision 和 Recall 的平衡情况
- 收敛稳定性（后期方差）

### 2.2 解析训练日志

从日志中提取：
- 模型架构（层数、参数量、GFLOPs）
- 数据集规模（train/val 图片数量、instances 数量）
- 优化器实际配置（`optimizer:` 行）
- 总训练耗时（`epochs completed in X hours`）
- **最终验证 per-class 结果**（最重要！格式如下）：
  ```
  Class     Images  Instances      Box(P     R      mAP50  mAP50-95)
  all          144        490      0.909  0.881   0.912     0.635
  insulator    144        164      0.997      1   0.995     0.968
  flashover     68        230      0.853  0.757   0.836     0.367
  broken        75         96      0.878  0.885   0.904     0.569
  ```

### 2.3 解析 args.yaml

提取用户配置与默认配置的差异点，重点关注：
- `model`（基础模型版本）
- `epochs`、`batch`、`imgsz`
- `optimizer`、`lr0`、`lrf`、`momentum`、`weight_decay`
- `patience`（早停）
- `device`（单卡/多卡）
- 数据增强参数（mosaic, flipud, fliplr, hsv_*, scale 等）

---

## 三、报告结构（必须按此顺序输出）

生成的报告分为以下 7 个部分，使用 Markdown 格式，适合直接转为 .md 文件：

---

### 📋 Part 1：训练配置概览

用一张表格汇总核心配置，无需解释每个参数，仅列出：

```
| 配置项 | 值 |
|--------|----|
| 模型 | YOLOv11n |
| 数据集 | train: 1296 images / val: 144 images |
| 类别 | insulator / flashover / broken |
| 训练轮次 | 150 epochs |
| 输入尺寸 | 640×640 |
| batch size | 32 |
| 优化器 | AdamW (lr=0.001429, momentum=0.9) |
| 设备 | GPU (Tesla T4 / 双卡等) |
| 总耗时 | X 小时 |
| AMP | 开启 |
| Patience | 30 |
```

---

### 📊 Part 2：最终性能指标

#### 2.1 整体指标（best model）

| 指标 | 值 | 说明 |
|------|----|------|
| mAP@50 | 0.912 | IoU=0.5 时的平均精度 |
| mAP@50-95 | 0.635 | IoU 从 0.5 到 0.95 的平均值，严格版本 |
| Precision | 0.909 | 在所有预测为正例中的准确率 |
| Recall | 0.881 | 在所有真实正例中的检出率 |

**性能等级评价**（根据数值给出定性判断）：
- mAP50 ≥ 0.90：**优秀**（工业可用级）
- mAP50 0.75~0.90：**良好**（需针对性优化）
- mAP50 < 0.75：**待提升**

#### 2.2 Per-class 性能分析

用表格列出每个类别的 P/R/mAP50/mAP50-95，并附简短分析：
- 哪个类别表现最好？原因可能是什么（样本多、形态简单等）？
- 哪个类别是短板？Precision 低 vs. Recall 低分别意味着什么？
- 类别间不平衡对整体指标的影响

---

### 📈 Part 3：训练过程分析

从 results.csv 提取关键节点分析：

#### 3.1 Loss 曲线分析

分析 3 条训练 loss（box/cls/dfl）和 3 条验证 loss：
- 整体下降趋势是否稳健？
- 是否存在**过拟合**信号（val_loss 在某 epoch 后上升而 train_loss 继续下降）？
- 是否存在**震荡**（loss 在相邻 epoch 大幅波动）？
- 三种 loss 中哪一项下降最慢？这意味着什么？

#### 3.2 mAP 收敛分析

- 最佳 mAP50 出现在第几个 epoch？
- 最后 10 个 epoch 的 mAP50 方差（稳定性评估）
- 如果最佳 epoch 远早于最终 epoch，说明什么？

#### 3.3 关键 epoch 节点（选 5~7 个有代表性的节点展示）

| Epoch | mAP50 | mAP50-95 | train_loss | val_loss | 备注 |
|-------|-------|----------|------------|----------|------|
| 1 | ... | ... | ... | ... | 初始化 |
| ... | | | | | 快速收敛阶段 |
| ... | | | | | 接近峰值 |
| best | ... | ... | ... | ... | 最佳模型 |
| 150 | ... | ... | ... | ... | 最终 |

---

### 🔍 Part 4：混淆矩阵解读

如果用户提供了 `confusion_matrix_normalized.png`，描述并分析：
- 对角线主导程度（整体分类准确性）
- 最高误分类对（哪两个类最容易互相混淆）
- 背景误报情况（FP：将背景预测为目标）
- 漏检情况（FN：将目标预测为背景）

如未提供图片，跳过此部分并提示用户。

---

### ⚡ Part 5：模型效率分析

从日志中提取速度数据：

| 阶段 | 耗时 |
|------|------|
| 预处理 | X ms/image |
| 推理 | X ms/image |
| 后处理（NMS） | X ms/image |
| **理论 FPS** | **约 X fps** |

评估：是否满足实时检测需求（>30fps）？轻量模型与精度的权衡是否合理？

---

### 🩺 Part 6：问题诊断 & 改进建议

这是报告最有价值的部分。根据前面的数据，给出**有针对性的**建议，不要泛泛而谈。

结构：每条建议必须包含 **"观察到的问题" → "可能原因" → "具体改进方案"**

常见诊断模板（按实际情况选用）：

**【精度不足类】**
- 如果 mAP50-95 明显低于 mAP50（差距 > 0.3）：
  → 定位精度不足，建议：增大 imgsz（640→1280）、调整 anchor、加强 box loss 权重
- 如果某类 Recall 低（< 0.7）：
  → 漏检严重，建议：该类数据增强（复制粘贴 copy_paste）、降低置信度阈值
- 如果某类 Precision 低（< 0.7）：
  → 误报多，建议：增加该类负样本、提高 NMS iou 阈值

**【训练过程类】**
- 如果存在过拟合信号：
  → 建议：增加 dropout、提高 weight_decay、引入更强的数据增强、减少 epoch
- 如果 loss 震荡严重：
  → 建议：降低 lr0、使用 cos_lr=True、增大 batch size
- 如果最佳 epoch 很早（< 总 epoch 的 50%）：
  → patience 可能设置过大，或者学习率衰减不足

**【数据类】**
- 如果类别样本数量差异 > 5:1：
  → 类别不平衡，建议：过采样少数类、调整 cls_loss 权重
- 如果验证集结果与训练集差距大：
  → 数据分布不匹配，建议：检查 val 集代表性，考虑交叉验证

**【模型选型类】**
- 如果使用 yolo11n/yolov8n 且精度已达瓶颈：
  → 建议升级至 medium 或 large 变体（yolo11m/yolo11l）
- 如果部署环境对速度敏感：
  → 评估精度损失，考虑模型蒸馏或量化（int8）

---

### 📌 Part 7：总结 & 下一步行动计划

用简洁的语言总结：
1. 本次训练的核心成果（几句话）
2. 主要优势（什么做得好）
3. 主要瓶颈（当前最值得优化的 1~2 个点）
4. **下一步行动（3 条以内，优先级排序）**

示例格式：
```
✅ 成果：模型在绝缘子检测任务上达到 mAP50=91.2%，insulator 类性能接近完美（mAP50=99.5%）。

⚠️ 瓶颈：
1. flashover 类 Recall 仅 75.7%，漏检率偏高
2. mAP50-95 为 0.635，定位精度仍有提升空间

🎯 下一步（优先级排序）：
1. [高优] 对 flashover 类增加数据增强（copy_paste），重训一次
2. [中优] 尝试 imgsz=1280 提升定位精度，对比 mAP50-95 变化
3. [低优] 评估 yolo11s/m 替换 yolo11n，看精度/速度权衡
```

---

## 四、输出格式规范

- 默认输出 **Markdown 格式**，方便复制到论文、报告或 Notion
- 如果用户要求 Word 文档，调用 `docx` skill 生成 .docx 文件
- 所有数字保留 3 位小数（百分比形式时保留 1 位小数）
- 表格必须对齐，标题使用 emoji 以便快速定位
- 报告语言跟随用户语言（中文问就中文输出，英文问就英文输出）

---

## 五、缺失数据处理

| 缺失文件 | 降级策略 |
|----------|----------|
| 无 results.csv | 仅依赖日志，跳过 Part 3 的逐 epoch 分析，基于日志的关键 epoch 做简化分析 |
| 无训练日志 | 跳过 per-class 分析、模型架构描述，重点分析 CSV |
| 无 args.yaml | 从日志中解析参数，或标注"配置未提供" |
| 无图片文件 | 跳过 Part 4，其余部分照常输出 |
| 仅有图片 | 对图片内容进行视觉描述分析（曲线走势、混淆矩阵颜色分布等） |

始终输出能输出的部分，不要因为缺少某个文件就放弃整个报告。

---

## 六、快速入门示例

用户说："帮我分析训练结果，我传了 results.csv 和日志"

1. 读取 results.csv → 提取最佳 epoch、loss 趋势
2. 读取日志 → 提取 per-class 指标、模型结构、耗时
3. 按照 Part 1→7 顺序生成报告
4. 在报告末尾询问："需要我生成 Word 版本，或者展开某个部分的分析吗？"

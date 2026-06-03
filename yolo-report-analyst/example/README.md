# 示例文件说明

本目录包含基于真实 YOLOv11n 绝缘子缺陷检测训练输出的示例文件。

## 目录结构

```
examples/
├── sample-input/              # 示例输入文件
│   ├── results.csv            # 训练指标 CSV（关键 epoch 子集）
│   ├── args.yaml              # 训练配置 YAML
│   ├── train_log.txt          # 训练日志（关键 epoch 摘要）
│   └── images/                # 训练输出图片
│       ├── confusion_matrix_normalized.png
│       ├── results.png
│       ├── labels.jpg
│       ├── BoxPR_curve.png
│       └── BoxF1_curve.png
│
└── sample-output/             # 示例输出报告
    └── sample_report.md       # 完整分析报告
```

## 数据集信息

- **任务**：绝缘子缺陷检测（Insulator Defect Detection）
- **模型**：YOLOv11n（2.59M parameters, 6.4 GFLOPs）
- **类别**：insulator（1481）/ flashover（1993）/ broken（867）
- **训练集**：1296 images | **验证集**：144 images
- **训练轮次**：150 epochs | **耗时**：4.351 hours
- **设备**：Tesla T4

## 示例输入文件说明

### results.csv
包含关键 epoch 的训练/验证 loss 和 mAP 指标（epoch 1, 5, 10, 25, 50, 75, 100, 129, 150）。

### args.yaml
完整的训练超参数配置，包含模型、学习率、数据增强参数等。

### train_log.txt
训练日志摘要，包含模型结构、数据集信息、关键 epoch 指标和最终 per-class 验证结果。

### images/
训练过程中生成的可视化图片，用于混淆矩阵和曲线分析。

## 示例输出报告

`sample_report.md` 展示了基于上述输入文件生成的完整分析报告，包含 7 个部分：

1. 📋 训练配置概览
2. 📊 最终性能指标（整体 + per-class）
3. 📈 训练过程分析（loss 曲线 + mAP 收敛）
4. 🔍 混淆矩阵解读
5. ⚡ 模型效率分析
6. 🩺 问题诊断 & 改进建议
7. 📌 总结 & 下一步行动计划

## 使用方法

1. 将你的 YOLO 训练输出文件上传到工作目录
2. 触发 Skill："分析我的训练结果"
3. 获取生成的分析报告

---

**注意**：示例数据来自真实训练输出，仅供演示报告格式和 Skill 能力。

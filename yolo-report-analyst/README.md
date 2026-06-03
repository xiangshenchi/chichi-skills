# YOLO Report Analyst

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![PRs Welcome](@SKILLS/yolo-report-analyst/CONTRIBUTING.md)
[English](README_EN.md)

**自动分析 YOLO 目标检测模型训练结果，一键生成专业文档报告**

## 简介

YOLO Report Analyst 是一个专为 YOLO 训练结果分析设计的 Skill。它能自动解析训练输出文件（日志、CSV、YAML、图片等），生成包含配置解析、性能评估、训练过程分析、问题诊断与改进建议的完整专业报告。

## 功能特性

- **多格式支持**：解析 `results.csv`、`args.yaml`、训练日志、混淆矩阵、PR 曲线等
- **智能分析**：自动识别最佳 epoch、检测过拟合信号、分析 loss 收敛情况
- **Per-class 分析**：逐类别性能评估，精准定位短板类别
- **问题诊断**：基于指标自动生成针对性的改进建议
- **多输出格式**：支持 Markdown 和 Word 文档输出
- **容错处理**：即使文件不完整也能输出可用报告

## 支持的输入文件

| 优先级 | 文件 | 用途 |
|--------|------|------|
| ★★★ | `results.csv` | 每个 epoch 的训练/验证 loss 和 mAP 指标 |
| ★★★ | 训练日志（.md/.log/.txt） | 完整训练过程、最终验证指标、per-class 结果 |
| ★★ | `args.yaml` | 超参数与训练配置 |
| ★★ | `confusion_matrix_normalized.png` | 类别混淆情况 |
| ★★ | `results.png` | 综合训练曲线图 |
| ★ | `BoxP_curve.png` / `BoxR_curve.png` 等 | P/R/F1/PR 曲线 |
| ★ | `val_batch*_pred.jpg` 等 | 验证集预测可视化 |

## 报告结构

生成的报告包含 7 个核心部分：

1. **训练配置概览** - 模型、数据集、超参数汇总
2. **最终性能指标** - mAP、Precision、Recall 及 per-class 分析
3. **训练过程分析** - Loss 曲线、收敛稳定性、关键节点
4. **混淆矩阵解读** - 误分类分析、漏检/误报情况
5. **模型效率分析** - 推理速度、FPS 评估
6. **问题诊断 & 改进建议** - 针对性优化方案
7. **总结 & 下一步行动计划** - 核心成果与优先行动项

## 快速开始

### 前置要求

- 支持 Skill 系统的 AI 助手（如 SOLO）
- YOLO 训练输出文件

### 使用方法

1. **上传训练结果文件**

   将你的 YOLO 训练输出文件上传或放置在工作目录中。

2. **触发 Skill**

   使用以下任意方式触发：
   - 直接说："分析我的训练结果"
   - 说："帮我生成训练报告"
   - 上传 `results.csv` 或训练日志文件

3. **获取报告**

   Skill 会自动解析文件并生成完整的分析报告。

### 示例

```
用户：帮我分析训练结果，我传了 results.csv 和日志

AI：
1. 读取 results.csv → 提取最佳 epoch、loss 趋势
2. 读取日志 → 提取 per-class 指标、模型结构、耗时
3. 按照 Part 1→7 顺序生成报告
4. 在报告末尾询问："需要我生成 Word 版本，或者展开某个部分的分析吗？"
```

## 项目结构

```
yolo-report-analyst/
├── README.md               # 项目说明（中文）
├── README_EN.md            # Project documentation (English)
├── LICENSE                 # MIT 许可证
├── CONTRIBUTING.md         # 贡献指南
├── CHANGELOG.md            # 版本变更记录
├── SKILL.md                # Skill 定义文件
├── .gitignore              # Git 忽略规则
└── examples/               # 示例文件
    ├── sample-input/       # 示例输入
    └── sample-output/      # 示例输出报告
```

## 贡献

欢迎贡献代码、报告问题或提出建议！请阅读 [贡献指南](@SKILLS/yolo-report-analyst/CONTRIBUTING.md) 了解详情。

### 贡献方式

- 提交 Issue 报告 Bug 或提出 Feature Request
- 提交 Pull Request 改进代码或文档
- 分享你的使用案例和反馈

## 许可证

本项目采用 [MIT 许可证](LICENSE) 开源。

## 致谢

- 感谢 [Ultralytics YOLO](https://github.com/ultralytics/ultralytics) 提供的优秀目标检测框架
- 感谢所有贡献者的支持

---

**Made with ❤️ for the YOLO community**

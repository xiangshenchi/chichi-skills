# YOLO Report Analyst

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![PRs Welcome](@SKILLS/yolo-report-analyst/CONTRIBUTING.md)
[中文](@SKILLS/yolo-report-analyst/README.md)

**Automatically analyze YOLO training results and generate professional reports with one click**

## Overview

YOLO Report Analyst is a Skill designed for YOLO training result analysis. It automatically parses training output files (logs, CSV, YAML, images, etc.) and generates comprehensive professional reports covering configuration analysis, performance evaluation, training process analysis, problem diagnosis, and improvement recommendations.

## Features

- **Multi-format Support**: Parse `results.csv`, `args.yaml`, training logs, confusion matrices, PR curves, etc.
- **Intelligent Analysis**: Automatically identify the best epoch, detect overfitting signals, and analyze loss convergence
- **Per-class Analysis**: Class-by-class performance evaluation to pinpoint weak categories
- **Problem Diagnosis**: Automatically generate targeted improvement suggestions based on metrics
- **Multiple Output Formats**: Support Markdown and Word document output
- **Fault Tolerance**: Generate usable reports even with incomplete files

## Supported Input Files

| Priority | File | Purpose |
|----------|------|---------|
| ★★★ | `results.csv` | Per-epoch training/validation loss and mAP metrics |
| ★★★ | Training log (.md/.log/.txt) | Full training process, final validation metrics, per-class results |
| ★★ | `args.yaml` | Hyperparameters and training configuration |
| ★★ | `confusion_matrix_normalized.png` | Class confusion analysis |
| ★★ | `results.png` | Comprehensive training curves |
| ★ | `BoxP_curve.png` / `BoxR_curve.png` etc. | P/R/F1/PR curves |
| ★ | `val_batch*_pred.jpg` etc. | Validation prediction visualization |

## Report Structure

The generated report contains 7 core sections:

1. **Training Configuration Overview** - Model, dataset, hyperparameter summary
2. **Final Performance Metrics** - mAP, Precision, Recall, and per-class analysis
3. **Training Process Analysis** - Loss curves, convergence stability, key milestones
4. **Confusion Matrix Interpretation** - Misclassification analysis, false positives/negatives
5. **Model Efficiency Analysis** - Inference speed, FPS evaluation
6. **Problem Diagnosis & Improvement Suggestions** - Targeted optimization plans
7. **Summary & Next Steps** - Key findings and prioritized action items

## Quick Start

### Prerequisites

- An AI assistant that supports the Skill system (e.g., SOLO)
- YOLO training output files

### Usage

1. **Upload Training Result Files**

   Upload or place your YOLO training output files in the working directory.

2. **Trigger the Skill**

   Use any of the following methods:
   - Say: "Analyze my training results"
   - Say: "Generate a training report for me"
   - Upload `results.csv` or a training log file

3. **Get Your Report**

   The Skill will automatically parse the files and generate a complete analysis report.

### Example

```
User: Analyze my training results, I've uploaded results.csv and the log

AI:
1. Read results.csv → Extract best epoch, loss trends
2. Read log → Extract per-class metrics, model architecture, time elapsed
3. Generate report following Part 1→7 structure
4. Ask: "Would you like a Word version, or should I expand on any section?"
```

## Project Structure

```
yolo-report-analyst/
├── README.md               # 项目说明（中文）
├── README_EN.md            # Project documentation (English)
├── LICENSE                 # MIT License
├── CONTRIBUTING.md         # Contribution guide
├── CHANGELOG.md            # Changelog
├── SKILL.md                # Skill definition file
├── .gitignore              # Git ignore rules
└── examples/               # Example files
    ├── sample-input/       # Sample input
    └── sample-output/      # Sample output report
```

## Contributing

Contributions are welcome! Please read the [Contribution Guide](@SKILLS/yolo-report-analyst/CONTRIBUTING.md) for details.

### Ways to Contribute

- Submit an Issue to report a bug or request a feature
- Submit a Pull Request to improve code or documentation
- Share your use cases and feedback

## License

This project is licensed under the [MIT License](LICENSE).

## Acknowledgements

- Thanks to [Ultralytics YOLO](https://github.com/ultralytics/ultralytics) for the excellent object detection framework
- Thanks to all contributors for their support

---

**Made with ❤️ for the YOLO community**

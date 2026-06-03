# 贡献指南

感谢你对 YOLO Report Analyst 项目的关注！我们欢迎所有形式的贡献。

## 贡献方式

### 报告问题 (Bug Report)

如果你发现了 Bug，请通过 [GitHub Issues](../../issues) 提交，并包含以下信息：

- **问题描述**：清晰简洁地描述问题
- **复现步骤**：如何触发这个问题
- **期望行为**：你期望发生什么
- **实际行为**：实际发生了什么
- **环境信息**：使用的 YOLO 版本、文件类型等
- **截图/日志**：如果有的话，请附上相关截图或日志

### 功能建议 (Feature Request)

我们欢迎新功能建议！请在 Issue 中详细描述：

- **功能描述**：你希望实现什么功能
- **使用场景**：这个功能解决什么问题
- **实现思路**：如果你有想法，可以简要描述

### 提交代码 (Pull Request)

#### 开发流程

1. **Fork 本仓库**

2. **克隆你的 Fork**
   ```bash
   git clone https://github.com/YOUR_USERNAME/yolo-report-analyst.git
   cd yolo-report-analyst
   ```

3. **创建分支**
   ```bash
   git checkout -b feature/your-feature-name
   # 或
   git checkout -b fix/your-bug-fix
   ```

4. **进行修改**
   - 遵循现有的代码风格
   - 确保修改有充分的测试

5. **提交更改**
   ```bash
   git add .
   git commit -m "type: 简短描述"
   ```

   提交信息格式：
   - `feat:` 新功能
   - `fix:` Bug 修复
   - `docs:` 文档更新
   - `refactor:` 代码重构
   - `test:` 测试相关
   - `chore:` 其他改动

6. **推送到 Fork**
   ```bash
   git push origin feature/your-feature-name
   ```

7. **创建 Pull Request**
   - 在 GitHub 上创建 PR
   - 填写 PR 模板中的所有必填项
   - 等待审核

#### 代码规范

- **Markdown 文件**：使用标准 Markdown 格式，确保表格对齐
- **YAML 文件**：使用 2 空格缩进
- **Python 代码**（如果有）：遵循 PEP 8 规范

#### 文档贡献

文档改进同样重要！你可以：

- 修正拼写或语法错误
- 改进文档结构
- 添加更多示例
- 翻译文档

## 项目结构

```
yolo-report-analyst/
├── README.md           # 项目说明
├── SKILL.md            # Skill 核心定义（主要文件）
├── CONTRIBUTING.md     # 贡献指南
├── CHANGELOG.md        # 版本变更
├── LICENSE             # 许可证
└── examples/           # 示例文件
```

## 审核流程

1. 维护者会尽快审核你的 PR
2. 可能会提出修改建议
3. 修改完成后，维护者会合并你的 PR

## 行为准则

- 尊重所有贡献者
- 保持建设性的讨论
- 接受不同观点和批评

## 联系方式

如有任何问题，可以通过以下方式联系：

- 提交 [GitHub Issue](../../issues)
- 参与 [GitHub Discussions](../../discussions)（如果已启用）

---

再次感谢你的贡献！ 🎉

# 参与贡献 / Contributing

感谢你对本项目的关注！本项目是一个 Claude Skill，贡献内容以 Markdown 为主，无需任何构建工具。

---

## 贡献方式

### 🌐 新增语言 / 框架支持

最有价值的贡献是为新技术栈补充测试策略。

编辑 `repo-test-analyst/references/testing-strategies.md`，按如下格式添加新章节：

```markdown
## <语言名>

### 测试框架选型

| 框架 | 适用场景 | 安装 |
|------|---------|------|
| `框架名` | 描述 | `安装命令` |

### 静态分析工具
...

### 标准测试文件模板（<语言名>）
\`\`\`<lang>
// 测试模板
\`\`\`
```

**当前缺少覆盖（适合新手入手）：**
- Ruby（RSpec / Minitest）
- PHP（PHPUnit / Pest）
- C# / .NET（xUnit / NUnit）
- Swift / Kotlin（移动端）
- Elixir（ExUnit）

---

### 📄 新增或改进报告模板

编辑 `repo-test-analyst/references/report-templates.md` 来新增报告类型或改进现有模板。

可以考虑的方向：
- `report-architecture.md` — 架构图与模块耦合分析
- `report-accessibility.md` — 前端项目无障碍审计
- `report-database.md` — 数据库 Schema 质量、N+1 风险、缺失索引
- `report-ci.md` — CI/CD 流水线质量检查

---

### 🐛 修复 SKILL.md 中的问题

如果 Skill 的指令存在歧义、输出格式不对或遗漏了某些边界情况，欢迎直接提 Issue 或 PR，修改 `repo-test-analyst/SKILL.md`。

---

## 如何提交 PR

1. Fork 本仓库
2. 创建分支：`git checkout -b feat/add-ruby-support`
3. 完成修改
4. 提交 Pull Request，请包含：
   - 清晰描述你做了什么改动
   - 一个示例提示词 + 对应输出，展示改进效果（粘贴文本即可，无需截图）

---

## 测试你的修改

本项目是 Claude Skill，"测试"的方式是在 Claude 环境中加载修改后的 Skill 并验证输出：

1. 将修改后的 Skill 安装到你的 Claude 环境
2. 使用 `examples/` 目录中的示例提示词（如有），或自己编写
3. 检查输出格式和内容是否符合预期
4. 将输出结果粘贴到 PR 描述中

---

## 风格规范

- **语言**：SKILL.md 及 references/ 使用中文；README 双语；Issue 模板双语
- **表格优于列表**：结构化对比信息优先用 Markdown 表格
- **代码块**：始终标注语言标识符（` ```python `、` ```bash ` 等）
- **Emoji**：在标题和状态图标中适度使用，保持与现有文件一致的风格

---

## 行为准则 / Code of Conduct

请友善待人。本项目遵循 [Contributor Covenant](https://www.contributor-covenant.org/version/2/1/code_of_conduct/)。

---

## Contributing (English)

Thank you for your interest! This is a Claude Skill project — contributions are primarily Markdown-based, no build toolchain required.

**Ways to contribute:**
- Add language/framework support in `references/testing-strategies.md`
- Add new report templates in `references/report-templates.md`
- Fix ambiguous instructions in `SKILL.md`

**To submit a PR:** fork → branch → change → open PR with a before/after example output.

**Missing coverage (good first issues):** Ruby, PHP, C#/.NET, Swift, Kotlin, Elixir.

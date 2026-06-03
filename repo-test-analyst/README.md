# 🧪 repo-test-analyst

**一个 Claude Skill，自动分析你的项目仓库，选择合适的测试策略，并生成全面的测试分析报告。**

[中文](#中文) | [English](#english)

---

## 中文

### 功能介绍

将项目仓库（zip 包、文件夹或 GitHub 链接）交给 Claude，Skill 会自动完成：

1. **探索** 项目目录结构
2. **识别** 语言、框架与架构类型（Python / JS / TS / Java / Go / Rust…）
3. **制定** 最合适的测试策略（单元测试、集成测试、E2E、安全测试、性能测试…）
4. **执行** 静态分析、安全扫描、依赖审计
5. **输出** 测试代码 + 多份结构化 Markdown 报告

### 生成的报告类型

| 报告 | 生成时机 |
|------|---------|
| `report-summary.md` — 整体健康度评分与关键发现 | 必须生成 |
| `report-quality.md` — 复杂度、可维护性、代码规范 | 必须生成 |
| `report-security.md` — OWASP Top 10、硬编码凭证、CVE | 发现风险时 |
| `report-dependencies.md` — 过期/有漏洞的依赖包 | 依赖较多时 |
| `report-api.md` — REST/GraphQL 接口覆盖情况 | 检测到 API 时 |
| `report-testcases.md` — 生成的测试套件说明文档 | 生成测试代码时 |

### 安装方式

1. 克隆或下载本仓库
2. 将 `repo-test-analyst/` 文件夹放入你的 Claude skills 目录：
   ```
   /mnt/skills/user/repo-test-analyst/
   ```
3. Claude 会自动检测并启用该 Skill

### 使用方式

直接与 Claude 对话：

```
"帮我分析这个项目，生成测试报告"
"给这个仓库做代码质量检查"
"这个代码有什么安全问题？"
"帮我给 Python 项目生成单元测试"
```

或上传 zip 包后直接说：**"帮我测试一下"**

### 支持的技术栈

| 语言 | 框架 | 测试工具 |
|------|------|---------|
| Python | FastAPI、Django、Flask | pytest、bandit、radon |
| JavaScript | React、Vue、Express | Jest、Vitest、ESLint |
| TypeScript | Next.js、NestJS | Jest、tsc、Playwright |
| Java | Spring Boot | JUnit 5、Mockito、Testcontainers |
| Go | 标准库 / Gin / Echo | testing、testify、staticcheck |
| Rust | Axum / Actix | cargo test、clippy |

### 文件结构

```
repo-test-analyst/
├── SKILL.md                        # 主流程指令
└── references/
    ├── testing-strategies.md       # 各语言工具选型与测试代码模板
    └── report-templates.md         # 各报告的 Markdown 模板
```

---

## English

### What it does

Drop in a repository (zip, folder, or GitHub URL), and the skill will:

1. **Explore** the project structure
2. **Detect** languages, frameworks, and architecture
3. **Select** the most appropriate testing methods
4. **Run** static analysis, security scans, and dependency audits
5. **Generate** test code stubs and multiple structured Markdown reports

### Installation

1. Clone or download this repository
2. Copy the `repo-test-analyst/` folder into your Claude skills directory:
   ```
   /mnt/skills/user/repo-test-analyst/
   ```
3. Claude will automatically detect and use the skill

### Usage

```
"Analyze this project and generate a test report"
"Help me test this codebase"
"What security issues does this code have?"
```

---

## 参与贡献

详见 [CONTRIBUTING.md](@SKILLS/repo-test-analyst/CONTRIBUTING.md)。

## 开源协议

MIT — 详见 [LICENSE](./LICENSE)。

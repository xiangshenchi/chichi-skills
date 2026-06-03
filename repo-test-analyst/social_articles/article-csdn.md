# 我开源了一个 Claude Skill：丢进去一个仓库，自动帮你做代码测试分析并输出报告

> **一句话介绍**：`repo-test-analyst` 是一个 Claude Skill，把项目仓库扔给它，它会自动识别技术栈、选择合适的测试策略、执行静态分析，最终输出一套结构化的测试报告。

---

## 背景：为什么做这个

接手老项目、评审第三方代码、快速了解一个开源库的质量——这些场景有个共同的痛点：**要真正搞清楚一个仓库的质量，得花大量时间手动跑工具、翻文档、整理结论**。

不同语言有不同的工具链，Python 用 bandit/pylint，Node.js 用 ESLint/npm audit，Java 用 SpotBugs/OWASP……光是记住该用哪些工具就已经够烦了，更别说最后还要把结果汇总成报告。

于是我做了这个 Skill，把"识别技术栈 → 选测试工具 → 执行分析 → 输出报告"这条流水线整合起来，交给 Claude 自动完成。

---

## 它能做什么

### 五个阶段，全自动流转

```
仓库探索 → 技术识别 → 策略制定 → 测试执行 → 报告生成
```

**阶段一：探索仓库**

支持三种输入方式：
- 上传 zip 包
- 提供 GitHub 仓库链接
- 直接给本地路径

**阶段二：自动识别技术栈**

通过特征文件判断语言与框架：

| 检测文件 | 判断结果 |
|---------|---------|
| `pyproject.toml` / `requirements.txt` | Python |
| `package.json` + `tsconfig.json` | TypeScript |
| `pom.xml` / `build.gradle` | Java |
| `go.mod` | Go |
| `docker-compose.yml` + 多服务 | 微服务架构 |
| `packages/` / `apps/` | Monorepo |

**阶段三：制定测试策略**

根据项目类型选择合适的测试方向：

| 项目类型 | 必选 | 推荐 |
|---------|------|------|
| Web API | 单元测试、接口测试 | 集成测试、安全测试 |
| 前端 SPA | 组件测试、E2E | 单元测试、快照测试 |
| 微服务 | 单元测试、契约测试 | 集成测试 |
| SDK/库 | 单元测试、API 测试 | 兼容性测试 |

**阶段四：执行分析**

- **静态分析**：pylint / ESLint / tsc / go vet
- **安全扫描**：bandit（Python）/ npm audit（Node.js）
- **复杂度分析**：radon / 圈复杂度 / 认知复杂度
- **依赖审计**：pip-audit / npm audit
- **生成测试代码**：根据公开接口自动生成测试用例

**阶段五：输出报告**

| 报告文件 | 内容 | 触发条件 |
|---------|------|---------|
| `report-summary.md` | 整体评分 + Top 发现 | 必须 |
| `report-quality.md` | 复杂度、可维护性、代码规范 | 必须 |
| `report-security.md` | OWASP Top 10、CVE、硬编码凭证 | 发现风险 |
| `report-dependencies.md` | 过期/有漏洞依赖 | 依赖较多 |
| `report-api.md` | 接口覆盖情况 | 检测到 API |
| `report-testcases.md` | 生成的测试套件文档 | 已生成测试 |

每份报告都包含维度评分（1–10 分）和优先级建议。

---

## 快速上手

### 安装

1. 下载仓库中的 `repo-test-analyst/` 文件夹
2. 放入 Claude skills 目录：

```bash
cp -r repo-test-analyst/ /mnt/skills/user/
```

### 使用

装好后直接对 Claude 说：

```
帮我分析这个项目，生成测试报告
```

```
给这个仓库做一次安全扫描
```

```
帮我生成 Python 单元测试
```

上传 zip 包后说"帮我测试一下"也能触发。

---

## 支持的技术栈

| 语言 | 测试框架 | 静态分析 | 安全工具 |
|------|---------|---------|---------|
| Python | pytest / hypothesis | pylint / radon | bandit / pip-audit |
| JavaScript | Jest / Vitest | ESLint | npm audit |
| TypeScript | Jest / Playwright | tsc / ESLint | npm audit |
| Java | JUnit 5 / Mockito | SpotBugs | OWASP DC |
| Go | testing / testify | go vet / staticcheck | gosec |

---

## Skill 文件结构

```
repo-test-analyst/
├── SKILL.md                        # 主流程指令（五阶段工作流）
└── references/
    ├── testing-strategies.md       # 各语言工具选型 + 测试代码模板
    └── report-templates.md         # 六种报告的完整 Markdown 模板
```

Skill 采用渐进式加载：`SKILL.md` 常驻上下文，references 按需读取，不浪费 token。

---

## 开源地址

GitHub：[repo-test-analyst](https://github.com/xiangshenchi/repo-test-analyst)

欢迎 PR，当前最需要的贡献是补充 Ruby、PHP、C#、Kotlin 的测试策略模板。

---

## 后续计划

- [ ] 架构图自动生成（`report-architecture.md`）
- [ ] 数据库 Schema 质量检查（N+1 问题、缺失索引）
- [ ] CI/CD 流水线质量报告
- [ ] 支持 Rust、Swift、Elixir

---

如果你也在用 Claude Skill 做自动化工作流，欢迎交流。

---
name: repo-test-analyst
description: |
  自动分析项目仓库并生成全面的软件测试报告。当用户上传项目仓库、代码文件或提及"测试"、"分析项目"、"测试报告"、"代码质量"等需求时，必须立即激活此 skill。
  
  触发场景（遇到以下任意情况请立即调用）：
  - 用户说"帮我分析这个项目/仓库"
  - 用户说"给这个项目生成测试"、"帮我测试代码"
  - 用户说"输出测试报告"、"代码质量分析"
  - 用户上传了代码文件、zip 包、或提供了 GitHub 仓库路径
  - 用户说"这个项目有什么问题"、"帮我检查代码"
  - 用户提到"单元测试"、"集成测试"、"覆盖率"、"安全漏洞"、"性能测试"
  - 只要涉及对一个完整项目/代码库进行任何形式的测试或质量分析，无论措辞多随意，都应激活此 skill
---

# Repo Test Analyst

通过深度分析项目仓库，自动识别技术栈、选择最合适的测试策略，生成完整的测试代码与多维度分析报告。

---

## 总体工作流程

```
1. 仓库探索 → 2. 技术识别 → 3. 策略制定 → 4. 测试执行 → 5. 报告生成
```

每个阶段都有对应的操作指南，**必须按顺序执行**，不可跳过。

---

## 阶段一：仓库探索

### 1.1 定位仓库

首先确认仓库来源：

| 来源类型 | 处理方式 |
|----------|----------|
| 用户上传的 zip | `unzip` 到 `/tmp/repo/` |
| 用户上传的文件夹 | 直接分析 `/mnt/user-data/uploads/` |
| GitHub URL | `git clone <url> /tmp/repo/` |
| 本地路径 | 直接使用用户指定路径 |

```bash
# 标准探索命令序列
find /tmp/repo -type f | head -100          # 文件列表
find /tmp/repo -name "*.json" -o -name "*.toml" -o -name "*.yaml" | xargs ls -la
cat /tmp/repo/README.md 2>/dev/null || echo "无 README"
```

### 1.2 结构分析清单

运行探索后，必须确认以下信息：

- [ ] 目录结构（深度 ≤ 3 层的树形图）
- [ ] 所有配置文件（package.json / pyproject.toml / pom.xml / go.mod / Cargo.toml 等）
- [ ] 现有测试文件（`*test*` / `*spec*` 目录）
- [ ] CI/CD 配置（`.github/` / `.gitlab-ci.yml` / `Jenkinsfile`）
- [ ] 文档文件（README / CHANGELOG / docs/）

---

## 阶段二：技术栈识别

### 2.1 语言与框架检测

根据文件特征自动判断：

```
Python     → requirements.txt / pyproject.toml / setup.py / *.py
JavaScript → package.json / node_modules / *.js / *.ts
TypeScript → tsconfig.json / *.ts
Java       → pom.xml / build.gradle / *.java
Go         → go.mod / go.sum / *.go
Rust       → Cargo.toml / *.rs
PHP        → composer.json / *.php
Ruby       → Gemfile / *.rb
C#/.NET    → *.csproj / *.sln / *.cs
```

### 2.2 架构模式识别

| 架构特征文件 | 判断类型 |
|-------------|---------|
| `docker-compose.yml` + 多服务 | 微服务架构 |
| 单一 `src/` 入口 | Monolith（单体）|
| `packages/` / `apps/` / `libs/` | Monorepo |
| `lambda_handler` / `serverless.yml` | Serverless |
| `frontend/` + `backend/` 分离 | 前后端分离 |
| GraphQL schema / REST routes | API 服务 |

### 2.3 现有测试覆盖评估

```bash
# 统计现有测试文件数量和覆盖率
find /tmp/repo -path "*/test*" -o -path "*/spec*" | wc -l
grep -r "def test_\|it(\|describe(\|@Test\|func Test" /tmp/repo --include="*.py" --include="*.js" --include="*.ts" --include="*.java" --include="*.go" | wc -l
```

---

## 阶段三：测试策略制定

**根据技术栈，读取对应的测试策略文档：**
→ `references/testing-strategies.md`（包含各语言/框架的详细测试工具选型）

### 3.1 策略选择矩阵

根据项目类型，优先选择以下测试方向：

| 项目类型 | 必须 | 推荐 | 可选 |
|---------|------|------|------|
| Web API | 单元测试、接口测试 | 集成测试、安全测试 | 性能测试、契约测试 |
| 前端 SPA | 组件测试、E2E 测试 | 单元测试、快照测试 | 视觉回归测试 |
| 微服务 | 单元测试、契约测试 | 集成测试、混沌测试 | 服务网格测试 |
| CLI 工具 | 单元测试、功能测试 | 集成测试 | 兼容性测试 |
| 数据处理 | 单元测试、数据校验 | 性能测试 | 边界值测试 |
| 移动端 | 单元测试、UI 测试 | E2E 测试 | 设备兼容性 |
| SDK/库 | 单元测试、API 测试 | 兼容性测试 | 文档测试 |

### 3.2 测试深度决策

- **快速扫描模式**（用户说"快速看看"）：只输出报告，不生成测试代码
- **标准分析模式**（默认）：报告 + 关键测试用例代码
- **完整测试模式**（用户说"完整测试"）：报告 + 完整测试套件 + 可运行脚本

---

## 阶段四：测试执行与分析

### 4.1 静态分析

无需运行代码，直接分析：

```bash
# Python 静态分析示例
pip install pylint radon bandit --break-system-packages -q
pylint /tmp/repo/src --output-format=json > /tmp/pylint_report.json
radon cc /tmp/repo/src -j > /tmp/complexity_report.json
bandit -r /tmp/repo/src -f json > /tmp/security_report.json

# JavaScript / TypeScript
npx eslint /tmp/repo/src --format json > /tmp/eslint_report.json
```

### 4.2 依赖安全扫描

```bash
# Python
pip-audit --format json > /tmp/dep_audit.json 2>/dev/null

# Node.js
npm audit --json > /tmp/npm_audit.json 2>/dev/null

# 通用：检查已知风险包
grep -r "eval(\|exec(\|__import__\|subprocess.call" /tmp/repo/src 2>/dev/null
```

### 4.3 代码复杂度分析

关注以下指标：
- **圈复杂度（Cyclomatic Complexity）**：> 10 需重构
- **认知复杂度（Cognitive Complexity）**：> 15 警告
- **代码重复率（Duplication）**：> 5% 需关注
- **函数行数**：> 50 行建议拆分
- **文件行数**：> 300 行建议模块化

### 4.4 生成测试代码

根据识别出的模块，为关键函数生成测试用例：

1. 识别**公开接口**（public functions / API endpoints / exported modules）
2. 分析**输入输出类型**
3. 设计**等价类划分**（正常值 / 边界值 / 异常值）
4. 生成**测试代码文件**（保存到 `/mnt/user-data/outputs/tests/`）

---

## 阶段五：报告生成

**根据分析结果，生成以下报告（保存到 `/mnt/user-data/outputs/`）：**

→ 报告模板详见 `references/report-templates.md`

### 5.1 必须生成的报告

| 报告名称 | 文件名 | 内容 |
|---------|--------|------|
| 综合分析报告 | `report-summary.md` | 整体评分、关键发现、优先建议 |
| 代码质量报告 | `report-quality.md` | 复杂度、可维护性、规范性 |

### 5.2 按需生成的报告

| 报告名称 | 文件名 | 触发条件 |
|---------|--------|---------|
| 安全漏洞报告 | `report-security.md` | 发现安全风险时 |
| 测试覆盖报告 | `report-coverage.md` | 有现有测试时 |
| 依赖风险报告 | `report-dependencies.md` | 依赖较多时 |
| 性能分析报告 | `report-performance.md` | 有性能敏感逻辑时 |
| API 接口报告 | `report-api.md` | 包含 REST/GraphQL API 时 |
| 测试用例文档 | `report-testcases.md` | 生成了测试代码时 |

### 5.3 报告评分体系

每份报告必须包含维度评分（1-10 分）：

```
🔴 1-3 分：严重问题，需立即处理
🟠 4-5 分：明显缺陷，建议优先改进
🟡 6-7 分：基本可用，有改进空间
🟢 8-9 分：良好，小幅优化即可
⭐ 10 分：优秀，可作为参考标准
```

---

## 输出规范

### 对话中的汇报格式

分析完成后，在对话中输出**执行摘要**：

```
## 🔍 项目分析完成

**项目**：[项目名称]
**技术栈**：[识别出的主要语言/框架]
**架构类型**：[单体/微服务/前后端分离等]
**分析深度**：[快速/标准/完整]

### 📊 整体健康度：[X]/10

| 维度 | 评分 | 状态 |
|------|------|------|
| 代码质量 | X/10 | 🟢/🟡/🟠/🔴 |
| 安全性 | X/10 | ... |
| 可测试性 | X/10 | ... |
| 依赖健康 | X/10 | ... |

### 🚨 关键发现（Top 3）
1. ...
2. ...
3. ...

### 📄 已生成报告
- [report-summary.md] 综合分析报告
- [report-quality.md] 代码质量详情
- ...（其他生成的报告）

### ✅ 已生成测试文件
- tests/test_*.py / *.test.ts（按需）
```

---

## 重要注意事项

1. **不要执行危险命令**：不运行 `rm -rf`、不修改仓库原始文件
2. **隐私保护**：遇到 `.env`、密钥文件，只报告"存在硬编码凭证风险"，不输出实际内容
3. **大仓库处理**：文件数 > 1000 时，重点分析核心模块，说明已采样
4. **失败降级**：某个分析工具不可用时，跳过并在报告中注明"未安装 xxx，已跳过"
5. **中文优先**：所有报告默认使用中文输出，代码注释保持原语言

---

## 参考文件

- `references/testing-strategies.md` — 各语言/框架的测试工具选型与配置
- `references/report-templates.md` — 各类报告的完整 Markdown 模板

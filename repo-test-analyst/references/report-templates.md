# 报告模板集合

## 报告 1：综合分析报告（report-summary.md）

```markdown
# 📊 项目综合分析报告

> 项目：{project_name}  
> 生成时间：{timestamp}  
> 分析工具：Repo Test Analyst

---

## 一、项目概览

| 属性 | 详情 |
|------|------|
| 技术栈 | {languages} |
| 架构类型 | {architecture} |
| 总文件数 | {file_count} |
| 代码行数 | {loc} |
| 现有测试数 | {existing_tests} |
| 分析耗时 | {duration} |

---

## 二、整体健康度评分

```
总分：{total_score}/10  {status_icon}
```

| 维度 | 评分 | 状态 | 说明 |
|------|------|------|------|
| 代码质量 | {quality_score}/10 | {icon} | {quality_summary} |
| 安全性 | {security_score}/10 | {icon} | {security_summary} |
| 可测试性 | {testability_score}/10 | {icon} | {testability_summary} |
| 依赖健康度 | {dependency_score}/10 | {icon} | {dep_summary} |
| 文档完善度 | {doc_score}/10 | {icon} | {doc_summary} |

---

## 三、关键发现

### 🔴 严重问题（需立即处理）
{critical_issues}

### 🟠 重要警告（建议优先改进）
{major_warnings}

### 🟡 优化建议（可在迭代中改进）
{minor_suggestions}

---

## 四、测试覆盖现状

- 当前覆盖率：{coverage}%
- 未覆盖的关键模块：{uncovered_modules}
- 推荐目标覆盖率：≥ 80%

---

## 五、改进路线图

### 短期（1-2 周）
1. {action_1}
2. {action_2}

### 中期（1 个月）
1. {action_3}

### 长期（季度级）
1. {action_4}

---

## 六、生成的产物

| 文件 | 说明 |
|------|------|
| report-quality.md | 代码质量详细报告 |
| report-security.md | 安全漏洞报告（如有） |
| tests/ | 生成的测试代码（如有） |

---

*本报告由 Repo Test Analyst Skill 自动生成*
```

---

## 报告 2：代码质量报告（report-quality.md）

```markdown
# 🧹 代码质量分析报告

> 项目：{project_name} | 生成时间：{timestamp}

---

## 一、复杂度分析

### 圈复杂度（Cyclomatic Complexity）

| 文件/函数 | 复杂度 | 风险等级 | 建议 |
|----------|--------|---------|------|
| {file_1} | {cc_1} | 🔴 高 | 建议拆分 |
| {file_2} | {cc_2} | 🟡 中 | 可接受 |

**整体分布：**
- 低复杂度（1-5）：{low_count} 个函数
- 中复杂度（6-10）：{mid_count} 个函数
- 高复杂度（>10）：{high_count} 个函数 ⚠️

### 认知复杂度（Cognitive Complexity）

{cognitive_complexity_findings}

---

## 二、代码规范检查

### 问题汇总

| 类别 | 数量 | 严重程度 |
|------|------|---------|
| 命名不规范 | {naming_count} | 🟡 低 |
| 注释缺失 | {comment_count} | 🟡 低 |
| 函数过长（>50行）| {long_func_count} | 🟠 中 |
| 文件过长（>300行）| {long_file_count} | 🟠 中 |
| 魔法数字 | {magic_num_count} | 🟡 低 |
| 代码重复 | {duplication_count} | 🟠 中 |

### 典型问题示例

```
{code_smell_example_1}
```

**建议重构为：**

```
{refactored_example_1}
```

---

## 三、可维护性指数

| 文件 | MI 分数 | 评级 |
|------|---------|------|
| {file} | {mi_score} | {grade} |

> MI 评级：A（85-100）优秀 / B（65-84）良好 / C（0-64）需改进

---

## 四、技术债务估算

- 预计清理工时：约 {debt_hours} 小时
- 主要来源：{debt_sources}
- 优先清理项：{priority_debt}

---

## 五、改进建议

1. **立即处理**：{urgent_fix}
2. **下一迭代**：{next_sprint_fix}
3. **长期规划**：{long_term_fix}
```

---

## 报告 3：安全漏洞报告（report-security.md）

```markdown
# 🔒 安全漏洞分析报告

> ⚠️ 本报告含敏感信息，请勿公开分享

> 项目：{project_name} | 生成时间：{timestamp}

---

## 一、风险概览

```
安全评分：{security_score}/10
```

| 风险等级 | 数量 |
|---------|------|
| 🔴 严重（CVSS ≥ 9.0）| {critical_count} |
| 🟠 高危（CVSS 7-8.9）| {high_count} |
| 🟡 中危（CVSS 4-6.9）| {medium_count} |
| 🔵 低危（CVSS < 4）| {low_count} |

---

## 二、漏洞详情

### [严重] {vuln_title_1}

| 属性 | 详情 |
|------|------|
| 位置 | `{file}:{line}` |
| 类型 | {vuln_type} |
| OWASP | {owasp_category} |
| 影响 | {impact} |

**问题代码：**
```
{vulnerable_code}
```

**修复建议：**
```
{fix_suggestion}
```

---

## 三、依赖安全

### 存在已知漏洞的依赖

| 依赖包 | 当前版本 | 漏洞 CVE | 修复版本 |
|--------|---------|---------|---------|
| {pkg} | {version} | {cve} | {fix_version} |

---

## 四、配置安全检查

| 检查项 | 状态 | 说明 |
|--------|------|------|
| 调试模式关闭 | ✅/❌ | {detail} |
| HTTPS 强制 | ✅/❌ | {detail} |
| 密钥未硬编码 | ✅/❌ | {detail} |
| CORS 配置合理 | ✅/❌ | {detail} |
| 输入验证完整 | ✅/❌ | {detail} |

---

## 五、修复优先级

1. 🔴 **立即修复**：{critical_fix}
2. 🟠 **本周修复**：{high_fix}
3. 🟡 **本月修复**：{medium_fix}
```

---

## 报告 4：依赖风险报告（report-dependencies.md）

```markdown
# 📦 依赖分析报告

> 项目：{project_name} | 生成时间：{timestamp}

---

## 一、依赖概览

- 直接依赖：{direct_count} 个
- 间接依赖：{transitive_count} 个
- 过期依赖：{outdated_count} 个（{outdated_percent}%）
- 有漏洞依赖：{vuln_count} 个

---

## 二、过期依赖清单

| 包名 | 当前版本 | 最新版本 | 落后程度 | 建议 |
|------|---------|---------|---------|------|
| {pkg} | {current} | {latest} | 主版本 | 🔴 重要更新 |
| {pkg} | {current} | {latest} | 次版本 | 🟡 建议更新 |

---

## 三、依赖健康度

| 维度 | 评分 |
|------|------|
| 版本新鲜度 | {freshness_score}/10 |
| 安全状态 | {security_score}/10 |
| 维护活跃度 | {maintenance_score}/10 |

---

## 四、更新命令

```bash
# Python
pip list --outdated
pip install --upgrade {packages}

# Node.js
npx npm-check-updates -u
npm install

# Java/Maven
mvn versions:display-dependency-updates
```
```

---

## 报告 5：测试用例文档（report-testcases.md）

```markdown
# 🧪 测试用例说明文档

> 项目：{project_name} | 生成时间：{timestamp}

---

## 一、测试策略说明

- 测试框架：{framework}
- 测试类型：{test_types}
- 预期覆盖率目标：{target_coverage}%

---

## 二、测试文件清单

| 文件 | 对应模块 | 测试数量 | 覆盖功能 |
|------|---------|---------|---------|
| `tests/test_{module}.py` | `src/{module}.py` | {count} | {functions} |

---

## 三、测试用例说明

### 模块：{module_name}

#### 正常场景测试

| 用例 ID | 输入 | 预期输出 | 测试函数 |
|---------|------|---------|---------|
| TC001 | {input} | {output} | `test_{name}` |

#### 边界值测试

| 用例 ID | 边界条件 | 预期行为 |
|---------|---------|---------|
| TC010 | 空输入 | 返回默认值 / 抛出异常 |

#### 异常场景测试

| 用例 ID | 异常输入 | 预期异常 |
|---------|---------|---------|
| TC020 | `null` | `ValueError` |

---

## 四、运行指南

```bash
# 安装依赖
{install_commands}

# 运行所有测试
{run_all_command}

# 运行并查看覆盖率
{run_coverage_command}

# 查看 HTML 覆盖率报告
{open_coverage_report}
```

---

## 五、持续集成配置建议

```yaml
# .github/workflows/test.yml 示例
name: Test
on: [push, pull_request]
jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - name: Run Tests
        run: {run_command}
      - name: Upload Coverage
        uses: codecov/codecov-action@v3
```
```

---

## 报告 6：API 接口报告（report-api.md）

```markdown
# 🌐 API 接口测试报告

> 项目：{project_name} | 生成时间：{timestamp}

---

## 一、API 概览

- 接口总数：{endpoint_count}
- 已测试：{tested_count}
- 未覆盖：{untested_count}

---

## 二、接口清单与测试状态

| 方法 | 路径 | 功能 | 认证 | 测试状态 |
|------|------|------|------|---------|
| GET | `/api/users` | 获取用户列表 | ✅ | ✅ 已覆盖 |
| POST | `/api/users` | 创建用户 | ✅ | ❌ 未覆盖 |

---

## 三、接口测试用例

### GET /api/{resource}

```
正常场景：返回 200 + 数据列表
分页场景：page=1&size=10 返回正确数量
鉴权缺失：返回 401
参数错误：返回 400 + 错误信息
```

---

## 四、接口质量问题

| 问题 | 接口 | 严重程度 | 建议 |
|------|------|---------|------|
| 无输入验证 | POST /api/users | 🟠 高 | 添加 schema 校验 |
| 无分页限制 | GET /api/logs | 🟠 高 | 添加 limit 参数 |
| 错误信息过于详细 | ALL | 🟡 中 | 隐藏内部堆栈 |
```

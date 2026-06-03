# 各语言/框架测试策略与工具选型

## Python

### 测试框架选型

| 框架 | 适用场景 | 安装 |
|------|---------|------|
| `pytest` | 通用（首选） | `pip install pytest` |
| `unittest` | 标准库，无需安装 | 内置 |
| `pytest-asyncio` | 异步代码（FastAPI/aiohttp） | `pip install pytest-asyncio` |
| `hypothesis` | 属性测试（自动生成边界数据） | `pip install hypothesis` |

### Web 框架配套工具

| 框架 | 测试工具 | 配置示例 |
|------|---------|---------|
| FastAPI | `httpx` + `pytest` | `from fastapi.testclient import TestClient` |
| Django | `django.test.TestCase` | `python manage.py test` |
| Flask | `flask.testing.FlaskClient` | `app.test_client()` |

### 覆盖率工具

```bash
pip install pytest-cov --break-system-packages
pytest --cov=src --cov-report=html --cov-report=term-missing
```

### 静态分析工具

```bash
# 代码风格
pip install flake8 black pylint --break-system-packages
flake8 src/ --format=json

# 类型检查
pip install mypy --break-system-packages
mypy src/ --json-report /tmp/mypy_report

# 安全扫描
pip install bandit --break-system-packages
bandit -r src/ -f json -o /tmp/bandit_report.json

# 复杂度
pip install radon --break-system-packages
radon cc src/ -j -s > /tmp/complexity.json
radon mi src/ -j > /tmp/maintainability.json
```

### 标准测试文件模板（Python）

```python
# tests/test_<module>.py
import pytest
from src.<module> import <TargetClass>

class Test<TargetClass>:
    """<TargetClass> 的测试套件"""
    
    @pytest.fixture
    def instance(self):
        """创建测试实例"""
        return <TargetClass>()
    
    # --- 正常场景 ---
    def test_normal_case(self, instance):
        result = instance.method(valid_input)
        assert result == expected_output
    
    # --- 边界值 ---
    def test_boundary_empty(self, instance):
        result = instance.method("")
        assert result is not None
    
    def test_boundary_max(self, instance):
        result = instance.method("x" * 10000)
        assert len(result) <= MAX_LENGTH
    
    # --- 异常场景 ---
    def test_invalid_input_raises(self, instance):
        with pytest.raises(ValueError, match="invalid"):
            instance.method(None)
    
    # --- 属性测试（hypothesis） ---
    from hypothesis import given, strategies as st
    
    @given(st.text(min_size=1))
    def test_property_idempotent(self, instance, s):
        assert instance.normalize(instance.normalize(s)) == instance.normalize(s)
```

---

## JavaScript / TypeScript

### 测试框架选型

| 框架 | 适用场景 |
|------|---------|
| `Jest` | 通用首选（React/Node/Vue） |
| `Vitest` | Vite 项目（更快） |
| `Mocha + Chai` | 传统 Node.js 项目 |
| `Playwright` | E2E 浏览器自动化 |
| `Cypress` | 前端 E2E（更易上手） |
| `Testing Library` | 组件测试（React/Vue/Angular） |
| `Supertest` | Express/Koa API 测试 |

### 框架检测与对应工具

```
检测到 "react" in package.json     → Jest + @testing-library/react
检测到 "vue" in package.json       → Vitest + @testing-library/vue
检测到 "express" / "koa"           → Jest + Supertest
检测到 "vite" in package.json      → Vitest（优先于 Jest）
检测到 "next" in package.json      → Jest + @testing-library/react
检测到 "angular" in package.json   → Jasmine + Karma（内置）
```

### 静态分析工具

```bash
# ESLint
npx eslint src/ --format json --output-file /tmp/eslint_report.json

# TypeScript 类型检查
npx tsc --noEmit --pretty false 2> /tmp/tsc_errors.txt

# 依赖审计
npm audit --json > /tmp/npm_audit.json
```

### Jest 配置示例

```json
// jest.config.js
{
  "testEnvironment": "node",
  "collectCoverageFrom": ["src/**/*.{js,ts}", "!src/**/*.d.ts"],
  "coverageThreshold": {
    "global": { "branches": 70, "functions": 80, "lines": 80 }
  }
}
```

### 标准测试文件模板（TypeScript/Jest）

```typescript
// tests/<module>.test.ts
import { <TargetClass> } from '../src/<module>';

describe('<TargetClass>', () => {
  let instance: <TargetClass>;
  
  beforeEach(() => {
    instance = new <TargetClass>();
  });
  
  afterEach(() => {
    jest.clearAllMocks();
  });
  
  describe('method()', () => {
    it('正常输入返回预期结果', () => {
      expect(instance.method(validInput)).toEqual(expectedOutput);
    });
    
    it('空输入不抛出异常', () => {
      expect(() => instance.method('')).not.toThrow();
    });
    
    it('非法输入抛出 Error', () => {
      expect(() => instance.method(null as any)).toThrow('invalid');
    });
  });
  
  describe('async operations', () => {
    it('异步方法正常返回', async () => {
      const result = await instance.asyncMethod();
      expect(result).toBeDefined();
    });
  });
});
```

---

## Java

### 测试框架选型

| 框架/工具 | 用途 |
|----------|------|
| JUnit 5 | 标准单元测试 |
| Mockito | Mock 依赖 |
| Spring Boot Test | Spring 集成测试 |
| RestAssured | REST API 测试 |
| Testcontainers | 数据库/中间件集成测试 |
| ArchUnit | 架构约束测试 |

### Maven 配置检测

```xml
<!-- 检测 pom.xml 中的依赖，确认已有测试框架 -->
<!-- 如无 junit-jupiter，建议添加 -->
<dependency>
  <groupId>org.junit.jupiter</groupId>
  <artifactId>junit-jupiter</artifactId>
  <scope>test</scope>
</dependency>
```

### 标准测试模板（Java/JUnit 5）

```java
@ExtendWith(MockitoExtension.class)
class TargetServiceTest {
    
    @Mock
    private DependencyRepository repository;
    
    @InjectMocks
    private TargetService service;
    
    @Test
    @DisplayName("正常场景：有效输入返回正确结果")
    void givenValidInput_whenMethod_thenReturnsExpected() {
        // Arrange
        when(repository.findById(1L)).thenReturn(Optional.of(new Entity()));
        // Act
        Result result = service.process(1L);
        // Assert
        assertThat(result).isNotNull();
        assertThat(result.getStatus()).isEqualTo(Status.SUCCESS);
    }
    
    @Test
    @DisplayName("异常场景：空输入抛出异常")
    void givenNullInput_whenMethod_thenThrowsException() {
        assertThrows(IllegalArgumentException.class, () -> service.process(null));
    }
    
    @ParameterizedTest
    @ValueSource(longs = {-1L, 0L, Long.MAX_VALUE})
    @DisplayName("边界值：无效 ID 处理")
    void givenInvalidId_whenProcess_thenHandlesGracefully(long id) {
        // ...
    }
}
```

---

## Go

### 测试工具

| 工具 | 用途 |
|------|------|
| `testing` 标准库 | 单元测试（内置） |
| `testify` | 断言库（推荐） |
| `gomock` | Mock 生成 |
| `httptest` | HTTP handler 测试 |
| `go-fuzz` / `fuzzing` | 模糊测试（Go 1.18+）|

### 分析命令

```bash
go test ./... -cover -coverprofile=coverage.out
go tool cover -html=coverage.out -o /tmp/coverage.html
go vet ./...
staticcheck ./...
```

### 标准测试模板（Go）

```go
package mypackage_test

import (
    "testing"
    "github.com/stretchr/testify/assert"
    "github.com/stretchr/testify/require"
)

func TestTargetFunction(t *testing.T) {
    t.Run("正常场景", func(t *testing.T) {
        result, err := TargetFunction(validInput)
        require.NoError(t, err)
        assert.Equal(t, expected, result)
    })
    
    t.Run("边界值-空输入", func(t *testing.T) {
        result, err := TargetFunction("")
        assert.Error(t, err)
        assert.Nil(t, result)
    })
}

// 表驱动测试
func TestTargetFunction_Table(t *testing.T) {
    tests := []struct {
        name    string
        input   string
        want    string
        wantErr bool
    }{
        {"正常", "hello", "HELLO", false},
        {"空串", "", "", true},
        {"特殊字符", "!@#", "!@#", false},
    }
    
    for _, tt := range tests {
        t.Run(tt.name, func(t *testing.T) {
            got, err := TargetFunction(tt.input)
            if tt.wantErr {
                assert.Error(t, err)
            } else {
                assert.NoError(t, err)
                assert.Equal(t, tt.want, got)
            }
        })
    }
}
```

---

## 安全测试专项

### 通用安全检查项

无论何种语言，都需检查：

```
1. 硬编码凭证
   grep -r "password\s*=\|api_key\s*=\|secret\s*=" --include="*.py" --include="*.js" --include="*.java"
   
2. SQL 注入风险
   grep -r "f\"SELECT\|string.Format.*SELECT\|%s.*WHERE" ...
   
3. 命令注入
   grep -r "shell=True\|subprocess.call\|exec(" ...
   
4. 不安全的反序列化
   grep -r "pickle.loads\|yaml.load\|eval(" ...
   
5. 敏感文件暴露
   find . -name "*.env" -o -name "*.key" -o -name "*.pem" | grep -v ".example"
```

### OWASP Top 10 映射

| 风险 | 检查方式 |
|------|---------|
| A01 访问控制失效 | 检查路由是否有鉴权中间件 |
| A02 加密机制失效 | 检查是否使用 MD5/SHA1 存密码 |
| A03 注入 | 检查 SQL/命令/LDAP 拼接 |
| A05 安全配置错误 | 检查 DEBUG=True / CORS * 等 |
| A06 易受攻击的组件 | 运行依赖审计工具 |
| A09 日志记录不足 | 检查异常是否被静默吞掉 |

---

## 性能测试专项

### 工具选型

| 场景 | 工具 |
|------|------|
| Python 代码性能 | `cProfile`, `py-spy`, `memory_profiler` |
| HTTP API 压测 | `locust`, `k6`, `wrk` |
| Node.js 性能 | `clinic.js`, `autocannon` |
| Java 性能 | JMH（微基准测试） |
| 数据库查询 | EXPLAIN ANALYZE |

### 识别性能敏感代码特征

```
- 循环内的数据库查询（N+1 问题）
- 未分页的全表查询
- 同步 I/O 在异步上下文中
- 大对象序列化/反序列化
- 无缓存的重复计算
- 递归无深度限制
```

# 各语言代码解析模式

## Python

### 类识别
```python
# 类定义
class MyClass(BaseClass):          # 继承
class MyClass(Interface1, Mixin):  # 多继承

# 属性（dataclass）
@dataclass
class User:
    name: str
    age: int = 0

# SQLAlchemy 模型（→ 生成ER图）
class User(Base):
    __tablename__ = 'users'
    id = Column(Integer, primary_key=True)
    name = Column(String)
    orders = relationship("Order", back_populates="user")
```

### 路由识别（→ 生成API流程/序列图）
```python
# Flask
@app.route('/api/users', methods=['GET', 'POST'])
def users(): ...

# FastAPI
@router.post('/login')
async def login(data: LoginSchema): ...

# Django URL
urlpatterns = [path('users/', views.UserView.as_view())]
```

### 依赖识别
```python
import module_a          # 模块依赖
from package import cls  # 包依赖
```

---

## JavaScript / TypeScript

### 类识别
```typescript
// 类与接口
interface IUser { id: number; name: string }
class UserService implements IUser { ... }
class AdminService extends UserService { ... }

// TypeORM 实体（→ ER图）
@Entity('users')
class User {
    @PrimaryGeneratedColumn() id: number
    @Column() name: string
    @OneToMany(() => Order, o => o.user) orders: Order[]
}

// Prisma Schema（→ ER图）
model User {
  id    Int    @id @default(autoincrement())
  email String @unique
  posts Post[]
}
```

### 路由识别
```typescript
// Express
router.post('/api/login', authMiddleware, loginController)

// NestJS
@Controller('users')
@Get(':id') findOne(@Param('id') id: string) { ... }
```

---

## Java

### 类识别
```java
public class OrderService extends BaseService implements IOrderService {
    @Autowired private UserRepository userRepo;
    @Autowired private PaymentGateway payGateway;
}

// JPA 实体（→ ER图）
@Entity @Table(name = "orders")
public class Order {
    @Id @GeneratedValue Long id;
    @ManyToOne @JoinColumn(name="user_id") User user;
    @OneToMany(mappedBy="order") List<OrderItem> items;
}
```

### Spring 组件识别（→ 组件图/架构图）
```java
@Controller   // → 表示层
@Service      // → 业务层
@Repository   // → 数据访问层
@Component    // → 通用组件
@RestController + @RequestMapping("/api/...")  // → API端点
```

---

## Go

### 结构体识别
```go
type User struct {
    ID    uint   `gorm:"primaryKey"`
    Name  string
    Email string `gorm:"unique"`
    Orders []Order `gorm:"foreignKey:UserID"`
}

// 接口（→ 类图接口关系）
type UserRepository interface {
    FindByID(id uint) (*User, error)
    Save(user *User) error
}

// 实现接口
type userRepo struct { db *gorm.DB }
func (r *userRepo) FindByID(id uint) (*User, error) { ... }
```

### 包依赖识别
```go
import (
    "github.com/project/internal/service"
    "github.com/project/internal/repository"
)
```

---

## 配置文件解析

### docker-compose.yml（→ 架构图/部署图）
```yaml
services:
  web:          # → 服务节点
    image: nginx
    ports: ["80:80"]
    depends_on: [api]  # → 依赖关系

  api:
    build: ./backend
    environment:
      DATABASE_URL: postgresql://db/myapp
    depends_on: [db, redis]

  db:           # → 数据库节点
    image: postgres

  redis:        # → 缓存节点
    image: redis
```

**解析规则**：
- 每个 `service` → 架构图中的一个节点
- `depends_on` → 箭头方向（A依赖B = A → B）
- `image` 判断类型：nginx=网关, postgres/mysql=DB, redis=缓存, rabbitmq/kafka=MQ

### Kubernetes YAML（→ 部署图）
- `kind: Deployment` → 应用服务
- `kind: Service` → 服务暴露
- `kind: Ingress` → 入口路由
- `kind: ConfigMap/Secret` → 配置

### package.json / requirements.txt / go.mod（→ 依赖图）
- 列出主要第三方依赖，归类：Web框架、ORM、认证、消息队列等

---

## SQL Schema（→ ER图）

```sql
CREATE TABLE users (
    id BIGINT PRIMARY KEY AUTO_INCREMENT,
    username VARCHAR(50) UNIQUE NOT NULL,
    email VARCHAR(100) NOT NULL
);

CREATE TABLE orders (
    id BIGINT PRIMARY KEY,
    user_id BIGINT REFERENCES users(id),  -- 外键 → 一对多关系
    status ENUM('pending','paid','shipped') DEFAULT 'pending'
);
```

**识别规则**：
- `PRIMARY KEY` → PK
- `REFERENCES other_table(id)` / `FOREIGN KEY` → 关联关系
- `UNIQUE` → 唯一约束（在ER图中标注）

---

## 关系推断规则

| 代码模式 | 推断关系 | 图表表示 |
|---------|---------|---------|
| `class B extends A` | 继承 | `A <\|-- B` |
| `class C implements I` | 实现接口 | `I <\|.. C` |
| `@OneToMany` / `List<B>` 字段 | 一对多 | `A \|\|--o{ B` |
| `@ManyToOne` / B类型字段 | 多对一 | `A }o--\|\| B` |
| `@ManyToMany` | 多对多 | `A }o--o{ B` |
| `@Autowired B` / `B b = new B()` | 组合依赖 | `A *-- B` |
| `import B` / 参数类型为B | 弱依赖 | `A ..> B` |

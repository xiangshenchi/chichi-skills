# Mermaid 图表模板参考

## 1. 类图 (classDiagram)

```mermaid
classDiagram
    class Animal {
        +String name
        +int age
        +makeSound() void
    }
    class Dog {
        +String breed
        +fetch() void
    }
    class Cat {
        +bool isIndoor
        +purr() void
    }
    Animal <|-- Dog : 继承
    Animal <|-- Cat : 继承
    Dog "1" --> "0..*" Owner : 属于
```

**语法说明**：
- `<|--` 继承
- `*--` 组合（强依赖）
- `o--` 聚合（弱依赖）
- `-->` 关联
- `..>` 依赖
- `+` public, `-` private, `#` protected
- `<<interface>>` / `<<abstract>>` 修饰符

---

## 2. ER 图 (erDiagram)

```mermaid
erDiagram
    USER {
        int id PK
        string username
        string email
        datetime created_at
    }
    ORDER {
        int id PK
        int user_id FK
        decimal total_amount
        string status
        datetime order_date
    }
    ORDER_ITEM {
        int id PK
        int order_id FK
        int product_id FK
        int quantity
        decimal unit_price
    }
    PRODUCT {
        int id PK
        string name
        decimal price
        int stock
    }

    USER ||--o{ ORDER : "下单"
    ORDER ||--|{ ORDER_ITEM : "包含"
    PRODUCT ||--o{ ORDER_ITEM : "被包含"
```

**关系符号**：
- `||` 精确一个
- `o|` 零或一
- `}|` 一或多
- `}o` 零或多

---

## 3. 序列图 (sequenceDiagram)

```mermaid
sequenceDiagram
    actor User as 用户
    participant FE as 前端
    participant API as API服务
    participant Auth as 认证服务
    participant DB as 数据库

    User->>FE: 提交登录表单
    FE->>API: POST /api/login
    API->>Auth: 验证凭据
    Auth->>DB: 查询用户
    DB-->>Auth: 返回用户信息
    Auth-->>API: 生成 JWT Token
    API-->>FE: 200 { token }
    FE-->>User: 跳转到首页

    alt 认证失败
        Auth-->>API: 401 Unauthorized
        API-->>FE: 401 错误
        FE-->>User: 显示错误提示
    end
```

**常用语法**：
- `->>` 实线箭头（同步调用）
- `-->>` 虚线箭头（响应/返回）
- `activate` / `deactivate` 激活框
- `alt` / `else` / `end` 条件分支
- `loop` 循环
- `par` 并行
- `Note over A,B:` 注释

---

## 4. 系统架构图 (graph / flowchart)

```mermaid
graph TD
    subgraph Client["客户端"]
        Web[Web Browser]
        Mobile[Mobile App]
    end

    subgraph Gateway["API 网关层"]
        Nginx[Nginx]
        LB[负载均衡]
    end

    subgraph Services["后端服务层"]
        UserSvc[用户服务\n:8001]
        OrderSvc[订单服务\n:8002]
        PaySvc[支付服务\n:8003]
    end

    subgraph Data["数据存储层"]
        MySQL[(MySQL\n主库)]
        Redis[(Redis\n缓存)]
        MQ[RabbitMQ\n消息队列]
    end

    subgraph External["外部服务"]
        Pay[支付宝/微信]
        SMS[短信服务]
    end

    Web --> Nginx
    Mobile --> Nginx
    Nginx --> LB
    LB --> UserSvc
    LB --> OrderSvc
    LB --> PaySvc
    UserSvc --> MySQL
    UserSvc --> Redis
    OrderSvc --> MySQL
    OrderSvc --> MQ
    PaySvc --> Pay
    MQ --> SMS
```

---

## 5. 状态图 (stateDiagram-v2)

```mermaid
stateDiagram-v2
    [*] --> 待支付 : 创建订单
    待支付 --> 已支付 : 完成支付
    待支付 --> 已取消 : 超时/主动取消
    已支付 --> 待发货 : 商家确认
    待发货 --> 已发货 : 发货
    已发货 --> 已完成 : 确认收货
    已发货 --> 退款中 : 申请退款
    退款中 --> 已退款 : 退款成功
    已完成 --> [*]
    已取消 --> [*]
    已退款 --> [*]
```

---

## 6. 组件/依赖图 (graph LR)

```mermaid
graph LR
    subgraph Core["核心模块"]
        Config[配置管理]
        Logger[日志模块]
        DB[数据库连接]
    end

    subgraph Features["功能模块"]
        Auth[认证模块]
        User[用户模块]
        Order[订单模块]
        Pay[支付模块]
    end

    subgraph API["API 层"]
        Router[路由]
        Middleware[中间件]
    end

    Router --> Middleware
    Middleware --> Auth
    Middleware --> User
    Middleware --> Order
    Auth --> DB
    Auth --> Logger
    User --> DB
    Order --> DB
    Order --> Pay
    Pay --> Logger
    Config -.-> DB
    Config -.-> Logger
```

---

## 7. 数据流图 (flowchart LR)

```mermaid
flowchart LR
    Input([用户输入]) --> Validate{数据校验}
    Validate -->|通过| Transform[数据转换]
    Validate -->|失败| Error([返回错误])
    Transform --> Enrich[数据富化]
    Enrich --> Store[(存储到DB)]
    Enrich --> Cache[(写入缓存)]
    Store --> Notify[发送通知]
    Cache --> Response([返回响应])
    Notify --> Queue[消息队列]
    Queue --> Email[邮件服务]
    Queue --> SMS[短信服务]
```

---

## 8. 部署图 (graph TB)

```mermaid
graph TB
    subgraph Internet["公网"]
        User[用户]
        CDN[CDN]
    end

    subgraph K8S["Kubernetes 集群"]
        subgraph Ingress["Ingress 层"]
            IG[Ingress Controller]
        end
        subgraph Pods["应用 Pod"]
            P1[App Pod 1]
            P2[App Pod 2]
            P3[App Pod 3]
        end
        subgraph Infra["基础设施 Pod"]
            Prom[Prometheus]
            Graf[Grafana]
        end
    end

    subgraph Cloud["云服务"]
        RDS[(RDS 数据库)]
        OSS[对象存储]
        MQ2[消息队列]
    end

    User --> CDN
    CDN --> IG
    IG --> P1
    IG --> P2
    IG --> P3
    P1 --> RDS
    P1 --> OSS
    P1 --> MQ2
    Prom --> P1
    Graf --> Prom
```

---

## Mermaid 常见错误与修复

| 错误现象 | 原因 | 修复方式 |
|---------|------|---------|
| 节点ID包含中文/空格 | ID 不合法 | 改为英文 ID，中文放标签里 `A["中文"]` |
| 箭头方向混乱 | 方向词错误 | `TD`=上到下, `LR`=左到右, `RL`=右到左, `BT`=下到上 |
| subgraph 渲染失败 | 缺少 `end` | 每个 `subgraph` 必须有对应 `end` |
| ER 图关系线错误 | 符号不对 | 参考标准符号 `||--o{` |
| 特殊字符导致解析失败 | 括号/引号冲突 | 用 `"..."` 包裹含特殊字符的标签 |

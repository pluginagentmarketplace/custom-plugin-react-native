---
description: Master backend systems, RESTful and GraphQL APIs, and server-side applications across multiple languages. Expert in Python, Go, Rust, Java, Kotlin, PHP, C++, Spring Boot, ASP.NET Core, Node.js, database design, microservices, and production-grade infrastructure.
capabilities:
  - Python backend with Django, FastAPI, Flask
  - Go language for high-performance systems
  - Rust for memory-safe, blazing-fast services
  - Java and Spring Boot for enterprise applications
  - Kotlin and modern JVM ecosystem
  - PHP with Laravel, Symfony frameworks
  - C++ for system-level performance
  - RESTful API design and best practices
  - GraphQL API development and optimization
  - Database design and optimization (SQL, NoSQL)
  - Authentication and authorization systems
  - Microservices architecture and patterns
  - Event-driven systems and message queues
  - Caching strategies and performance
  - Testing frameworks and coverage
  - CI/CD integration and deployment
  - Monitoring, logging, and observability
  - Security hardening and vulnerability prevention
---

# 🔧 Backend & API Development Agent

Complete guide to mastering server-side development. Build scalable, reliable, and performant backend systems that power global applications.

## 🎯 Agent Specialization

**Covers 17+ Roadmaps:**
Backend, Backend Beginner, Python, Go, Rust, Java, Kotlin, PHP, C++, Spring Boot, ASP.NET Core, Node.js, API Design, Databases (PostgreSQL, MongoDB, Redis)

**Mission:** Transform you from basic server-side coding to architecting enterprise-grade backend systems handling millions of requests

---

## 📚 DETAILED LEARNING DOMAINS

### 1. **Backend Fundamentals** (Week 1-4)

**Core Concepts:**
- Server-client architecture
- HTTP/HTTPS protocols
- Request-response cycle
- Stateless vs stateful systems
- Synchronous vs asynchronous operations
- Process vs threads vs async I/O

**RESTful Principles:**
- Resource-oriented design
- HTTP verbs (GET, POST, PUT, DELETE, PATCH)
- Status codes (2xx, 3xx, 4xx, 5xx)
- HATEOAS and hypermedia
- Content negotiation
- API versioning strategies

**Key Projects:**
- Simple REST API (todos, notes)
- Multi-endpoint API
- Error handling implementation
- Request validation

---

### 2. **Programming Languages Deep Dive** (Week 5-24)

#### **Python Backend** (Week 5-8)
**Why Python:**
- Rapid development
- Large ecosystem
- Data science integration
- Easy to learn

**Frameworks:**
- **FastAPI**: Modern, fast, auto-documentation (Pydantic validation)
- **Django**: Full-stack, batteries-included (ORM, admin panel, auth)
- **Flask**: Lightweight, flexible, scalable

**Production Skills:**
```python
# FastAPI example
from fastapi import FastAPI
from sqlalchemy import create_engine
from typing import List

app = FastAPI()
db = create_engine("postgresql://...")

@app.post("/users/")
async def create_user(user: UserSchema):
    # Validation automatic, type hints for docs
    db_user = User(**user.dict())
    session.add(db_user)
    session.commit()
    return db_user
```

**Key Areas:**
- Virtual environments and dependency management
- Async programming with asyncio
- ORM with SQLAlchemy
- Database migrations with Alembic
- Task queues (Celery)
- Testing with pytest

---

#### **Go Language** (Week 9-12)
**Why Go:**
- Compiled, fast execution
- Lightweight concurrency
- Built-in HTTP support
- Cloud-native

**Frameworks:**
- **Gin**: High-performance web framework
- **Echo**: Minimalist, flexible
- **Fiber**: Express.js-like syntax

**Production Skills:**
```go
package main

import (
    "github.com/gin-gonic/gin"
    "gorm.io/gorm"
)

func main() {
    r := gin.Default()

    r.POST("/users", func(c *gin.Context) {
        var user User
        if err := c.ShouldBindJSON(&user); err != nil {
            c.JSON(400, err)
            return
        }
        db.Create(&user)
        c.JSON(201, user)
    })

    r.Run(":8080")
}
```

**Key Areas:**
- Goroutines and channels
- Interface{} patterns
- Package management with Go modules
- Testing with testing package
- Concurrency patterns

---

#### **Rust Backend** (Week 13-16)
**Why Rust:**
- Memory safety without GC
- Performance critical systems
- Compile-time guarantees
- WebAssembly capable

**Frameworks:**
- **Axum**: Type-safe async web framework
- **Actix**: Actor-based framework
- **Rocket**: Syntax-friendly framework

**Production Skills:**
```rust
use axum::{
    routing::{post, Router},
    Json, extract::Path,
};
use sqlx::PgPool;

async fn create_user(Json(user): Json<User>) -> Json<User> {
    let pool = PgPool::connect("postgres://...").await.unwrap();
    sqlx::query_as::<_, User>("INSERT INTO users ...")
        .fetch_one(&pool)
        .await
        .unwrap()
}

#[tokio::main]
async fn main() {
    let app = Router::new()
        .route("/users", post(create_user));

    axum::Server::bind(&"0.0.0.0:8080".parse().unwrap())
        .serve(app.into_make_service())
        .await
        .unwrap();
}
```

**Key Areas:**
- Ownership and borrowing
- Error handling (Result, Option)
- Lifetime annotations
- Async/await with tokio
- Macros and derive

---

#### **Java & Spring Boot** (Week 17-20)
**Why Java:**
- Enterprise adoption
- Mature ecosystem
- Strong typing
- JVM performance

**Spring Boot Stack:**
- Spring Data JPA (ORM)
- Spring Security (auth)
- Spring Cloud (microservices)

**Production Skills:**
```java
@RestController
@RequestMapping("/api/users")
@RequiredArgsConstructor
public class UserController {

    private final UserService userService;

    @PostMapping
    public ResponseEntity<UserDTO> createUser(@Valid @RequestBody UserDTO dto) {
        return ResponseEntity
            .status(HttpStatus.CREATED)
            .body(userService.create(dto));
    }
}

@Service
@Transactional
public class UserService {
    public User create(UserDTO dto) {
        User user = new User(dto.getEmail(), dto.getName());
        return userRepository.save(user);
    }
}
```

**Key Areas:**
- Spring Boot starter packs
- Dependency injection (IoC)
- Data JPA and Hibernate
- Spring Security and JWT
- Actuator for metrics

---

#### **Kotlin & PHP** (Week 21-24)
**Kotlin:** Modern JVM alternative, safer than Java
**PHP:** Web-focused, Laravel ecosystem, fast deployment

---

### 3. **API Design Excellence** (Week 25-28)

**RESTful Design Patterns:**
- Resource hierarchy (`/api/users/{id}/posts`)
- Pagination (`?page=1&limit=20`)
- Filtering (`?status=active&role=admin`)
- Sorting (`?sort=created_at&order=desc`)
- Sparse fields (`?fields=id,name,email`)

**API Documentation:**
```yaml
# OpenAPI/Swagger
openapi: 3.0.0
paths:
  /api/users:
    post:
      summary: Create user
      requestBody:
        required: true
        content:
          application/json:
            schema:
              $ref: '#/components/schemas/User'
      responses:
        '201':
          description: User created
```

**Error Handling:**
```json
{
  "error": {
    "code": "VALIDATION_ERROR",
    "message": "Email is invalid",
    "details": [
      {
        "field": "email",
        "message": "Must be valid email"
      }
    ]
  }
}
```

**GraphQL Basics:**
- Schema definition
- Queries and mutations
- Subscriptions for real-time
- Resolvers and data fetching

**Key Projects:**
- Full REST API with comprehensive docs
- GraphQL API with schema
- API versioning strategy
- Rate limiting implementation

---

### 4. **Database Mastery** (Week 29-36)

**SQL Databases (PostgreSQL Focus):**
- Schema design and normalization
- Indexes for performance
- Query optimization
- Transactions and ACID
- Replication and failover
- Connection pooling

**Advanced SQL:**
```sql
-- Window functions
SELECT
    user_id, amount,
    ROW_NUMBER() OVER (PARTITION BY user_id ORDER BY created_at) as seq,
    SUM(amount) OVER (PARTITION BY user_id) as total
FROM orders;

-- CTEs for complex queries
WITH user_stats AS (
    SELECT user_id, COUNT(*) as order_count
    FROM orders
    GROUP BY user_id
)
SELECT * FROM user_stats WHERE order_count > 10;
```

**NoSQL Databases:**
- Document stores (MongoDB)
- Key-value stores (Redis)
- Time-series databases
- Graph databases

**ORM Usage:**
- Query building
- Relationships (1-1, 1-N, N-N)
- Lazy vs eager loading
- N+1 query prevention

**Key Projects:**
- Database schema design for e-commerce
- Complex query optimization
- Data migration scripts
- Backup and recovery procedures

---

### 5. **Authentication & Security** (Week 37-40)

**Authentication Methods:**
```python
# JWT Token-based auth
from fastapi import Depends, HTTPException
from jose import JWTError, jwt

async def get_current_user(token: str = Depends(oauth2_scheme)):
    try:
        payload = jwt.decode(token, SECRET_KEY, algorithms=[ALGORITHM])
        user_id: str = payload.get("sub")
    except JWTError:
        raise HTTPException(status_code=401)
    return await get_user(user_id)

@app.post("/token")
async def login(credentials: LoginSchema):
    user = authenticate_user(credentials.email, credentials.password)
    access_token = create_access_token(user.id)
    return {"access_token": access_token}
```

**OAuth2 & SSO:**
- GitHub, Google authentication
- Third-party integration
- Token refresh strategies

**Security Best Practices:**
- Password hashing (bcrypt, argon2)
- SQL injection prevention
- CORS configuration
- Rate limiting
- Input validation
- HTTPS enforcement

**Key Projects:**
- Complete auth system (signup, login, JWT)
- OAuth2 integration
- 2FA implementation
- Permission/role system

---

### 6. **Advanced Architecture** (Week 41-48)

**Microservices Patterns:**
- API Gateway pattern
- Service discovery
- Inter-service communication
- Circuit breaker pattern
- Distributed transactions

**Event-Driven Systems:**
```python
# Message queue pattern with Celery/RabbitMQ
from celery import shared_task

@shared_task
def send_welcome_email(user_id):
    user = User.objects.get(id=user_id)
    send_email(user.email, "Welcome!")

# Triggered by user creation
@app.post("/users")
async def create_user(user: UserSchema):
    new_user = db.create(User(**user.dict()))
    send_welcome_email.delay(new_user.id)  # Async task
    return new_user
```

**Caching Strategies:**
- In-memory caching (Redis)
- Cache invalidation patterns
- Cache-aside, write-through strategies
- CDN caching

**Monitoring & Observability:**
- Structured logging (JSON format)
- Distributed tracing (Jaeger, DataDog)
- Metrics collection (Prometheus)
- Health checks and alerting

**Key Projects:**
- Microservices system
- Event-driven workflow
- Complex caching strategy
- Full monitoring stack

---

## 🎓 LEARNING PATH PROGRESSIONS

### 🟢 Beginner Path (3-4 months, 300+ hours)

**Month 1: Fundamentals**
- HTTP/REST basics
- Choose one language
- Simple CRUD API
- Database basics

**Month 2: Framework Mastery**
- Framework deep dive
- ORM usage
- Request validation
- Error handling

**Month 3: Production Ready**
- Authentication
- Testing frameworks
- Deployment setup
- Documentation

**Month 4: First Backend**
- Build complete API
- Deploy to production
- Monitor and maintain

---

### 🟡 Intermediate Path (4-6 months, 400+ hours)

**Month 1-2: Advanced Language Features**
- Async programming
- Advanced patterns
- Performance optimization

**Month 3: Database Mastery**
- Complex queries
- Optimization
- Scaling strategies

**Month 4: API Excellence**
- GraphQL
- API versioning
- Advanced caching

**Month 5-6: Production Systems**
- Microservices
- Event-driven
- Monitoring setup

---

### 🔴 Advanced Path (6-12 months, 800+ hours)

**Module 1: Language Deep Dive (8 weeks)**
- Advanced language features
- Performance optimization
- Memory profiling

**Module 2: Distributed Systems (8 weeks)**
- Microservices patterns
- Service mesh
- Distributed transactions

**Module 3: Performance Engineering (8 weeks)**
- Database optimization
- Caching strategies
- Load testing

**Module 4: Security Hardening (8 weeks)**
- Threat modeling
- Penetration testing
- Compliance standards

---

## 💼 CAREER PROGRESSION

### Junior Backend Developer (0-1 year)
**Focus:** Building core skills, shipping features
- **Skills:** Basic HTTP, CRUD operations, ORM usage
- **Tools:** VS Code/IDE, Git, basic Docker
- **Responsibilities:** Feature development, bug fixes

### Mid-Level Backend Engineer (1-3 years)
**Focus:** Ownership, optimization, code quality
- **Skills:** Advanced language features, caching, testing
- **Tools:** Docker, CI/CD, monitoring tools
- **Responsibilities:** Feature ownership, code reviews

### Senior Backend Engineer (3+ years)
**Focus:** Architecture, performance, scaling
- **Skills:** Microservices, system design, optimization
- **Tools:** Kubernetes, monitoring stacks, infra tools
- **Responsibilities:** Architecture decisions, mentoring

---

## 🛠️ ESSENTIAL TOOLS & SETUP

**Development Stack:**
```bash
# Language runtimes
python3 --version
go version
cargo --version
java -version

# Package managers
pip, npm/yarn, go mod, cargo, maven

# Databases
PostgreSQL, MongoDB, Redis setup

# Tools
Docker, Docker Compose, Postman/REST Client
Git, GitHub CLI
```

**Monitoring & Observability:**
- ELK Stack (Elasticsearch, Logstash, Kibana)
- Prometheus + Grafana
- DataDog or New Relic
- OpenTelemetry

---

## 📖 INDUSTRY BEST PRACTICES

### Code Quality
- ✅ 80%+ test coverage
- ✅ Type hints/static typing
- ✅ Code linting enabled
- ✅ Peer code reviews
- ✅ Security scanning in CI/CD

### Performance Standards
- ✅ API response < 200ms (p95)
- ✅ Database queries < 50ms
- ✅ Memory usage stable
- ✅ Zero security vulnerabilities
- ✅ 99.9% uptime SLA

### API Standards
- ✅ Versioned APIs
- ✅ Comprehensive docs
- ✅ Rate limiting enforced
- ✅ CORS properly configured
- ✅ Error responses standardized

---

## 🎯 HANDS-ON PROJECTS (30+ Ideas)

### Tier 1: Beginner
1. Todo API - CRUD operations
2. Weather API - External API integration
3. User Management - Authentication
4. Blog API - Relationships
5. Calculator API - Basic operations

### Tier 2: Intermediate
6. E-commerce Backend - Complex features
7. Real-time Chat Server - WebSockets
8. Analytics API - Complex queries
9. File Upload Service - S3 integration
10. Payment Processing - Stripe integration

### Tier 3: Advanced
11. Microservices Platform - Multiple services
12. Real-time Collaboration App - WebSockets + DB
13. Analytics Engine - Distributed computing
14. Message Queue System - Event-driven
15. Multi-tenant SaaS - Complete platform

---

## 🔗 AGENT SYNERGIES

**Work with Frontend Agent for:**
- API design for frontend consumption
- CORS configuration
- Real-time data synchronization
- Authentication flow

**Work with DevOps Agent for:**
- Deployment automation
- CI/CD pipeline setup
- Database management
- Monitoring implementation

**Work with Architecture Agent for:**
- System design patterns
- Scalability decisions
- Technology selection

---

## 📚 SUPPLEMENTARY SKILLS

**Skill: backend-technologies** - Quick-start guides, production code examples, architecture patterns
- Framework-specific best practices
- Database optimization techniques
- API design patterns
- Security implementations

---

**Next Steps:**
- Use `/learn` to choose your backend language
- Use `/assess` to evaluate your backend skills
- Explore `/browse-agent` for full-stack context

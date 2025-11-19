---
name: system-architecture
description: Design scalable and maintainable systems using design patterns, system architecture principles, and database design. Master architecture for large-scale applications.
---

# System Architecture Skill

## Quick Start

System architecture provides the blueprint for building scalable applications. Here's how to think about architecture:

### Architecture Patterns

```
Monolithic Architecture:
┌─────────────────────────────────────┐
│       Single Deployment Unit        │
├─────────────────┬──────────┬────────┤
│   API Layer     │ Business │ Data   │
│                 │ Logic    │ Layer  │
└─────────────────┴──────────┴────────┘
       └──────────────────────────┘
              Database

Microservices Architecture:
┌──────────────────┐  ┌──────────────────┐  ┌──────────────────┐
│  User Service    │  │ Product Service  │  │ Order Service    │
├──────────────────┤  ├──────────────────┤  ├──────────────────┤
│ - API            │  │ - API            │  │ - API            │
│ - Business Logic │  │ - Business Logic │  │ - Business Logic │
│ - Database       │  │ - Database       │  │ - Database       │
└──────────────────┘  └──────────────────┘  └──────────────────┘
       │                     │                      │
       └─────────┬───────────┴──────────┬───────────┘
                 │   Message Queue      │
                 │   (RabbitMQ/Kafka)   │
```

### Design Patterns

#### Model-View-Controller (MVC)
```
User Request → Router → Controller → Model → View → Response

Controller handles business logic
Model manages data
View renders presentation
```

#### Model-View-ViewModel (MVVM)
```
User Input → ViewModel → Update → View
                ↑
         Data Binding
                ↓
             Model
```

### Database Design

```sql
-- Schema design for e-commerce
CREATE TABLE users (
    id SERIAL PRIMARY KEY,
    email VARCHAR(255) UNIQUE NOT NULL,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    INDEX idx_email (email)
);

CREATE TABLE products (
    id SERIAL PRIMARY KEY,
    name VARCHAR(255) NOT NULL,
    price DECIMAL(10, 2) NOT NULL,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

CREATE TABLE orders (
    id SERIAL PRIMARY KEY,
    user_id INT NOT NULL,
    order_date TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    total DECIMAL(10, 2) NOT NULL,
    status VARCHAR(50),
    FOREIGN KEY (user_id) REFERENCES users(id),
    INDEX idx_user_id (user_id),
    INDEX idx_status (status)
);

CREATE TABLE order_items (
    id SERIAL PRIMARY KEY,
    order_id INT NOT NULL,
    product_id INT NOT NULL,
    quantity INT NOT NULL,
    unit_price DECIMAL(10, 2) NOT NULL,
    FOREIGN KEY (order_id) REFERENCES orders(id),
    FOREIGN KEY (product_id) REFERENCES products(id)
);
```

### System Design Example: URL Shortener

```
┌───────────────────────────────────────────────┐
│           Load Balancer / API Gateway         │
└───────────────────┬───────────────────────────┘
                    │
        ┌───────────┼───────────┐
        │           │           │
    ┌───────┐   ┌───────┐   ┌───────┐
    │  API  │   │  API  │   │  API  │
    │   1   │   │   2   │   │   3   │
    └───────┘   └───────┘   └───────┘
        │           │           │
        └───────────┼───────────┘
                    │
        ┌───────────┼───────────┐
        │           │           │
    ┌───────────┐┌──────────┐┌──────────┐
    │  Cache    ││Database  ││ Message  │
    │  (Redis)  ││(Primary) ││ Queue    │
    └───────────┘└──────────┘└──────────┘
```

## Key Areas

### 1. Design Patterns
- Creational patterns (Singleton, Factory, Builder)
- Structural patterns (Adapter, Decorator, Proxy)
- Behavioral patterns (Observer, Strategy, Command)
- Architectural patterns (MVC, MVVM, Repository)
- Concurrency patterns

### 2. System Design Principles
- Scalability (vertical and horizontal)
- Performance and optimization
- Reliability and fault tolerance
- Security and compliance
- Maintainability and extensibility
- Cost optimization

### 3. Database Design
- Schema design normalization
- Indexing strategies
- Query optimization
- Replication and sharding
- Transaction management
- Connection pooling

### 4. API Design
- RESTful principles
- GraphQL design
- Error handling and versioning
- Rate limiting and throttling
- Documentation standards
- Security and authentication

### 5. Scalability Patterns
- Load balancing strategies
- Caching layers (distributed caches)
- Database partitioning
- Service isolation
- Async processing
- Event-driven architecture

### 6. Microservices Architecture
- Service decomposition
- Inter-service communication
- API gateway pattern
- Circuit breaker pattern
- Service discovery
- Distributed tracing

## Advanced Concepts

### SOLID Principles

```python
# Single Responsibility Principle
class UserRepository:
    def save(self, user): pass
    def find(self, user_id): pass

class UserService:
    def __init__(self, repo: UserRepository):
        self.repo = repo
    def create_user(self, email): pass

# Open/Closed Principle
class PaymentProcessor:
    def process(self, payment): pass

class CreditCardProcessor(PaymentProcessor):
    def process(self, payment): pass

class PayPalProcessor(PaymentProcessor):
    def process(self, payment): pass
```

### Circuit Breaker Pattern
```python
from enum import Enum
from datetime import datetime, timedelta

class CircuitState(Enum):
    CLOSED = "closed"      # Normal operation
    OPEN = "open"          # Failing, reject requests
    HALF_OPEN = "half_open"  # Testing recovery

class CircuitBreaker:
    def __init__(self, failure_threshold=5, timeout=60):
        self.failure_threshold = failure_threshold
        self.timeout = timeout
        self.state = CircuitState.CLOSED
        self.failures = 0
        self.last_failure_time = None

    def call(self, func, *args, **kwargs):
        if self.state == CircuitState.OPEN:
            if self._should_attempt_reset():
                self.state = CircuitState.HALF_OPEN
            else:
                raise Exception("Circuit breaker is OPEN")

        try:
            result = func(*args, **kwargs)
            self._on_success()
            return result
        except Exception as e:
            self._on_failure()
            raise e

    def _on_success(self):
        self.failures = 0
        self.state = CircuitState.CLOSED

    def _on_failure(self):
        self.failures += 1
        self.last_failure_time = datetime.now()
        if self.failures >= self.failure_threshold:
            self.state = CircuitState.OPEN

    def _should_attempt_reset(self):
        return datetime.now() - self.last_failure_time > timedelta(seconds=self.timeout)
```

## Resources

- **System Design Primer**: https://github.com/donnemartin/system-design-primer
- **Design Patterns**: https://refactoring.guru/design-patterns
- **SOLID Principles**: https://en.wikipedia.org/wiki/SOLID
- **Microservices Patterns**: https://microservices.io/
- **Architecture Decision Records (ADR)**: https://adr.github.io/

## Real-World Projects

1. **URL Shortener**: Database design and scalability
2. **Social Media Feed**: Caching and distributed systems
3. **E-commerce Platform**: Microservices decomposition
4. **Chat Application**: Real-time systems and websockets
5. **Video Streaming Service**: Scalability at scale
6. **Analytics Dashboard**: Data processing and visualization

---

**Next Steps:** Master design patterns, practice system design interviews, and study real-world architectures.

---
name: 06-architecture-design-systems
description: Master system design, software architecture, and design patterns. Expert in scalable systems, microservices, database design, UX/UI design, design systems, and engineering leadership.
model: sonnet
tools: All tools
sasmp_version: "1.3.0"
eqhm_enabled: true
---

# 🏛️ Architecture, Design & Systems Agent

Master system design and technical architecture. Build scalable, maintainable systems that support growth from startup to enterprise scale.

## 🎯 Agent Specialization

**Covers 11+ Roadmaps:** Software Architect, Engineering Manager, System Design, Software Design & Architecture, Data Structures & Algorithms, UX Design, Product Manager

**Mission:** Transform you from implementation-focused to architecture and strategic thinking

---

## 📚 DETAILED LEARNING DOMAINS

### 1. **Data Structures & Algorithms** (Week 1-8)

**Arrays & Lists:**
```python
# Array manipulation
arr = [1, 2, 3, 4, 5]
reversed_arr = arr[::-1]
unique = list(set(arr))

# Two-pointer technique
def two_sum(arr, target):
    left, right = 0, len(arr) - 1
    while left < right:
        current_sum = arr[left] + arr[right]
        if current_sum == target:
            return [left, right]
        elif current_sum < target:
            left += 1
        else:
            right -= 1
```

**Trees & Graphs:**
```python
# Binary Search Tree
class TreeNode:
    def __init__(self, val=0, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right

def inorder_traversal(node):
    if not node:
        return []
    return inorder_traversal(node.left) + [node.val] + inorder_traversal(node.right)

# Graph DFS
def dfs(graph, start, visited=None):
    if visited is None:
        visited = set()
    visited.add(start)
    for neighbor in graph[start]:
        if neighbor not in visited:
            dfs(graph, neighbor, visited)
    return visited
```

---

### 2. **System Design Fundamentals** (Week 9-16)

**Scalability Patterns:**
```
Vertical Scaling:
- Increase CPU, RAM per machine
- Limited by hardware
- Simple but expensive

Horizontal Scaling:
- Add more machines
- Load balancing required
- Database sharding needed
- Cost-effective at scale
```

**Load Balancing:**
```
Strategies:
- Round Robin: Simple, equal distribution
- Least Connections: Route to least busy
- IP Hash: Consistent routing
- Weighted: Based on server capacity
```

**Caching Strategy:**
```python
# Cache-aside pattern
def get_user(user_id):
    cached = redis.get(f"user:{user_id}")
    if cached:
        return json.loads(cached)

    user = db.query("SELECT * FROM users WHERE id = ?", user_id)
    redis.setex(f"user:{user_id}", 3600, json.dumps(user))
    return user
```

---

### 3. **Database Design** (Week 17-20)

**Schema Normalization:**
```sql
-- 1NF: Atomic values
CREATE TABLE users (
    id INT PRIMARY KEY,
    name VARCHAR(100),
    email VARCHAR(100)
);

-- 2NF/3NF: No partial/transitive dependencies
CREATE TABLE orders (
    id INT PRIMARY KEY,
    user_id INT,
    order_date DATETIME,
    FOREIGN KEY (user_id) REFERENCES users(id)
);

CREATE TABLE order_items (
    id INT PRIMARY KEY,
    order_id INT,
    product_id INT,
    quantity INT,
    FOREIGN KEY (order_id) REFERENCES orders(id)
);
```

**Indexing Strategy:**
```sql
-- Single column index
CREATE INDEX idx_user_email ON users(email);

-- Composite index
CREATE INDEX idx_order_user_date ON orders(user_id, order_date);

-- Query with index
SELECT * FROM orders WHERE user_id = 1 AND order_date > '2024-01-01';
```

---

### 4. **Microservices Architecture** (Week 21-24)

**Service Decomposition:**
```
Monolith → Microservices:

User Service:
- Authentication
- Profile management
- User data

Product Service:
- Product catalog
- Inventory
- Pricing

Order Service:
- Order management
- Payment coordination
- Fulfillment
```

**Inter-service Communication:**
```python
# Synchronous (REST/gRPC)
class OrderService:
    def create_order(self, user_id, items):
        # Call Product Service
        inventory = requests.post(
            'http://product-service/check-inventory',
            json={'items': items}
        )
        if not inventory.json()['available']:
            raise Exception('Out of stock')

        # Create order
        return self.db.create_order(user_id, items)

# Asynchronous (Message Queue)
class UserService:
    def on_user_created(self, user):
        self.message_queue.publish('user.created', user)

class OrderService:
    def subscribe_user_created(self):
        self.message_queue.subscribe('user.created', self.welcome_email)
```

---

### 5. **UX/UI Design Principles** (Week 25-28)

**Design Process:**
1. Research & Discovery
2. Wireframing
3. Prototyping
4. User Testing
5. Iteration

**Accessibility (WCAG):**
```html
<!-- Semantic HTML -->
<nav> Navigation menu </nav>
<main> Page content </main>
<article> Main content </article>
<aside> Sidebar </aside>

<!-- ARIA labels -->
<button aria-label="Close menu">X</button>
<img alt="Product image" src="...">

<!-- Color contrast: 4.5:1 minimum -->
```

**Design Systems:**
```
Component Library:
- Button (primary, secondary, disabled)
- Input (text, number, date, select)
- Card (with image, title, description)
- Modal (with title, body, actions)

Design Tokens:
- Colors: primary, secondary, success, error
- Typography: heading, body, caption
- Spacing: xs, sm, md, lg, xl
- Shadows, borders, radius
```

---

### 6. **Design Patterns** (Week 29-32)

**Creational Patterns:**
```python
# Singleton
class Database:
    _instance = None

    def __new__(cls):
        if cls._instance is None:
            cls._instance = super().__new__(cls)
        return cls._instance

# Factory
class ComponentFactory:
    @staticmethod
    def create_component(component_type):
        if component_type == 'button':
            return Button()
        elif component_type == 'input':
            return Input()
```

**Behavioral Patterns:**
```python
# Observer
class EventEmitter:
    def __init__(self):
        self.subscribers = {}

    def on(self, event, callback):
        if event not in self.subscribers:
            self.subscribers[event] = []
        self.subscribers[event].append(callback)

    def emit(self, event, data):
        for callback in self.subscribers.get(event, []):
            callback(data)

# Usage
emitter = EventEmitter()
emitter.on('user_created', lambda user: send_email(user.email))
emitter.emit('user_created', {'email': 'user@example.com'})
```

---

## 🎓 LEARNING PATHS

### 🟢 Beginner Path (3-4 months, 300+ hours)

**Month 1:** Data structures, basic algorithms
**Month 2:** System design fundamentals, database basics
**Month 3:** API design, design patterns intro
**Month 4:** First architecture decisions

### 🟡 Intermediate Path (4-6 months, 400+ hours)

**Months 1-2:** System design interviews
**Months 3-4:** Microservices patterns
**Months 5-6:** UX/UI fundamentals, design systems

### 🔴 Advanced Path (6-12 months, 800+ hours)

**Module 1:** Complex system design
**Module 2:** Enterprise architecture patterns
**Module 3:** Leadership and mentoring
**Module 4:** Strategic technology planning

---

## 🛠️ ESSENTIAL TOOLS

**Design:** Figma, Adobe XD, Sketch
**Architecture:** LucidChart, Draw.io, Miro
**Prototyping:** Framer, Prototype.app
**Analysis:** Notion, Confluence, Miro

---

## 📖 BEST PRACTICES

- ✅ SOLID principles
- ✅ DRY (Don't Repeat Yourself)
- ✅ KISS (Keep It Simple, Stupid)
- ✅ Fail fast, learn faster
- ✅ Iteration and user feedback

---

## 🎯 HANDS-ON PROJECTS

**Beginner:** Implement data structures, simple API design
**Intermediate:** Design e-commerce system, build design system
**Advanced:** Multi-service system design, large-scale architecture

---

**Next Steps:** Master fundamentals, practice system design interviews!

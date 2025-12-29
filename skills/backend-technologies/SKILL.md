---
name: backend-technologies
description: Work with backend languages (Python, PHP, Go, Rust, Java, Kotlin, C++) and frameworks (Django, Laravel, Spring Boot, ASP.NET Core). Master API development, database integration, and server-side programming.
sasmp_version: "1.3.0"
bonded_agent: 01-frontend-web-development
bond_type: PRIMARY_BOND
---

# Backend Technologies Skill

## Quick Start

Backend development powers the server-side logic that applications depend on. Here's getting started:

### Python with FastAPI
```python
from fastapi import FastAPI, HTTPException
from pydantic import BaseModel
from typing import Optional

app = FastAPI()

class User(BaseModel):
    id: int
    name: str
    email: str

users_db = {}

@app.get("/users/{user_id}")
async def get_user(user_id: int):
    if user_id not in users_db:
        raise HTTPException(status_code=404, detail="User not found")
    return users_db[user_id]

@app.post("/users/")
async def create_user(user: User):
    users_db[user.id] = user
    return {"message": "User created", "user": user}
```

### Go for Performance
```go
package main

import (
    "encoding/json"
    "net/http"
    "github.com/gorilla/mux"
)

type User struct {
    ID    int    `json:"id"`
    Name  string `json:"name"`
    Email string `json:"email"`
}

func getUser(w http.ResponseWriter, r *http.Request) {
    user := User{ID: 1, Name: "Alice", Email: "alice@example.com"}
    w.Header().Set("Content-Type", "application/json")
    json.NewEncoder(w).Encode(user)
}

func main() {
    router := mux.NewRouter()
    router.HandleFunc("/users/{id}", getUser).Methods("GET")
    http.ListenAndServe(":8000", router)
}
```

### Java with Spring Boot
```java
@RestController
@RequestMapping("/api/users")
public class UserController {

    @Autowired
    private UserService userService;

    @GetMapping("/{id}")
    public ResponseEntity<User> getUser(@PathVariable Long id) {
        Optional<User> user = userService.findById(id);
        return user.map(ResponseEntity::ok)
                   .orElse(ResponseEntity.notFound().build());
    }

    @PostMapping
    public ResponseEntity<User> createUser(@RequestBody User user) {
        User created = userService.save(user);
        return ResponseEntity.status(HttpStatus.CREATED).body(created);
    }
}
```

### Rust for Safety
```rust
use actix_web::{web, App, HttpServer, HttpResponse};
use serde::{Deserialize, Serialize};

#[derive(Serialize, Deserialize, Clone)]
pub struct User {
    id: i32,
    name: String,
    email: String,
}

async fn get_user(id: web::Path<i32>) -> HttpResponse {
    let user = User {
        id: id.into_inner(),
        name: "Alice".to_string(),
        email: "alice@example.com".to_string(),
    };
    HttpResponse::Ok().json(user)
}

#[actix_web::main]
async fn main() -> std::io::Result<()> {
    HttpServer::new(|| {
        App::new()
            .route("/users/{id}", web::get().to(get_user))
    })
    .bind("127.0.0.1:8000")?
    .run()
    .await
}
```

## Key Areas

### 1. Server-Side Languages
- **Python**: Django, FastAPI, Flask
- **PHP**: Laravel, Symfony
- **Go**: Simplicity and performance
- **Rust**: Memory safety and speed
- **Java**: Enterprise applications
- **Kotlin**: Modern JVM language
- **C++**: System-level performance

### 2. Web Frameworks
- Route handling and middleware
- Request/response processing
- Template rendering
- Authentication middleware
- Error handling
- Logging and monitoring

### 3. RESTful API Design
- HTTP methods (GET, POST, PUT, DELETE)
- Status codes and error responses
- Request/response serialization (JSON/XML)
- API versioning strategies
- Rate limiting and throttling
- API documentation (Swagger/OpenAPI)

### 4. Database Integration
- SQL query optimization
- ORM usage (SQLAlchemy, Hibernate)
- Transaction management
- Connection pooling
- Migration management
- Caching strategies

### 5. Authentication & Authorization
- JWT tokens
- OAuth2 flow
- Session management
- Password hashing
- CORS handling
- Rate limiting

### 6. Testing & Quality
- Unit testing
- Integration testing
- Mock services
- Performance testing
- Load testing

## Key Concepts

### Database Transactions
```python
# Transaction management in Python
from sqlalchemy import create_engine
from sqlalchemy.orm import sessionmaker

engine = create_engine('postgresql://user:pass@localhost/db')
Session = sessionmaker(bind=engine)
session = Session()

try:
    # Make changes
    session.add(new_user)
    session.commit()
except Exception as e:
    session.rollback()
    raise e
finally:
    session.close()
```

### API Error Handling
```python
# Consistent error responses
from http import HTTPStatus

@app.exception_handler(ValueError)
async def value_error_handler(request, exc):
    return JSONResponse(
        status_code=HTTPStatus.BAD_REQUEST,
        content={
            "error": "Validation Error",
            "message": str(exc)
        }
    )
```

## Resources

- **FastAPI Docs**: https://fastapi.tiangolo.com/
- **Django Documentation**: https://www.djangoproject.com/
- **Spring Boot**: https://spring.io/projects/spring-boot
- **Go by Example**: https://gobyexample.com/
- **Rust Book**: https://doc.rust-lang.org/book/

## Real-World Projects

1. **REST API**: User management with CRUD operations
2. **Authentication System**: JWT-based auth service
3. **Data Processing**: Background job workers
4. **WebSocket Server**: Real-time communications
5. **Microservice**: Independent domain service

---

**Next Steps:** Choose a language that resonates with you, master async patterns, and practice API design.

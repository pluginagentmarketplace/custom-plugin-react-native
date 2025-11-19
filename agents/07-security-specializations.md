---
description: Master cybersecurity, compliance, blockchain, QA, and specialized technologies. Expert in application security, cryptography, vulnerability assessment, smart contracts, testing strategies, and technical communication.
capabilities:
  - Cybersecurity fundamentals and best practices
  - Application security and OWASP Top 10
  - Cryptography and encryption techniques
  - Vulnerability assessment and penetration testing
  - Blockchain and smart contract development
  - Quality assurance and testing strategies
  - Technical writing and documentation
  - Developer relations and advocacy
  - Version control and Git workflows
  - Compliance and regulatory standards
---

# 🔒 Security, Specializations & Best Practices Agent

Master cybersecurity and specialized technologies. Protect applications while mastering emerging technologies like blockchain and AI.

## 🎯 Agent Specialization

**Covers 8+ Roadmaps:** Cyber Security, Blockchain, QA, Technical Writing, DevRel, Prompt Engineering, Git & GitHub

**Mission:** Transform you from security-unaware to building hardened, compliant systems

---

## 📚 DETAILED LEARNING DOMAINS

### 1. **Security Fundamentals** (Week 1-4)

**OWASP Top 10:**
```python
# ❌ VULNERABLE: SQL Injection
query = f"SELECT * FROM users WHERE email = '{user_email}'"

# ✅ SECURE: Parameterized query
query = "SELECT * FROM users WHERE email = %s"
cursor.execute(query, (user_email,))
```

```python
# ❌ VULNERABLE: Cross-Site Scripting (XSS)
html = f"<h1>Welcome {user_input}</h1>"

# ✅ SECURE: Escape/sanitize output
from html import escape
html = f"<h1>Welcome {escape(user_input)}</h1>"
```

```python
# ❌ VULNERABLE: Hardcoded secrets
API_KEY = "sk_live_abc123xyz"

# ✅ SECURE: Environment variables
import os
API_KEY = os.getenv('API_KEY')
```

---

### 2. **Cryptography** (Week 5-12)

**Hashing:**
```python
import hashlib
from werkzeug.security import generate_password_hash, check_password_hash

# Password hashing (bcrypt)
hashed = generate_password_hash("user_password", method='pbkdf2:sha256')
is_valid = check_password_hash(hashed, "user_password")

# Never use plain MD5 or SHA1 for passwords!
# ❌ WRONG: hashlib.md5(password.encode()).hexdigest()
```

**Symmetric Encryption:**
```python
from cryptography.fernet import Fernet

# Generate key (store securely!)
key = Fernet.generate_key()
cipher = Fernet(key)

# Encrypt sensitive data
encrypted = cipher.encrypt(b"sensitive_data")
decrypted = cipher.decrypt(encrypted)
```

**Asymmetric Encryption (RSA):**
```python
from cryptography.hazmat.primitives.asymmetric import rsa
from cryptography.hazmat.primitives import hashes, serialization

# Generate key pair
private_key = rsa.generate_private_key(
    public_exponent=65537,
    key_size=2048,
)
public_key = private_key.public_key()

# Sign and verify
signature = private_key.sign(b"message", padding.PSS(...), hashes.SHA256())
public_key.verify(signature, b"message", padding.PSS(...), hashes.SHA256())
```

---

### 3. **Application Security** (Week 13-20)

**Authentication:**
```python
# JWT Token-based auth
import jwt
from datetime import datetime, timedelta

def create_token(user_id):
    payload = {
        'user_id': user_id,
        'exp': datetime.utcnow() + timedelta(hours=24),
        'iat': datetime.utcnow()
    }
    return jwt.encode(payload, 'secret_key', algorithm='HS256')

def verify_token(token):
    try:
        payload = jwt.decode(token, 'secret_key', algorithms=['HS256'])
        return payload['user_id']
    except jwt.ExpiredSignatureError:
        return None
    except jwt.InvalidTokenError:
        return None
```

**Authorization (RBAC):**
```python
from functools import wraps
from enum import Enum

class Permission(Enum):
    ADMIN = "admin"
    USER = "user"
    GUEST = "guest"

def require_permission(required):
    def decorator(func):
        @wraps(func)
        def wrapper(*args, **kwargs):
            user = get_current_user()
            if user.permission.value != required.value:
                raise PermissionError("Access denied")
            return func(*args, **kwargs)
        return wrapper
    return decorator

@require_permission(Permission.ADMIN)
def delete_user(user_id):
    # Only admins can delete users
    pass
```

**CORS Security:**
```python
from fastapi.middleware.cors import CORSMiddleware

app.add_middleware(
    CORSMiddleware,
    allow_origins=["https://trusted-domain.com"],
    allow_credentials=True,
    allow_methods=["GET", "POST"],
    allow_headers=["Content-Type"],
)
```

---

### 4. **Quality Assurance** (Week 21-28)

**Test Automation:**
```python
import pytest
from unittest.mock import Mock, patch

class TestUserService:
    @pytest.fixture
    def user_service(self):
        return UserService(db=Mock())

    def test_create_user(self, user_service):
        # Arrange
        user_service.db.insert = Mock(return_value={"id": 1, "email": "test@example.com"})

        # Act
        result = user_service.create_user("test@example.com", "password")

        # Assert
        assert result["id"] == 1
        user_service.db.insert.assert_called_once()

    def test_invalid_email(self, user_service):
        with pytest.raises(ValueError):
            user_service.create_user("invalid-email", "password")
```

**Integration Testing:**
```python
import requests
from testcontainers.postgres import PostgresContainer

def test_api_with_database():
    with PostgresContainer() as postgres:
        # Setup
        db = postgres.get_connection_client()

        # Test
        response = requests.post('http://api:8000/users', json={'email': 'test@example.com'})
        assert response.status_code == 201
```

---

### 5. **Blockchain & Web3** (Week 29-36)

**Smart Contracts (Solidity):**
```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract UserRegistry {
    struct User {
        address wallet;
        string name;
        uint256 joinDate;
    }

    mapping(address => User) public users;

    event UserRegistered(address indexed wallet, string name);

    function registerUser(string memory _name) public {
        require(bytes(_name).length > 0, "Name cannot be empty");
        require(users[msg.sender].wallet == address(0), "User already registered");

        users[msg.sender] = User({
            wallet: msg.sender,
            name: _name,
            joinDate: block.timestamp
        });

        emit UserRegistered(msg.sender, _name);
    }
}
```

**Web3.py Interaction:**
```python
from web3 import Web3

# Connect to blockchain
w3 = Web3(Web3.HTTPProvider('http://localhost:8545'))

# Contract interaction
contract = w3.eth.contract(address=contract_address, abi=contract_abi)
result = contract.functions.registerUser("Alice").call()

# Transaction
account = w3.eth.account.from_key(private_key)
tx = contract.functions.registerUser("Bob").build_transaction({
    'from': account.address,
    'nonce': w3.eth.get_transaction_count(account.address),
})
signed_tx = account.sign_transaction(tx)
tx_hash = w3.eth.send_raw_transaction(signed_tx.rawTransaction)
```

---

### 6. **Technical Writing** (Week 37-40)

**API Documentation:**
```yaml
openapi: 3.0.0
info:
  title: User API
  version: 1.0.0
  description: User management API

paths:
  /users:
    get:
      summary: List all users
      responses:
        '200':
          description: List of users
          content:
            application/json:
              schema:
                type: array
                items:
                  $ref: '#/components/schemas/User'
    post:
      summary: Create a new user
      requestBody:
        required: true
        content:
          application/json:
            schema:
              $ref: '#/components/schemas/UserInput'
      responses:
        '201':
          description: User created
```

**Architecture Documentation (ADR):**
```markdown
# Architecture Decision Record (ADR)

## ADR-001: Microservices Architecture

### Context
Monolithic app becoming hard to scale. Team growth requires independent deployment.

### Decision
Adopt microservices architecture with API gateway.

### Consequences
Positive:
- Independent scaling
- Faster deployments
- Technology flexibility

Negative:
- Distributed system complexity
- Data consistency challenges
- Operational overhead
```

---

### 7. **Git & GitHub Workflows** (Week 41-44)

**Feature Branch Workflow:**
```bash
# Create feature branch
git checkout -b feature/user-authentication

# Commit changes
git add .
git commit -m "feat: implement JWT authentication"

# Push and create PR
git push origin feature/user-authentication

# Merge after review
git merge feature/user-authentication
```

**Commit Message Convention:**
```
feat: add user authentication
fix: resolve database connection leak
docs: update API documentation
test: add integration tests
refactor: reorganize module structure
perf: optimize database queries
ci: add GitHub Actions workflow
```

---

## 🎓 LEARNING PATHS

### 🟢 Beginner Path (3-4 months, 300+ hours)

**Month 1:** Security fundamentals, OWASP Top 10
**Month 2:** Cryptography basics, authentication
**Month 3:** QA fundamentals, testing strategies
**Month 4:** First secure application

### 🟡 Intermediate Path (4-6 months, 400+ hours)

**Months 1-2:** Penetration testing, vulnerability assessment
**Months 3-4:** Blockchain basics, smart contracts
**Months 5-6:** Security compliance, advanced testing

### 🔴 Advanced Path (6-12 months, 800+ hours)

**Module 1:** Security architecture and design
**Module 2:** Blockchain and Web3 advanced
**Module 3:** DevRel and technical advocacy
**Module 4:** Security consulting and auditing

---

## 🛠️ ESSENTIAL TOOLS

**Security:** Burp Suite, OWASP ZAP, Snyk, Dependabot
**Testing:** Jest, Pytest, Cypress, Selenium
**Blockchain:** MetaMask, Hardhat, Remix IDE, Truffle
**Documentation:** Swagger/OpenAPI, Confluence, GitHub Wiki

---

## 📖 BEST PRACTICES

- ✅ Never hardcode secrets
- ✅ Keep dependencies updated
- ✅ Use security headers
- ✅ Regular security audits
- ✅ Least privilege principle
- ✅ Comprehensive logging
- ✅ Incident response plan

---

## 🎯 HANDS-ON PROJECTS

**Beginner:** Implement auth system, write security tests, deploy secure app
**Intermediate:** Build blockchain dapp, security audit, complete test suite
**Advanced:** Enterprise security architecture, blockchain protocol, DevRel strategy

---

**Next Steps:** Start with OWASP Top 10, master authentication, then specialize!

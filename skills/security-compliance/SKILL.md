---
name: security-compliance
description: Implement security best practices, cryptography, application security, and compliance standards. Protect applications from vulnerabilities and ensure data privacy.
---

# Security & Compliance Skill

## Quick Start

Security is fundamental to every application. Here's how to build secure systems:

### Common Vulnerabilities (OWASP Top 10)

```python
# ❌ VULNERABLE: SQL Injection
user_id = request.args.get('id')
query = f"SELECT * FROM users WHERE id = {user_id}"  # DANGEROUS!

# ✅ SECURE: Parameterized Query
from sqlalchemy import text
query = text("SELECT * FROM users WHERE id = :id")
result = db.execute(query, {"id": user_id})
```

```python
# ❌ VULNERABLE: Cross-Site Scripting (XSS)
user_input = request.args.get('name')
html = f"<h1>Hello {user_input}</h1>"  # DANGEROUS!

# ✅ SECURE: Escape Output
from flask import escape
html = f"<h1>Hello {escape(user_input)}</h1>"
```

```python
# ❌ VULNERABLE: Hardcoded Credentials
DB_PASSWORD = "super_secret_password_123"

# ✅ SECURE: Environment Variables
import os
DB_PASSWORD = os.getenv('DB_PASSWORD')
```

### Encryption & Hashing

```python
# Password hashing
from werkzeug.security import generate_password_hash, check_password_hash

# Store hashed password
password_hash = generate_password_hash("user_password")

# Verify password
if check_password_hash(password_hash, "user_password"):
    print("Password is correct")
```

```python
# Symmetric encryption (AES)
from cryptography.fernet import Fernet

# Generate key
key = Fernet.generate_key()
cipher = Fernet(key)

# Encrypt data
encrypted = cipher.encrypt(b"sensitive data")
decrypted = cipher.decrypt(encrypted)
```

```python
# Asymmetric encryption (RSA)
from cryptography.hazmat.primitives.asymmetric import rsa
from cryptography.hazmat.primitives import serialization

# Generate key pair
private_key = rsa.generate_private_key(
    public_exponent=65537,
    key_size=2048,
)
public_key = private_key.public_key()

# Sign message
from cryptography.hazmat.primitives import hashes
from cryptography.hazmat.primitives.asymmetric import padding

signature = private_key.sign(
    b"message",
    padding.PSS(
        mgf=padding.MGF1(hashes.SHA256()),
        salt_length=padding.PSS.MAX_LENGTH
    ),
    hashes.SHA256()
)
```

### Authentication & Authorization

```python
# JWT Token Authentication
import jwt
from datetime import datetime, timedelta

# Generate token
payload = {
    'user_id': 123,
    'exp': datetime.utcnow() + timedelta(hours=24),
    'iat': datetime.utcnow()
}
token = jwt.encode(payload, 'secret_key', algorithm='HS256')

# Verify token
try:
    decoded = jwt.decode(token, 'secret_key', algorithms=['HS256'])
    user_id = decoded['user_id']
except jwt.ExpiredSignatureError:
    print("Token has expired")
except jwt.InvalidTokenError:
    print("Invalid token")
```

```python
# Role-based access control (RBAC)
from enum import Enum

class Role(Enum):
    ADMIN = "admin"
    USER = "user"
    GUEST = "guest"

def require_role(required_role):
    def decorator(func):
        def wrapper(*args, **kwargs):
            current_user_role = get_current_user_role()
            if current_user_role.value not in [required_role.value]:
                raise PermissionError("Access denied")
            return func(*args, **kwargs)
        return wrapper
    return decorator

@require_role(Role.ADMIN)
def delete_user(user_id):
    # Only admins can delete users
    pass
```

### API Security

```python
# CORS configuration
from flask_cors import CORS

app = Flask(__name__)
CORS(app,
    resources={r"/api/*": {"origins": ["https://trusted-domain.com"]}},
    methods=["GET", "POST"],
    max_age=3600
)

# Rate limiting
from flask_limiter import Limiter

limiter = Limiter(app)

@app.route("/api/login", methods=["POST"])
@limiter.limit("5 per minute")
def login():
    # Only 5 login attempts per minute
    pass

# HTTPS enforced
@app.before_request
def enforce_https():
    if not request.is_secure:
        return redirect(request.url.replace('http://', 'https://'))
```

## Key Areas

### 1. Secure Coding
- Input validation and sanitization
- Output encoding
- Authentication implementation
- Authorization checking
- Error handling without information leakage
- Secure logging (no sensitive data)
- Dependency management

### 2. Cryptography
- Encryption algorithms (AES, RSA)
- Hashing functions (bcrypt, Argon2)
- Digital signatures
- Key management
- TLS/SSL protocols
- Certificate management
- Post-quantum cryptography

### 3. Application Security
- OWASP Top 10 vulnerabilities
- Injection attacks (SQL, command)
- Cross-Site Scripting (XSS)
- Cross-Site Request Forgery (CSRF)
- Broken authentication
- Sensitive data exposure
- XML External Entities (XXE)
- Broken access control
- Security misconfiguration
- Using components with known vulnerabilities

### 4. Network Security
- Firewalls and network segmentation
- VPNs and tunneling
- DDoS protection
- Intrusion detection systems
- Network monitoring
- Port security
- Wireless security

### 5. Data Privacy
- GDPR compliance
- Data encryption at rest and in transit
- Data retention policies
- Privacy by design
- Data classification
- Personally Identifiable Information (PII) protection

### 6. Compliance & Standards
- PCI-DSS (payment processing)
- HIPAA (healthcare)
- SOC 2 compliance
- ISO 27001 (information security)
- NIST Cybersecurity Framework
- CIS Controls

## Advanced Concepts

### Web Application Firewall (WAF) Rules
```
// Block SQL injection attempts
SecRule ARGS "@rx (union|select|insert|delete|update|drop)" \
    "id:1,phase:2,deny,status:403"

// Block XSS attempts
SecRule ARGS "@rx <script" \
    "id:2,phase:2,deny,status:403"

// Rate limiting
SecAction "id:3,phase:1,initcol:ip=%{REMOTE_ADDR},pass"
SecRule IP:blocked "@eq 1" \
    "id:4,phase:2,deny,status:429"
```

### OAuth2 Authorization Flow
```
User → Application → OAuth Provider → Token → API
```

## Resources

- **OWASP Top 10**: https://owasp.org/www-project-top-ten/
- **OWASP Cheat Sheets**: https://cheatsheetseries.owasp.org/
- **CWE Top 25**: https://cwe.mitre.org/top25/
- **Cryptography.io**: https://cryptography.io/
- **Security Best Practices**: https://www.owasp.org/

## Real-World Projects

1. **Secure Login System**: Password hashing and sessions
2. **API Authentication**: JWT token implementation
3. **Data Encryption**: Encrypt sensitive information
4. **Penetration Testing**: Identify vulnerabilities
5. **Compliance Audit**: Verify security standards
6. **Incident Response**: Handle security breaches

---

**Next Steps:** Learn OWASP Top 10, implement secure authentication, and practice threat modeling.

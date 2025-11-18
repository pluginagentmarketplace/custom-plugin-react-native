---
description: Master cloud platforms, containerization, orchestration, and infrastructure automation. Expert in AWS, Docker, Kubernetes, Terraform, CI/CD, monitoring, and production deployment.
capabilities:
  - AWS cloud platform and services
  - Docker containerization and optimization
  - Kubernetes orchestration at scale
  - Terraform infrastructure as code
  - CI/CD pipeline design and automation
  - Linux system administration
  - Database management (PostgreSQL, MongoDB, Redis)
  - Monitoring and observability (Prometheus, Grafana)
  - Security and compliance
  - Disaster recovery and high availability
---

# ☁️ DevOps, Cloud & Infrastructure Agent

Master cloud deployment and infrastructure management. Build scalable, reliable systems supporting millions of users globally.

## 🎯 Agent Specialization

**Covers 12+ Roadmaps:** DevOps, AWS, Cloudflare, Linux, Terraform, Docker, Kubernetes, PostgreSQL, SQL, Redis, MongoDB

**Mission:** Transform you from basic Linux to architecting enterprise-grade production systems

---

## 📚 DETAILED LEARNING DOMAINS

### 1. **Linux & System Administration** (Week 1-4)

**Essential Commands:**
```bash
# File and directory management
ls -la              # List with permissions
cd, pwd, mkdir      # Navigation and creation
cp, mv, rm          # File operations
chmod, chown        # Permissions

# User management
useradd user1       # Create user
passwd user1        # Set password
sudo visudo         # Edit sudo rules

# Package management
apt update && apt upgrade  # Debian/Ubuntu
yum install package        # RHEL/CentOS

# Processes and monitoring
ps aux              # List processes
top, htop          # Real-time monitoring
systemctl start service  # Service management

# Networking
netstat -an        # Network statistics
ss -an             # Socket stats
iptables           # Firewall rules
```

**Scripting:**
```bash
#!/bin/bash

# Variables and arrays
NAME="devops"
SERVERS=("server1" "server2" "server3")

# Loops and conditionals
for server in "${SERVERS[@]}"; do
    if ssh "$server" 'systemctl is-active myservice' > /dev/null; then
        echo "$server is healthy"
    else
        echo "$server is down"
    fi
done

# Functions
deploy() {
    local service=$1
    echo "Deploying $service..."
    systemctl restart "$service"
}
```

---

### 2. **Docker Containerization** (Week 5-8)

**Dockerfile Optimization:**
```dockerfile
# Multi-stage build for minimal size
FROM python:3.11 as builder
WORKDIR /app
COPY requirements.txt .
RUN pip install --user --no-cache-dir -r requirements.txt

FROM python:3.11-slim
WORKDIR /app
COPY --from=builder /root/.local /root/.local
COPY . .

ENV PATH=/root/.local/bin:$PATH
EXPOSE 8000
CMD ["python", "-m", "uvicorn", "main:app", "--host", "0.0.0.0"]
```

**Docker Compose:**
```yaml
version: '3.8'
services:
  api:
    build: .
    ports:
      - "8000:8000"
    environment:
      DATABASE_URL: postgresql://user:pass@db:5432/myapp
    depends_on:
      - db
    healthcheck:
      test: ["CMD", "curl", "-f", "http://localhost:8000/health"]
      interval: 30s
      timeout: 10s
      retries: 3

  db:
    image: postgres:15
    environment:
      POSTGRES_USER: user
      POSTGRES_PASSWORD: pass
      POSTGRES_DB: myapp
    volumes:
      - postgres_data:/var/lib/postgresql/data
    healthcheck:
      test: ["CMD-SHELL", "pg_isready -U user"]
      interval: 10s
      timeout: 5s
      retries: 5

  redis:
    image: redis:7-alpine
    ports:
      - "6379:6379"

volumes:
  postgres_data:
```

---

### 3. **Kubernetes Orchestration** (Week 9-16)

**Deployment Manifest:**
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: api-server
  namespace: production
spec:
  replicas: 3
  selector:
    matchLabels:
      app: api-server
  template:
    metadata:
      labels:
        app: api-server
    spec:
      containers:
      - name: api
        image: myregistry.azurecr.io/api:v1.0.0
        ports:
        - containerPort: 8000
        env:
        - name: DATABASE_URL
          valueFrom:
            secretKeyRef:
              name: db-credentials
              key: url
        resources:
          requests:
            cpu: 100m
            memory: 128Mi
          limits:
            cpu: 500m
            memory: 512Mi
        livenessProbe:
          httpGet:
            path: /health
            port: 8000
          initialDelaySeconds: 30
          periodSeconds: 10
        readinessProbe:
          httpGet:
            path: /ready
            port: 8000
          initialDelaySeconds: 5
          periodSeconds: 5

---
apiVersion: v1
kind: Service
metadata:
  name: api-service
spec:
  selector:
    app: api-server
  type: LoadBalancer
  ports:
  - protocol: TCP
    port: 80
    targetPort: 8000
```

**Helm Chart:**
```yaml
# values.yaml
image:
  repository: myapp
  tag: "1.0.0"
replicas: 3
resources:
  limits:
    cpu: 500m
    memory: 512Mi
```

---

### 4. **Terraform Infrastructure as Code** (Week 17-20)

**AWS Infrastructure:**
```hcl
terraform {
  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "~> 5.0"
    }
  }
  backend "s3" {
    bucket = "terraform-state"
    key    = "prod/terraform.tfstate"
    region = "us-east-1"
  }
}

provider "aws" {
  region = "us-east-1"
}

# VPC
resource "aws_vpc" "main" {
  cidr_block           = "10.0.0.0/16"
  enable_dns_hostnames = true
  tags = { Name = "main-vpc" }
}

# Security Group
resource "aws_security_group" "app" {
  vpc_id = aws_vpc.main.id

  ingress {
    from_port   = 80
    to_port     = 80
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"]
  }

  egress {
    from_port   = 0
    to_port     = 0
    protocol    = "-1"
    cidr_blocks = ["0.0.0.0/0"]
  }
}

# RDS Database
resource "aws_db_instance" "main" {
  allocated_storage    = 20
  storage_type         = "gp3"
  engine               = "postgres"
  engine_version       = "15.3"
  instance_class       = "db.t4g.micro"
  username             = "admin"
  password             = var.db_password
  skip_final_snapshot  = false
}

# EC2 Instance
resource "aws_instance" "app" {
  ami             = "ami-0c55b159cbfafe1f0"
  instance_type   = "t3.micro"
  security_groups = [aws_security_group.app.id]
}

# Output
output "app_ip" {
  value = aws_instance.app.public_ip
}
```

---

### 5. **CI/CD Pipelines** (Week 21-24)

**GitHub Actions:**
```yaml
name: Deploy

on:
  push:
    branches: [main]

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3

      - name: Set up Python
        uses: actions/setup-python@v4
        with:
          python-version: '3.11'

      - name: Install dependencies
        run: |
          python -m pip install --upgrade pip
          pip install -r requirements.txt

      - name: Run tests
        run: pytest

      - name: Build Docker image
        run: |
          docker build -t myapp:${{ github.sha }} .
          docker tag myapp:${{ github.sha }} myapp:latest

      - name: Push to registry
        env:
          REGISTRY_PASSWORD: ${{ secrets.REGISTRY_PASSWORD }}
        run: |
          echo $REGISTRY_PASSWORD | docker login -u myuser --password-stdin
          docker push myapp:latest

      - name: Deploy to Kubernetes
        run: |
          kubectl set image deployment/api api=myapp:latest
          kubectl rollout status deployment/api
```

---

### 6. **Monitoring & Observability** (Week 25-28)

**Prometheus & Grafana:**
```yaml
# prometheus.yml
global:
  scrape_interval: 15s

scrape_configs:
  - job_name: 'api-server'
    static_configs:
      - targets: ['localhost:8000']
```

**Application Metrics:**
```python
from prometheus_client import Counter, Histogram, generate_latest

request_count = Counter('http_requests_total', 'Total requests', ['method', 'endpoint'])
request_duration = Histogram('http_request_duration_seconds', 'Request duration')

@app.middleware("http")
async def add_metrics(request, call_next):
    request_count.labels(method=request.method, endpoint=request.url.path).inc()
    with request_duration.time():
        response = await call_next(request)
    return response

@app.get("/metrics")
async def metrics():
    return generate_latest()
```

---

## 🎓 LEARNING PATHS

### 🟢 Beginner Path (3-4 months, 300+ hours)

**Month 1:** Linux fundamentals, Git basics, Docker intro
**Month 2:** Docker mastery, basic Kubernetes
**Month 3:** Cloud platform (AWS) basics, CI/CD intro
**Month 4:** First deployment to production

### 🟡 Intermediate Path (4-6 months, 400+ hours)

**Months 1-2:** Kubernetes advanced patterns
**Months 3-4:** Terraform infrastructure as code
**Months 5-6:** Monitoring, security, multi-cloud

### 🔴 Advanced Path (6-12 months, 800+ hours)

**Module 1:** Kubernetes at scale
**Module 2:** Infrastructure automation and GitOps
**Module 3:** Advanced networking and security
**Module 4:** Cost optimization and multi-cloud

---

## 🛠️ ESSENTIAL TOOLS

- **Containers:** Docker, Podman
- **Orchestration:** Kubernetes, Docker Swarm
- **IaC:** Terraform, CloudFormation
- **CI/CD:** GitHub Actions, GitLab CI, Jenkins
- **Monitoring:** Prometheus, ELK Stack, DataDog
- **Cloud:** AWS, Azure, GCP

---

## 📖 BEST PRACTICES

- ✅ Infrastructure as Code for all infra
- ✅ Automated testing in CI/CD
- ✅ Blue-green or canary deployments
- ✅ Comprehensive monitoring and alerting
- ✅ Disaster recovery planning

---

## 🎯 HANDS-ON PROJECTS

**Beginner:** Deploy app in Docker, K8s single node, Terraform basic infra
**Intermediate:** Multi-service K8s, GitOps pipeline, Multi-region setup
**Advanced:** Highly available system, Auto-scaling, Disaster recovery

---

**Next Steps:** Start with Linux, progress to Docker, then Kubernetes!

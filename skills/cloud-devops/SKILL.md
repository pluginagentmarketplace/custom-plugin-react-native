---
name: cloud-devops
description: Deploy and manage infrastructure using AWS, Docker, Kubernetes, Terraform, and CI/CD pipelines. Master cloud platforms, containerization, and infrastructure as code.
---

# Cloud & DevOps Skill

## Quick Start

DevOps enables rapid and reliable deployment of applications. Here's how to get started:

### Docker Containerization
```dockerfile
# Dockerfile for a Node.js application
FROM node:18-alpine

WORKDIR /app

COPY package*.json ./
RUN npm ci --only=production

COPY . .

EXPOSE 3000

CMD ["node", "server.js"]
```

```bash
# Build and run image
docker build -t my-app:1.0.0 .
docker run -p 3000:3000 my-app:1.0.0
```

### Kubernetes Deployment
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: my-app
spec:
  replicas: 3
  selector:
    matchLabels:
      app: my-app
  template:
    metadata:
      labels:
        app: my-app
    spec:
      containers:
      - name: my-app
        image: my-app:1.0.0
        ports:
        - containerPort: 3000
        env:
        - name: NODE_ENV
          value: "production"
        resources:
          limits:
            memory: "256Mi"
            cpu: "250m"
---
apiVersion: v1
kind: Service
metadata:
  name: my-app-service
spec:
  type: LoadBalancer
  ports:
  - port: 80
    targetPort: 3000
  selector:
    app: my-app
```

### Terraform Infrastructure as Code
```hcl
# AWS provider configuration
terraform {
  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "~> 5.0"
    }
  }
}

provider "aws" {
  region = "us-east-1"
}

# VPC and networking
resource "aws_vpc" "main" {
  cidr_block           = "10.0.0.0/16"
  enable_dns_hostnames = true
}

resource "aws_subnet" "main" {
  vpc_id            = aws_vpc.main.id
  cidr_block        = "10.0.1.0/24"
  availability_zone = "us-east-1a"
}

# EC2 instance
resource "aws_instance" "web" {
  ami           = "ami-0c55b159cbfafe1f0"
  instance_type = "t2.micro"
  subnet_id     = aws_subnet.main.id

  tags = {
    Name = "web-server"
  }
}

# Output
output "instance_ip" {
  value = aws_instance.web.public_ip
}
```

### GitHub Actions CI/CD
```yaml
name: Deploy Application

on:
  push:
    branches: [main]
  pull_request:
    branches: [main]

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
    - uses: actions/checkout@v2

    - name: Set up Node.js
      uses: actions/setup-node@v2
      with:
        node-version: '18'

    - name: Install dependencies
      run: npm ci

    - name: Run tests
      run: npm test

    - name: Build application
      run: npm run build

    - name: Deploy to AWS
      env:
        AWS_ACCESS_KEY_ID: ${{ secrets.AWS_ACCESS_KEY_ID }}
        AWS_SECRET_ACCESS_KEY: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
      run: |
        aws s3 sync dist/ s3://my-bucket/
```

## Key Areas

### 1. Linux System Administration
- File systems and permissions
- User and group management
- Package management (apt, yum)
- System monitoring and processes
- Shell scripting (bash)
- Network configuration
- Log management

### 2. Containerization
- Docker fundamentals
- Dockerfile best practices
- Image optimization
- Docker Compose
- Container networking
- Container security
- Image registries (Docker Hub, ECR)

### 3. Kubernetes
- Pod management
- Deployments and ReplicaSets
- Services and networking
- ConfigMaps and Secrets
- Persistent volumes
- Helm package manager
- Autoscaling
- Resource limits

### 4. Cloud Platforms
- **AWS**: EC2, S3, RDS, VPC, Lambda, CloudWatch
- **Azure**: Virtual Machines, App Service, SQL Database
- **GCP**: Compute Engine, Cloud Storage, BigQuery
- IAM and security
- Cost optimization

### 5. Infrastructure as Code
- Terraform fundamentals
- Terraform state management
- Modules and composition
- Variables and outputs
- Workspace management
- Version control integration

### 6. CI/CD Pipeline
- Version control workflow (Git)
- Automated testing
- Build automation
- Artifact management
- Deployment automation
- Release management
- Monitoring and alerting

## Advanced Concepts

### Docker Multi-Stage Build
```dockerfile
FROM node:18 AS builder
WORKDIR /app
COPY package*.json ./
RUN npm ci

FROM node:18-alpine
WORKDIR /app
COPY --from=builder /app/node_modules ./node_modules
COPY . .
CMD ["node", "server.js"]
```

### Kubernetes StatefulSet
```yaml
apiVersion: apps/v1
kind: StatefulSet
metadata:
  name: mysql
spec:
  serviceName: mysql
  replicas: 3
  selector:
    matchLabels:
      app: mysql
  template:
    metadata:
      labels:
        app: mysql
    spec:
      containers:
      - name: mysql
        image: mysql:8.0
        ports:
        - containerPort: 3306
        volumeMounts:
        - name: data
          mountPath: /var/lib/mysql
  volumeClaimTemplates:
  - metadata:
      name: data
    spec:
      accessModes: ["ReadWriteOnce"]
      resources:
        requests:
          storage: 10Gi
```

### Terraform Modules
```hcl
# modules/web-server/main.tf
resource "aws_security_group" "web" {
  name = "${var.app_name}-web"

  ingress {
    from_port   = 80
    to_port     = 80
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"]
  }
}

resource "aws_instance" "web" {
  ami           = var.ami_id
  instance_type = var.instance_type
  security_groups = [aws_security_group.web.name]
}

# Output
output "public_ip" {
  value = aws_instance.web.public_ip
}
```

## Resources

- **Docker Documentation**: https://docs.docker.com/
- **Kubernetes Documentation**: https://kubernetes.io/docs/
- **Terraform Documentation**: https://www.terraform.io/docs/
- **AWS Documentation**: https://docs.aws.amazon.com/
- **GitHub Actions**: https://github.com/features/actions

## Real-World Projects

1. **Dockerize Application**: Containerize an existing app
2. **Kubernetes Deployment**: Deploy multi-container app
3. **Infrastructure as Code**: Provision AWS resources with Terraform
4. **CI/CD Pipeline**: Automate testing and deployment
5. **Monitoring Stack**: Set up observability with Prometheus/Grafana
6. **Disaster Recovery**: Implement backup and recovery strategies

---

**Next Steps:** Start with Docker and basic Linux, master Kubernetes, then explore infrastructure as code and CI/CD.

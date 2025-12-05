<div align="center">

# infrastructure-templates

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Terraform](https://img.shields.io/badge/Terraform-1.6+-purple)](https://www.terraform.io/)
[![Kubernetes](https://img.shields.io/badge/Kubernetes-1.28+-blue)](https://kubernetes.io/)
[![Docker](https://img.shields.io/badge/Docker-Ready-blue)](https://www.docker.com/)

**Production-ready infrastructure templates and configurations for rapid deployment**

[Terraform](#-terraform) • [Kubernetes](#-kubernetes) • [Docker](#-docker) • [CI/CD](#-cicd)

</div>

---

## 🎯 Overview

Comprehensive Infrastructure as Code (IaC) templates for deploying production-grade systems across AWS, GCP, Azure, and on-premises environments.

## ✨ Features

- 🏛️ **Multi-Cloud** - AWS, GCP, Azure, DigitalOcean
- 🚀 **Production-Ready** - Battle-tested configurations
- 🔒 **Security Hardened** - Best practices built-in
- 📦 **Modular Design** - Reusable components
- 🔄 **GitOps Ready** - CI/CD integrated
- 📊 **Monitoring Included** - Prometheus, Grafana, ELK
- ⚙️ **Auto-Scaling** - Dynamic resource management
- 📄 **Well Documented** - Complete usage guides

## 📁 Structure

```
infrastructure-templates/
├── terraform/
│   ├── aws/
│   │   ├── vpc/
│   │   ├── eks/
│   │   ├── rds/
│   │   └── s3/
│   ├── gcp/
│   │   ├── gke/
│   │   └── cloud-sql/
│   ├── azure/
│   └── modules/
├── kubernetes/
│   ├── applications/
│   ├── ingress/
│   ├── monitoring/
│   ├── logging/
│   └── security/
├── docker/
│   ├── base-images/
│   ├── applications/
│   └── utilities/
├── ansible/
│   ├── playbooks/
│   └── roles/
└── cicd/
    ├── github-actions/
    ├── gitlab-ci/
    └── jenkins/
```

## 🏛️ Terraform

### AWS Templates

#### EKS Cluster
```hcl
module "eks" {
  source = "./terraform/aws/eks"
  
  cluster_name    = "production"
  cluster_version = "1.28"
  vpc_id          = module.vpc.vpc_id
  subnet_ids      = module.vpc.private_subnets
  
  node_groups = {
    general = {
      desired_size = 3
      min_size     = 2
      max_size     = 10
      instance_types = ["t3.large"]
    }
  }
}
```

#### RDS Database
```hcl
module "rds" {
  source = "./terraform/aws/rds"
  
  engine         = "postgres"
  engine_version = "15.3"
  instance_class = "db.t3.medium"
  allocated_storage = 100
  
  db_name  = "production"
  username = var.db_username
  password = var.db_password
  
  backup_retention_period = 7
  multi_az = true
}
```

### GCP Templates

#### GKE Cluster
```hcl
module "gke" {
  source = "./terraform/gcp/gke"
  
  project_id = var.project_id
  region     = "us-central1"
  
  cluster_name = "production-gke"
  
  node_pools = [
    {
      name         = "default-pool"
      machine_type = "n1-standard-2"
      min_count    = 2
      max_count    = 10
    }
  ]
}
```

## ☸️ Kubernetes

### Application Deployment

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: web-app
  namespace: production
spec:
  replicas: 3
  selector:
    matchLabels:
      app: web-app
  template:
    metadata:
      labels:
        app: web-app
    spec:
      containers:
      - name: web-app
        image: garrettc123/web-app:latest
        ports:
        - containerPort: 8080
        resources:
          requests:
            memory: "256Mi"
            cpu: "250m"
          limits:
            memory: "512Mi"
            cpu: "500m"
        livenessProbe:
          httpGet:
            path: /health
            port: 8080
          initialDelaySeconds: 30
          periodSeconds: 10
```

### Ingress Configuration

```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: web-app-ingress
  annotations:
    cert-manager.io/cluster-issuer: letsencrypt-prod
    nginx.ingress.kubernetes.io/ssl-redirect: "true"
spec:
  tls:
  - hosts:
    - app.garrettc123.com
    secretName: web-app-tls
  rules:
  - host: app.garrettc123.com
    http:
      paths:
      - path: /
        pathType: Prefix
        backend:
          service:
            name: web-app
            port:
              number: 80
```

## 🐳 Docker

### Base Images

```dockerfile
# Python Base Image
FROM python:3.11-slim

WORKDIR /app

# Install dependencies
RUN apt-get update && apt-get install -y \
    build-essential \
    libpq-dev \
    && rm -rf /var/lib/apt/lists/*

COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

COPY . .

EXPOSE 8000

CMD ["python", "app.py"]
```

### Multi-Stage Build

```dockerfile
# Build stage
FROM node:18-alpine AS builder
WORKDIR /app
COPY package*.json ./
RUN npm ci
COPY . .
RUN npm run build

# Production stage
FROM nginx:alpine
COPY --from=builder /app/dist /usr/share/nginx/html
COPY nginx.conf /etc/nginx/nginx.conf
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
```

## 🔄 CI/CD

### GitHub Actions

```yaml
name: Deploy to Production

on:
  push:
    branches: [ main ]

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      
      - name: Configure AWS credentials
        uses: aws-actions/configure-aws-credentials@v2
        with:
          aws-access-key-id: ${{ secrets.AWS_ACCESS_KEY_ID }}
          aws-secret-access-key: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
          aws-region: us-east-1
      
      - name: Setup Terraform
        uses: hashicorp/setup-terraform@v2
      
      - name: Terraform Init
        run: terraform init
      
      - name: Terraform Plan
        run: terraform plan
      
      - name: Terraform Apply
        run: terraform apply -auto-approve
```

## 📊 Monitoring Stack

### Prometheus + Grafana

```yaml
# Deploy monitoring stack
kubectl apply -f kubernetes/monitoring/prometheus/
kubectl apply -f kubernetes/monitoring/grafana/
kubectl apply -f kubernetes/monitoring/alertmanager/
```

### Logging Stack (ELK)

```yaml
# Deploy ELK stack
kubectl apply -f kubernetes/logging/elasticsearch/
kubectl apply -f kubernetes/logging/logstash/
kubectl apply -f kubernetes/logging/kibana/
```

## 🚀 Quick Start

### Deploy AWS Infrastructure

```bash
cd terraform/aws/eks
terraform init
terraform plan
terraform apply
```

### Deploy Application to Kubernetes

```bash
kubectl apply -f kubernetes/applications/web-app/
kubectl apply -f kubernetes/ingress/
```

### Build and Push Docker Image

```bash
docker build -t garrettc123/app:latest .
docker push garrettc123/app:latest
```

## 📚 Documentation

- [Terraform Guide](docs/terraform.md)
- [Kubernetes Guide](docs/kubernetes.md)
- [Docker Guide](docs/docker.md)
- [CI/CD Setup](docs/cicd.md)
- [Security Best Practices](docs/security.md)

## 🔒 Security

- TLS/SSL everywhere
- Network policies
- Pod security policies
- Secret management with Vault
- RBAC configuration
- Security scanning in CI/CD

## 📄 License

MIT License - See [LICENSE](LICENSE)

## 🔗 Related Projects

**Garrettc123 Ecosystem:**
- [documentation-hub](https://github.com/Garrettc123/documentation-hub)
- [enterprise-devops-platform](https://github.com/Garrettc123/enterprise-devops-platform)
- [All Repositories](https://github.com/Garrettc123?tab=repositories)

---

<div align="center">

Made with ❤️ by [Garrettc123](https://github.com/Garrettc123)

Last Updated: 2025-12-05

</div>
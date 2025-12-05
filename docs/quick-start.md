# Quick Start Guide

## Prerequisites

- Terraform >= 1.6
- kubectl >= 1.28
- Docker >= 24.0
- Cloud provider CLI (AWS CLI, gcloud, az)

## Step 1: Clone Repository

```bash
git clone https://github.com/Garrettc123/infrastructure-templates.git
cd infrastructure-templates
```

## Step 2: Choose Your Stack

### AWS + EKS
```bash
cd terraform/aws/eks
cp terraform.tfvars.example terraform.tfvars
# Edit terraform.tfvars with your values
terraform init
terraform apply
```

### GCP + GKE
```bash
cd terraform/gcp/gke
cp terraform.tfvars.example terraform.tfvars
terraform init
terraform apply
```

## Step 3: Deploy Application

```bash
# Update kubeconfig
aws eks update-kubeconfig --name production-cluster

# Deploy application
kubectl apply -f kubernetes/applications/web/

# Check deployment
kubectl get pods
```

## Step 4: Set Up Monitoring

```bash
kubectl apply -f kubernetes/monitoring/prometheus/
kubectl apply -f kubernetes/monitoring/grafana/

# Access Grafana
kubectl port-forward -n monitoring svc/grafana 3000:80
```

## Step 5: Configure CI/CD

```bash
# Copy GitHub Actions workflow
cp cicd/github-actions/deploy.yml .github/workflows/

# Add secrets to GitHub repository
# AWS_ACCESS_KEY_ID
# AWS_SECRET_ACCESS_KEY
# KUBE_CONFIG
```

## Next Steps

- [Configure monitoring alerts](monitoring.md)
- [Set up logging](logging.md)
- [Security hardening](security.md)
- [Cost optimization](cost-optimization.md)

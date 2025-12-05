# Terraform Templates

## AWS

### Compute
- [EC2 Instances](aws/ec2/)
- [EKS Cluster](aws/eks/)
- [Lambda Functions](aws/lambda/)

### Networking
- [VPC](aws/vpc/)
- [Load Balancer](aws/alb/)
- [API Gateway](aws/api-gateway/)

### Storage
- [S3 Buckets](aws/s3/)
- [EBS Volumes](aws/ebs/)
- [EFS](aws/efs/)

### Database
- [RDS](aws/rds/)
- [DynamoDB](aws/dynamodb/)
- [ElastiCache](aws/elasticache/)

## GCP

### Compute
- [GKE Cluster](gcp/gke/)
- [Compute Engine](gcp/compute/)
- [Cloud Run](gcp/cloud-run/)

### Database
- [Cloud SQL](gcp/cloud-sql/)
- [Firestore](gcp/firestore/)

## Azure

### Compute
- [AKS Cluster](azure/aks/)
- [Virtual Machines](azure/vm/)

### Database
- [Azure SQL](azure/sql/)
- [Cosmos DB](azure/cosmos/)

## Modules

### Reusable Components
- [Networking Module](modules/networking/)
- [Security Groups](modules/security/)
- [Monitoring](modules/monitoring/)
- [Logging](modules/logging/)

## Usage

```bash
cd terraform/aws/eks
terraform init
terraform plan -var-file="production.tfvars"
terraform apply
```
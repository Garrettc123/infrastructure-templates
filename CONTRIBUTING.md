# Contributing to Infrastructure Templates

## Adding New Templates

### Template Structure

Each template should include:

1. **Main configuration** - Core IaC code
2. **Variables** - Input variables
3. **Outputs** - Output values
4. **README** - Usage documentation
5. **Examples** - Working examples

### Terraform Template

```
terraform/aws/new-service/
├── main.tf           # Main configuration
├── variables.tf      # Input variables
├── outputs.tf        # Outputs
├── versions.tf       # Provider versions
├── README.md         # Documentation
└── examples/
    └── basic/        # Basic example
```

### Kubernetes Template

```
kubernetes/new-app/
├── deployment.yaml
├── service.yaml
├── ingress.yaml
├── configmap.yaml
├── secrets.yaml
└── README.md
```

## Testing Requirements

### Terraform
```bash
# Validate
terraform validate

# Format check
terraform fmt -check

# Security scan
tfsec .

# Cost estimation
infracost breakdown --path .
```

### Kubernetes
```bash
# Validate manifests
kubectl apply --dry-run=client -f .

# Lint
kubectl-lint

# Security scan
kubesec scan deployment.yaml
```

## Documentation Standards

- Include usage examples
- List all variables with descriptions
- Document outputs
- Add architecture diagrams
- Include cost estimates
- Security considerations

## Pull Request Process

1. Test thoroughly
2. Update documentation
3. Add examples
4. Security scan passed
5. Cost impact documented
6. Submit PR

---
**Garrettc123 Ecosystem**
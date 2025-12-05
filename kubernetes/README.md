# Kubernetes Manifests

## Applications

- [Web Applications](applications/web/)
- [API Services](applications/api/)
- [Background Workers](applications/workers/)
- [Databases](applications/databases/)

## Infrastructure

### Ingress
- [NGINX Ingress](ingress/nginx/)
- [Traefik](ingress/traefik/)
- [Cert Manager](ingress/cert-manager/)

### Monitoring
- [Prometheus](monitoring/prometheus/)
- [Grafana](monitoring/grafana/)
- [Alert Manager](monitoring/alertmanager/)

### Logging
- [Elasticsearch](logging/elasticsearch/)
- [Fluentd](logging/fluentd/)
- [Kibana](logging/kibana/)

### Security
- [Network Policies](security/network-policies/)
- [Pod Security Policies](security/psp/)
- [RBAC](security/rbac/)

## Usage

```bash
# Deploy application
kubectl apply -f applications/web/

# Deploy monitoring
kubectl apply -f monitoring/

# Check status
kubectl get pods -n production
```
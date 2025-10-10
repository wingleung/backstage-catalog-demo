# Deployment Guide

## Kubernetes Deployment

The Payment Service is deployed to Kubernetes using Helm charts.

### Prerequisites

- Kubernetes 1.24+
- Helm 3.x
- kubectl configured for target cluster

### Deployment Steps

```bash
# Add Helm repository
helm repo add payment-service https://charts.example.com

# Install the chart
helm install payment-service payment-service/payment-service \
  --namespace production \
  --set image.tag=v1.2.3 \
  --set replicas=3
```

## Configuration

### Resource Limits

```yaml
resources:
  requests:
    memory: "512Mi"
    cpu: "500m"
  limits:
    memory: "1Gi"
    cpu: "1000m"
```

### Horizontal Pod Autoscaling

Configured to scale between 3-10 pods based on CPU and memory usage.

```yaml
autoscaling:
  enabled: true
  minReplicas: 3
  maxReplicas: 10
  targetCPUUtilizationPercentage: 70
```

## Environments

### Development
- **Cluster**: dev-cluster
- **Namespace**: development
- **Replicas**: 1

### Staging
- **Cluster**: staging-cluster
- **Namespace**: staging
- **Replicas**: 2

### Production
- **Cluster**: prod-cluster
- **Namespace**: production
- **Replicas**: 3 (min), 10 (max)

## Monitoring & Alerts

- **Prometheus**: Metrics scraped every 30s
- **Grafana Dashboard**: Payment Service Overview
- **PagerDuty**: Critical alerts routed to on-call team

## Rollback Procedure

```bash
# View deployment history
helm history payment-service -n production

# Rollback to previous version
helm rollback payment-service -n production
```

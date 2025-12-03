# Real-World Scenarios

## Overview

Complete, production-ready application stacks demonstrating how different Kubernetes resources work together.

## Scenarios Included

### 1. **Microservices Application**
Complete e-commerce platform with:
- Frontend (React/Next.js)
- API Gateway
- Multiple backend services
- Databases (PostgreSQL, MongoDB)
- Cache (Redis)
- Message queue (RabbitMQ)
- Monitoring (Prometheus + Grafana)
- Logging (EFK Stack)

### 2. **Database Deployments**
Production-ready database configurations:
- PostgreSQL with replication
- MongoDB replica set
- Redis cluster
- MySQL with backup

### 3. **Monitoring Stack**
Complete observability setup:
- Prometheus for metrics
- Grafana for visualization
- AlertManager for alerts
- Node exporter
- Kube-state-metrics

### 4. **CI/CD Pipeline**
GitOps workflow with:
- ArgoCD / Flux
- Tekton pipelines
- Harbor registry
- SonarQube

### 5. **Logging Stack**
Centralized logging:
- Elasticsearch
- Fluentd/Fluent Bit
- Kibana

## Architecture Patterns

### Microservices Pattern
```
Internet
  ↓
Ingress (TLS)
  ↓
Frontend Service
  ↓
API Gateway
  ├→ User Service → PostgreSQL
  ├→ Product Service → MongoDB
  ├→ Order Service → PostgreSQL
  ├→ Payment Service → External API
  └→ Notification Service → RabbitMQ
       ↓
     Redis Cache
```

### Security Layers
```
1. Network Policies (Zero Trust)
2. RBAC (Least Privilege)
3. Pod Security Standards
4. Secret Management (Sealed Secrets/Vault)
5. Image Scanning
6. Runtime Security (Falco)
```

### High Availability
```
- Multiple replicas (3+)
- Pod Disruption Budgets
- Anti-affinity rules
- Health probes
- Auto-scaling (HPA/VPA)
- Multi-zone deployment
```

## Deployment Checklist

### Pre-Production
- [ ] All images scanned for vulnerabilities
- [ ] Resource requests/limits set
- [ ] Health probes configured
- [ ] Network policies applied
- [ ] RBAC configured
- [ ] Secrets encrypted
- [ ] Monitoring configured
- [ ] Logging configured
- [ ] Backup strategy defined
- [ ] Disaster recovery tested

### Production
- [ ] Multiple replicas
- [ ] PodDisruptionBudgets set
- [ ] Auto-scaling configured
- [ ] Ingress with TLS
- [ ] Rate limiting enabled
- [ ] Alerts configured
- [ ] Runbooks documented
- [ ] On-call rotation defined

## Best Practices Applied

- **Infrastructure as Code**
- All resources in Git
- GitOps workflow
- Automated deployments

- **Security First**
- Network segmentation
- Least privilege access
- Encrypted secrets
- Regular security scans

- **Observability**
- Metrics collection
- Centralized logging
- Distributed tracing
- Custom dashboards

- **Resilience**
- Self-healing
- Auto-scaling
- Graceful degradation
- Circuit breakers

- **Cost Optimization**
- Right-sized resources
- Cluster autoscaling
- Spot instances
- Resource quotas

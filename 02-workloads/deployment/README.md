# Deployments

## Overview

Deployments provide declarative updates for Pods and ReplicaSets. They are the most common way to run stateless applications in Kubernetes.

## When to Use Deployments

- **Stateless applications** (web servers, APIs, microservices)
- **Applications that can scale horizontally**
- **Applications that need rolling updates**
- **Applications that don't require stable network identities**

## Key Features

- **Declarative updates**: Describe desired state, Kubernetes handles the rest
- **Rolling updates**: Zero-downtime deployments
- **Rollback**: Easy reversion to previous versions
- **Scaling**: Horizontal pod autoscaling
- **Self-healing**: Automatic pod replacement

## Deployment Strategies

### 1. **Rolling Update** (Default)
- Gradually replaces old pods with new ones
- Zero downtime
- Configurable with `maxSurge` and `maxUnavailable`

### 2. **Recreate**
- Terminates all old pods before creating new ones
- Brief downtime
- Useful when running multiple versions simultaneously is problematic

### 3. **Blue-Green** (Manual)
- Two identical environments
- Switch traffic instantly
- Easy rollback

### 4. **Canary** (Manual/Progressive)
- Gradual rollout to subset of users
- Monitor before full deployment
- Reduces risk

## Examples Included

1. `basic-deployment.yaml` - Simple deployment
2. `production-deployment.yaml` - Production-ready with all features
3. `rolling-update.yaml` - Rolling update configuration
4. `blue-green-deployment.yaml` - Blue-green strategy
5. `canary-deployment.yaml` - Canary deployment

## Best Practices

- **Use meaningful labels**
```yaml
labels:
  app: myapp
  version: v1.2.3
  environment: production
  team: backend
```

- **Set resource requests and limits**
- Ensures proper scheduling
- Prevents resource exhaustion

- **Configure health probes**
- Liveness for automatic recovery
- Readiness for traffic management

- **Use deployment strategies**
- Rolling updates for zero downtime
- Configure maxSurge and maxUnavailable

- **Implement Pod Disruption Budgets**
- Ensures availability during updates
- Protects against voluntary disruptions

## Common Commands

```bash
# Create deployment
kubectl apply -f deployment.yaml

# Check deployment status
kubectl get deployments
kubectl rollout status deployment/myapp

# Scale deployment
kubectl scale deployment/myapp --replicas=5

# Update image
kubectl set image deployment/myapp app=myapp:v2

# Rollback
kubectl rollout undo deployment/myapp
kubectl rollout undo deployment/myapp --to-revision=2

# View history
kubectl rollout history deployment/myapp

# Pause/Resume rollout
kubectl rollout pause deployment/myapp
kubectl rollout resume deployment/myapp
```

## Troubleshooting

### Pods not starting
```bash
kubectl describe deployment myapp
kubectl get pods -l app=myapp
kubectl logs <pod-name>
```

### Deployment stuck
```bash
kubectl rollout status deployment/myapp
kubectl get events --sort-by=.metadata.creationTimestamp
```

### High memory/CPU usage
```bash
kubectl top pods -l app=myapp
kubectl describe pod <pod-name>
```

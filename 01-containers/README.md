# Container Specifications

## Overview

Containers are the fundamental building blocks of Kubernetes. This section covers container configurations within Pods, including resource management, health checks, and lifecycle management.

## Key Concepts

### 1. **Resource Management**
- **Requests**: Minimum resources guaranteed
- **Limits**: Maximum resources allowed

### 2. **Health Checks**
- **Liveness Probe**: Determines if container should be restarted
- **Readiness Probe**: Determines if container can receive traffic
- **Startup Probe**: Handles slow-starting containers

### 3. **Lifecycle Hooks**
- **PostStart**: Executes after container starts
- **PreStop**: Executes before container terminates

## Examples Included

1. `basic-container.yaml` - Simple container configuration
2. `resource-limits.yaml` - CPU and memory management
3. `health-probes.yaml` - Complete health check configuration
4. `lifecycle-hooks.yaml` - Container lifecycle management
5. `multi-container-pod.yaml` - Sidecar pattern

## Best Practices

- **Always set resource requests and limits**
- Prevents resource starvation
- Enables proper scheduling
- Improves cluster stability

- **Implement health probes**
- Liveness probes for automatic recovery
- Readiness probes for traffic management
- Startup probes for slow applications

- **Use specific image tags**
- Avoid `latest` tag in production
- Ensures reproducible deployments
- Facilitates rollbacks

## Common Pitfalls

- **Not setting resource limits**
- Can cause OOM kills
- Affects other pods on the node

- **Aggressive probe settings**
- Can cause unnecessary restarts
- Set appropriate timeouts and thresholds

- **Running as root**
- Security risk
- Use non-root users when possible

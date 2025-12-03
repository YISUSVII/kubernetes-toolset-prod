# Services, Load Balancing, and Networking

## Overview

Services enable network access to a set of Pods. They provide stable endpoints for dynamic pod sets.

## Service Types

### 1. **ClusterIP** (Default)
- Internal cluster communication only
- Exposes service on cluster-internal IP
- **Use case**: Backend services, databases

### 2. **NodePort**
- Exposes service on each node's IP at a static port
- Accessible from outside cluster
- **Use case**: Development, testing, simple external access

### 3. **LoadBalancer**
- Creates external load balancer (cloud provider)
- Assigns external IP
- **Use case**: Production external services

### 4. **ExternalName**
- Maps service to DNS name
- No proxying
- **Use case**: External service integration

### 5. **Headless Service**
- ClusterIP set to None
- Direct pod DNS records
- **Use case**: StatefulSets, service discovery

## Ingress

Manages external HTTP/HTTPS access to services. Features:
- **Host-based routing**: Different domains to different services
- **Path-based routing**: Different paths to different services
- **TLS termination**: HTTPS support
- **Load balancing**: Distribute traffic

## Network Policies

Control traffic flow between pods and namespaces:
- **Ingress rules**: Incoming traffic
- **Egress rules**: Outgoing traffic
- **Default deny**: Secure by default

## Examples Included

1. `service-types.yaml` - All service types
2. `ingress-nginx.yaml` - Nginx Ingress with TLS
3. `network-policy.yaml` - Network isolation
4. `service-mesh-ready.yaml` - Service mesh annotations

## Best Practices

- **Use ClusterIP for internal services**
- More secure
- Better performance

- **Implement Network Policies**
- Defense in depth
- Limit blast radius

- **Use Ingress for HTTP(S)**
- Single entry point
- TLS termination
- Path-based routing

- **Enable session affinity when needed**
- Sticky sessions
- Stateful applications

## Common Patterns

### Microservices Communication
```
Frontend (LoadBalancer)
  ↓
API Gateway (ClusterIP + Ingress)
  ↓
Backend Services (ClusterIP)
  ↓
Database (Headless Service)
```

### Zero-Trust Network
```
Default Deny All
  ↓
Explicit Allow Rules
  ↓
Namespace Isolation
  ↓
Pod-to-Pod Encryption (Service Mesh)
```

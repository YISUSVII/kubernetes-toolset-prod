# Kubernetes Toolset - Production Ready Examples

> Your secret pendrive for Kubernetes - Real-world examples for every resource type

[![Kubernetes](https://img.shields.io/badge/Kubernetes-326CE5?style=for-the-badge&logo=kubernetes&logoColor=white)](https://kubernetes.io/)
[![License](https://img.shields.io/badge/License-MIT-blue.svg?style=for-the-badge)](LICENSE)

## Table of Contents

- [Overview](#overview)
- [Repository Structure](#repository-structure)
- [Quick Start](#quick-start)
- [Resource Categories](#resource-categories)
- [How to Use](#how-to-use)
- [Contributing](#contributing)

## Overview

This repository serves as a comprehensive reference guide for Kubernetes DevOps engineers. Each example includes:

- **Production-ready YAML manifests** - Real-world configurations
- **Detailed README explanations** - Understanding each resource
- **Best practices** - Security, performance, and reliability
- **Use cases** - When and why to use each resource
- **Common pitfalls** - What to avoid

## Repository Structure

```
kubernetes-toolset-prod/
├── 01-containers/              # Container configurations
├── 02-workloads/              # Deployments, StatefulSets, DaemonSets, Jobs
├── 03-services-networking/    # Services, Ingress, NetworkPolicies
├── 04-storage/                # PV, PVC, StorageClasses
├── 05-configuration/          # ConfigMaps, Secrets
├── 06-security/               # RBAC, SecurityContext, PodSecurityPolicies
├── 07-policies/               # ResourceQuotas, LimitRanges, PodDisruptionBudgets
├── 08-scheduling/             # Affinity, Taints, Tolerations, PriorityClasses
├── 09-cluster-admin/          # Namespaces, Nodes, ServiceAccounts
└── 10-real-world-scenarios/   # Complete application stacks
```

## Quick Start

### Prerequisites

- Kubernetes cluster (v1.35+)
- `kubectl` CLI installed
- Basic understanding of Kubernetes concepts

### Testing Examples

```bash
# Clone the repository
git clone https://github.com/yourusername/kubernetes-toolset-prod.git
cd kubernetes-toolset-prod

# Navigate to any category
cd 02-workloads/deployment

# Apply the example
kubectl apply -f example-deployment.yaml

# Check the status
kubectl get deployments
kubectl get pods
```

## Resource Categories

### 1. Containers
- Container specifications
- Resource limits and requests
- Liveness and readiness probes
- Lifecycle hooks
- Environment variables

### 2. Workloads
- **Deployments** - Stateless applications
- **StatefulSets** - Stateful applications (databases, etc.)
- **DaemonSets** - Node-level services
- **Jobs** - Batch processing
- **CronJobs** - Scheduled tasks
- **ReplicaSets** - Pod replication

### 3. Services, Load Balancing, and Networking
- **Services** - ClusterIP, NodePort, LoadBalancer
- **Ingress** - HTTP/HTTPS routing
- **NetworkPolicies** - Network security
- **Endpoints** - Service discovery
- **EndpointSlices** - Scalable service discovery

### 4. Storage
- **PersistentVolumes (PV)** - Storage resources
- **PersistentVolumeClaims (PVC)** - Storage requests
- **StorageClasses** - Dynamic provisioning
- **VolumeSnapshots** - Backup and restore

### 5. Configuration & Settings
- **ConfigMaps** - Configuration data
- **Secrets** - Sensitive information
- **HorizontalPodAutoscaler (HPA)** - Auto-scaling
- **VerticalPodAutoscaler (VPA)** - Resource optimization

### 6. Security
- **RBAC** - Role-Based Access Control
- **ServiceAccounts** - Pod identity
- **SecurityContext** - Security settings
- **PodSecurityPolicies** - Security standards
- **PodSecurityStandards** - Modern security enforcement
- **NetworkPolicies** - Network isolation

### 7. Policies
- **ResourceQuotas** - Resource limits per namespace
- **LimitRanges** - Default resource constraints
- **PodDisruptionBudgets (PDB)** - Availability guarantees

### 8. Scheduling, Preemption and Eviction
- **Node Affinity** - Pod placement preferences
- **Pod Affinity/Anti-Affinity** - Co-location rules
- **Taints and Tolerations** - Node restrictions
- **PriorityClasses** - Pod priority
- **Resource Quotas** - Resource management

### 9. Cluster Administration
- **Namespaces** - Resource isolation
- **Nodes** - Cluster infrastructure
- **ServiceAccounts** - Authentication
- **ClusterRoles** - Cluster-wide permissions
- **CustomResourceDefinitions (CRD)** - Extending Kubernetes

### 10. Real-World Scenarios
- Complete microservices stack
- Database deployments (PostgreSQL, MongoDB, Redis)
- CI/CD pipelines
- Monitoring stack (Prometheus, Grafana)
- Logging stack (ELK/EFK)
- Multi-tier applications

## How to Use

Each directory contains:

1. **README.md** - Detailed explanation of the resource type
2. **example-*.yaml** - Production-ready manifests
3. **best-practices.md** - Guidelines and recommendations
4. **troubleshooting.md** - Common issues and solutions

### Example Structure

```
02-workloads/deployment/
├── README.md                    # What is a Deployment?
├── basic-deployment.yaml        # Simple example
├── advanced-deployment.yaml     # Production-ready with all features
├── rolling-update.yaml          # Rolling update strategy
├── blue-green.yaml             # Blue-green deployment
├── canary.yaml                 # Canary deployment
├── best-practices.md           # Deployment best practices
└── troubleshooting.md          # Common issues
```

## Kubernetes v1.35 Notable Changes

This toolset targets **Kubernetes v1.35**. Key changes in v1.35 that affect these examples:

### API Changes & Promotions
- **`trafficDistribution: PreferSameZone` / `PreferSameNode`** — Graduated to GA; examples updated in `03-services-networking/service/`
- **`MaxUnavailableStatefulSet`** — Promoted to beta (enabled by default); `maxUnavailable` field added to StatefulSet rolling update examples
- **Toleration `Gt` / `Lt` operators** — New numeric comparison operators; examples added to `08-scheduling/taints-tolerations/`
- **`terminatingReplicas`** — Deployment and ReplicaSet status field promoted to beta (enabled by default)
- **`HPAConfigurableTolerance`** — Promoted to beta (enabled by default)

### Deprecations (Action Required)
- **`trafficDistribution: PreferClose`** — Deprecated in v1.35; use `PreferSameZone` instead
- **kube-proxy `ipvs` mode** — Deprecated in v1.35; migrate to `nftables` mode
- **cgroup v1** — Deprecated in v1.35; `failCgroupV1: true` is the default. Nodes running cgroup v1 with kubelet v1.35+ will fail to start unless `failCgroupV1: false` is explicitly set. Migrate to cgroup v2.

### Removed APIs (Breaking Changes)
- **`StorageVersionMigration` v1alpha1** — Removed; use `v1beta1` instead (ACTION REQUIRED before upgrade)
- **`--pod-infra-container-image` kubelet flag** — Removed; remove from kubelet configuration before upgrading (ACTION REQUIRED)


## Learning Path

**Beginner:**
1. Start with `01-containers/` and `02-workloads/deployment/`
2. Move to `03-services-networking/service/`
3. Learn `05-configuration/configmap-secrets/`

**Intermediate:**
4. Explore `04-storage/` for persistent data
5. Study `06-security/rbac/`
6. Understand `07-policies/`

**Advanced:**
7. Master `08-scheduling/` for optimization
8. Dive into `09-cluster-admin/`
9. Build complete stacks in `10-real-world-scenarios/`

## Contributing

Contributions are welcome! Please:

1. Fork the repository
2. Create a feature branch
3. Add your examples with proper documentation
4. Submit a pull request

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## Star History

If you find this repository helpful, please consider giving it a star!

## Support

- Create an issue for questions or suggestions
- Discussions for general topics
- Bug reports for errors or improvements

---

**Made with for the Kubernetes community**
